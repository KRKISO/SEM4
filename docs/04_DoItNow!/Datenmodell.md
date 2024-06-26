---
layout: default
title: 4.5 Datenmodell
parent: 4. DoItNow!
nav_order: 5
---

# 4.5 ERD (Entity-Relationship Diagram)

Das folgende ERD (Entity-Relationship Diagram) zeigt die Beziehung zwischen den Hauptentitäten der "DoItNow"-Applikation:

![ERD](../../resources/images/ERD.png)

### Beschreibung des ERD

1. **User**:
   - Jeder Benutzer kann mehrere Aufgabenlisten (TaskLists) haben.
   - Die Beziehung zwischen User und TaskList ist eine 1:n Beziehung, was bedeutet, dass ein Benutzer viele Aufgabenlisten haben kann.

2. **TaskList**:
   - Jede Aufgabenliste (TaskList) kann mehrere Aufgaben (Tasks) enthalten.
   - Die Beziehung zwischen TaskList und Task ist ebenfalls eine 1:n Beziehung, was bedeutet, dass eine Aufgabenliste viele Aufgaben enthalten kann.

3. **Task**:
   - Jede Aufgabe gehört zu einer spezifischen Aufgabenliste.

Diese Struktur ermöglicht es, die Aufgaben in der Anwendung effektiv zu organisieren und zu verwalten, indem sie in benutzerdefinierte Listen gruppiert werden.

## 4.6 Entscheidung für MySQL

### Warum MySQL?

MySQL ist eine relationale Datenbank, die sich besonders gut für Anwendungen eignet, bei denen die Datenstruktur und Beziehungen zwischen den Daten gut definiert sind. Es bietet folgende Vorteile:

- **Datenintegrität**: MySQL unterstützt Transaktionen und stellt sicher, dass die Datenkonsistenz über verschiedene Tabellen hinweg gewahrt bleibt. Dies ist wichtig, um sicherzustellen, dass die Beziehungen zwischen Benutzern, Aufgabenlisten und Aufgaben korrekt gehandhabt werden.
- **Komplexe Abfragen**: Mit MySQL können komplexe Abfragen und Joins durchgeführt werden, um effizient auf die miteinander verbundenen Daten zuzugreifen.
- **Bewährte Zuverlässigkeit**: MySQL ist eine weit verbreitete und bewährte Datenbanklösung, die robuste und zuverlässige Datenverwaltung bietet.

### Warum NoSQL nicht passend wäre

NoSQL-Datenbanken sind gut geeignet für unstrukturierte oder semi-strukturierte Daten, und sie bieten Flexibilität in der Datenspeicherung. Allerdings gibt es für dieses Projekt einige Gründe, warum eine NoSQL-Datenbank nicht ideal wäre:

- **Strukturierte Daten**: Die "DoItNow"-Applikation arbeitet mit stark strukturierten Daten, die durch klare Beziehungen definiert sind (z.B. Benutzer zu Aufgabenlisten zu Aufgaben). Eine relationale Datenbank wie MySQL passt besser zu dieser Struktur.
- **Datenintegrität und Transaktionen**: NoSQL-Datenbanken bieten oft keine Unterstützung für ACID-Transaktionen (Atomicity, Consistency, Isolation, Durability), was zu Problemen bei der Gewährleistung der Datenintegrität führen könnte.
- **Komplexität von Beziehungen**: Die Fähigkeit von SQL-Datenbanken, komplexe Beziehungen und Abfragen zu verwalten, macht sie besser geeignet für Anwendungen, bei denen diese Beziehungen von zentraler Bedeutung sind.
