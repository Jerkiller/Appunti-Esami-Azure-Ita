
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
  * `dps`

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

### DPS

Prezzo fisso: solo tier S1

***

## Pacchetti DotNet

* `Microsoft.Azure.Devices`
  * `class CloudToDeviceMethod` ▶ Metodo eseguibile su device
    * `CloudToDeviceMethod(str name)`
  * `class ServiceClient` ▶ Per mandare messaggi ai device
    * `CreateFromConnectionString(str conn)` ▶ crea un service client
    * `InvokeDeviceMethodAsync(str deviceId, CloudToDevice method)` ▶ invoca un metodo su un device

* `Microsoft.Azure.Devices.Client`
  * `Enum TransportType` ▶ Protocollo Amqp|Mqtt|Http...
  * `class Message` ▶ Msg di comunicaz tra cloud e device, con prop
    * `Message(byte[] msg)` ▶ ctor x creare msg con body
    * `Dictionary<str,str> Properties` ▶ dizionario di proprietà tipo *metadati*
  * `class DeviceClient` ▶ Per mandare messaggi al cloud
    * `CreateFromConnectionString(str conn, TransportType t)` ▶ crea comunicaz verso cloud
    * `SendEventAsync(Message msg)` ▶ Invia messaggio al cloud
    * `SetMethodHandlerAsync(str methodName, MethodCallback callback, obj ctx)` ▶ Registra un metodo per un device, per rispondere a un comando da cloud (come fosse un'API)
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

## Moduli Edge

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
