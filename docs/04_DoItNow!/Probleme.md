---
layout: default
title: 4.9 Probleme bei der Umsetzung
parent: 4. DoItNow!
nav_order: 9
---

# 4.9 Probleme bei der Umsetzung
Wie üblich bei einem Software Projekt, tauchen diverse Probleme bei der Umsetzung auf.
Ich möchte folgend auf zwei spezifische Probleme eingehen.

## 4.9.1 Einbinden der Externen API
Um dem Benutzer bei erstellung der Taskliste den Standort anzeigen zu können, musste ich eine externe API integrieren um die IP Adresse aufzulösen.
Da beim Applikations Container lediglich die IP-Adresse des NGINX Proxys ankommt, musste ich hier eine Java Script lösung implementieren um die IP Adresse des effektiven Benutzers zu erhalten.

Das folgnede Java Script wird im Browser des Benutzers ausgeführt, aus diesem Grund wird hier die richtige IP-Adresse anhand eines AWS Services des Benutzers aufgelöst.
Das Java Script wurde auf der Main Page der Applikation eingebunden und sendet anschliessend die IP an eine neu implementierte API in der Applikation. (/store_ip)

```
    <script src="/static/js/script.js"></script>
    <link rel="stylesheet" href="/static/css/style.css">
    <script>
      async function sendIPAddress() {
          try {
              let response = await fetch('https://checkip.amazonaws.com');
              if (!response.ok) {
                  throw new Error(`Network response was not ok ${response.statusText}`);
              }
              let ip = await response.text();
              ip = ip.trim();
  
              // Send the IP address to the server to store in the session
              await fetch('/store_ip', {
                  method: 'POST',
                  headers: {
                      'Content-Type': 'application/json'
                  },
                  body: JSON.stringify({ ip_address: ip })
              });
          } catch (error) {
              console.error('There was a problem:', error);
          }
      }
  
      window.onload = sendIPAddress;
```

Diese Umsetzung ist meiner Meinung nach nicht optimal aufgrund von Performance und Sicherheit.

## 4.9.2 Testing Implementierung
Da für die Applikation Flask Sessions verwendet wurde, musste dies auch im Testing inkludiert werden um sich korrekt Authentifizieren zu können.

Ein CSRF-Token (Cross-Site Request Forgery Token) ist ein Sicherheitsmechanismus, der verwendet wird, um Cross-Site Request Forgery-Angriffe zu verhindern. CSRF-Angriffe zielen darauf ab, eine authentifizierte Sitzung eines Benutzers auszunutzen, um unerwünschte Aktionen auf einer Webanwendung durchzuführen. Der Angreifer täuscht den Benutzer dazu, eine schädliche Anfrage zu senden, während dieser bei der Zielanwendung angemeldet ist.

Ein CSRF-Token ist ein einzigartiger, geheim gehaltener Wert, der vom Server generiert und an den Client gesendet wird. Dieser Token muss bei jeder zustandsverändernden Anfrage (z.B. POST, PUT, DELETE) zurück an den Server gesendet werden. Da der Token für jede Sitzung einzigartig ist, kann der Server überprüfen, ob die Anfrage vom authentifizierten Benutzer stammt und nicht von einem Angreifer.

In der Flask-Anwendung wurden CSRF-Tokens verwendet, um sicherzustellen, dass nur legitime Anfragen verarbeitet werden. Hier wird beschrieben, wie dies in den Tests umgesetzt wurde, um authentifizierte Aktionen zu simulieren.
Zuerst wird eine Funktion definiert, um den CSRF-Token von einer bestimmten URL abzurufen. Diese Funktion sendet eine GET-Anfrage an die URL, analysiert die Antwort und extrahiert den CSRF-Token aus dem HTML-Formular.

```python
def get_csrf_token(client, url):
    """Holt den CSRF-Token von der angegebenen URL."""
    response = client.get(url)
    soup = BeautifulSoup(response.data, 'html.parser')
    csrf_token = soup.find('input', {'name': 'csrf_token'})['value']
    return csrf_token
```

Bei zustandsverändernden Anfragen wird der CSRF-Token in den Anfragedaten mitgesendet. Dies stellt sicher, dass die Anfragen authentifiziert sind und vom Server akzeptiert werden.

Beispiel: Aktualisierung des Benutzerkontos
Dieser Test überprüft, ob ein authentifizierter Benutzer seine Kontodaten aktualisieren kann. Der CSRF-Token wird von der Konto-Seite abgerufen und zusammen mit den anderen Formulardaten gesendet.

```python
def test_update_account(authenticated_client):
    logger.info("Startet Test: test_update_account")
    
    # Sicherstellen, dass die Sitzung authentifiziert ist
    with authenticated_client.session_transaction() as session:
        session['username'] = 'test_user1'

    # CSRF-Token von der Konto-Seite abrufen
    csrf_token = get_csrf_token(authenticated_client, url_for('user.account'))
    logger.info(f"CSRF-Token abgerufen: {csrf_token}")
    
    # CSRF-Token und korrekte Benutzerdaten einschließen
    response = authenticated_client.post("/account", data={
        "csrf_token": csrf_token,
        "username": "new_test_user1",
        "password": "password1",  # Aktuelles Passwort zur Bestätigung der Änderungen
        "new_password": "new_password1"  # Neues Passwort
    }, follow_redirects=True)  # Weiterleitungen folgen, um die endgültige Antwort zu erfassen
    
    # Sitzung nach der Anfrage protokollieren
    with authenticated_client.session_transaction() as session:
        flash_messages = session.get('_flashes', [])
        logger.info(f"Flash-Nachrichten: {flash_messages}")

        # Überprüfen, ob die spezifische Erfolgsmeldung in den Flash-Nachrichten enthalten ist
        success_message_found = any(
            category == 'success' and 'Account updated successfully.' in message
            for category, message in flash_messages
        )
        
        assert success_message_found, "Erwartete Erfolgsmeldung nicht in den Flash-Nachrichten gefunden."
        logger.info("test_update_account bestanden.")
```