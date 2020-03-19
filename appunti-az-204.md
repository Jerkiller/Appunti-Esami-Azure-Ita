
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

## Resource Group e organizzazione delle risorse

Resource group gruppo logico risorse.
No gruppi annidati. I gruppi di sottoscrizioni (management group) possono esserlo.

Nomenclatura suggerita: `msftlearn-core-infrastructure-rg`.

Organizzabili per:

* Gruppo aziendale/Business unit es. Marketing, RnD
* Ambiente es. Test/Prod/Demo
* Ciclo di vita es. TestPowerBi
* Tipo risorsa es. VM-RG tutte le VM
* Fatturaz./centrodicosto es. proj-manhattan-rg

### Spostamenti


* È possibile spostare gruppi e risorse in altri sottoscrizioni o gruppi
* In fase di spostamento viene lockato l'RG!
* Gli spostamenti sono logici e non di datacenter
* Non tutte le risorse possono essere spostate

### Tag

* Altre categorizzazioni si possono ottenere con i TAG.
* Sono elementi chiave-valore
* Si possono applicare su quasi tutte le risorse
* Si possono applicare ai gruppi ma le risorse non li ereditano

### Policies/Criteri

Permettono di applicare criteri su risorse.
Es. impedire la creazione di risorse senza un certo tag.
Es. creazione solo in certe aree geografiche 
Es. no certe risorse o no certe tier

### RBAC

Gestione di ruoli a gruppi e per gruppi di risorse.
Buone prassi:

* Dare permessi minimi e non lassisti
* Usare i lock sulle risorse che non devono essere eliminate
  * Lock: per gruppo, sottoscriz o risorsa
  * Lock di delete o di RO. Il secondo è + stringente -> risultati imprevisti

### Cost Management

I costi variano in base a:

* Tipologia risorsa
* Taglio risorsa (tier), scaling ecc.
* Posizione risorsa (datacenter)
* Tipologia cliente (es. Enterprise)
* Risorsa del Marketplace? Tipologia fatturaz. diversa

