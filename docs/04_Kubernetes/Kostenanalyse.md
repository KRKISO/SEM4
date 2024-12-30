---
layout: default
title: 4.3 Kostenanalyse
parent: 4. Kubernetes und DevOps
nav_order: 3
---

# 4.3 Fiktive Kostenanalyse für die Semesterarbeit

Die Kostenanalyse vergleicht die monatlichen Ausgaben zwischen einem Azure Kubernetes Cluster (AKS) und einem kostenfrei zur Verfügung gestellten Kubernetes-Cluster der Schule. Ziel ist es, die wirtschaftlich sinnvollere Option für das Projekt zu identifizieren. Dabei werden auch zusätzliche Kosten für den Azure DevOps Account und die Azure Container Registry berücksichtigt.

## Vergleich der monatlichen Kosten

| **Komponente**                      | **Azure Kubernetes Cluster (AKS)** | **Kostenloser Schul-Cluster** |
|-------------------------------------|------------------------------------|--------------------------------|
| **Kubernetes Cluster**              | 150 € - 300 € (je nach Größe)      | 0 €                            |
| **Azure DevOps (Free Tier)**        | 0 €                                | 0 €                            |
| **Azure Container Registry (Basic)**| 4,65 €                             | 4,65 €                         |
| **Netzwerk- und Speicherressourcen**| 50 € - 100 €                       | 0 €                            |
| **Gesamtkosten**                    | **204,65 € - 404,65 €**            | **4,65 €**                     |

### Erläuterungen

1. **Kubernetes Cluster:**
   - **Azure Kubernetes Cluster (AKS):** Die Kosten variieren je nach Clustergröße und den gewählten Konfigurationen. Für kleine bis mittlere Projekte werden Kosten von mindestens 150 € pro Monat erwartet, können jedoch bei höherer Auslastung auf bis zu 300 € steigen.
   - **Schul-Cluster:** Dieser Cluster wird kostenfrei zur Verfügung gestellt und verursacht daher keine zusätzlichen monatlichen Ausgaben.

2. **Azure DevOps Account:**
   - Das Free-Tier von Azure DevOps reicht für dieses Projekt aus und ist in beiden Szenarien kostenlos.

3. **Azure Container Registry:**
   - Die Basisversion der Azure Container Registry (Basic Tier) kostet monatlich 4,65 € und ist notwendig, um Container-Images für die Deployments zu speichern.

4. **Netzwerk- und Speicherressourcen:**
   - In einem Azure Kubernetes Cluster fallen zusätzliche Kosten für Netzwerk- und Speicherressourcen an, während der Schul-Cluster diese Ressourcen bereits integriert und kostenlos anbietet.

### Fazit

Die Analyse zeigt, dass die Nutzung eines Azure Kubernetes Clusters (AKS) für das Projekt mit erheblichen Kosten verbunden wäre. Im Gegensatz dazu bietet der kostenfreie Schul-Cluster eine kosteneffiziente Alternative, da nur die minimalen Ausgaben für die Azure Container Registry anfallen. Für die Anforderungen der Semesterarbeit ist der Schul-Cluster daher die wirtschaftlich sinnvollere Lösung.
