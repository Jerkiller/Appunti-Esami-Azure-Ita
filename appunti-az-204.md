
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
* Job - viene assegnato a un pool, è una seq di Task (comando,script o app eseguito su un nodo).


P.23

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
* Evento (http, msg...)
* Dati
  * Polling (frequenza * intervallo check)
  * Pushing (implementato con webhook)

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
