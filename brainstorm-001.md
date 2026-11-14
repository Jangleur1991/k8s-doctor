# Projekt: K8s AI Diagnose-CLI
**ID:** brainstorm-001
**Projektname:** `k8s-doctor-ai-001`
**Themen:** Kubernetes (CKAD), AI/Agents (RAG, MCP, Agent-Loop), Java, TypeScript
**Stand:** 2026-11-13
**Status:** neu

## Warum dieses Projekt?
CKAD-Vorbereitung durch Bauen statt Auswendiglernen. Jedes Feature zwingt dich, ein CKAD-Thema anzufassen. Nebenbei: Java wieder reinkommen, moderne AI-Architektur (RAG, MCP, eigener Agent-Loop) — Themen, die in Stellenausschreibungen gerade oft gefordert sind.

## Tech-Stack
- **Java 25 + Spring Boot (Backend, im Cluster)**: Diagnose-Engine, Agent-Loop, KI-Client, MCP-Server. Hauptarbeit (~60%).
- **Fabric8 Kubernetes Client (Java)**: K8s-API aus Java ansprechen (Pods, Events, Logs, Patches). Etablierter Java-Client, gute Doku.
- **Spring AI**: ChatClient → Ollama, VectorStore für RAG, `@Tool`-Annotationen, MCP-Server-Starter.
- **TypeScript (CLI, lokal)**: Terminal-Frontend. Strikt TypeScript (`strict: true`), kein handgeschriebenes JavaScript. Ausführen mit `tsx` oder kompiliert. Libs nur mit Typen (z.B. `commander` hat eingebaute Typen).
- **Ollama**: Lokales LLM als Pod im Cluster. Kein Cloud, kein API-Key.
- **ChromaDB**: Lokale Vektor-DB für RAG (K8s-Doku als Wissensbasis).
- **Kubernetes**: Docker Desktop, 3 Nodes (bestehender Cluster).

## Was du baust
CLI `k8s-doctor` mit 4 Befehlsgruppen. CLI (TypeScript) spricht per HTTP mit Java-Backend im Cluster. Backend macht alles Schwere.

| Befehl | Was passiert |
|---|---|
| `k8s-doctor diagnose [deploy]` | Backend sammelt Pod-Status, Events, Logs → KI erklärt Ursache + schlägt Fix vor |
| `k8s-doctor create <resource>` | KI generiert gültiges YAML (RAG mit K8s-Doku) → du prüfst → apply. **Nie mehr YAML auswendig.** |
| `k8s-doctor explain <thema>` | KI erklärt K8s-Konzept mit Beispiel aus deinem Cluster |
| `k8s-doctor agent start` | Agent-Loop im Backend überwacht Cluster, meldet Probleme, schlägt Fixes vor (Apply nur nach deinem OK) |

## Architektur
```
[TypeScript CLI]  --HTTP (port-forward)-->  [Java Spring Boot Backend]  (Namespace: k8s-doctor)
   lokal                                        |-- Fabric8 → K8s-API (via ServiceAccount + RBAC)
                                                |-- Spring AI → Ollama-Pod (LLM)
                                                |-- Spring AI → ChromaDB-Pod (RAG)
                                                '-- MCP-Server (Tools: logs, events, patch)
```

## Der Einstieg (Mini-Projekt, ~30–60 Min)
Ziel: Backend läuft im Cluster und findet einen absichtlich kaputten Pod. Noch keine KI, keine CLI. Nur `curl`.

**Schritt 0 — Kaputten Pod erzeugen (dein "Patient")**
- `kubectl create namespace playground`
- `kubectl run broken --image=nginx:gibtsnicht -n playground`
- Erwartung: `kubectl get pods -n playground` zeigt `ImagePullBackOff`. Das soll dein Tool später selbst finden.

