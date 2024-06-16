---
layout: default
title: 2.6 Risikomanagement
parent: 2. Projektorganisation
nav_order: 6
---

# 2.6 Risikomanagement

Für die Planung und Durchführung dieser Semesterarbeit wird Jira von Atlassian verwendet. Diese umfassende Softwarelösung erlaubt die Verwaltung von Risiken als separate "Issue-Typen", die nahtlos mit anderen Aufgaben verknüpft werden können. Zur Unterstützung wird Jira durch die App "Risk Manager" von SoftComply ergänzt, welche eine detaillierte Übersicht der Risiken in Form von Tabellen und grafischen Darstellungen ermöglicht.

![2024_jira_riskikomanagement](../../resources/images/Jira_Risikomanagement.png)

## Risikokategorien

Die Risiken werden nach ihrer Eintrittswahrscheinlichkeit und möglichen Auswirkungen bewertet. Diese Bewertung hilft bei der Priorisierung und der Entscheidung, wie mit den Risiken umgegangen werden soll. Nachdem Gegenmaßnahmen ergriffen wurden, wird das Restrisiko neu eingeschätzt, um die verbleibenden Gefahren besser zu verstehen.

### Kategorien der Eintrittswahrscheinlichkeit

- **Frequent / Häufig**: Hohe Wahrscheinlichkeit, dass das Risiko eintritt (z.B. regelmäßig bei jedem Sprint).
- **Probable / Wahrscheinlich**: Realistische Chance, dass das Risiko auftritt (z.B. einmal pro Projektphase).
- **Occasional / Gelegentlich**: Das Risiko tritt sporadisch auf (z.B. einmal pro Projekt).
- **Remote / Selten**: Das Risiko ist unwahrscheinlich, kann aber vorkommen (z.B. einmal in einigen Jahren).
- **Improbable / Unwahrscheinlich**: Sehr geringe Wahrscheinlichkeit (z.B. einmal in zehn Jahren).

### Kategorien der Auswirkung

- **Negligible / Unbedeutend**: Geringfügige Auswirkungen (z.B. leichte Verzögerung von wenigen Stunden).
- **Minor / Gering**: Geringfügige negative Auswirkungen (z.B. zusätzliche Arbeit von ein bis zwei Tagen).
- **Serious / Ernsthaft**: Bedeutende Auswirkungen, die das Projekt merklich beeinträchtigen können (z.B. Verzögerung von mehreren Tagen).
- **Critical / Kritisch**: Schwere Auswirkungen, die den Projektfortschritt erheblich stören (z.B. Verzögerung um Wochen).
- **Catastrophic / Katastrophal**: Sehr schwerwiegende Auswirkungen, die zum Scheitern des Projekts führen könnten (z.B. Abbruch des Projekts).

## Zusätzliche Risiken

Neben den allgemeinen Risiken gibt es spezifische Herausforderungen in diesem Projekt:

- **Technische Kompetenzlücken**: Da ich kein Fullstack-Entwickler bin, könnte es Schwierigkeiten geben, ein reibungslos funktionierendes Frontend zu entwickeln. Besonders Herausforderungen könnten bei der Arbeit mit React auftreten, was den Entwicklungsprozess verzögern könnte.
- **Zeitdruck**: Das Projekt muss bis zum 05.07.2024 abgeschlossen sein. Dieser enge Zeitrahmen kann den Druck erhöhen und möglicherweise die Qualität der Arbeit beeinträchtigen.

## Risikomanagement-Prozess

Die Risiken werden wöchentlich im PO-Sync-Meeting gemeinsam mit dem Business Owner besprochen und bewertet. Diese regelmäßige Besprechung stellt sicher, dass alle Risiken kontinuierlich überwacht und entsprechend ihrer Priorität bearbeitet werden. Die automatisierte Risikomatrix bietet eine umfassende Übersicht über alle aktuellen Risiken und ermöglicht eine effiziente Analyse. Zusätzlich können die Risiken in verschiedenen Formaten exportiert oder in tabellarischer Form angezeigt werden.

## Kostenanalyse

### Problemstellung / Ausgangslage

"DOITNOW!" hat festgestellt, dass viele der derzeit verfügbaren ToDo-Listen-Anwendungen zu komplex und überladen sind. Ziel dieser Semesterarbeit ist es, eine minimalistische Anwendung zu entwickeln, die den Benutzern ermöglicht, ihre Aufgaben effizient zu verwalten und abzuschließen.

### Potential der Semesterarbeit

Die Entwicklung einer Microservices-Architektur bietet "DOITNOW!" die Flexibilität, schnell auf zukünftige Anforderungen zu reagieren und die Anwendung bei Bedarf zu erweitern. Docker ermöglicht es, die verschiedenen Komponenten der Anwendung isoliert und effizient bereitzustellen. Die Verwendung von Flask API für das Backend und React für das Frontend ermöglicht eine klare Trennung zwischen Backend- und Frontend-Logik, was die Wartbarkeit und Skalierbarkeit der Anwendung verbessert. Zudem wird eine CI/CD-Pipeline über GitLab eingerichtet, die den Entwicklungs- und Deployment-Prozess vereinfacht.

### Fiktive Kostenanalyse für die Semesterarbeit

Da es sich um eine akademische Arbeit handelt und die Arbeit nicht in den Echtbetrieb übergeht, wird hier eine fiktive Kostenanalyse durchgeführt. Die Semesterarbeit ist auf maximal 50 Arbeitsstunden begrenzt. Die Berechnung erfolgt in CHF, wobei ein Stundensatz von 50 CHF angesetzt wird.

- **Entwicklungskosten**:
  - Backend-Entwicklung mit Flask: 15 Stunden x 50 CHF/Stunde = 750 CHF
  - Frontend-Entwicklung mit React: 20 Stunden x 50 CHF/Stunde = 1.000 CHF
  - Integration und Testing der Microservices: 10 Stunden x 50 CHF/Stunde = 500 CHF

- **Infrastrukturkosten**:
  - Nutzung von Docker: 50 CHF (für lokale Testumgebungen und Einrichtung)
  - Testlaufzeiten in der Cloud (AWS): 100 CHF (für Hosting und Skalierung während der Tests)

- **CI/CD-Setup-Kosten**:
  - Einrichtung der CI/CD-Pipeline über GitLab: 3 Stunden x 50 CHF/Stunde = 150 CHF
  - Wartung und Anpassungen: 2 Stunden x 50 CHF/Stunde = 100 CHF

- **Lizenz- und Abonnementkosten**:
  - Jira Software Lizenz: 10 CHF/Monat x 3 Monate = 30 CHF
  - Nutzung der "Risk Manager"-App von SoftComply: 5 CHF/Monat x 3 Monate = 15 CHF
  - GitHub Packages für Container-Images: 5 CHF/Monat x 3 Monate = 15 CHF

- **Schulungskosten**:
  - Einarbeitung in React und CI/CD-Management: 5 Stunden x 50 CHF/Stunde = 250 CHF

- **Risiken und Unvorhergesehenes**:
  - Budgetreserve für technische Herausforderungen und Anpassungen: 100 CHF

**Gesamtkosten: 3.060 CHF**

Diese Kostenanalyse bietet eine Übersicht über die erwarteten Aufwände und ermöglicht eine gezielte Ressourcenplanung für das Projekt.
