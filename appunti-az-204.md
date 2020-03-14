
# Appunti AZ-204

## Link utili

* [Scheda corso](https://docs.microsoft.com/en-us/learn/certifications/exams/az-204)
* [Scheda corso PDF](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4oZ7B)

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

### Durable Function

Estensione di Az Fun per C# e Node.Js.

* Client - Avvia un flusso
* Orchestrator - Avvia le attività con logica. Parallelismo, Ordine, Async, Modalità varie.
* Activity

Pattern tipici

* Function chaining - Output di una funz è input di un'altra
* Fan-out/fan-in - Appena tutte le attività sono concluse, sputo un output
* Async API - la funz http-triggered ritorna l'endpoint da pollare, intanto viene avviata un'attività async e viene aggiornato lo stato nell'endpoint
* Monitoraggio - Esegui un'attività appena una condizione si verifica
* Interazione umana - Azione viene eseguita solo dopo intervento umano. Logiche di timeout

Come crearle
Kudu > CMD > cd D:\home\site\wwwroot > npm install durable-functions

### Functions Core Tools
CLI per gestire functions
* `func`
  * `init` - Inizializza un progetto di function nella dir corrente
  * `new` - Crea una function dentro al progetto
  * `azure functionapp publish "FUNCTIONAPP"` - Publica func su Azure

## Events vs Messages

Messaggi usati tra due componenti: mittente M e destinatario D.
* Servono a garantire comunicazione e ricezione.
* Un messaggio ha un contenuto **esaustivo**. D non deve fare altro per sapere l'info.
* M e D devono essere vivi
* M e D hanno un contratto di dati

Eventi usati tra 0 o molte componenti (broadcast).
* Spesso Publish-subscribe.
* Un evento spesso è + leggero e ha solo riferimento a un contenuto

### Oggetti Azure

#### Service Bus Namespace (SBN)

Serve per l'invio di messaggi.
Garantisce accoppiamento debole e quindi serve a gestire picchi di domanda.
Contiene:

* code
* topic/argomenti
* inoltri

Ha 3 tiers:

* Basic (B)
* Standard (S)
* Premium (P)

Pacchetto per scrivere codice: `Microsoft.Azure.ServiceBus`

#### Service Bus Queue (SBQ)

A differenza della Storage Queue, la SBQ ha feature come:
* Messaggi > 256K (S) o 1M (P) <> SQ 64K
* Supportati sia *at least once* che *at most once* (rilevamento duplicati)
* FIFO garantito
* transazioni di msg
* batch di msg
* Ruoli di sicurezza (RBAC)
* Il destinatario viene pushato
* milioni di msg... ma fino a 80GB <> SQ no limiti!!! e con log di tutti i msg (audit trail)

##### Codice

* `QueueClient(str connStr, str queueName)`
  * `SendAsync(Msg msg)`
  * `CloseAsync()`
  * `RegisterMessageHandler(MessageHandler, opts)`
  * `CompleteAsync(msg.SystemProperties.LockToken)`
  
* `TopicClient(str connStr, str topicName)`
  * `SendAsync(Msg msg)`
* `SubscriptionClient(str connStr, str topicName, str subscription)`
  * `RegisterMessageHandler(MessageHandler, opts)`
  * `CompleteAsync(msg.SystemProperties.LockToken)`

#### Service Bus Topic (SBT)

* Garantisce più sottoscrittori
* Ha un sistema a code interno
* Non supportati in (B)

#### Service Bus Relay (SBR)

* Serve per connessione tra componenti/app e non per messaggi.
* È un canale di comunicazione bidirezionale.
* Serve a bypassare limiti di rete come firewall.


#### Storage Queue (SQ)

* Dentro a uno storage account
* Fino a 500TB
* Dati replicati
* Standard = HD Premium = SSD

Come accedere?
* AAD ruoli per client
* Primary Key - accesso a tutto l'account
* SAS Token - revocabile e temporale

##### Codice
* `WindowsAzure.Storage`
  * `CloudStorageAccount` rappresenta l'account di archiviazione di Azure.
    * `Parse(str connStr)`
    * `CreateCloudQueueClient()`
  * `CloudQueueClient` rappresenta Archiviazione code di Azure.
    * `GetQueueReference(str queueName)`
  * `CloudQueue` rappresenta una delle istanze della coda.
    * `CreateIfNotExistsAsync()`
    * `AddMessageAsync(CQM msg)`
    * `GetMessageAsync()`
    * `DeleteMessageAsync(CQM msg)`
  * `CloudQueueMessage(str message)` rappresenta un messaggio.

#### Event Grid (EG)

EG - broker di eventi altamente scalabile (SF).
Posso sottoscrivere eventi custom ed eventi di sistema
Event Publishers: RG, SUB, ACR, EH, IOT, SBN, VHD, MEDIA, API
Event Handler = sottoscrittori: AFA, WebHook, LA, FLOW
pro:
* pay per event
* semplice ma potente (filtri avanzati)
* ha, scale

#### Event Hub (EH)

EH - broker ad alta velocità x bigdata (milioni/sec a bassa latenza)

Invio in AMQP per alta freq invii, HTTPS per invio poco frequente e con bassa latenza iniziale.

Ad esso si attaccano M autori/pubblicatori in scrittura e N consumatori/sottoscrittori in lettura.

* partizionato
* archivio immediato (capture)
* autenticato - può fungere da gateway x l'esterno
* autoscale
* dati ridondati su + server fino a 7 gg di retention + capture su blob/ADLS
* da EH si può attaccare SA e fare analisi e aggregazione
* Compatibilità vrs Kafka

* Possibilità di elaborare lo stesso messaggio in parallelo con i Consumer Group

Tiers:

* Basic
* Standard (20CG, 1000 connessioni)
* Dedicato

##### Codice

* `EventHubReceiver`
* `EventProcessorHost`

### Dati in Azure

* Strutturati - Id, entità, relazioni, tutto molto bello
* Semi strutturati - Serializzazioni i oggetti disomogenei. Es. Yaml, XML, Json
* Non struttrati - Binari, es. video, pdf, ecc.

Analisi dei dati:
* Con che velocità voglio i dati? es. BI lenta, caricamento lento, lettura veloce
* Che dati voglio?
* Come li voglio i dati?
* Come e quanto spesso li inserisco e elimino
* Quali campi identificano i dati?

Operazioni in transazione
ACID: Atomicità, Coerenza, Isolamento e Durabilità
OLTP: online TRANSACTIONAL processing - db transazionale
OLAP: online ANALYTICAL processing - db + lunghi e da analisi

CosmosDB
* dati semistrutturati
* bassa latenza
* molte letture e scritture
* query complesse
* georeplica
* ACIDo

SQL
* dati strutturati
* bassa latenza
* molte letture e scritture
* query complesse e possibilità di avere un modello semantico con SSAS

BLOB
* dati NON strutturati
* bassa latenza (se usato con CDN bassissima)
* molte letture e scritture
* poco costo


### AZ CLI
variable="value" in ps $variable="value"
az --version
az find "az xyz abc"
az login
* `az group`
  * `create --name <name> --location <location>`
  * `list --output table`
  * `list --query "[?name == '$RESOURCE_GROUP']"` -> [Jmes path](https://jmespath.org/)
* `az functionapp`
  * `create --name {name} --runtime node`
  * `deployment source config-zip --src <zip-file>`
* `az storage
  * `queue`
    * `exists/create/delete`
  * `message peek`
  * `container create --connection-string ...`
  * `account`
    * `create`
    * `show-connection-string`
* `az eventhubs
  * `namespace create`
  * `eventhub
    * `create`
    * `show`
az vm restart -g MyResourceGroup -n MyVm
* `az appservice plan` 
  * `create --name $AZURE_APP_PLAN --resource-group $RESOURCE_GROUP --location $AZURE_REGION --sku FREE`
  * `list --output table`
* `az webapp`
  * `create --name $AZURE_WEB_APP --resource-group $RESOURCE_GROUP --plan $AZURE_APP_PLAN`
  * `list --output table`
  * `deployment source config --name $AZURE_WEB_APP --resource-group $RESOURCE_GROUP --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration`
