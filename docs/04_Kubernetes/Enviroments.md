---
layout: default
title: 4.8 Test und Produktionsumgebung
parent: 4. Kubernetes und DevOps
nav_order: 8
---

# 4.8 Test und Produktionsumgebung

## 4.8.1 Testumgebung vs. Produktionsumgebung

Das aktuelle Setup ist als **Testumgebung** konzipiert und **nicht für den produktiven Einsatz geeignet**. Es fehlen wichtige Sicherheitsmaßnahmen, Monitoring-Anpassungen und Konzepte zur Skalierung in einem realen Betrieb.  

Ein produktives Kubernetes-Setup benötigt verschiedene **Environments (Umgebungen)**, um eine **sichere, stabile und kontrollierte Entwicklung und Bereitstellung** von Anwendungen zu gewährleisten.


## 4.8.2 Mehrere Environments umsetzen

Eine typische Architektur für verschiedene Umgebungen umfasst mindestens folgende Stufen:

1. **Development (dev):**  
   - Entwickler testen Code in einer nicht stabilen Umgebung.  
   - Automatische Deployments nach jedem Commit.  
   - Nutzung von Mock-Datenbanken oder Test-APIs.

2. **Testing / Staging (test/stage):**  
   - Simulation einer produktionsnahen Umgebung.  
   - Integrationstests und Lasttests werden durchgeführt.  
   - Manuelle oder automatisierte QA-Prozesse.

3. **Production (prod):**  
   - Live-Umgebung für Endnutzer.  
   - Strenge Sicherheitsrichtlinien und Performance-Optimierung.  
   - Fehlerhafte Änderungen können schwerwiegende Auswirkungen haben.

### Umsetzung mit Kubernetes Namespaces

Eine übliche Methode zur Trennung der Environments in Kubernetes ist die Nutzung von **Namespaces**:

```sh
kubectl create namespace dev
kubectl create namespace test
kubectl create namespace prod
```

Anschließend können Deployments in den jeweiligen Umgebungen angewendet werden:

```sh
kubectl apply -f deployment.yaml -n dev
kubectl apply -f deployment.yaml -n test
kubectl apply -f deployment.yaml -n prod
```

### Vorteile einer Multi-Environment-Strategie

| Vorteil | Beschreibung |
|---------|-------------|
| **Bessere Trennung** | Änderungen werden zuerst in Dev getestet, bevor sie produktiv gehen. |
| **Schnelleres Debugging** | Probleme können frühzeitig erkannt werden, ohne die Live-Umgebung zu beeinträchtigen. |
| **Sicherheit** | Produktionsumgebung ist abgeschottet und unterliegt höheren Sicherheitsanforderungen. |
| **Skalierbarkeit** | Jede Umgebung kann unabhängig skaliert und optimiert werden. |


## 4.8.3 Anforderungen für ein Produktivsystem

Für eine **echte** produktive Umgebung müssen weitere Aspekte beachtet werden:

### **1. Sicherheit**
- **RBAC (Role-Based Access Control)**: Zugriffskontrolle für unterschiedliche Nutzerrollen.
- **Secrets Management**: Nutzung von Vault oder Kubernetes-Secrets für sichere Konfigurationen.
- **Netzwerk-Security**: Einsatz von Network Policies, um ungewollten Traffic zu verhindern.

### **2. Skalierung & Verfügbarkeit**
- **Horizontal Pod Autoscaler (HPA)**: Automatisches Skalieren basierend auf Last.
- **Cluster Autoscaler**: Automatische Erweiterung des Clusters bei hoher Nutzung.
- **Load Balancer**: Nutzung von Ingress-Controllern für intelligentes Traffic-Routing.

### **3. Monitoring & Logging**
- **Zentrales Logging** mit ELK-Stack oder Loki.
- **Metrics & Alerts** mit Prometheus und Grafana.
- **Tracing für Microservices** (z. B. mit Jaeger oder OpenTelemetry).

### **4. Deployment-Strategien**
In einer professionellen Umgebung werden spezielle **Deployment-Strategien** verwendet, um Risiken zu minimieren:

| Strategie | Beschreibung |
|-----------|-------------|
| **Blue-Green Deployment** | Zwei parallele Umgebungen, um einen reibungslosen Wechsel zwischen Versionen zu ermöglichen. |
| **Canary Deployment** | Neue Versionen werden schrittweise auf kleine Nutzergruppen ausgerollt. |
| **Rolling Updates** | Pods werden schrittweise aktualisiert, ohne Downtime. |

### Beispiel für ein Rolling Update Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  namespace: prod
spec:
  replicas: 5
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
  template:
    spec:
      containers:
      - name: my-container
        image: my-app:v2
```

## 4.8.4 Fazit

Das aktuelle Kubernetes-Setup ist ein **guter erster Schritt**, aber für den produktiven Einsatz müssen **mehrere Environments eingerichtet werden**. Sicherheit, Skalierung und Automatisierung sind dabei entscheidend.

Ein zukünftiger Ausbau könnte folgendes beinhalten:
- Einführung von **GitOps mit ArgoCD oder Flux**.
- Implementierung eines **automatisierten CI/CD-Pipelines für mehrere Environments**.
- Nutzung von **Managed Kubernetes-Diensten (z. B. AKS, EKS, GKE)** für bessere Verfügbarkeit.

Diese Maßnahmen würden das System professioneller und produktionsreif machen.
