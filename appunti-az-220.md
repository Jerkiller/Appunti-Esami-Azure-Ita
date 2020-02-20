
# Appunti AZ-220

## Link utili

* [Scheda corso](https://docs.microsoft.com/en-us/learn/certifications/courses/az-220t00)
* [Scheda corso PDF](http://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4nBeC)
* [Laboratori](https://microsoftlearning.github.io/AZ-220-Microsoft-Azure-IoT-Developer/)

## Software utili

* [`Azure/azure-cli-iot-ext`](https://github.com/Azure/azure-iot-cli-extension) - Estensione della azure shell per comandi su iot
* [`Azure/azure-iot-sdk-csharp/tools/DeviceExplorer`](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) - Desktop app per interagire con IotHub. Deprecato il 30/10/2019
* [`Azure/azure-iot-explorer`](https://github.com/Azure/azure-iot-explorer) - Nuova implementazione ufficiale del device explorer
* [`Azure/iotedge`](https://github.com/azure/iotedge) - Progetto OSS di IotEdge
* [`Azure/iotedgehubdev`](https://github.com/Azure/iotedgehubdev) - Pacchetto py per sviluppo, creazione, test, esecuzione con simulatore di moduli per iot edge.
* [`Azure/IoT Security Agent C#`](https://github.com/Azure/Azure-IoT-Security-Agent-CS) - Software che fa audit sulla sicurezza a livello OS del device e invia a Azure i risultati
* [`Microsoft/vscode-azure-iot-toolkit`](https://github.com/Microsoft/vscode-azure-iot-toolkit) - Estensione VS Code x iot
* [`Microsoft/vscode-iot-workbench`](https://github.com/Microsoft/vscode-iot-workbench) - Estensione VS Code x iot
* [`Microsoft/vscode-azure-iot-edge`](https://github.com/Microsoft/vscode-azure-iot-edge/) - Estensione VS Code x edge

## Esempi codice

* [Codice Laboratori](https://github.com/MicrosoftLearning/AZ-220-Microsoft-Azure-IoT-Developer)
* [Azure Samples IotHub](https://github.com/Azure-Samples/azure-iot-samples-csharp)
* [IotEdge E2E Example](https://github.com/Azure-Samples/iotedge-end2end-messageflow)
* [Architettura IoT (Edge Hub TSI SA EG DA DL)](https://github.com/Azure-Samples/azureiotlabs)
* [IoTEdge module](https://github.com/Azure-Samples/iot-edge-sample-dotnet)
* [Event Grid - IotEdge](https://github.com/Azure-Samples/azure-iot-edge-event-grid-sample)

***

## Sistemi Operativi IOT

* Windows IotCore
* Ubuntu Core
* Riot, QNX, Android Automotive

## Comandi AZ CLI

* `az login`
* `az account set -s {GUID}`
* `az group list`
* `az configure -d group={RgName}`
* `az iot`
  * `hub`
    * `create -n {Name}`
    * `list -g {RgName}`
    * `show -g {RgName} -n {Name}`
    * `show-connection-string -g {RgName} -n {Name}`
    * `device-identity` # (IOT-CLI)
      * `create -d {Device} -n {Name} --edge-enabled`
      * `show-connection-string -d {Device} -g {RgName} -n {Name}`
    * `monitor-events -n {Name}` # (IOT-CLI)
    * `query -n {Name} -q "{Query}"`
  * `dps`
    * `linked-hub list`

***

## Prezzi e limiti

### IotHub

* 5 Protocolli: AMQP AMQP-WS MQTT MQTT-WS HTTPS
* 3 Tiers: Free / Basic / Standard
* Per tutte max 1M device
* SLA 99.9%
* 50 IotHub / sottoscriz

La free non ha:

* Dimensione messaggio 4KB ma solo 0.5KB
* Solo 8K msg/u
* No upgrade ad altre tier
* Solo 1 IotHub / sottoscriz

Basic non ha:

* Device Streams
* Comandi Cloud-to-device
* Device Management
* Device Twin, Module Twin
* IoT Edge

| Resource | S1 | S2 | S3 | F1 |
| --- | --- | --- | --- | --- |
| Messages/day |400K |6M |300M |8K |
| Maximum units |200 |200 |10 |1 |
| Throughput/day | 1.5 GB/unit | 22.8 GB/unit | 1.14 TB/unit

D2C:

* Telemetria
* Reported State del Twin
* File Upload

C2D:

* Comandi (monodirezionali)
* Metodi invocati (acknowledgement)
* Desired State del Twin

#### Routing

**Default Endpoint**: Event Hub da cui leggere
**Custom Endpoint**:

* Event Hub
* Service Bus Topic (no session o duplicati)
* Service Bus Queue (no session o duplicati)
* Storage Container (ADLS o Blob)

Il routing aumenta latenza. È bene usare endpoint nella stessa area geografica.
Il routing verso custom endpoint può essere condizionale.

Così posso ottenere:

* *Filtraggio* messaggi con logiche di business
* Instradamento/*duplicazione* dello stesso messaggio su più catene
* *Arricchimento* del msg con proprietà statiche o tag del twin

**Route di fallback**: se non soddisfa alcuna condizione,
instrada verso un endpoint "discarica".

Posso gestire anche messaggi sul ciclo di vita del device.

Posso usare il routing anche con **Event Grid** attaccato a iotHub.
Si noti però che:

* Costa di più di iothub routing (msg = $)
* Peggiore per le telemetrie
* È integrato con tutti gli endpoint possibili
* Non garantisce ordinamento

#### Device Twin

* Tags: non visibili dal device
* Desired properties: stato desiderato specificato dal backend
* Reported properties: stato del device
* Device identity properties: info sul device. Read-only

### DPS

* Prezzo fisso: solo tier S1
* URL XXXXXXXXX.azure-devices-provisioning.net/
* Provisioning dei singoli device o a gruppi (enrollment group)
* Reprovisioning, Deprovisioning, blacklisting temporaneo (anche a livello di gruppi)

Vantaggi di un DPS

* **Scalabilità** verso più IoTHub
* Possibilità di fare provisioning di 1 device all'hub con **meno latenza** (geo-sharding)
* Non serve **inchiodare la connessione a IotHub** in catena di produzione
* Possibilità di gestire **multi-tenancy** con 1 hub / cliente

Tipologie di provisioning:

* *Evenly Weighted* - Round robin
* *Low Latency* - Minore latenza Device-IotHub
* *Static Config* - In base a un Enrollment (es. single tenant)
  * Enrollment singolo - con attestazione TPM o x509
  * Enrollment group - con x509 o SymmKeys - per molti device similmente configurati o dello stesso tenant

### Certificati

* x.509 standard x certificati a chiave pubblica (asimmetrica)
* emessi da CA per ambiente produzione
* emessi con openSSL o autofirmati per test
* file .pem o .cer è la parte pubblica
* è possibile creare catene di certificati.
  * certificati root
  * certificati intermedi/intermediate
  * certificati foglia/leaf
* Azure verifica il possesso dei certificati
  * viene caricato il pem/cer
  * azure genera un codice di verifica
  * viene firmato il codice con chiave privata
  * viene caricato il codice di verifica criptato come "proof of validity"
* Possibilità di gestire 2 certificati (primario/secondario) per evitare downtime 

***

## Pacchetti DotNet

* `Microsoft.Azure.Devices`
  * `class CloudToDeviceMethod` ▶ Metodo eseguibile su device
    * `CloudToDeviceMethod(str name)`
  * `class ServiceClient` ▶ Per mandare messaggi ai device
    * `CreateFromConnectionString(str conn)` ▶ crea un service client
    * `InvokeDeviceMethodAsync(str deviceId, CloudToDevice method)` ▶ invoca un metodo su un device
  * `class RegistryManager` ▶ Per gestire device e twins
    * `CreateFromConnectionString(str conn)` ▶ Crea un registry Manager
    * `GetTwinAsync(str deviceId)` ▶ Mi da il json di un twin
    * `UpdateTwinAsync(str deviceId, patch, twin.ETag)` ▶ Aggiorna tale twin
    * `CreateQuery(str sqlQuery, 100)` ▶ Esegue una query sulla registry dispositivi

* `Microsoft.Azure.Devices.Client`
  * `Enum TransportType` ▶ Protocollo Amqp|Mqtt|Http...
  * `class Message` ▶ Msg di comunicaz tra cloud e device, con prop
    * `Message(byte[] msg)` ▶ ctor x creare msg con body
    * `Dictionary<str,str> Properties` ▶ dizionario di proprietà tipo *metadati*
  * `class DeviceClient` ▶ Per mandare messaggi al cloud
    * `CreateFromConnectionString(str conn, TransportType t)` ▶ crea comunicaz verso cloud
    * `SendEventAsync(Message msg)` ▶ Invia messaggio al cloud
    * `SetMethodHandlerAsync(str methodName, MethodCallback callback, obj ctx)` ▶ Registra un metodo per un device, per rispondere a un comando da cloud (come fosse un'API)
    * `UpdateReportedPropertiesAsync(TwinCollection tc)` ▶ Invia lo stato che riporta il device al twin
    * `GetTwinAsync()` ▶ Mi restituisce i dati del twin
    * `SetDesiredPropertyUpdateCallbackAsync(callback, ...)` ▶ Setta un metodo al cambio del twin desiderato
  * `class MethodRequest` ▶ Msg per invocare metodi del device come fosse un'API
    * `byte[] Data` ▶ Contenuto richiesta
    * `str Name` ▶ Nome richiesta
  * `class MethodResponse` ▶ Risposta del device (come fosse un'API)
    * `MethodRequest(byte[] req, int httpCode)` ▶ ctor x obj di risposta

* `Microsoft.Azure.Devices.Provisioning.Client`
  * `class ProvisioningDeviceClient` ▶ Client per i device che usano DPS
    * `Create(str endpoint, str scope, SecurityProvider sp, ...)` ▶ Crea il client
    * `RegisterAsync()` ▶ Registra tramite DPS un device
  * `class DeviceRegistrationResult` ▶ Il risultato della registrazione del device
    * `str DeviceId` ▶ id device
    * `str AssignedHub` ▶ hub associato al device
    * `Status` ▶ Assigned|NotAssigned

* `Microsoft.Azure.EventHubs`
  * `class EventData` ▶ Messaggio di EH
  * `class EventHubClient` ▶ Client x leggere da EH o IoT
    * `CreateFromConnectionString(string conn)` ▶ Crea istanza di client
    * `CreateReceiver(string consGroup, string partId, EventPosition e)` ▶ Crea un Partition Receiver da cui leggere
  * `class PartitionReceiver` ▶ Lettore di partiz
    * `ReceiveAsync(int max)` ▶ Leggi {max} msg

* `Microsoft.Azure.Devices.Shared`
  * `class SecurityProviderSymmetricKey`
    * `SecurityProviderSymmetricKey(str registrationId, str pk, str sk)`
  * `class SecurityProvider`
    * `GetRegistrationID()`

* `AzureMapsRestServices/AzureMapsRestToolkit` ▶ NuGet per posizioni, geocoding, spatial route e cose così

***

## Architetture

* Lambda - Cold/warm/cool/hot path
* EH = messaging broker con + throughput
* ASA = message stream ad alta frequenza
  * Stream Input: Blob, EH, IotHub
  * Reference Input: Blob, SQL (metadati, es. x fare join)
  * Functions: Windowing, SQL Query, UDF.js, ML, Anomaly Detections, Spatial, Routing
    * 4 Windowing nel GROUP BY: Tumbling, Hopping, Sliding, Session
  * Output: EH, SQL, Blob, ADLS, Table, SBT, SBQ, CDb, PBi, AF
* EG = IotHub pubblica le seguenti famiglie di eventi:
  * Device Created/Deleted
  * Device Connected/Disconnected
  * Telemetry
* PBi
  * Connectors: SQL, ADX, Blob, Table, Cosmos, ADLS, HDFS, Spark
  * 3 livelli di view:
    * Data View - Tabelle coi dati e fonti
    * Model View - Relazioni fra tabelle e aggregate
    * Design View - Interfaccia e presentazione dato tramite le Visualizations (grafici)

***

## Moduli Edge

### Concetti Comuni

* Module Image: Container con il sw del modulo
  * Custom Image - Proprio codice (es. modulo di ML)
  * Other Images - Codice di altri (es. Azure Stream Analytics)
* Module Instance: Image che gira in un device
* Module Identity: Informazioni identificative di un modulo
* Module Twin: Json con info sul modulo. Dati e metadati

### Stato di un modulo

* Downloading
* Running
* Unhealthy
* Failed
* Stopped

## Edge Hub

Implementa AMQP e MQTT e fa da proxy locale x iot Hub.
* Demultiplexa le connessioni di + device in 1 connessione a iotHub in modo trasparente
* È una cache e consente di raggruppare i messaggi x risparmiare banda
* Funge da Bus nella comunicazione tra edge modules

## Edge Agent

* Gestisce ciclo di vita dei moduli
* Invia l'health dei moduli a IotHub
* Viene creato da Edge Security allo startup

### Struttura modulo

* `.env` contiene credenziali per ACR
* `deployment.template.json` Template per deploy
  * Sez. `systemModules` con dentro edgeHub e edgeAgent (runtime iot edge)
  * Sez. `modules` immagini custom da deployare
  * Sez. `$edgeHub.routes` qui si impostano le route tra moduli. Di default tempSimulator -> modulo -> iotHub
* `modules` Qui troviamo il codice dei moduli custom
  * `XYZ` Nome del modulo creato
    * `Program.cs` Entry point del modulo
      * `Init()` Inizializza modulo: crea client, apre conn e registra PipeMessage come handler dei msg
      * `PipeMessage(Message msg, object ctx)` Logica gestione msg

***

## IotCentral

* Device Templates - Gestione ontologia del device
  * Interfaces
    * Capability (Command|Telemetry|Property)
  * Views (dashboard personalizzate)
* Devices - Gestione asset. Lista device per interfaccia
  * New Device - Possibile creare nuovo device anche simulato
  * Vista device - Cliccando sul singolo device si vedono le custom views e si possono inviare comandi
* Jobs - Processi batch es. settare una property per gruppi di device. es. Optimal Temperature
* Analytics - grafici valori sui device



> P 221