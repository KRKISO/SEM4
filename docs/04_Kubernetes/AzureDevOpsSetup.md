---
layout: default
title: 4.5 Azure DevOps Setup
parent: 4. Kubernetes und DevOps
nav_order: 5
---

# 4.5 Azure DevOps Setup

## Übersicht der Infrastruktur

![Sprint1Zeitplan](../../../resources/images/DevOps.PNG)

Dieses Diagramm zeigt die Verbindungen zwischen dem Azure Container Registry (ACR), dem Azure DevOps Agent und dem Kubernetes-Cluster (K0S). Der DevOps Agent steuert sowohl das Pushen der Docker-Images zum ACR als auch die Bereitstellung dieser Images im Kubernetes-Cluster.


## Einführung

Dieser Abschnitt beschreibt die Einrichtung der Azure DevOps-Umgebung für die Integration eines Azure Container Registry (ACR), eines Azure DevOps Agents und eines Kubernetes-Clusters (K0S). Die Schritte umfassen:

1. Einrichtung eines Azure Container Registry (ACR).
2. Bereitstellung eines Azure DevOps Agents auf einem separaten virtuellen Server.
3. Verbindung des ACR, des DevOps Agents und des Kubernetes-Clusters.
4. Beschreibung der CI/CD-Pipeline in Azure DevOps.


## 1. Einrichtung eines Azure Container Registry (ACR)

Das Azure Container Registry dient als Speicher für Docker-Images, die im Kubernetes-Cluster verwendet werden.

### Schritte:
1. **Erstelle ein neues ACR**
   - Navigiere im Azure-Portal zu **Container Registry** und klicke auf **Create**.
   - Wähle die Subscription und die Resource Group (z. B. `Sem4`).
   - Gib einen Namen ein (z. B. `SEM4ACR`).
   - Setze die SKU auf `Basic` für einfache Projekte.
   - Bestätige mit **Review + Create**.

2. **Berechtigungen einrichten:**
   ```bash
   az ad sp create-for-rbac --name acr-pull --scopes $(az acr show --name SEM4ACR --resource-group Sem4 --query id --output tsv) --role acrpull --query password --output tsv
   ```
   - Notiere die generierten Zugangsdaten (AppID, Passwort, Tenant).
   - Diese werden später für Kubernetes als Secret verwendet.


## 2. Bereitstellung eines Azure DevOps Agents

Ein Azure DevOps Agent ist erforderlich, um CI/CD-Pipelines auszuführen. Dieser wurde auf einem separaten virtuellen Server bereitgestellt.

### Warum ein separater Server?
- **Sicherheit:** Vermeidet direkte Abhängigkeiten im Kubernetes-Cluster.
- **Docker-in-Docker:** Erlaubt die Erstellung und Verwaltung von Containern im Agent.

### Schritte:
1. **Virtuelle Maschine erstellen:**
   - Erstelle einen neuen virtuellen Server (z. B. Ubuntu).
   - Installiere Docker:
     ```bash
     sudo apt update && sudo apt install docker.io -y
     sudo usermod -aG docker $USER
     ```

2. **Azure DevOps Agent einrichten:**
   - Lade den Agent herunter:
     ```bash
     mkdir myagent && cd myagent
     curl -O https://vstsagentpackage.azureedge.net/agent/2.213.2/vsts-agent-linux-x64-2.213.2.tar.gz
     tar zxvf vsts-agent-linux-x64-2.213.2.tar.gz
     ```
   - Konfiguriere den Agent:
     ```bash
     ./config.sh --url https://dev.azure.com/<organization> --auth PAT --token <personal-access-token>
     ```
   - Starte den Agent:
     ```bash
     sudo ./svc.sh install
     sudo ./svc.sh start
     ```

## 3. Verbindung zwischen ACR, DevOps Agent und Kubernetes

### Schritte:
1. **Kubernetes-Secret für ACR erstellen:**
   ```bash
   kubectl create secret docker-registry acr-secret      --docker-server=SEM4ACR.azurecr.io      --docker-username=<appId>      --docker-password=<password>      --docker-email=<email>
   ```
   **Ergänzung:**
   - Stelle sicher, dass die Zugangsdaten korrekt sind, indem du den Secret-Inhalt überprüfst:
     ```bash
     kubectl get secret acr-secret -n default -o jsonpath='{.data.\.dockerconfigjson}' | base64 -d
     ```

2. **ServiceAccount für Azure DevOps einrichten:**
   - Erstelle die Datei `azure-devops-sa.yaml`:
     ```yaml
    kind: ServiceAccount
    metadata:
    name: azure-devops-sa
    namespace: default
    ---
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRoleBinding
    metadata:
    name: azure-devops-rolebinding
    roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: ClusterRole
    name: cluster-admin
    subjects:
    - kind: ServiceAccount
    name: azure-devops-sa
    namespace: default
     ```
   - Anwenden:
     ```bash
     kubectl apply -f azure-devops-sa.yaml
     ```

