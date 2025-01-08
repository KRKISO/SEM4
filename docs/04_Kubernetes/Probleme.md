---
layout: default
title: 4.8 Probleme
parent: 4. Kubernetes und DevOps
nav_order: 8
---

# 4.8 Probleme

Auf meinem Weg zum funktionierenden K8S Cluster mit Azure DevOps integration bin ich auf diverse Probleme gestossen.
Ich möchte hier auf drei Probleme spezifisch eingehen.

## Disk Space

Wie bereits beschrieben, wurde der K8S Cluster mit hilfe von Terraform aufgesetzt. Leider habe ich hier zu wenig Disk Space ausgewählt, was dazu führe, dass das K0S API nicht mehr erreichbar war.
Der folgende Output beschreibt leider nicht wirklich das eingetliche Problem. 

```sh
ubuntu@cloud-hf-16-c1:~$ kubectl get namespaces
E1125 17:27:12.472964   48208 memcache.go:265] "Unhandled Error" err="couldn't get current server API group list: Get \"https://10.0.24.147:6443/api?timeout=32s\": dial tcp 10.0.24.147:6443: connect: connection refused"
```

Schlussendlich musste ich das syslog und journal deaktivieren, um persistent eine lösung zu haben, ohne den K8S Cluster neu aufzusetzen.
Eine andere Option wäre, mit LogRotate die Logs zu rotieren, leider habe ich hier aus der Vergangenheit keine guten Erfahrungen.

```sh
1.2G	/var/log/journal
1.2G	/var/log

sudo nano /etc/systemd/journald.conf
#Storage=volatile
sudo systemctl restart systemd-journald
```

## Azure DevOps Pipeline

Im Deployment.yaml des Nginx Deployment muss die Image Version spezifiziert werden.

```yaml
image: sem4acr.azurecr.io/nginx-static-content:$(acr_version)
```

Diese variable $(acr_version) wird leider beim ausführen der Pipeline nicht mit der eigentlichen Version des Images ersetzt.
Aus diesem Grund habe ich in der Pipeline folgende Schritte hinzugefügt um die korrekte Version zu setzen.

```yaml
    - script: |
        echo "Replacing '\$(acr_version)' with Build ID: $(Build.BuildId)"
        sed -i 's/$(acr_version)/$(Build.BuildId)/g' kubernetes/deployment.yaml
        echo "Modified deployment.yaml:"
        cat kubernetes/deployment.yaml  # Debugging output
      displayName: Replace Variable with Version
```


## HPA und Prometheus

Um einen HPA zu installieren gibt es diverse Optionen. 
Prometheus bietet einen eigenen Metrics Server an, welcher dann vom HPA konsumiert wird. Leider hatte ich mit diesem Prometheus-Adapter einige schwierigkeiten.

Folgende Befehle sollten die Metriken des Prometheus-Adapter ausgeben.
Wie jedoch zu sehen ist, hat dies einmal funktioniert und einmal nicht. 

Ich habe diverse Ressourcen Einstellungen im Adapter vorgenommen um diesen Stabiler zu machen, leider ohne Erfolg. 

```sh
ubuntu@cloud-hf-16-c1:~/deployments/hpa$ kubectl get --raw "/apis/custom.metrics.k8s.io/v1beta1" | jq .
{
  "kind": "APIResourceList",
  "apiVersion": "v1",
  "groupVersion": "custom.metrics.k8s.io/v1beta1",
  "resources": []
}
ubuntu@cloud-hf-16-c1:~/deployments/hpa$ kubectl get --raw "/apis/custom.metrics.k8s.io/v1beta1" | jq .
Error from server (ServiceUnavailable): the server is currently unable to handle the request
```

Da der Adapter aus mir unerklärlichen Gründen nicht zuverlässig funktionierte, und dadurch auch der HPA die Daten nicht richtig prozessieren konnte, habe ich mich dazu entschieden einen anderen Metric Server zu verwenden.
Die Installation habe ich im Punkt **4.7 Horizontaler Autoscaler** beschrieben.