**Schritt 1 — REST-Endpoint `GET /diagnose/{namespace}`**
- Spring Boot Projekt (start.spring.io: Web, Actuator) + Dependency `io.fabric8:kubernetes-client`.
- Endpoint holt alle Pods im Namespace über Fabric8.
- Filtert "ungesunde" Pods: Phase ≠ `Running`/`Succeeded`, ODER Container nicht `ready`, ODER `restartCount > 0`.
- Gibt JSON zurück, z.B.:
  ```json
  [{ "pod": "broken", "phase": "Pending", "reason": "ImagePullBackOff", "restarts": 0 }]
  ```
- `reason` steht im Container-Status (`state.waiting.reason`). Das ist genau das, was später an die KI geht.
- Lokal testen: App starten (nutzt deine `~/.kube/config`), `curl localhost:8080/diagnose/playground`.

**Schritt 2 — `@Scheduled` Watcher (Vorstufe vom Agent-Loop)**
- `@EnableScheduling` + Methode mit `@Scheduled(fixedRate = 30000)`.
- Ruft dieselbe Logik wie Schritt 1 auf (Service-Klasse wiederverwenden, nicht Controller aufrufen).
- Loggt nur Änderungen: `WARN Neuer Problem-Pod: playground/broken (ImagePullBackOff)`. Gleiches Problem nicht alle 30s erneut loggen → Set mit bekannten Problemen merken.
- Das ist später der Agent: heute "sehen + loggen", später "sehen + KI fragen + Fix vorschlagen".

**Schritt 3 — Im Cluster deployen (`kubectl apply`)**
Dateien in `k8s/`, alle selbst schreiben (Doku erlaubt, nicht kopieren):
1. `namespace.yaml` — Namespace `k8s-doctor`.
2. `rbac.yaml` — **ServiceAccount** `k8s-doctor` + **ClusterRole** (verbs `get`, `list`, `watch` auf `pods`, `events`, `pods/log`) + **ClusterRoleBinding**. Ohne das: Backend bekommt `403 Forbidden` → guter erster Troubleshooting-Moment.
3. `deployment.yaml` — 1 Replica, `serviceAccountName: k8s-doctor`, Container-Port 8080, Liveness/Readiness auf `/actuator/health/liveness` bzw. `/readiness`, `resources.requests/limits`.
4. `service.yaml` — Typ `ClusterIP`, Port 8080.
- Image bauen: `docker build -t k8s-doctor:0.1 .` Prüfen ob Docker-Desktop-Cluster das lokale Image sieht. Falls `ErrImagePull`: lokale Registry nutzen oder Image in Nodes laden → nächster Troubleshooting-Moment.
- `kubectl apply -f k8s/` → `kubectl get pods -n k8s-doctor` bis `1/1 Running`.

**Schritt 4 — Erreichbar machen (`kubectl port-forward`)**
- `kubectl port-forward svc/k8s-doctor 8080:8080 -n k8s-doctor`
- Leitet `localhost:8080` durch den Tunnel zum Service im Cluster. Kein Ingress nötig.
- `curl localhost:8080/diagnose/playground` → `broken`-Pod erscheint. **Mini-Projekt fertig.**
- `kubectl logs -n k8s-doctor deploy/k8s-doctor` → Scheduled-Warnung sichtbar.

**CKAD schon nach Mini-Projekt:** Deployment, Service, Probes, Requests/Limits, ServiceAccount, RBAC, port-forward, logs, Troubleshooting.

## Wenn es Spaß macht (V1 → V3)
1. **V1 — KI-Diagnose:** Ollama als Deployment + Service + PVC (Modell-Cache). Spring AI ChatClient schickt Diagnose-JSON → strukturierte Antwort (`cause`, `fix`, `yamlPatch`). ConfigMap für Modellname, Secret-Übung für Tokens.
2. **V1 — TypeScript CLI:** `k8s-doctor diagnose playground` → Tabelle + KI-Erklärung im Terminal.
3. **V2 — RAG + `create`:** K8s-Doku in ChromaDB (StatefulSet + PVC). `k8s-doctor create cronjob --schedule "*/5 * * * *"` → gültiges YAML, `kubectl apply --dry-run=server` validiert vor Ausgabe.
4. **V2 — MCP-Server:** Java-Tools `get_pod_logs`, `get_events`, `describe_resource`, `apply_patch`. KI fragt selbst nach. Auch aus Claude Code nutzbar.
5. **V3 — Agent-Loop:** Watcher aus Schritt 2 wird Agent: sehen → KI + Tools → Plan → User-OK → Patch → verifizieren. History der Fixes merken. Patch-Rechte via eigener Role nur auf Namespace `playground` (Least Privilege).