3. **Service-Account-Token abrufen:**
   ```bash
   kubectl describe secret $(kubectl get secrets | grep azure-devops-sa | awk '{print $1}') -n default
   ```
   Notiere das `data.token`, um es in Azure DevOps zu verwenden.

   ```bash
   ubuntu@cloud-hf-16-c1:~$ kubectl get secrets
    NAME                    TYPE                                  DATA   AGE
    acr-secret              kubernetes.io/dockerconfigjson        1      33d
    azure-devops-sa-token   kubernetes.io/service-account-token   3      33d
   ```
   
4. **Azure DevOps Kubernetes-Verbindung einrichten:**
   - Gehe zu **Project Settings > Service connections**.
   - Erstelle eine neue Verbindung vom Typ **Kubernetes**.
   - Füge folgende Daten ein:
     - **Kubernetes cluster URL:** `https://<cluster-ip>:6443`
     - **Service Account Token:** Das oben notierte Token.
     - **Kubernetes namespace:** `default` (oder dein bevorzugter Namespace).

5. **ImagePull-Test:**
   - Stelle sicher, dass Kubernetes das Image ziehen kann:
     ```yaml
     apiVersion: v1
     kind: Pod
     metadata:
       name: test-pod
     spec:
       containers:
       - name: nginx
         image: sem4acr.azurecr.io/nginx:latest
       imagePullSecrets:
       - name: acr-secret
     ```
     ```bash
     kubectl apply -f test-pod.yaml
     ```
     Prüfe den Status:
     ```bash
     kubectl describe pod test-pod
     ```
## 4. CI/CD-Pipeline Beschreibung

Die Pipeline wurde mit einer `azure-pipelines.yml`-Datei konfiguriert. Diese Datei wurde so angepasst, dass sie NodePort-Dienste verwendet, da kein Ingress-Controller genutzt wird. Dies ist sinnvoll, da das Setup nicht produktiv ist und ein Ingress ohne spezifischen Host wenig Mehrwert bringt.

### Inhalte der Pipeline:
```yaml
trigger:
- main

pool:
  name: Sem4

variables:
  ACR_NAME: 'SEM4ACR'
  IMAGE_NAME: 'nginx-static-content'
  IMAGE_TAG: '$(Build.BuildId)'

stages:
- stage: BuildAndPush
  displayName: Build and Push Image
  jobs:
  - job: Build
    displayName: Build Docker Image
    steps:
    - task: Docker@2
      displayName: Build and Push Image to ACR
      inputs:
        containerRegistry: 'SEM4_ACR_CONNECTION'
        repository: '$(IMAGE_NAME)'
        command: 'buildAndPush'
        Dockerfile: 'Dockerfile'
        tags: |
          $(IMAGE_TAG)
    - script: |
        echo "Replacing '\$(acr_version)' with Build ID: $(Build.BuildId)"
        sed -i 's/$(acr_version)/$(Build.BuildId)/g' kubernetes/deployment.yaml
        echo "Modified deployment.yaml:"
        cat kubernetes/deployment.yaml  # Debugging output
      displayName: Replace Variable with Version
    - task: PublishPipelineArtifact@1
      displayName: Publish Updated Manifest
      inputs:
        targetPath: 'kubernetes/deployment.yaml'
        artifact: 'deployment-manifest'

- stage: Deploy
  displayName: Deploy to Kubernetes
  dependsOn: BuildAndPush
  jobs:
  - job: DeployToKubernetes
    displayName: Deploy NGINX to K0S
    steps:
    - task: DownloadPipelineArtifact@2
      displayName: Download Updated Manifest
      inputs:
        artifactName: 'deployment-manifest'
        targetPath: 'kubernetes'
    - script: |
        echo "Final deployment.yaml before applying:"
        cat kubernetes/deployment.yaml
        echo "Applying Kubernetes manifests:"
        kubectl delete deployment nginx-deployment
        kubectl apply -f kubernetes/deployment.yaml
        kubectl apply -f kubernetes/nginx_service.yaml
        kubectl apply -f kubernetes/metrics_service.yaml
      displayName: Apply Kubernetes Manifests
```

