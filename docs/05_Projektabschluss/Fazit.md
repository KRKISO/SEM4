---
layout: default
title: 5.1 Fazit und Reflexion
parent: 5. Projektabschluss
nav_order: 1
---

# Fazit und Reflexion

## 1. Projektbewertung

Das Projekt hat erfolgreich die Zielsetzung erfüllt, eine skalierbare und automatisierte Kubernetes-Umgebung mit DevOps-Integration zu erstellen. Die Implementierung von k0s als Kubernetes-Plattform, kombiniert mit Azure DevOps für CI/CD, hat die Effizienz der Deployment-Prozesse erheblich verbessert. Besonders die Monitoring-Lösung mit Prometheus und Grafana ermöglicht eine detaillierte Überwachung der Systemressourcen.

## 2. Lessons Learned

- Eine frühzeitige Ressourcenplanung hätte einige Probleme vermeiden können (z. B. Speicherplatz und API-Erreichbarkeit).
- Die Wahl der richtigen Monitoring-Tools ist entscheidend für ein stabiles System.
- Automatisierung ist ein kritischer Faktor für effiziente Deployments, aber eine manuelle Fehleranalyse bleibt unverzichtbar.

## 3. Abschlussbewertung

Das Projekt hat gezeigt, dass mit Kubernetes und DevOps eine leistungsfähige Infrastruktur geschaffen werden kann. Es konnten wesentliche Erkenntnisse über die Automatisierung und Skalierung von Anwendungen gewonnen werden. Zukünftig könnte der Einsatz weiterer DevOps-Tools (z. B. ArgoCD für GitOps) untersucht werden, um den Deployment-Prozess weiter zu optimieren.


## 4. Persönliches Fazit

Wie bei vielen IT-Projekten hat sich auch hier gezeigt, dass Zeitmanagement eine der größten Herausforderungen ist. Trotz sorgfältiger Planung und Strukturierung gab es immer wieder unerwartete Probleme, die zusätzlichen Aufwand erforderten. Gerade die Arbeit mit Kubernetes und DevOps-Tools wie Azure DevOps zeigte, dass eine stabile Automatisierung viel Detailarbeit erfordert – und oft mehr Zeit in Anspruch nimmt als ursprünglich gedacht.

Ein weiteres großes Learning war der Umgang mit technischen Schwierigkeiten und Debugging. Beispielsweise hat sich herausgestellt, dass der Prometheus Metrics Server für den Horizontal Pod Autoscaler (HPA) nicht zuverlässig funktionierte, wodurch ich letztendlich auf den Kubernetes Metrics Server umsteigen musste. Solche unerwarteten Änderungen erfordern Flexibilität und schnelles Troubleshooting, was mich im Laufe des Projekts gezwungen hat, meine analytischen Fähigkeiten weiter zu schärfen.

Auch die Arbeit mit Terraform und k0s brachte Herausforderungen mit sich. Besonders die Speicherplatzprobleme im Cluster haben gezeigt, wie wichtig es ist, Ressourcenmanagement von Anfang an richtig zu planen. Solche vermeidbaren Fehler kosten am Ende unnötig Zeit und führen zu Frustration – aber sie helfen auch, in zukünftigen Projekten besser vorbereitet zu sein.

Ein weiteres Learning war die Kommunikation und Dokumentation. Gerade in einem komplexen technischen Projekt ist es essenziell, die eigenen Schritte und Entscheidungen sauber zu dokumentieren. Während der Implementierung habe ich bemerkt, dass einige frühe Entscheidungen nicht mehr ganz nachvollziehbar waren, weil ich sie nicht klar genug dokumentiert hatte. In zukünftigen Projekten werde ich daher noch mehr darauf achten, alle relevanten Informationen kontinuierlich festzuhalten.

Trotz aller Herausforderungen war dieses Projekt eine wertvolle Erfahrung. Ich konnte mein Wissen über Kubernetes, DevOps und Infrastructure-as-Code weiter vertiefen und viele neue Best Practices lernen. Auch wenn nicht alles nach Plan verlief, habe ich am Ende eine funktionierende, automatisierte Infrastruktur geschaffen – und das ist für mich der größte Erfolg.