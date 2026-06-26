# Monitoring (Bonus)

Die Plattform wird hinsichtlich **Ressourcenverbrauch** und **Gesundheit** überwacht –
sowohl aus Sicht der Applikations-Owner als auch der Plattform-Administration.

## Metrik-Pipeline

- **Google Managed Prometheus** ist am GKE-Cluster aktiviert
  (`monitoring_config.managed_prometheus.enabled = true` in
  [`infra-iac-gke/main.tf`](https://github.com/pfhrmn/infra-iac-gke/blob/main/main.tf)).
- Aktive GKE-Monitoring-Komponenten u. a.: `SYSTEM_COMPONENTS`, `POD`,
  `DEPLOYMENT`, `STATEFULSET`, `DAEMONSET`, `HPA`, `STORAGE`, `CADVISOR`, `KUBELET`.
- Die Metriken landen in **Cloud Monitoring** und werden dort visualisiert.

## Dashboard

Cloud-Monitoring-Dashboard **„Group H – Tenant App Monitoring"** mit vier Panels,
jeweils nach Tenant-Namespace gruppiert:

| Panel | Metrik | Aussage |
|-------|--------|---------|
| Container CPU usage (cores) | `kubernetes.io/container/cpu/core_usage_time` | CPU-Verbrauch pro Tenant |
| Container memory used (bytes) | `kubernetes.io/container/memory/used_bytes` | Speicherverbrauch pro Tenant |
| Container restarts | `kubernetes.io/container/restart_count` | Healthiness (Crash-Loops sichtbar) |
| Running pods | `kubernetes.io/container/uptime` | Anzahl laufender Pods pro Tenant |

- **Applikations-Owner** sehen über den Namespace-Filter die Gesundheit und den
  Verbrauch ihrer eigenen App.
- **Plattform-Admins** sehen alle Namespaces nebeneinander und damit den
  Gesamtverbrauch der Plattform.

## Dashboard-as-Code

Das Dashboard ist als IaC reproduzierbar (aus dem laufenden Projekt exportiert):

- [`infra-iac-gke/monitoring.tf`](https://github.com/pfhrmn/infra-iac-gke/blob/main/monitoring.tf)
  – `google_monitoring_dashboard`
- [`infra-iac-gke/monitoring/tenant-app-dashboard.json`](https://github.com/pfhrmn/infra-iac-gke/blob/main/monitoring/tenant-app-dashboard.json)
  – das Dashboard-JSON

![Group H – Tenant App Monitoring](screenshots/monitoring.png)
