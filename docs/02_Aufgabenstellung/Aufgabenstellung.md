---
layout: default
title: 2.1 Aufgabenstellung 
parent: 2. Aufgabenstellung
nav_order: 1
---

# Aufgabenstellung

##  Titel der Facharbeit
Kubernetes und DevOps Deployment 

## Ausgangslage
Ein Unternehmen möchte eine E-Commerce-Seite bereitstellen und deren Frontend statisch hosten. Gleichzeitig soll eine Basisinfrastruktur aufgebaut werden, um zukünftig Microservices für Bestell- und Kundenmanagement schnell integrieren zu können. Die Lösung soll so gestaltet werden, dass sie leicht skalierbar und wartbar ist. 

Die Arbeit nutzt moderne Cloud-Technologien und Infrastruktur-Tools, die den realen Anforderungen von Unternehmen entsprechen, und zeigt das Potential von Kubernetes und CI/CD zur Effizienzsteigerung und Kostensenkung. 

## Aufgabenstellung
Die Semesterarbeit beschäftigt sich mit der Automatisierung des Deployments und der Verwaltung einer Container-basierten Anwendung in einem Azure Kubernetes Cluster (AKS) mithilfe von Azure DevOps. Das Ziel ist es, eine skalierbare und automatisierte Plattform für eine fiktive E-Commerce-Website zu entwickeln. Der Fokus liegt auf der Bereitstellung eines Nginx-Webservers, der als Frontend für statische Inhalte (HTML-Seiten) dient. 

## Ziele
**Bereitstellung eines skalierbaren Frontend-Systems:** 
Deployment eines Nginx-Webservers im AKS-Cluster, der statische Inhalte (HTML/CSS/JavaScript) hostet. 

**Automatisierte CI/CD-Pipeline einrichten:** 
Implementierung einer CI/CD-Pipeline in Azure DevOps, die automatisch Änderungen am Frontend (HTML-Seiten) überprüft und in den Cluster deployt. 

**Monitoring und Skalierung:** 
Einrichtung eines Monitoringsystems (Prometheus/Grafana), um die Performance des Nginx-Webservers zu überwachen (z.B. Latenzzeiten, Auslastung). 
Implementierung eines Horizontal Pod Autoscalers (HPA), um bei steigender Last automatisch weitere Instanzen des Nginx-Servers bereitzustellen. 

**Wirtschaftliche Analyse:**
Bewertung der Implementierungskosten und Skalierungsmöglichkeiten, um aufzuzeigen, wie das Unternehmen durch Automatisierung und Cloud-Nutzung IT-Kosten senken und Ausfallzeiten minimieren kann. 

## Aufgaben
1. **Analyse und Planung**
   - Festlegung der Anforderungen und Funktionalitäten des Clusters
   - Planung der Architektur und der Technologien

2. **Kubernetes Cluster**
   - Bereitstellung eines skalierbaren Frontend-Systems
   - Automatisierte CI/CD-Pipeline einrichten
   - Monitoring und Skalierung
   - Wirtschaftliche Analyse

3. **Dokumentation**
   - Erstellung einer detaillierten technischen Dokumentation
   - Beschreibung der Architektur, Technologien und Implementierungsschritte
   - Aufbereitung von Screenshots und Diagrammen zur Veranschaulichung

4. **Testing und Deployment**
   - Bereitstellung der Anwendung in einer Cloud-Umgebung (z.B. Azure oder MAAS)

## Lieferbare Ergebnisse
- Voll funktionsfähiger Kubernetes Cluster mit CI/CD Pipeline
- Technische Dokumentation
- Präsentation der Arbeitsergebnisse

## Bewertungskriterien

| Kriterien                                               | Kommentare | Punkte |
|---------------------------------------------------------|------------|--------|
| **1. Substanz, Aufbau des Inhalts**                     |            | (0 bis 5 Punkte) |
| **2. Darstellung der Theorie**<br>(Form, Sprache, Quellen) |            | (0 bis 5 Punkte) |
| **3. Verknüpfung von Theorie und Praxis**<br>(formell)  |            | (0 bis 5 Punkte) |
| **4. Verknüpfung von Theorie und Praxis**<br>(fachlich) |            | (0 bis 5 Punkte) |
| **5. Reflexionstiefe**                                  |            | (0 bis 5 Punkte) |
| **Punkte gesamt**                                       | (erreichte Punktzahl) | (max. 25 Punkte) |

## Notenschlüssel:
Erreichte Punkte * 5 / max. Punkte + 1
