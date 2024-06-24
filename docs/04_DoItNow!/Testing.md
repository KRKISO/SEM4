---
layout: default
title: 4.7 Testing
parent: 4. DoItNow!
nav_order: 7
---

# 4.7 Testing

In diesem Abschnitt wird beschrieben, wie das Testing der `DoItNow!`-Anwendung mithilfe von GitLab CI/CD und pytest durchgeführt wird. Zusätzlich wird auf die Implementierung des Testings in der Anwendung eingegangen.

## GitLab CI/CD Pipeline

Die `DoItNow!`-Anwendung verwendet GitLab CI/CD, um eine kontinuierliche Integration und Bereitstellung zu gewährleisten. Die Konfiguration für die CI/CD-Pipeline ist in der Datei `.gitlab-ci.yml` definiert. Diese Datei steuert, wie die verschiedenen Phasen der Pipeline ausgeführt werden, einschließlich des Testings.

### Struktur der `.gitlab-ci.yml` Datei

Die `.gitlab-ci.yml` Datei definiert verschiedene Stages wie `build`, `test` und `deploy`. Hier ist ein Beispiel für die `test`-Stage:

```yaml
stages:
  - build
  - test
  - deploy

unit-test-job:
    stage: test    # It only starts when the job in the build stage completes successfully.
    image: python:3.10
    before_script:
        - pip install -r requirements.txt
        - pip install pytest
    script:
        - echo "Running unit tests... This will take about 60 seconds."
        - python -m pytest
```

### Erklärung der `test`-Stage

- **stage**: Die Pipeline-Phase, in der dieser Job ausgeführt wird. Hier wird `test` angegeben.
- **image**: Das Docker-Image, das für diesen Job verwendet wird. In diesem Fall wird ein Python 3.10 Image verwendet.
- **script**: Eine Liste von Befehlen, die in dieser Stage ausgeführt werden. Es werden die Abhängigkeiten installiert und pytest ausgeführt.

## Testing mit pytest

Für das Unit-Testing der `DoItNow!`-Anwendung wird pytest verwendet. pytest ist ein weit verbreitetes Test-Framework für Python, das einfach zu verwenden ist und viele erweiterte Funktionen bietet.

### Konfiguration von pytest

Die `pytest.ini` Datei im Projekt enthält Konfigurationseinstellungen für pytest:

```ini
[pytest]
log_cli = true
log_cli_level = INFO
log_cli_format = %(asctime)s - %(name)s - %(levelname)s - %(message)s
log_cli_date_format = %Y-%m-%d %H:%M:%S
log_level = INFO
```

Diese Konfiguration beschreibt hauptsächlich das Logging, um bei der Pipeline ausführung einen detailierten log zu erhalten.

### Testimplementierung in der Anwendung

Die Testimplementierung in der `DoItNow!`-Anwendung umfasst mehrere Dateien, die spezifische Aufgaben im Testprozess übernehmen:

#### `Dockerfile.test`

Diese Dockerfile wird verwendet, um eine Umgebung für das Testen der Anwendung zu erstellen. Sie enthält alle notwendigen Anweisungen, um die Testumgebung zu konfigurieren. Hier ist ein typisches Beispiel, wie diese Datei aussehen könnte:

#### `compose.test.yaml`

Diese Docker Compose-Datei wird verwendet, um die Testdienste zu orchestrieren. Sie definiert, wie verschiedene Container interagieren, um die Testumgebung zu simulieren. Ein Beispiel für eine solche Datei könnte sein:

#### `app/test/conftest.py`

Die `conftest.py` Datei definiert allgemeine Fixtures und Konfigurationen, die von pytest verwendet werden. Diese Datei könnte z.B. eine Fixture für die Datenbankverbindung enthalten:


#### `app/test/create_test_data.py`

Diese Datei enthält Funktionen zur Erstellung von Testdaten. Diese Daten werden verwendet, um sicherzustellen, dass die Tests unter kontrollierten Bedingungen ausgeführt werden:


#### `app/test/test_app.py`

In dieser Datei sind die eigentlichen Tests der Anwendung implementiert. Sie enthält verschiedene Testfälle, um die Funktionalität der Anwendung zu überprüfen:

## Weitere Informationen

Die beschriebenen Skripts sind im GitLab Projekt abgelegt.

[GitLab DOITNOW!](https://gitlab.com/it-cne23/doitnow/-/tree/main/app/test?ref_type=heads)
