---
layout: default
title: 3.4 Metoden & Werkzeuge
parent: 3. Projektorganisation
nav_order: 4
---

# 3.4 Metoden & Werkzeuge

Das Projekt wird mit hilfe von verschiedensten Tools und Methoden durchgeführt.

## 3.4.1 Jira

Jira ermöglicht es Struktur in das Projekt zu bringen. Es wird ein Scrum/Kanban Board erstellt um den Fortschritt zu prüfen.

SprintBoards/Kanban besteht aus sogenannten Statis. Diese Statis wurden wie folgt festgelgegt:

| **Todo**                                            | **in Bearbeitung**      | **Validieren**                       | **Abgeschlossen**      |  
| --------------------------------------------------- | ----------------------- | ------------------------------------ | ---------------------- |
| Offene Tasks, welche noch bearbeitet werden müssen. | Task in bearbeitung.    | Task wird validiert und Kontrolliert | Task ist Abgeschlossen |

### Tasks

Jeder Task wird mit den folgenden Tags ausgestattet um diesen zu speizifieren.

| **Tag**                                  | **Beschreibung**                                                                                      |
| ---------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Assignees**                            | Person, die für die Umsetzung der Aufgabe verantwortlich ist.                                         |
| **Status**                               | Der aktuelle Fortschritt der Aufgabe: Draft, Backlog, To Do, In Progress, Waiting, Done.              |
| **Epic**                                 | Ein übergeordnetes Ticket, das größere Arbeitsaufgaben zusammenfasst, die in kleinen Schritten bearbeitet werden. |
| **Linked Pull Requests / Repository Branch** | Verknüpfung mit den entsprechenden Git-Branches.                                                      |
| **Start Date**                           | Das Datum, an dem die Arbeit an der Aufgabe beginnt.                                                  |
| **End Date**                             | Das Datum, an dem die Arbeit an der Aufgabe abgeschlossen sein soll.                                   |
| **Priority**                             | Die Dringlichkeit der Aufgabe: Low, Normal, High.                                                     |
| **Sprint**                               | Verknüpfung mit dem Sprint, in dem die Aufgabe durchgeführt wird.                                      |
| **Story Points**                         | Eine Schätzung des Aufwands, basierend auf Komplexität und Zeitaufwand.                                |


## 3.4.2 GitHub Repository & Pages

Die Dokumentation dieser Arbeit wird in Markdown geschrieben und in einem Git-Repository auf GitHub verwaltet. Dieses Repository dient als zentraler Speicherort für alle Projektdateien und ermöglicht eine einfache Versionierung und Zusammenarbeit.

Mit [GitHub Pages](https://pages.github.com/) kann diese Arbeit als statische Website veröffentlicht und betrachtet werden. Es bietet eine unkomplizierte Möglichkeit, Webinhalte direkt aus einem GitHub-Repository zu hosten und ist ideal für statische Seiten.

[Jekyll](https://jekyllrb.com/) wird verwendet, um die in Markdown geschriebenen Dateien in HTML umzuwandeln und ein passendes Design zu integrieren. Jekyll ermöglicht es, das gesamte User Interface der Website einfach zu gestalten und anzupassen.

## 3.4.3 GitLab Repository

Das eigentliche Projekt wird in einem GitLab Repository umgesetzt aufgrund der bisherigen Erfahrung mit GitLab. 

[GitLab DOITNOW! Projekt](https://gitlab.com/it-cne23/doitnow)