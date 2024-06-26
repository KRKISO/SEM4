---
layout: default
title: 4.6 API Documentation
parent: 4. DoItNow!
nav_order: 6
---

# 4.6 API Documentation für "DoItNow"

Die "DoItNow"-Applikation stellt eine RESTful API bereit, die es ermöglicht, die verschiedenen Funktionen der Anwendung programmatisch zu nutzen. Diese API ermöglicht die Interaktion mit Benutzerdaten, Aufgabenlisten und Aufgaben sowie das Senden von Benachrichtigungen.

Die API ist nach dem OpenAPI-Standard definiert, was die Integration und Nutzung durch verschiedene Clients erleichtert. Im Folgenden sind die Hauptkomponenten und Routen der API beschrieben.

<iframe src="../../resources/artifacts/swagger.html" width="100%" height="600px">
</iframe>

## 4.6.1 API-Struktur

Die API der "DoItNow"-Applikation ist in mehrere Hauptbereiche unterteilt:

1. **Benutzerverwaltung**
   - Registrierung und Authentifizierung von Benutzern.
   - Verwaltung der Benutzersitzungen.

2. **Aufgabenverwaltung**
   - Erstellen, Abrufen, Aktualisieren und Löschen von Aufgaben.
   - Verwaltung von Aufgabenlisten.

3. **Benachrichtigungen**
   - Senden von Benachrichtigungen basierend auf bestimmten Ereignissen.

## API-Routen

### Benutzerverwaltung

#### POST /auth/register

- **Beschreibung:** Registriert einen neuen Benutzer.
- **Parameter:** 
  - `username` (string): Der Benutzername des neuen Benutzers.
  - `email` (string): Die E-Mail-Adresse des neuen Benutzers.
  - `password` (string): Das Passwort des neuen Benutzers.
- **Antworten:**
  - `201 Created`: Benutzer erfolgreich registriert.
  - `400 Bad Request`: Ungültige Eingabedaten.

#### POST /auth/login

- **Beschreibung:** Authentifiziert einen Benutzer und startet eine Sitzung.
- **Parameter:** 
  - `username` (string): Der Benutzername.
  - `password` (string): Das Passwort.
- **Antworten:**
  - `200 OK`: Benutzer erfolgreich authentifiziert.
  - `401 Unauthorized`: Ungültige Anmeldeinformationen.

#### POST /auth/logout

- **Beschreibung:** Beendet die Benutzersitzung.
- **Antworten:**
  - `204 No Content`: Sitzung erfolgreich beendet.

### Aufgabenverwaltung

#### GET /tasks

- **Beschreibung:** Gibt eine Liste der Aufgaben des Benutzers zurück.
- **Parameter:**
  - `listId` (string, optional): Die ID der Aufgabenliste, aus der die Aufgaben abgerufen werden sollen.
- **Antworten:**
  - `200 OK`: Aufgabenliste erfolgreich abgerufen.
  - `404 Not Found`: Aufgabenliste nicht gefunden.

#### POST /tasks

- **Beschreibung:** Erstellt eine neue Aufgabe.
- **Parameter:**
  - `title` (string): Der Titel der Aufgabe.
  - `description` (string, optional): Eine Beschreibung der Aufgabe.
  - `dueDate` (string, optional): Das Fälligkeitsdatum der Aufgabe.
  - `listId` (string, optional): Die ID der Aufgabenliste, zu der die Aufgabe hinzugefügt wird.
- **Antworten:**
  - `201 Created`: Aufgabe erfolgreich erstellt.
  - `400 Bad Request`: Ungültige Eingabedaten.

#### PUT /tasks/{taskId}

- **Beschreibung:** Aktualisiert eine bestehende Aufgabe.
- **Parameter:**
  - `taskId` (string): Die ID der zu aktualisierenden Aufgabe.
  - `title` (string, optional): Der neue Titel der Aufgabe.
  - `description` (string, optional): Die neue Beschreibung der Aufgabe.
  - `dueDate` (string, optional): Das neue Fälligkeitsdatum der Aufgabe.
- **Antworten:**
  - `200 OK`: Aufgabe erfolgreich aktualisiert.
  - `404 Not Found`: Aufgabe nicht gefunden.

#### DELETE /tasks/{taskId}

- **Beschreibung:** Löscht eine bestehende Aufgabe.
- **Parameter:**
  - `taskId` (string): Die ID der zu löschenden Aufgabe.
- **Antworten:**
  - `204 No Content`: Aufgabe erfolgreich gelöscht.
  - `404 Not Found`: Aufgabe nicht gefunden.

### Benachrichtigungen

#### POST /notifications/send

- **Beschreibung:** Sendet eine Benachrichtigung an den Benutzer.
- **Parameter:**
  - `userId` (string): Die ID des Benutzers, der die Benachrichtigung erhält.
  - `message` (string): Die Nachricht, die gesendet werden soll.
- **Antworten:**
  - `202 Accepted`: Benachrichtigung erfolgreich gesendet.
  - `400 Bad Request`: Ungültige Eingabedaten.

## 4.6.2 Nutzung der API

Die API ist über HTTP-Anfragen zugänglich und verwendet JSON für den Datenaustausch. Jede Route wird über die entsprechenden HTTP-Methoden (GET, POST, PUT, DELETE) aufgerufen. Die Authentifizierung und Autorisierung erfolgen durch Flask-Sessions, die bei der Anmeldung erstellt und bei jedem nachfolgenden Aufruf überprüft werden.

Diese OpenAPI-Spezifikation ermöglicht es, die API leicht in verschiedene Anwendungen und Systeme zu integrieren und bietet Entwicklern eine umfassende Dokumentation und interaktive Nutzungsmöglichkeit über die Swagger-Oberfläche.

---

