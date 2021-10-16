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
* **APM - Application Performance Management** sistema di tracciamento di una app per vedere lo stato di salute e il monitoraggio di requisiti non-funzionali. Di solito manda notifiche quando ci sono degradaz performance. È un hub di messaggi di rete.
  * il controller dell'APM verifica regole
  * regole di avviso 1: valore supera o scende sotto quantità
  * regole di avviso 2: media di valori supera o scende sotto soglia
  * regole di avviso 3: uno o più valori sono fuori intervallo di tolleranza
  * regole di avviso 4: applicazione arrestata in modo anomalo
  * avvisi inviati tramite servizio spesso esterno, tipo mail o SMS
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
* **QoS** - qualità del servizio. È possibile e auspicabile definire delle metriche di base che determinano il buon funzionamento di un servizio. Queste metriche vanno rappresentate in Dashboard

## Processo di sviluppo

modalità di orchestraz
* waterfall
  * ogni fase dà il via alla successiva
  * globalmente + lento
  * meno resiliente al cambiamento
  * spesso il testing è lasciato x utlimo
* [agile - 2001](https://agilemanifesto.org/) (non è un processo ma una filosofia)
  * recapito incrementale
  * collaborazione tra team
  * pianificazione continua
  * apprendimento continuo
  * sviluppo a iterazioni (sprint 2-4w)
  * più resiliente ai cambiamenti
  * sviluppo in team verticali e interfunzionali (più full-stack, più scalabili, più orientati al prodotto), non orizzontali (catena di montaggio)
  * cambiamento culturale
    * efficacia riunioni e comunicazione
    * interruzioni da mail/call limitata e prioritizzata
    * soft skill come gestione conflitti
    * buona integrazione lavoro in presenza/da remoto
    * utili sw quali Teams, Skype, Slack, Hangouts, Asana, Trello, GoToMeeting...
> [Approfondisci](https://docs.microsoft.com/it-it/devops/plan/what-is-agile-development)
Obbiettivo: massimizzare valore, minimizzare spreco

Strumento: mappa di flusso del valore, o VSM
* si tracciano le attività a catena, si stimano i giorni e si individuano quelle che danno valore diretto al cliente
* serve a focalizzare il processo di sviluppo, individuare sprechi, accorgersi dell'efficienza globale


Process Time: T di valore diretto x il cliente
Total Lead Time: T di rilascio di una feature
Activity Ratio: Rapporto Process/Lead - % di efficienza

Strumento: Design Thinking. Capire il team e l'azienda, esigenze, obbiettivi e compiti di ciascuno

## Delivery Plan

* problema: comunicazione tra team diversi è difficile, la pianificazione deve considerare dipendenze
* soluzione: i **Delivery Plan** sono uno strumento per allineare scadenze, rilasci, sprint e attività di tutti i team.
* i DP devono essere visibili a tutti, in ottica di no-blame culture, di trasparenza
* i DP aiutano a predirre rischio di interdipendenze ed evitare schedule chicken
* utile l'estensione x AzDO che visualizza backlog in parallelo nel tempo con le sprint. Aggiunta marker x tracciare eventi (milestone) sulla base dei quali schedulare

Approfondisci gestione di più progetti ([portfolio-management](https://docs.microsoft.com/en-us/azure/devops/boards/plans/?view=azure-devops#portfolio-management))
> *Schedule Chicken* situazione che si verifica quando più team concorrono a un obbiettivo, ma nessuno dichiara ritardo perché spera lo faccia prima l'altro.

## DevOps
Cos'è
* unione di persone, processi e prodotto
* graduale, si possono adottare servizi e strumenti un po' alla volta
* percorso da seguire per diventare un team con prestazioni alte (rilasci frequenti e di qualità)
Cosa non è
* una posizione lavorativa, un software o una metodologia

> https://www.azuredevopslabs.com/

### Boards

> pianificazione, analisi e monitoraggio del lavoro e coordinamento tra team

* entità centrale: work item (cosa da fare/problema da risolvere). Si declina in entità diverse (WIT work item type) a seconda del processo adottato
* 4 modelli (process)
  * CMMI
    * per chi: complicato, x grandi aziende
    * segue il modello formal change management activities
    * Stati task: Original Estimate, Remaining Work, Completed Work
    * WIT: Epic, feature, requirement task / bug task / CR review issue risk
    * Ereditarietà dei backlog. Personalizzazioni aziendali con portfolio backlog
  * Agile
    * segue modello della Agile Alliance
    * Per chi lavora con scrum
    * WIT: epic feature US task / bug task / issue
    * Stati task: Original Estimate, Remaining Work, and Completed Work.
  * Scrum
    * meno complicato segue modello della Scrum Organization. Per chi lavora con scrum
    * WIT: epic feature PBI task / bug task / IMPEDIMENT
    * Stati task: Remaining Work
  * Basic
    * Stati task: todo doing done
    * modello per valutare devops, neofiti
    * WIT: epic, issue, task

> Molte estensioni utili, si scaricano da [visualstudio.marketplace.com](visualstudio.marketplace.com).

### Pipelines

> compilare, testare e distribuire software in qualsiasi linguaggio e piattaforma. CI/CD

### Test Plans

> testing esplorativo e manuale

### Repos

> repo illimitati, privati e su cloud. Oppure git pubblici

### Artifacts

> Creare, ospitare e condividere pacchetti (NPM, Maven, Nuget)

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

I dati coinvlti del monitoraggio sono 3:
* log - record di eventi
  * permettono di ricostruire una cronologia di eventi
  * in linux il servizio syslog raccoglie in `/var/log`
  * una *Log Monitoring Platform* è un software apposito che legge cartelle, streaming, database di log e li raccoglie centralmente. Garantisce correlazione, normalizzazione e reportistica
* metriche - valori quantitativi riferiti a un'unità temporale
  * descrivono integrità, stabilità e disponibilità di un sistema
  * orientate all'interno (logiche IT) e all'esterno (ai clienti e agli utenti)
    * es- esterno: valutazione dell'utente, durata sessione
    * es. interno: num errori, cpu, tempo risposta servizio web, volume coda richieste
* tracce - percorsi per programmi e servizi, sequenze di istruzioni
  * si usano codici di correlazione o probe (sonde) per capire collegamenti e dipendenze
  * cloud dà vantaggi di fluidità delle risorse -> mappa dell'applicazione fluida -> necessità di tracciare flussi
  * transazioni distribuite: quando un'operazione è suddivisa in vari passaggi tra servizi diversi
  * utile visualizzare queste in un grafo delle dipendenze

altre considerazioni:

* complessità e osservabilità sono indipendenti. Sistemi semplici a volte sono complessi da monitorare
* monitoraggio di un sistema ne garantisce la qualità (misurabile = migliorabile) es. supply chain altamente monitorata
* workflow: unisce componenti fisici e virtuali in un IT dept
* possono basarsi su un agente di monitoraggio opp no (basandosi sullo stato corrente di un server, es. ping a un URL)

Full stack monitoring - monitorare app a tutti i livelli.
* Monitorare a liv di sviluppo e in produzione.
* Monitor anche aspetti di sicurezza come IP sospetti o leakage di dati
* Come lo si ottiene in Azure?
  * usando AppInsights e Azure Monitor x performance, liveness
  * usando Azure Security Center e Sentinel x indagini e suggerimenti di sicurezza

Agente di monitoraggio

* componente remota che fa ping verso hub/controller centrale
* oppure daemon che funge da proxy del controller
* agente anche lato client, codice JS che comunica verso comp centrale, es. tempi di caricamento pagina

### Metriche comuni

Di base:

* richieste al minuto/sec
* Tempi di risposta (spesso mediano), TTFB
* Connessioni inattive
* Disponibilità servizi 99.5% (confronta con SLA)
* CPU %
* Errori %
* GC (utile sporattutto x interpretati - Java)

Più complessi (indicatori su metriche):

* punto di saturazione richieste
  * non facile distinguere utilizzo da sovrasaturazione.
  * Un server degrada le risposte rallentandole, a che punto si può dire che il server è sovraccarico?
  * Una possibilità è quando il server è così lento che il client va in timeout
* apdex
  * le metriche oggettive a volte non rispecchiano la realtà (spesso troppo complessa)
  * esistono metriche qualitative derivanti dal feeling di utilizzo dell'app
  * apdex = range 0 e 100: "soddisfazione", "tolleranza" e "frustrazione"
  * apdex calcolata sul tempo di caricamento pagina in una certa azione. es 0.5 soglia di tolleranza, 2 soglia di frustrazione.
  * critiche: puoi impostare le soglie come vuoi, tempi di caricamento dipendono dalla connessione del cliente
* USE - aggregazione di tre metriche usata in Oracle
  * Utilizzo (% attività)
  * Saturazione (caso in cui il server riceve migliaia di richieste al minuto. Il server ne risponde cento, senza problemi, rallentamenti o errori. Però la coda aumenta)
  * Errori
* RED - derivato di USE in Google, usato x Kube
  * Rate - richieste/min
  * Error - % richieste fallite
  * Duration - Tempo medio di risposta

### Correzione

* reattiva - qualcosa non va, fixo
* proattiva - qualcosa potrebbe non andare, provvedo

* soluzioni di ticketing (priorità) per gestire correzione reattiva
* tendenza attuale al focus su obbiettivi e non sui problemi (ticketing dà l'idea che problemi=ticket)
* prestazioni dell'app = ricavi. Il sistema influenza l'utente.
* monitoraggio KPI es. tempo medio compilazione modulo.
  * i KPI sono allineati con obb aziendali
  * servono a mandare un allarme ai responsabili

Correzione continua/giornaliera (proattiva)
* obbiettivi si evolvono nel tempo. Coinvolgimento dei dev nel processo
* piccoli cambiamenti monitorati
* conoscenza pratica del sistema per triage dei problemi
* sicurezza e performance vanno di pari passo. Un carico eccessivo è una vulnerabilità


### New Relic (One)

* 4 tipi di agenti:
  * agente APM da installare in sw compilati
  * agente app mobile
  * agente per browser. Si inserisce come tag HTML
  * agente per infrwatruttura. Si installa in VM o PC remoto
* agenti inviano dati ai server centrali di NR
* applicaz cloud di APM
* Insights, sistema di reportistica, avvisi e dashboard.
* Elenco dei servizi - grafo dei componenti con agenti in una web app

### Sumo Logic

* tecnologia agent-less (basata sui log) che però di fatto ha un agente di raccolta
* si ispira a hadoop con map-reduce. Algoritmo chiamato LogReduce
* query language proprietario per interrogare il db interno

### Prometeus

* creato da SoundCloud. Nato per monitorare ambienti a micro-servizi (es. Kube)
* Server centrale con TSDB e un agente per il recupero dei dati
* Servizi generalmente installano delle librerie x esporre endpoint di metriche da cui server fa pull
* server centrale invia metriche a alert manager
* Visualizzazione con Grafana o Web. PromQL per query

### Azure Monitor

* Raccoglie dati da applicazioni, OS di VM, Azure (risorse, sottoscriz e tenant, AD), altre fonti.
* I dati raccolti sono log (evento testuale, data e metadati) o metriche (data e info numerica, salvate su TSDB)
* Li mette a disposizione x altri servizi Azure
* Possono essere estesi:
  * abilitazione diagnostica (es. opt-in su Azure SQL x avere più metriche)
  * aggiunta di un Agente di Log Analytics su una VM
  * invio dati custom invocando un API REST via SDK
* Log e metriche interrogabili via Kusto, anche per visualizzazione su Dashboard
* monitoraggio di pod su Kube con *Azure Monitor container insights*
* monitoraggio di VM con agente, con *Azure Monitor VM insights* dashboard che riunisce tutte le VM in un'unica schermata
 
### Amazon CloudWatch

In AWS equivalente di APM su Azure
* meglio soluz cloud (AWS o AZURE) integrata xk consente autoscalabilità, meglio rispetto a New Relic o Sumo Logic

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
* dati divisi per Workspace. Raccomandazione: usane pochi. Gestisci accesso ai workspace tramite RBAC

### Application Insights

* Permette APM, monitoraggio delle metriche ed eventi (soprattutto in app web)
* Fa "health check" dei servizi web per valutare se sono vivi
* Può tracciare utenti tramite eventi di navigazione e profilarli
* **Live metrics streams** metriche in tempo reale
* **Metrics explorer** metriche storiche
* **Alerts** configura regole di notifica e soglie
* **Profiler** mostra come performano gruppi di richieste. es. pagine lente
* **Application Map** crea grafico di topologia applicativa aggiornato dal live check e performance. Utile x *bottleneck analysis*
* **Usage analysis** metriche utente, a mo' di Google Analytics
* Due modalità di configurazione di AI (instrumentation):
  * *runtime instrumentation*, config a caldo, no-code, per dati di base
  * *Build-time instrumentation*, va settato SDK, NuGET, dati completi
* Due versioni di AI:
  * server-side, quella standard. Gli applicativi ASP.NET (+Core) sono totalmente integrati con un NuGET. A Java, NodeJS serve SDK e codice.
    * SDK si installa così: `dotnet add package Microsoft.ApplicationInsights.AspNetCore`
    * Si setta la `APPINSIGHTS_INSTRUMENTATIONKEY`
    * Si invoca il metodo `.AddApplicationInisghtsTelemetry()` all'avvio
  * client-side, un file JS che manda dati di tempi di caricamento, tracciamento utenti, e altro. In windows può essere no-code anche questa, con l'environment flag `APPINSIGHTS_JAVASCRIPT_ENABLED`. In linux va cambiato HTML x forza.
* Visualizzazione dati:
  * da portale, e dashboards. Kusto per interrogare o fare grafici custom
  * su VS
  * su PowerBI
  * tramite le API su qualisasi applicativo custom
* metriche sono pre-aggregate e multidimensionali (es. InterazioniUtente = # like nell'ultimo minuto, # dislikenel minuto)

### Event Grid

* Hub per gestione eventi. Affidabile, veloce, semplice. Quindi, non economico.
* Eventi da EventHub, Sottoscrizioni, RG, publisher di eventi personalizzati, blob...
* Azioni di vario tipo (Function, Logic App, Runbook, WebHook, ServiceBus...)
* Gestione a "Topic" per filtrare i messaggi ai subscriber

### Azure Security Center

* monitoraggio centralizzato sicurezza della soluzione (sia on-cloud che on-premise)
* raccomandazioni su misure da adottare contro vulnerabilità, interventi di mitigazione
* raccoglie dati da Azure Monitor Logs
* integrato in PaaS. in IaaS va installato agent
* aiuta a classificare gli incidenti in: positivi, falsi positivi e i dubbi
* permette di eseguire playbook, sequenze di mitigazioni a un evento
* es. JIT access x VM (in Azure Defender), es. analisi processi con ML (adaptive application controls)

### Azure Sentinel

* raccoglie da varie fonti anche esterne, multi-cloud, on-prem (utile x unificare)
* analizza con IA le minacce distinguendo veri e falsi positivi. Piattaforma **investigativa** e di **rilevazione**
* consente di allertare con Logic App in caso di incidente (playbook)
* aiuta a classificare gli incidenti in: positivi, falsi positivi e i dubbi
* crea una mappa "investigation map" che collega gli elementi e aiuta con drill down
* raccoglie dati da Azure Monitor Logs (prima crea il workspace!)
* unico ambiente centrlaizzato dove correlare metriche di performance, sicurezza, e problemi

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

## GitHub
 
GitHub (GH) è una suite per VCS e CI/CD.

Le GH **Actions** sono come le Pipeline di DevOps. Si basano su un perverso sistema di credito a minuti mensili.
 
> **Ocio**! I minuti delle actions sono un credito.
> Usando 1 minuto di Actions su Linux consumo 1 minuto di credito.
> Usando 1 minuto di Actions su Windows consumo 2 minuti di credito.
> Usando 1 minuto di Actions su Mac consumo 10 minuti di credito.

Il sistema di archiviazione ha una fatturazione a sé sempre su base mensile.

In buona sostanza, conviene usare **Git LFS** (Large File Storage) per file pesanti.
 
* Versioni: Free/Pro/Team/Enterprise
* Account: Personale/Organizz/Aziendale

* **Free** - dev singoli poveri, gruppi e organizzioni
  * 2K minuti di GitHub actions
  * 500 MB archiviazione
* **Pro** - dev + pro
  * wiki, branch protetti, github pages, PR, owner, grafici, riferimenti atomatici
  * 3K minuti di Actions
  * 2 GB archiviazione
* **Team** - come pro ma x team (per PR)
* **Enterprise** - GHE
  * può stare on-premise
  * supporto tecnico
  * autenticazione integrata con azienda (SSO SAML)
  * ti fanno pure il caffè
* **GHE Cloud**
  * ACL per GH pages
  * SLA 99.9%
  * fatturaz centralizzata
  * 50K minuti Actions
  * 50 GB archiviazione
 