* Lo strumento fondamentale è il [pricing calculator](https://azure.microsoft.com/it-it/pricing/calculator/)
* Per migrare verso il cloud c'è il tool [TCO calculator](https://azure.microsoft.com/pricing/tco/)
* A valle della crez. risorse, si usano Advisor, Cost analyis e Budget
* Licenze VS dann credito AZ:
  * 45 eur/mese VS Professional
  * 130 eur/mese Enterprise

Modi di "sparagnare":
* VM: Deallocazione se non usate (solo disco), ridimensionamento, Reserved Instance, spegnimento notturno, uso licenze on-prem su VM on-cloud
* Località a basso costo
* Limiti di spesa che bloccano spese
* Go PAAS!
* Offerte x sviluppo e test
* SQL: uso licenze on-prem su cloud (BYOL), istanze vincolate (es. DB che non supererà mai i 100GB)

---

## VM

Risorse collegate: RG - PageBlob (VHD) - VNet - NIC (Network Interface)
Azure storage service encryption SSE -> encrypt/decrypt trasparente x rest
* x encrypt vhd -> az disk encryption service - (kv)
* divise per famiglie in base allo scopo (es. GPU, RAM, CPU, IO, ...)
* Managed disk o su storage account. Disco esterno temporaneo. Altri dischi vnno montati. Dischi in GiB = 1074 MB.
* diversi SO, immagini, stack installabili
* Availability set
  * Fault domain - se va giù una macchina ce n'è un'altra
  * Update domain - se una macchina aggiorna, non l'altra
* Az. Site Recovery - gestire il failover verso una VM secondaria
* Backup di Azure - sia x VM che macchine fisiche, PC
  * Agente di Backup di Azure
  * System Center Data Protection Manager
  * Server di Backup di Azure
  * Estensione macchina virtuale di Backup di Azure
* NSG tipo firewall. Priorità bassa: nega tutto 65.500 poi regole. Di default è vietato entrare e uscire.

### Codice

* `ssh-keygen -t rsa -b 4096` - genera coppia di chiavi
* `ls -la /` - visualizza la radice del disco
* `ps -l` - mostra tutti i processi in esecuzione
* `dmesg` - elenca tutti i messaggi del kernel
* `lsblk` - elenca tutti i dispositivi a blocchi: qui verranno visualizzate le unità
* `(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc` crea una patiz primaria
* `sudo mkfs -t ext4 /dev/sdc1` crea Fie System nella partiz primaria
* `sudo mkdir /data & sudo mount /dev/sdc1 /data` Monta l'unità nel File System corrente
* `sudo apt-get install apache2 -y` installa Apache
* ``
* ``
* ``

* ``

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

### Web App

Tiers: Free, Shared, Basic, Standard, Premium, PremiumV2, Isolated

Slot di distribuz:
* Solo dalla Standard in su
* Hot swap delle app
* Possibilità di scambiare anche config (var d'ambiente) insieme agli slot
* Pipelining: es. pubblico su develop, deploy in staging. Porto su master, swap con prod slot

Distribuzione continua tramite WebHook con Az Container Regitry

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

## Databricks

RDD Resilient Distributed Datasets
* Resilient - Se una parte di operaz fallisce, DataBricks ricrea i dati
* Distributes - Su più nodi
* Dataset - Dataframe colonnari come db relaz

* Programmi sono i notebook (markdown + script) scritti in Python, R, Scala
* Formati csv, json, xml, parquet
* ADLS, Blob (ABFS), Hadoop (hdfs), joining di più fs ...
* Integrato con KeyVault
* Integrato con JDBC e tutti i SQL che lo supportano
* Integrabile con Kafka, CosmosDB e tutti i pacchetti Maven o Py


### Comunicazione

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
* bassa latenza (se usato con CDN bassissima). Tuttavia è meno bassa di 1 FileSystem
* molte letture e scritture. Difficile querying. (Azure Search)
* poco costo
* simulaz struttura gerarchica con prefissi es. `a/b/c/d.jpeg`
* lease e access condition x gestire concorrenze

### Storage Account SA

* 2 tier:  Standard (HD) e Premium (SSD) - solo page blob.
* Tipologia storage: Blob (legacy), Generic V1 (legacy) e **Generic V2**
* Vari tipi di ridondanza LRS, ZRS, GRS GRS-RA...
* accesso freq o sporadico
* Max 500 TB, max 200 account
* Endpoint `xxxx.[table|blob|queue|file].core.windows.net`

Può contenere:

* Code - 64Kb x milioni di msg
* Tabelle - NoSql
* Blob (block, page, append)
  * Block Blob - es. video
  * Page Blob - es. dischi virtuali (VHD)
  * Append Blob - come block ma con append testuale. Max 195 GB
* Files - SMB, utile per + VM con file comuni es. log o config centralizzate

#### Sicurezza

* Blob criptati con AES 256 non disativabile
* VHD criptabili con BitLocker (Win) o DM-Crypt (Linux) chiavi in KV
* Crittografia in transito. Usare HTTPS. Può essere messo obbligatorio. Per files può essere messo SMB 3.0
* RBAC in gestione account e gestione dati
* CORS policies
* IAM - controllo accessi
* Doppia chiave rigenerabile, SAS Token a livello di account o risorsa
* Controllo di rete - Firewall, Blacklist o Whitelist di IP
* Advanced Threat Protection - in preview x Blob: notifica se ci sono sospetti
* in ADLS RBAC sul filesystem con permessi POSIX in base al grafo AD

#### Codice **WindowsAzure.Storage**

* `CloudStorageAccount`
  * `Parse(str connStr)`
  * `CreateCloudBlobClient()`
* `CloudBlobClient`
  * `GetContainerReference(str containerName)`
* `CloudBlobContainer`
  * `CreateAsync()`
  * `GetBlockBlobReference(str blobName)`
* `CloudBlockBlob`
  * `UploadFromFileAsync(str fileName)`
* `CloudBlobDirectory`
  * `ListBlobs(options)`

### SQL
Suffisso: xxxxxxxxxx.database.windows.net
* On premise
* On-Cloud
  * VM con SQL
  * SQL di Azure - una via di mezzo
  * SQL Database - completamente gestito
  
Unità di costo
* DTU - misura combinata di risorse di calcolo, archiviazione e I/O
* vCore - misura di core virtuali. È possibile configurare che risorse prediligere

#### Sicurezza SQL

* Autenticazione SQL o AD per n account o gruppi
* Ruoli
* Autorizzazione per vNet
* Regole firewall (per impostare accesso da Azure disabilitare SSL) Ip, porte
* Filtro IP per database (no porte). Non presente in SQL Warehouse
  * `EXECUTE sp_set_database_firewall_rule N'VM linux', 'ip', 'ip'`
* Connessioni SSL (evitare mitm)
* Transparent Data Encryption (TDE) attiva by design.
  * File di database, file di log e file di backup
* Dynamic Data Masking - certi utenti non vedono una colonna ma solo xxxx-xxxxxxx. Es. carte credito, mail...
* Advanced Data Security (ADS) - Analisi dati, vulenrabilità, minacce, ecc.
  * Si attiva su "Sicurezza dei dati avanzata"
  * SQL Injection?
  * GDPR compliant?

#### Regole di confronto

Es. `SQL_Latin1_General_CP1_CI_AS`

* **Latin1_General** si riferisce alla famiglia di lingue dell'Europa occidentale
* **CP1** tabella codici 1252, una codifica char latin
* **CI** - Case Insensitive (HeLLo = hello)
* **AS** - Accent Sensitive (Però != Pero)

#### Codice

* `System.Data.SqlClient`
  * `SqlConnection(connectionString)`
    * `Open()`
    * `Close()`
  * `SqlCommand(str query, conn)`
    * `ExecuteNonQuery()`
    * `ExecuteReader()`
  * `SqlDataReader`
    * `Read()`
  * `SqlException`

### Alternativa: Database di Azure per PostgreSQL
  * xxxxxxxxxxxxxx.postgres.database.azure.com.
  * Estensibile, con retention configurabile e backup, scalabile...
  * Sicurezza:
    * account e ruoli
    * vNet
    * firewall (per impostare accesso da Azure disabilitare SSL)
    * Connessioni SSL (evitare mitm)
  * Client CLI: `psql`. Per lanciare comandi speciali si usa `\`.
      * Es. `\l` fa il listing.
      * Es. `\?` mostra tutti i comandi.

* Provisioning risorse deve essere fatto per ecceso
* Elastic pool: quando ho più db con utilizzo medio e picchi sporadici
  * Aggiungere pool se ho somma delle capacità >= 1.5 volte la capacità dell'elastic pool
  * Pool: fino a 100 o 500 db
  * eDTU: Unità asegnabili al pool intero. Al db puoi assegnare min e max DTU

## Powershell

* Installabile su tutte le piattaforme
* Estendibile con moduli (dll) che contengono i cmdlet. `Get-Module` per vedere i moduli attivi ora.
* Si basa sui *cmdlet*: commandlet con sintassi Verbo-Sostantivo
  * Es. `Get-Help Get-ChildItem -detailed`
* Moduli installabili dalla `PowerShell Gallery`
  * `Install-Module`
  * es. `Install-Module -Name Az -AllowClobber`
* Modulo per Azure è **-Az**. Precedentemente **-AzureRM**
* Se la Get-ExecutionPolicy è restricted, non possono essere eseguiti moduli dalla PSGallery. Occorre lanciare `Set-ExecutionPolicy RemoteSigned`
* Per eseguire scriptoni conviene usare *Powershell ISE*. File in formato `.ps1` eseguibili da PS con `\.deploy.ps1  -size 5 -location "East US"`
* Cicli e varibili come coding

```powershell
$ResourceGroupName = "prod-ciccio-rg"

Import-Module Az
Connect-AzAccount
Select-AzSubscription -Subscription "Visual Studio Enterprise"
# Format-Table può essere abbreviato in `ft`
Get-AzResourceGroup | Format-Table
New-AzResourceGroup -Name $ResourceGroupName -Location "<location>"
New-AzVm 
       -ResourceGroupName $ResourceGroupName
       -Name "<machine name>"
       -Credential "<credentials object>"
       -Location "<location>"
       -Image "<image name>"
```

## AZ CLI

variable="value" in ps $variable="value"

* `az --version`
* `az find "az xyz abc"
* `az login`
* `az group`
  * `create --name <name> --location <location>`
  * `list --output table`
  * `list --query "[?name == '$RESOURCE_GROUP']"` -> [Jmes path](https://jmespath.org/)
* `az functionapp`
  * `create --name {name} --runtime node`
  * `deployment source config-zip --src <zip-file>`
* `az storage`
  * `queue`
    * `exists/create/delete`
  * `message peek`
  * `container create --connection-string ...`
  * `account`
    * `create`
    * `show-connection-string`
* `az eventhubs`
  * `namespace create`
  * `eventhub`
    * `create`
    * `show`
* `az vm`
  * `restart -g <groupName> -n <name>`
  * `create -n ... -g ... --image UbuntuLTS --admin-username jonc --admin-password aReallyGoodPasswordHere`
  * `open-port -n ... -g ... --port 80`
* `az appservice plan` 
  * `create -n ... -g ...P --location $AZURE_REGION --sku FREE`
  * `list --output table`
* `az webapp`
  * `create -n ... -g ... --plan $AZURE_APP_PLAN`
  * `list --output table`
  * `up` - Crea un pacchetto e lo pubblica su WA. Se non esiste la WA la crea
  * `deployment source config -n ... -g ... --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration`
* `az resource tag --tags Department=Finance`
* `az rest --method post --uri "<enter the correct REST operation URI here>"`
* `az resource`
  * `show -g <group-name> -n <res-name> --query id --output tsv`
  * `move --destination-group <destination-group-name> --ids $yourResource`
* `az acr`
  * `create -n ... -g ... --sku standard --admin-enabled true`
  * `build --file Dockerfile --registry myregistry --image myimage .` - Esegue un dockerfile creando un'immagine docker sul ACR
  * `task create --registry <container_registry_name> --name buildwebapp --image webimage --context https://github.com/MicrosoftDocs/mslearn-deploy-run-container-app-service.git --branch master --file Dockerfile --git-access-token <access_token>` - Definisce un task "buildWebApp"
* `az postgres server create -n ... -g ... --sku-name SKU_NAME --admin-user ADMINISTRATOR_LOGIN`
* `az sql`
  * `server create --location westeurope --admin-user xxx --admin-password yyy`
  * `db`
    * `create [--elastic-pool-name xyz] --server serverNme [--sample-name AdventureWorksLT]`
    * `show-connection-string --client sqlcmd -n ... --server $SERVERNAME | jq -r`
  * `elastic-pools create`