## CKAD-Abdeckung (offizielle 5 Domains)
Jede Domain = konkretes Feature oder Übung im Projekt.

**1. Application Design and Build (20%)**
- Container-Image bauen (Dockerfile, Multi-Stage-Build für Java)
- Passende Workload wählen: Backend = Deployment, ChromaDB = StatefulSet, Report = CronJob, optional Log-Agent = DaemonSet
- Multi-Container-Pods: Init-Container lädt Ollama-Modell vor, Sidecar sammelt Logs
- Volumes: PVC für Ollama-Modelle + ChromaDB, `emptyDir` für Temp-Daten

**2. Application Deployment (20%)**
- Rolling Update + `kubectl rollout status/undo/history` bei neuer Backend-Version
- Blue/Green oder Canary: zwei Backend-Versionen, Service-Selector umschalten
- Kustomize: `base/` + `overlays/dev/` + `overlays/prod/` (unterschiedliche Replicas, Limits, Modell)
- Helm: Ollama oder ChromaDB per Helm-Chart installieren (Charts nutzen, nicht nur lesen)

**3. Application Observability and Maintenance (15%)**
- Liveness/Readiness/Startup-Probes (Spring Boot Actuator)
- `kubectl logs`, `describe`, `get events`, `top` — das Tool macht genau das, du verstehst es durch Bauen
- Debugging: `kubectl exec`, `kubectl debug`, absichtlich kaputte Pods als Testfälle
- API-Deprecations: `kubectl explain`, apiVersion prüfen → als `k8s-doctor`-Check einbauen

**4. Application Environment, Configuration and Security (25%) — größte Domain**
- ConfigMaps + Secrets (`envFrom`, als Volume gemountet)
- ServiceAccount + Role/ClusterRole + Bindings (Backend braucht K8s-API-Zugriff)
- SecurityContext: `runAsNonRoot`, `readOnlyRootFilesystem`, Capabilities droppen
- Requests/Limits, ResourceQuota + LimitRange im Namespace `playground`
- Optional: CRD `DiagnosisReport` — Agent schreibt Ergebnisse als eigene K8s-Ressource

**5. Services and Networking (20%)**
- ClusterIP-Services zwischen Backend ↔ Ollama ↔ ChromaDB (DNS: `ollama.k8s-doctor.svc.cluster.local`)
- NetworkPolicy: nur Backend darf Ollama/ChromaDB erreichen, Rest blockiert
- Ingress: Backend über Hostname statt port-forward
- NodePort zum Vergleich ausprobieren

**Bonus-Feature fürs Lernen:** `k8s-doctor drill` — Backend erzeugt zufällig einen kaputten Pod (falsches Image, falscher Probe-Port, fehlendes Secret, zu wenig Memory, blockierende NetworkPolicy). Du fixt es mit `kubectl` auf Zeit, danach erklärt die KI die Lösung. Wie killer.sh, aber in deinem Cluster.

## Ghostty-Modus
Mini-Demo ist Foundation. Wenn es Spaß macht: V1 KI-Diagnose + CLI → V2 RAG + MCP → V3 Agent-Loop + Drill-Modus → Release auf GitHub.

## Weiter mit diesem Projekt
---
Du bist im Brainstormer-Modus. Projekt brainstorm-001 ist aktiv.
Nächster Schritt: Mini-Projekt Schritt 0–4 (kaputter Pod, `/diagnose`-Endpoint, `@Scheduled`-Watcher, Deployment mit RBAC, port-forward).
