
# Appunti AZ-204

## Link utili

* [Scheda corso](https://docs.microsoft.com/en-us/learn/certifications/exams/az-204)
* [Scheda corso PDF](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4oZ7B)
* [Laboratori](https://microsoftlearning.github.io/AZ-220-Microsoft-Azure-IoT-Developer/)

## Deploy
* Azure
* ARM
* Az CLI / Powershell
* Rest API
* SDK Azure Management

## VM

Risorse collegate: RG - PageBlob (VHD) - VNet - NIC (Network Interface)
Azure storage service encryption SSE -> encrypt/decrypt trasparente x rest
x encrypt vhd -> az disk encryption service - (kv)

## AZ Batch

X carichi grandi di elaborazione. Feature del serverless, pay as you go.
SDK .NET, Java, Python, Node.JS, REST API
Gestione con PS, AzCLI, Portal, Batch Explorer, Batch Shipyard, Az Storage Explorer

* Batch Account - qui dentro vengono svolti tutti i workload. Può ess collegato a uno storage
  * Name - URL - Key
* Pool - cluster di "Compute Node". Gestisce lo scheduling e le risorse
* Job - viene assegnato a un pool, può ess schedulato è una seq di Task (comando,script o app eseguito su un nodo).

## Container

Dove posso deployare un docker container?

* EDGE - IoTEdge
* SF - Service Fabric
* BA - Azure Batch Job
* WA - Azure Web Apps for Container
* ACR - Azure Container Registry (No esecuzione)
  * Auth AD Login
  * Auth Admin account (evita in prod)
  * Auth AD Service Principals
* ACI - Azure Container Instance
* ACS - Azure container service - Cluster Kube o Docker Swarm o DC/OS
* AKS - Cluster K8S gestito
  * Cluster Master Nodes (core orchestration su una serie di nodi nello stesso tenant - trasparente e gratuito. Gestito da Az)
    * API server - utilizzate da kubectl e dashboard per interagire col cluster
    * Scheduler - assegna workload ai nodi, avvia il workload
    * ETCD - KV store x gestire stato e config del cluster
    * Controller manager - gestisce i controller dei vari nodi x verificare e aggiornare ciclicamente lo stato
  * Nodes - VM dove sono eseguiti i container
    * kubelet (K8s agent) - riceve gli ordini e li esegue su un nodo
    * kube-proxy - Getsisce la Vnet del nodo, gli IP dei servizi e dei pod e fa da router
    * container runtime - consente ai container di girare e accedere alle risorse

### Kubernetes
* Pod - singola unit dove gira 1 container (di solito)
* Deployment - Direttiva che deploya un applicativo su N pod e alloca risorse
* Yaml Manifest - dove è specificato uno o + deployment
* StatefulSet - Deployment persistente che muore salvando lo stato
* DaemonSet - Deployment garantito in ogni nodo (es. logging del nodo)



## Workflow

Ogni workflow ha dei task (input, condiz, azioni, output).
In az, due approcci:
* Design-first
  * Logic App (json behind)
  * MS Flow (meno tecnico, x utonti)
* Code-First
  * Web Job (costa di +, meno usato ora. + Personalizzazione es. x retry speciali)
  * AZ Function (pay as u go, snello)

Trigger di un Workflow
* Ora
  * CronEx
    * Base: `{second} {minute} {hour} {day} {month} {day of the week}`
    * `*` - Ogni valore di un campo
    * `-` - Intervallo
    * `,` - Separatore AND
    * `/` - Incremento. Ogni x ore/minuti
* Evento (http, msg...)
  * HTTP
    * Auth di 3 tipologie
      * Admin - serve chiave host
      * Function - Chiave host o chiave func
      * Anonymous - No chiave
* Dati
  * Polling (frequenza * intervallo check)
  * Pushing (implementato con webhook)
  * Blob (polling)

## Events vs Messages

Messaggi usati tra due componenti (entrambe devono essere vive).
Servono a garantire comunicazione e ricezione. Un messaggio ha un contenuto esaustivo.

Eventi usati tra 0 o molte componenti (broadcast).
Spesso Publish-subscribe. Un evento spesso è leggero e ha solo riferimento a un contenuto.

### Oggetti Azure

Azure fornisce code e topic. Storage Queue VS SBT.
Coda ha feature come:
 * at least once
 * at most once (rilecvamento duplicati)
 * FIFO
E poi...
* transazioni di msg
* batch di msg
* milioni di msg...
SBT garantisce + sottoscrittori (sistema a code interno)

EG - broker di eventi altamente scalabile (SF).
Posso sottoscrivere eventi custom ed eventi di sistema
Event Publishers: RG, SUB, ACR, EH, IOT, SBN, VHD, MEDIA, API
Event Handler = sottoscrittori: AFA, WebHook, LA, FLOW
pro:
* pay per event
* semplice ma potente (filtri avanzati)
* ha, scale

EH - broker ad alta velocità x bigdata (milioni/sec a bassa latenza)
* partizionato
* archivio immediato (capture)
* autenticato - può fungere da gateway x l'esterno
* da EH si può attaccare SA e fare analisi e aggregazione
