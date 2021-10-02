# AZ400

* [Pdf del corso](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3VP8d)
* [Skillpipe](https://www.skillpipe.com/#/bookshelf/books)


## Core concepts

* **UI testing** processo automatizzato di testing funz della parte grafica e esp utente
* **CI Continuous integration**. Pratica di eseguire più volte verifiche automatizzate x verificare che il SW è in uno stato di funzionamento corretto.
* **CD Continuous Delivery**. Pratica che ad ogni feature del SW procede al rilascio automatizzato.
* **Device Configuration** combinazione di sistema operativo e modello dispositivo. es. *iPhone XS con iOS 12.4*
* **Device Tier** fascia di dispositivi, dove 1 sono i + nuovi e 3 i - utilizzati e vecchi
* **Device Set** gruppo di device configuration x task automatizzati come testing
* **Test Run** esecuzione di test di un binario su un device set
* **Distribution Group** gruppo di utenti che ricevono un rilascio (pubblico o privato). Es. alpha-beta-prerelease-QA tester
* **Analytics** analisi dati, atta a esaminare pattern. Es. app analytics = raccolta dati x utilizzo app da parte di utenti
* **Resources Consumption** consumo computazionale da parte di un app. È misurabile.
  * Unità fisiche come % CPU, RAM, banda
  * Unità virtuali come RU (ComsosDB), DTU/VCore (SQL).
* **Scaling** processo di ridimensionamento di risorse per soddisfare requisiti diversi (es. carico). Questo processo garantisce minima interruzione e nessuna perdita dati
  * *Vertical scaling* quando si passa alla stessa risorsa con più o meno potenzialità computazionali. Si parla di *scaling up/down*.
  * *Horizontal* quando si passa a più/meno istanze della stessa risorsa. Si parla di *scaling out/in*.
  * *Autoscaling* automazione, permessa grazie al cloud, del processo di scaling. Può avvenire in base a schedulazione oraria oppure su metriche (es. 5.00-17.00 oppure CPU>80%). Molto usato nel pattern serverless.
  * *Strategia di scaling* va misurata in base ai carichi, ai servizi offerti (stateless, stateful), alla rapidità di avvio/deallocaz servizi.
* **Throttling** in un ottica di servizio, quando un servizio non riesce a soddisfare le richieste, ne ignora quelle in più (strozzamento). Usato con API.
* **Container** sistema di virtualizzazione a livello di OS. Più leggero, e quindi ottimale per lo scaling. In azure possono essere usati su Web App, ACI, AKS... Si può collegare ACI in AKS tramite virtual Kubelet es. per carichi superiori a AKS
* **Latency** Na brutta bestia. Tempo di comunicazione su una rete. Rountrip è tempo di comunicazione A/R.
  * La latenza peggiora esperienza utente
  * Data dalla somma delle latenze tra componenti architetturali es. utente-webserver-api-database.
  * Strategie miglioramento latenza
    * zona Azure unica e vicina all'utente finale
    * Traffic manager che decide a che zona instradare traffico (tramite DNS resolution)
    * DB distribuiti o replicati cm Cosmos o SQL o Sharding
    * Cache come Redis o CDN
    * ExpressRoute per collegare con VPN site-to-site da on-prem a Azure a bassa latenza
* **Storage Performance** misure di accesso alla memoria
  * si misura in IOPS I/O per second
  * Strategie - dipendono dalle esigenze dell'applicazione
    * Nelle VM (IaaS), utilizzo di Premium Storage SSD per produz. (Il local SSD storage è solo per dati temporanei). D2s_v3 dove s indica premium SSD. Si può abilitare il disk striping (I/O in parallelo, come nei db).
    * Caching delle risorse (es. redis)
    * Polyglot persistence (usare DB e fonti dati diverse per risorse diverse). A volte è dura sincronizzare dati diversi
    * Eventual Consistency - permette di migliorare accesso a risorse in RW. Può fornire dati non sempre aggiornatissimi.
* **Nonfunctional requirements** come l'applicazione deve rispondere. Es. soglia di secondi caricamento pagina.
  * Vanno definiti a priori
  * Utili per impostare regole di monitoraggio
* **APM - Application Performance Management** sistema di tracciamento di una app per vedere lo stato di salute e il monitoraggio di requisiti non-funzionali. Di solito manda notifiche quando ci sono degradaz performance.
* **microservizi** sw isolato a componenti che viene scalato, rilasciato indipendentemente. I microservizi possono essere scritti in linguaggi diversi da team diversi. Spesso si interfacciano via rest.
* **Infrastructure as code (IaC)** gestione delle infrastrutture tramite un modello descrittivo, versionabile, come fosse codice. È paradigma del DevOps. Risolve il problema del Environment Drift.
  * imperative approach - via CLI. Problema di mantenibilità, difficile scalabilità
  * declarative approach - Azure Resource Manager (ARM) template: parameters, variables, resources, e outputs
  * misto - spesso si crea un ambiente, ma poi le VM vanno popolate. Fare attenzione ai tempi di avvio, deploy e a come aggiornare le immagini.
   * custom images. Occhio a tenerle aggiornate tramite un proc apposito.
   * Post-deployment scripting - come per esempio scriptoni powershell o Azure Automation Desired State Configuration (DSC).
  * operazioni schedulabili - Azure Automation. Es. cercare dischi orfani, spegnere/accendere VM, Patchare sistemi operativi
  * Azure DevTest Labs per creare ambienti di sviluppo isolati, molto flessibli. Spegni accendi e paghi.
* **Shift Left** paradigma che prevede che i test siano fatti più a sx nel tempo, verso lo sviluppo e non verso il rilascio. Gli errori costano meno.

## Testing

* manuale - Azure Testing Plans
  * più costosi
* automatico - Azure Pipelines
  * unit tests - test veloci (~30") sul codice. Sintassi, correttezza, verifica con I/O. Sia su codice puro che su infrastrutturale
  * smoke test - test su singola componente (~15'). Correttezza, verifica I/O.
  * integration test - test multi-componente (~ore). Spesso schedulati di notte. Verifica interoperabilità tra componenti.

### Acceptance testing

Risponde alla domanda: una determinata versione del software è rilasciabile?

* blue/green deployment - parte utenza reindirizzata a nuova versione. Se tutto va bene, indirizza tutti alla nuova versione e decommissiona la precedente (deployment slot di ASP)
* canary release - gruppo di utenti selezionato x avere una nuova funzionalità (spesso tramite *feature flag*)
* A/B testing - test x validare efficacia di una funzionalità. 50% utenti hanno la funz, 50% non ce l'hanno. Con delle metriche (es. tasso di conversione) valido esperimento. (User Behavior Analytic su Application Insights)

### Altri test

* stress test - importante testare l'infrastruttura per carichi improvvisi. Occorre verificare che dopo uno spike, torni al normale funzionamento.
* fault injection - con la chaos engineering si prova a mettere in crisi il sistema. Si testa disaster recovery, ungraceful shutdown, rallentamenti di rete
* security test - verifica vulnerabilità, sw automatici e analisi statica. Oppure tecnica del *red team* che prova a bucare il nostro SW.

## Monitoring

Diviso in 3 grandi aree:

* Core Monitoring - activity log (attività su Portal Azure), health services (stato servizi azure), metrics and diagnostics, raccomandazioni e best practice (sicurezza o costo)
* Deep App Monitoring - App Insights - permette di vedere log applicativi, comportamento utente e performance applicative. Es. carico del webserver, #404, tempo medio risposta...
* Deep infrastructure monitoring - Log Analytics, metriche specifiche raccolte da IaaS o prodotti cm SQL server o Windows Server AD. Es. mail quando SQL è all'90% memoria

### Azure Monitor

* Raccoglie dati da applicazioni, OS di VM, Azure (risorse, sottoscriz e tenant, AD), altre fonti.
* I dati raccolti sono log (evento testuale, data e metadati) o metriche (data e info numerica, salvate su TSDB)
* Li mette a disposizione x altri servizi Azure
* Possono essere estesi:
  * abilitazione diagnostica (es. opt-in su Azure SQL x avere più metriche)
  * aggiunta di un Agente di Log Analytics su una VM
  * invio dati custom invocando un API REST via SDK
* Log e metriche interrogabili via Kusto, anche per visualizzazione su Dashboard

#### Kusto

Kusto Query Language (KQL) è usato per serie temporali organizzate in tabelle con righe e colonne.
* MS ha palesemente portato la sintassi di Splunk facendo qualche replace
* Una query ha almeno una fonte dati (es. Metrics, Heartbeat)
* Linguaggio case-sensitive. Generalmente in snake_case. Tabelle e Colonne in PascalCase.
* Sequenze di istruzioni separate da `;`
* Concatenazione di operazioni separate da `|`
* `where`, `count`, extend`, `summarize`, `take 10`, `top 10 by Xyz`
* Tipi base come `datetime(2018-11-01)`

### Log Analytics

* Permette di analizzare e fare query su dati raccolti da varie fonti
* Fonti sono Azure Monitor, App Insights, Az Security Center, API, Powershell
* È un hub centrale di raccolta e permette varie azioni tra cui:
  * invocazione di funzioni: Logic App, Alert, Script o WebHook API
  * visualizzazione: Azure Dahsboard, PowerBI, Analytics, Workbooks (tipo dashboard ma con vari elementi grafici, testo)

### Application Insights

* Permette APM, monitoraggio delle metriche
* Fa "health check" dei servizi web per valutare se sono vivi

## Logging

### App Logging

* In ASP.Net apps System.Diagnostics.Trace, a 4 livelli: error, warning, information, and verbose.
* In ASP.NET Core apps, c'è il logger che espone 6 metodi con livelli diversi: Trace, Debug, Info, Warning, Error, Critical.
* In Node.JS vengono usati console.log e console.error.

* Su App Service Plan WIN, i log vengono sempre gestiti da IIS.
* Su ASP LINUX, i log vengono gestiti a seconda della docker img, es. PHP vs Py.

I log possono essere scritti su:

* File System logging viene rimesso a OFF dopo 12h.
* Blob Storage, impostando la retention x svecchiarli. (un tempo nn si potevano scrivere su blob con Linux, ora sì)

I log possono essere consultati con Live Streaming quando:
* sono pochi (fase iniziale o al deploy)
* sono di una app sola (non multi istanza)

#### Impostare logging via CLI

Solo per logging su file system:

`az webapp log config --application-logging true --level verbose --name <app-name> --resource-group <RG-name>`

Impostando a false, nn si disattiva, ma si imposta a error only.

#### Impostare App Log da Azure

Sulla WebApp, blade principale, sezione Monitoring Logs, voce App Service Logs.

#### Vedere i log

In live streaming con CLI:
`az webapp log tail --name <app name> --resource-group <RGname>`

Staticamente con CLI:
`az webapp log show --name <app-name> --resource-group <resource-group-name>`

Via CURL:
Fa leva su FTP. Si crea un utente FTP per il deploy.
`az webapp deployment user set --user-name <name-of-user-to create> --password <new-password>`
Poi si lancia il comando `curl -u {username} https://{sitename}.scm.azurewebsites.net/api/logstream`

Su Windows ASP:

Sono nella cartella `D:\Home\LogFiles`. Nelle sottocartelle Application, Application, DetailedErrors, http, W3SVC404, W3SVC500...
Sono divisi in sottocartelle a seconda di anno, mese, giorno, ora. Sono in CSV.

Su Linux ASP:

Si accedono via SSH alla Docker img.

`az webapp log download --log-file <filename>.zip  --resource-group .. --name ..`

Via Kudu:

Kudu è piattaforma di **Source Control Management (SCM)**. Si accede a https://<app name>.scm.azurewebsites.net con credenziali di deployment.
Oppure si accede da Azure con *Development Tools* > *Advanced Tools*.
Da Kudu, poi si può scaricare lo zip dei file da Windows, e da Linux lo zip dei log di docker

Alternative x Blob Storage:
  
Si accede ai log su Blob e si aprono in excel. Come accedere? Da portal azure, da cli o da Azure Storage Explorer.

### Application Insights

Aggiungi codice per raccogliere in un logger centrale. AI funge anche da bidone x le metriche.

## VS App Center
 
Prodotto che consente di compilare app per IoS, Android, ecc. Consente CI/CD, UI testing, raccolta metriche utilizzo app.
Il portale PaaS è ubicato a https://appcenter.ms.
È vantaggioso perché permette di integrare e migliorare i processi aziendali, efficienza e qualità.
 
### App Center Build

* Infrastruttura cloud di build x il rilascio di App.
* Rilascio può essere manuale o automatizzato (es. al commit su branch).
* Rilasci possono essere firmati.
* Possono essere lanciati script custom con hook di pre-build, post-build, ecc.
* Può fare screen di come app renderizza su vero device.

Alcuni vantaggi:

* VM pulita a ogni build, no side-effect
* VM aggiornata al latest, più sicura
* VM pronta per OS specifico, velocità di setup
* Test run effettuato su dispositivi fisici
* Distribution group gestiti con AAD
* Shared Distri Group (per + app)
* semplificaz deploy IoS su Distri Group (non serve gestire UDID su IoS portale)
* portale centrale, CLI o API per effettuare release
 
#### Supporto
 
Piattaforme: iOS, Android, UWP, e tvOS
Linguaggi: Swift, Kotlin, Xamarin, Java, Unity, o React Native
VCS: GitHub, Bitbucket, Azure DevOps
Store: Apple App Store, Google Play Store, MS Intune

### App Center Diagnostics
 
* SDK per alimentare un HUB di diagnostica.
* Ad ogni utente è associato un ID. Oltre ai dati evento e id utente possono essere mandati altri dati in KV pairs.
* Il portale notifica, aggrega errori simili e li fa visualizzare.
* Retention dati configurabile 28-90d. Estendibile con export continuo su blob.
 
Metodi invio:

* Quando App Crasha, salva stato su memoria dispositivo. Alla successiva apertura manda i dati diagnostici + metadati evento
* Programmaticamente, es. in un Try/catch usando `Crashes.TrackError`, solo in Xamarin, Unity, WPF, and WinForms SDKs
 
### App Center Analytics
 
* raccolta dati utenti e analisi in un portale.
* vanno individuate delle Key Metrics. Per esempio, qual è il SO degli utenti, il device, la localizzazione, la durata sessioni, gli utenti attivi
* possono essere racoolti eventi custom per metriche specifiche. Es: tipo file uploadato, pagina in cui abbandonano
* lo storico di una sessione aiuta a ricostruire un bug (pagine visitate, azioni intraprese)
* Retention dati configurabile 28-90d. Estendibile con export continuo su blob.

## DevOps

Unione di persone, processi e prodotti per fornire valore continuo all'utente finale.
Suite di tool diversi
* Boards - lavagne kanban e scrum, backlog e dashboard per i team
* Repos - VCS, integrato con GitHub ma aziendalmente privato
* Pipelines * strumenti di CI/CD