### Zusätzliche Dateien:
1. **`kubernetes/deployment.yaml`:**
   Beschreibt die Bereitstellung des Docker-Containers im Kubernetes-Cluster.
   ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: nginx-deployment
    namespace: default
    spec:
    replicas: 1
    selector:
        matchLabels:
        app: nginx
    template:
        metadata:
        labels:
            app: nginx
        spec:
        containers:
        - name: nginx
            image: sem4acr.azurecr.io/nginx-static-content:$(acr_version)
            ports:
            - containerPort: 80
            imagePullPolicy: Always
            resources:
            requests:
                cpu: "250m"    # Mindestanforderungen
                memory: "256Mi"
            limits:
                cpu: "500m"    # Maximale Nutzung
                memory: "512Mi"
        - name: nginx-prometheus-exporter
            image: nginx/nginx-prometheus-exporter:latest
            args: ["-nginx.scrape-uri", "http://localhost/nginx_status"]
            ports:
            - containerPort: 9113
            resources:
            requests:
                cpu: "100m"
                memory: "128Mi"
            limits:
                cpu: "200m"
                memory: "256Mi"
        imagePullSecrets:
        - name: acr-secret
   ```

2. **`kubernetes/nginx_service.yaml`:**
   Konfiguriert den Service als NodePort.
   ```yaml
    apiVersion: v1
    kind: Service
    metadata:
    name: nginx-public-service
    namespace: default
    labels:
        app: nginx-public
    spec:
    type: NodePort
    ports:
        - name: public
        protocol: TCP
        port: 80           # Port for accessing the service
        targetPort: 80     # Maps to the NGINX container port
        nodePort: 30001    # NodePort for external access
    selector:
        app: nginx
   ```

3. **`Dockerfile`:**
   Definiert das Docker-Image.
   ```dockerfile
    FROM nginx:latest
    COPY ./html /usr/share/nginx/html
    COPY ./nginx/nginx.conf /etc/nginx/nginx.conf
   ```

4. **`html/index.html`:**
   Eine Beispiel-HTML-Datei.
   ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Sample HTML Page</title>
        <style>
            body {
                font-family: Arial, sans-serif;
                background-color: #f0f0f0;
                margin: 0;
                padding: 0;
            }
            header {
                background-color: #333;
                color: white;
                padding: 10px 0;
                text-align: center;
            }
            main {
                padding: 20px;
                text-align: center;
            }
            footer {
                background-color: #333;
                color: white;
                padding: 10px 0;
                text-align: center;
                position: fixed;
                width: 100%;
                bottom: 0;
            }
            a {
                color: #007BFF;
                text-decoration: none;
            }
        </style>
    </head>
    <body>
        <header>
            <h1>Welcome to My Sample HTML Page</h1>
        </header>
        <main>
            <p>Das ist die Sample HTML Page der SEM4.</p>
            <p>version: 1.0.</p>
        </main>
        <footer>
            <p>&copy; 2024 Sample HTML Page. All rights reserved.</p>
        </footer>
    </body>
    </html>
   ```

5. **Sample Ingress File:**
   Demonstriert die Konfiguration eines Ingress Controllers für nginx (für spätere produktive Umgebungen).
   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: nginx-ingress
     annotations:
       nginx.ingress.kubernetes.io/rewrite-target: /
   spec:
     rules:
     - host: example.com
       http:
         paths:
         - path: /
           pathType: Prefix
           backend:
             service:
               name: nginx-service
               port:
                 number: 80
   ```

## Verbindung zwischen Azure DevOps und Kubernetes-Cluster

### 1. Erstellen eines Service-Accounts im Kubernetes-Cluster
Damit Azure DevOps Zugriff auf den Cluster erhält, wird ein spezieller Service-Account benötigt.

**YAML-Datei für den Service-Account und die Berechtigungen:**
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: azure-devops-sa
  namespace: default
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: azure-devops-rolebinding
subjects:
  - kind: ServiceAccount
    name: azure-devops-sa
    namespace: default
roleRef:
  kind: ClusterRole
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io
```

**Anwenden im Cluster:**
```bash
kubectl apply -f azure-devops-sa.yaml
```

### 2. Abrufen des Service-Account-Tokens
Das Service-Account-Token wird benötigt, um die Verbindung von Azure DevOps zum Cluster einzurichten.

**Token abrufen:**
```bash
kubectl describe secret $(kubectl get secrets | grep azure-devops-sa | awk '{print $1}') -n default
```

Das Token wird unter `data.token` ausgegeben. Dieses Token sollte sicher gespeichert werden.

### 3. Konfigurieren der Kubernetes-Service-Verbindung in Azure DevOps
In Azure DevOps wird eine Service-Verbindung eingerichtet, um den Cluster mit den Pipelines zu verbinden.

**Schritte:**
1. Navigiere in Azure DevOps zu **Project Settings > Service connections**.
2. Klicke auf **New service connection** und wähle **Kubernetes**.
3. Gib folgende Daten ein:
   - **Kubernetes cluster URL:** Die Cluster-API-URL (z. B. `https://<cluster-ip>:6443`).
   - **Service Account Token:** Das zuvor abgerufene Token.
   - **Kubernetes namespace:** `default` (oder ein anderer gewünschter Namespace).
4. Teste die Verbindung und speichere sie.

### 4. Verknüpfen der Service-Verbindung mit der Pipeline
Die zuvor erstellte Service-Verbindung wird in der Pipeline verwendet.


Mit diesen Schritten ist die Azure DevOps-Integration erfolgreich abgeschlossen. Die NodePort-Konfiguration bietet einfachen Zugriff auf die Anwendung, ohne einen Ingress-Controller zu benötigen, was für Entwicklungsumgebungen ideal ist. Für produktive Umgebungen könnte der bereitgestellte Ingress-Beispiel genutzt werden.
