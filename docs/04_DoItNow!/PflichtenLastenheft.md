---
layout: default
title: 4.2 Pflichten und Lastenheft
parent: 4. DoItNow!
nav_order: 2
---


# Pflichtenheft

## Funktionale Anforderungen / Soll-Zustand

### Aufgabenverwaltung
- Benutzer sollen Aufgaben und Aufgabenlisten hinzufügen, bearbeiten, markieren und löschen können.

### Benutzerverwaltung
- Benutzer sollen sich registrieren, anmelden und ihr Profil verwalten können.

### API-Schnittstelle
- Die Flask API soll alle CRUD-Operationen für Aufgaben und Benutzer bereitstellen.
- Die API soll sicherstellen, dass nur autorisierte Benutzer auf ihre eigenen Daten zugreifen können.
- Es soll eine externe API eingebunden werden, um den Standort des Benutzers bei TaskListen anzuzeigen. 

### Sicherheit
- Zugriffsrechte sollen gemäß den Prinzipien der minimalen Berechtigung gewährt werden.

### Skalierbarkeit und Erweiterbarkeit
- Die Microservices-Architektur soll eine einfache Skalierung der Anwendung ermöglichen.
- Die Anwendung soll flexibel genug sein, um zukünftige Erweiterungen wie zusätzliche Funktionen oder Integrationen zu unterstützen.

## Nicht-funktionale Anforderungen
- **Performance:** Die Anwendung soll schnell reagieren und effizient arbeiten, auch bei einer wachsenden Anzahl von Benutzern und Aufgaben.
- **Benutzeroberfläche:** Das Frontend soll intuitiv und benutzerfreundlich gestaltet sein.
- **Sicherheit:** Daten müssen sicher gespeichert und verarbeitet werden, um Datenschutz und -sicherheit zu gewährleisten.
- **Wartbarkeit:** Der Code soll gut strukturiert und dokumentiert sein, um eine einfache Wartung und Weiterentwicklung zu ermöglichen.


# Lastenheft

## Ausgangslage / Ist-Zustand

Die derzeitige Situation zeigt, dass es keine zentralisierte Plattform gibt, die eine einfache Verwaltung und Organisation von Aufgaben ermöglicht. Bestehende Lösungen sind entweder zu komplex oder bieten nicht genügend Flexibilität für verschiedene Benutzeranforderungen.

## Funktionale Anforderungen

### Todolist Applikation
Es gibt bisher keine Applikation, somit müssen alle Anforderungnen von Grund auf aufgebaut werden.

## Nicht-funktionale Anforderungen

- **Performance:** Die aktuelle Lösung ist anfällig für Leistungsengpässe bei steigender Benutzerzahl.
- **Benutzeroberfläche:** Die Benutzeroberfläche ist einfach und funktional, jedoch nicht optimiert für mobile Geräte.
- **Sicherheit:** Es gibt keine speziellen Sicherheitsvorkehrungen für den Schutz von Benutzerdaten.
- **Wartbarkeit:** Der vorhandene Code ist einfach, aber schwer erweiterbar und anpassbar.

