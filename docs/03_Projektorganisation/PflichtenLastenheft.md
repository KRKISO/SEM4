---
layout: default
title: 3.6 Pflichten und Lastenheft
parent: 3. Projektorganisation
nav_order: 6
---

# 3.6.1 Pflichtenheft

Das Pflichtenheft beschreibt die detaillierten Anforderungen und Ziele, die während der Umsetzung des Projekts erfüllt werden sollen. Es dient als verbindliche Grundlage für die Projektarbeit und legt den angestrebten Soll-Zustand fest.

## Funktionale Anforderungen / Soll-Zustand

### Sicherheit
- Einrichtung eines sicheren Kubernetes-Clusters (k0s) mit eingeschränktem Zugriff.
- Implementierung von Secrets Management zur geschützten Speicherung von sensiblen Daten (z. B. Zugangsdaten).
- Regelmäßige Überprüfung und Aktualisierung von Sicherheitskonfigurationen.
- Integration eines Monitoring-Systems zur Erkennung von sicherheitsrelevanten Vorfällen.

### Skalierbarkeit und Erweiterbarkeit
- Der Cluster muss horizontal skalierbar sein, um steigenden Anforderungen gerecht zu werden.
- Integration einer modularen Infrastruktur, die eine einfache Hinzufügung neuer Dienste oder Microservices ermöglicht.
- Aufbau einer CI/CD-Pipeline, die automatisierte Deployments und Tests unterstützt.

### DevOps-Integration
- Einrichtung einer Azure DevOps-Pipeline zur kontinuierlichen Integration und Bereitstellung (CI/CD).
- Automatisierte Tests und Deployments als fester Bestandteil des Entwicklungsprozesses.
- Möglichkeit, Container-Images direkt aus dem Repository in den Cluster zu deployen.

## Nicht-funktionale Anforderungen
- Verfügbarkeit von mindestens 99,5 % während der Betriebszeit.
- Reduktion der Deployment-Zeit auf unter 5 Minuten.
- Dokumentation der gesamten Umgebung und Konfiguration.
- Unterstützung für mehrere Entwickler durch zentrale Tools und gemeinsame Standards.

---

# 3.6.2 Lastenheft

Das Lastenheft beschreibt die Ausgangslage des Projekts sowie die Anforderungen und Rahmenbedingungen, die vom Projektteam umzusetzen sind. Es bildet die Grundlage für die Planung und Umsetzung.

## Ausgangslage / Ist-Zustand
- Es gibt keinen k0s-Cluster, der als Kubernetes-Plattform dient.
- Ein keinen Azure DevOps-Account.
- Es gibt keine bestehende Automatisierung für die Bereitstellung containerisierter Anwendungen.
- Die Infrastruktur muss noch auf Sicherheit und Skalierbarkeit optimiert werden.

## Funktionale Anforderungen
- Einrichtung eines Kubernetes-Clusters mit k0s zur lokalen Verwaltung von Containern.
- Konfiguration von Azure DevOps als CI/CD-Tool für automatisierte Build-, Test- und Deployment-Prozesse.
- Bereitstellung eines Nginx-Webservers zur Auslieferung statischer Inhalte.
- Monitoring- und Logging-Lösungen zur kontinuierlichen Überwachung der Anwendungen und Infrastruktur.

## Nicht-funktionale Anforderungen
- Sicherstellung einer kosteneffizienten Implementierung unter Berücksichtigung der vorhandenen Ressourcen.
- Dokumentation der verwendeten Technologien, Tools und Konfigurationen.
- Benutzerfreundlichkeit und Übersichtlichkeit bei der Verwaltung der Infrastruktur.
- Optimierung der Plattform für zukünftige Erweiterungen, wie die Integration neuer Microservices oder Anwendungen.
