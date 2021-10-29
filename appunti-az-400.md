# AZ400

* [Pdf del corso](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3VP8d)
* [Skillpipe](https://www.skillpipe.com/#/bookshelf/books)
* [examtopics - test gratuito](https://www.examtopics.com/exams/microsoft/az-400/)
* [ravikirans - materiali vari](https://ravikirans.com/az-400-azure-exam-study-guide/)

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
* ChatOps = automazione per mezzo di chat e bot di processi come CI/CD
* **VCS** sistema di condivisione codice tra persone/team.
  * centralizzato (TFVC) dove tutti lavorano sullo stesso codice (se uno fa un bad commit blocca tutti)
  * distribuito (Git) dove tutti possono avere una copia del codice (clone), poi alla bisogna si sincronizza (magari con regole di review)
  * privato o pubblico in base alla visibilità
  * hosting è sui propri server o su web-based solution come GitHub o Bitbucket
  * spesso si basa su meccanismi di branch. Il branch principale è main o trunk.
* package - componente software, libreria con sw riutilizzabile tra più progetti.
  * In ling compilati => codice compilato DLL, class
  * In ling interpretati => codice sorgente puro (js py)
  * Spesso zippati, con nome, descriz e versione
  * Servono ad evitare la duplicazione => deriva (drift), a ridurre dimens proj
  * Problema è che deve essere scritto e testato x funzionare bene dappertutto (è un altra codebase da mantenere)
  * individuare componente sw?
    * dove c'è duplicazione può esserci riuso
    * dove c'è alta coesione tra parti di codice, alto accoppiamento, può essere una componente unica
    * dove c'è basso accoppiamento, va separato il + possibile
    * dove c'è un ciclo di vita individuale, in + parti di codice, splittare. Eventualmente potrebbero mantenerlo due team diversi?
    * dove c'è stabilità e poco cambiamento, forse è ora di portare il pacchetto in libreria
    * dove c'è indipendenza, qualcosa può essere isolato
    * dove ci viene segnalato dai tool di dipendenze e codice duplicato
  * NuGet, NPM, Maven, Docker
* Approccio OKR - Objectives & Key Results. Obbiettivi e risultati concreti e verificabili. Di solito un ciclo è di 3 mesi. OKR in tutti i team in azienda
  * ogni obbiettivo deve stare in una riga. Ogni azione dell'azienda deve essere allineata a un obbiettivo. Impegno, rilevamento.
  * OKR spinge aziende oltre i propri limiti
  * OKR è collegato a scrum. Mission/vision = OKR livello alto, viene declinata su N obbeittivi OKR. Ciascuno di questi hanno M risultati chiave (scenari o iniziative). Queste sono le epics.
* Continuous Delivery è testing automatico, build (CI), integrazione ma ha un rilascio manuale (struca boton)
* Continuous Deploy invece è tutto automatico. Dalla CI al test di accettazione. Più rilasci al giorno
* Continuous Integration è la parte di testing e build automatico di artefatti. Prerequisito x CD. Senza di questa: debito, codice buggato, branch lunghi, conflitti, lacrime, urla, hackering, merda, molto tempo perso
* Continuous Quality - modo totale di intendere la qualità. Test sempre, da tutti, su tutto. I bug possono essere prevenuti. Root causa analysis. I bug non li fanno solo i dev ma anche il Product Owner, lo story Writer, il cliente... Qualità prima di tutto vuol dire avere meno debito tech, meno bug, meno ticket.
* Continuous Collaboration - paradigm shift di cooperazione tra team e persone (in base al manifesto agile). I team a silos, fanno soluzioni a silos. Team interfunzionali creano soluzioni + creative e aperte
  * continuous feedback - MVP, release spesso, hypothesis-driven development, feedback al cliente
* Contiuous Improvement - misurare x migliorare. Cosa misurare? Misurare l'impatto, non l'azione. es. lead time,freq deploy, MTTR, % regressioni su cambiamenti. Performance app... Cosa NON va misurato? velocity, num. bug, LoC, burndown, stime e consuntivi. Bisogna misurare sulla base del valore e migliorare su quello. Es. lead time
* Continuous Monitoring
* MTTR = Mean time to restore//repair = MTTI+MTTK+MTTF+MTTV = Identify+Know+Fix+Verify
* Osservabilità - capacità di rendere disponibli dati di un sistema
* 5 disfunzionalità di un team:
  * Assenza di fiducia
  * Paura del conflitto
  * Mancanza di impegno
  * Evitare le responsabilità
  * Disattenzione nei confronti dei risultati
* Inner Source - pratiche dell'open source portate a livello aziendale (tutti contribuiscono nelle 4 mura dell'azienda)
* Gerarchia dei livelli di reliability di Dickerson - piramide ispirata a quella di Maslow per descrivere i livelli di affidabilità di un sistema. [dickersons-hierarchy-of-service-reliability](https://www.zenoss.com/blog/dickersons-hierarchy-of-service-reliability).

## Processo di sviluppo

modalità di orchestraz

* waterfall
  * ogni fase dà il via alla successiva
  * globalmente + lento
  * meno resiliente al cambiamento
  * spesso il testing è lasciato x utlimo
  * un progetto su 10 riesce
* [agile - 2001](https://agilemanifesto.org/) (non è un processo ma una filosofia)
  * recapito incrementale
  * collaborazione tra team
  * pianificazione continua
  * apprendimento continuo
  * 4 progetti su 10 riescono
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

### Data-driven DevOps

* **Process Time**: T di valore diretto x il cliente
* **Total Lead Time**: T di rilascio di una feature
* **Activity Ratio**: Rapporto Process/Lead - % di efficienza
* **Lead time** measures the total time elapsed from the creation of work items to their completion.
* **Cycle time** measures the time it takes for your team to complete work items once they begin actively working on them.
* **Idle Time** tempo morto in cui non viene prtato avanti un work item
* **Burndown** lavoro rimanente

Strumento: Design Thinking. Capire il team e l'azienda, esigenze, obbiettivi e compiti di ciascuno

## pianificazione continua

in ottica agile, è importante avere piani resilienti, non resistenti ai cambiamenti.
diverso da pianificazione statica (triangolo di ferro)

* semplicità ha valore
* agile manifesto
* design thinking (viability, feasibility, and desirability) - MVP
* sviluppo incrementale
* lean management - miglioramento continuo, capire il flusso di valore, eliminare sprechi di valore
* stime accurate - stima fatta dai tecnici è chiave. stima accurata e precisa sono concetti diversi. dalla stima dipendono obbiettivi e commitment

## Delivery Plan

* problema: comunicazione tra team diversi è difficile, la pianificazione deve considerare dipendenze
* soluzione: i **Delivery Plan** sono uno strumento per allineare scadenze, rilasci, sprint e attività di tutti i team.
* i DP devono essere visibili a tutti, in ottica di no-blame culture, di trasparenza
* i DP aiutano a predirre rischio di interdipendenze ed evitare schedule chicken
* utile l'estensione x AzDO che visualizza backlog in parallelo nel tempo con le sprint. Aggiunta marker x tracciare eventi (milestone) sulla base dei quali schedulare

Approfondisci gestione di più progetti ([portfolio-management](https://docs.microsoft.com/en-us/azure/devops/boards/plans/?view=azure-devops#portfolio-management))
> *Schedule Chicken* situazione che si verifica quando più team concorrono a un obbiettivo, ma nessuno dichiara ritardo perché spera lo faccia prima l'altro.

## DevOps

Cos'è:

* unione di persone, processi e prodotto per fornire valore continuo all'utente finale.
* graduale, si possono adottare servizi e strumenti un po' alla volta
* percorso da seguire per diventare un team con prestazioni alte (rilasci frequenti e di qualità)

Cosa non è:

* una posizione lavorativa, un software o una metodologia

> [DevOps Labs](https://www.azuredevopslabs.com/)

**Azure DevOps** - Suite di tool diversi

* Boards - lavagne kanban e scrum, backlog e dashboard per i team
* Repos - VCS, integrato con GitHub ma aziendalmente privato
* Pipelines * strumenti di CI/CD

profili:

* Stakeholder
* Basic & Visual Studio Professional (VSE) - feature complete
* Basic + Test Plans - estensione con i test plan

### Az CLI estensione

* estensione x az cli si installa `az extension add --name azure-devops`
* consente gestione amministrativa come:
  * gestione Teams
  * gestione Progetti
  * gestione Wiki
  * gestione Sicurezza
  * getsione [Banner](https://docs.microsoft.com/it-it/azure/devops/organizations/settings/manage-banners?view=azure-devops)

### Integrazione ADO Teams

* Connettori da Teams: consentono di essere notificati:
  * CRUD su Release/PR/Code/WorkItem/build
  * Filtri su Teams, AreaPath, WorkItem Type, Filtri testuali, Branch, Gruppo utente, Build Status, Definition
* Aggiunta TAB su un Team riguardo Dashboard o Kanban Board
* Aggiunta come Teams App di **Azure Pipelines**
  * Viene aggiunto a mo di BOT, lo si richiama con il mention `@azure pipelines`
  * Prima cosa da fare `@azure pipelines signin`
  * Sottoscriversi a un prog `@azure pipelines subscribe [project url]`
  * Gestire sottoscrizioni `@azure pipelines subscriptions`
* Aggiunta come Teams App di **Azure DevOps**
  * Con quest'app si possono cercare Work Item e formattare la preview del link in un'ottica collaborativa

### Integrazione ADO tramite Service Hooks

* Meccanismo Pub-sub: all'evento su ADO, scatena l'inferno sui vari subscriber
* Funziona tramite API di terze parti autenticate spesso con Token
* Servizi integrabili:
  Pipelines|Collaborate|Customer support|Plan and track|Integrate
  -|-|-|-|-
  AppVeyor|Flowdock|UserVoice|Trello|Azure Service Bus
  Bamboo|HipChat (No longer supported)|Zendesk||Azure Storage
  Jenkins|Hubot|||Grafana
  MyGet|Office 365|||Web Hooks
  Slack|Slack||Slack|Zapier
  Microsoft Teams|Microsoft Teams||Microsoft Teams|Datadog
* Se la repo è su GH, ADO pipelines può essere eseguita autenticandosi con **GH App**. In alternativa anche OAuth o PAT, ma non può partire in automatico la pipeline.

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
>
> Boards integrabile con GitHub come OAuth App su una repository

### Pipelines

> compilare, testare e distribuire software in qualsiasi linguaggio e piattaforma. CI/CD

Vantaggi:

* compilazione su agente cloud elimina costi mantenimento server e tempo.

#### Pipeline

* si collega a un repo come: GitHub, SubVersion, BitBucket
* parte manualmente o automaticamente
* un *build agent* esegue serie di task
* viene emesso un *build artifact* (libreria, docker img, pacchetto, app compressa, VM, installer .msi)
* nella pipeline si possono usare **variabili**. $(myVar) che viene rimpiazzata a runtime. Ci sono var di default come $(Build.BuildId)
* nella pipeline si possono ri-usare dei task come template ${{ myTasks }} parametrizzabili
* utile settare variabili segrete usando il pipeline editor (ovviamente nn vanno in YAML)
* right-click publish è + utile di CD se vuoi fare un progetto piccolo e fare 1 o 2 deploy.

> parti dai comandi bash, poi mappa ogni comando con un task della pipeline

Trigger funziona con black/white listing

trigger:
  branches:
    include:
    - '*'     # build all branches
  paths:
    exclude:
    - docs/*  # exclude the docs folder

Altri trigger sono: PR, CI trigger, Scheduled trigger, build completion trigger

> Pipeline > Stage > Job > Task/Step

Condition: un sistema per determinare se un job/stage ecc verrà eseguito o no.
Utile per gestire il release management: es. se il branch è release fai release anche su prod.

```yml
condition: |
  and
  (
    succeeded(),
    eq(variables['Build.SourceBranchName'], 'release')
  )
```

Altre condition sono le **release approval**: dei gate manuali che rendono la pipeline un proc semi-automatico.
Utile per passare da uno stage/environment all'altro. Utile x verificare a k step la qualità degrada, dove si rompe la catena di solito?

**Environment**: rappresentazione astratta dell'ambiente. Con una **service connection** può essere usato x rilasciare su quel resource group/risorsa/subscription ad esempio.

* Dev - ambiente dove i dev si divertono e testano
* Test/QA - ambiente x validare e fare acceptance test
* Staging - altri stress test e demo interne al management
* Demo - ambiente con cui i commericiali mostrano feature
* Production/Release - ambiente cliente finale

* [Service Connection](https://docs.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints?tabs=yaml&view=azure-devops) - auth di 2 tipi
  * service principal auth - una specie di acount che agisce automaticamente per conto di qualcuno
  * managed identities 4 azure resources - Feature di AAD, servizi hanno un'identità nel tenant e si collegano automaticamente

* Strategy: Modalità di esecuz pipeline di deploy, es. RunOnce  BlueGreen o Canary, vedi *"deployment pattern"*

* Library - serve a gestire variable group x non cablare valori

* Pipelines Analytics - stats sulle pipelines
  * feed OData disponibile x integrare con Slack, Teams, PowerBi
  * KPI
    * pass rate della pipeline
    * pass rate dei test
    * durata media

* Integrazioni: CI fatta con CircleCI, GitLab, Jenkins e poi deploy fatto con Azure Pipelines

* Documentazione - andrebbe automatizzata insieme alla pipeline. Es usando una [Azure Function con Web Hook triggerata alla release](https://docs.microsoft.com/en-us/samples/azure-samples/azure-devops-release-notes/azure-devops-release-notes-generator/).

#### Build Agent

* può essere self-hosted o cloud-hosted
  * self-hosted
    * (++) installa un agente su linux, macos, win o linux docker img
    * (++) Puoi estenderlo aggiungendo tutto il sw che ti serve (es. estensioni, script, ecc.)
    * (++) Risorse le definisci te (quanta RAM vuoi)
    * (++) durata compilazione illimitata
    * (++) puoi accedere direttamente
    * (++) puoi scegliere tra compilazione pulita o incrementale
    * (--) devi pulirlo se "si sporca"
    * (--) devi aggiornartelo a mano
    * (--) può costare
    * (--) difficile installarlo
  * cloud-hosted
    * (++) ogni volta una VM nuova di pallino, pulita
    * (++) aggiornato da altri
    * (++) free tier
    * (++) set up veloce
    * (--) puoi estenderla in pipeline, ma modifiche impermanenti
    * (--) pipeline usano DS2v2 (7GB ram e 2CPU)
    * (--) compilazione ha tempo limitato
    * (--) NON puoi accedere direttamente
* organizzati anche in **agent pool**, gruppi di agenti a livello di organizzazione. Permessi:
  * Reader - vedere agent pool
  * Service Account - aggiungere pool a un project pool
  * User - aggiungere pool a una pipeline
  * Admin - gestire i pool, eliminare agent, ecc
* i job da eseguire sono organizzati in **agent build queues**
* possibilità di sfruttare concorrenza con i **parallel jobs** (nn x agent cloud linux o mac)
* pricing sulla base dei parallel jobs (free tier mensile di base)
* come dimensionare i parallel jobs?
  * regola empirica: ogni 4-5 xsn che usano le pipelines
  * sulla base di: app multiple, branch multipli, team multipli

```yml
# cloud-hosted
pool:
  vmImage: 'ubuntu-20.04'
  demands:
  - npm

# private
pool:
  name: 'MyAgentPool'
  demands:
  - npm
# same as:
pool: 'MyAgentPool'
```

#### Self-hosted agent

Puoi installarlo:

* manualmente (ottimo se te ne serve uno solo)
* in automatico con Terraform o ARM template (utile per replicare e mantenere)
* manualmente e poi *snapshottare* l'immagine per poterla replicare (utile per replicare, ma meno mantenibile xk update di sicurezza implicano ri-snapshottare. Utile Packer di Hashicorp x aggiornare immagini)

Installazione manuale su macos, linux o windows funziona tramite **PAT** generato da DevOps e configurato. Va aggiunto un agent pool personalizzato e il proprio agent su devops. Poi va lanciata l'installazione dell'agente e infine configurato l'agente a usare PAT, URL dell'organizzazione, nome agente, nome pool. Infine va settato come demone o servizio in background. Il gioco è fatto.

#### Deployment pattern

* modalità di fare rollout/deploy minimizzando downtime, magari progressivamente per validare che tutto sia ok

* Blue-green deployment - due ambienti live, un load-balancer o switch. Si reindirizza il traffico a seconda della versione
* Canary releases - rilascio la feature a un piccolo gruppo di utenti, se vedo che è stabile, la rilascio a tutti
* Feature toggles/flags - rilascio feature in un if. a runtime posso accenderla e spegnerla
  * utile quando voglio poter tornare indietro facilmente
  * può causare problemi se si abusa. Molti condizionali, molti casi da testare, molte configuraz possibili
  * possono ess configurati per cliente es. feature flag a database
* Dark launches - come la canary ma gli utenti-canarini sono inconsapevoli di fare da beta tester
* A/B testing - rilascio feature "sperimentale" x metà utenti e valuto se tenerla o no (validazione esperimento)
* Progressive-exposure deployment - estensione della canary. Amplio la feature a cerchi concentrici di utenti

##### Blue Green con DEployment Slots

```bash
az webapp deployment slot create \
  --name $staging --resource-group tailspin-space-game-rg \
  --slot swap

az webapp deployment slot list \
    --name $staging --resource-group tailspin-space-game-rg \
    --query [].hostNames --output tsv
```

```yml
- task: AzureWebApp@1
  displayName: 'Azure App Service Deploy: website'
  inputs:
    azureSubscription: 'Resource Manager - Tailspin - Space Game'
    # New!
    deployToSlotOrASE: 'true'
    resourceGroupName: 'tailspin-space-game-rg'
    # New!
    slotName: 'swap'
    appName: '$(WebAppNameStaging)'
    package: '$(Pipeline.Workspace)/drop/$(buildConfiguration)/*.zip'
# New!
- task: AzureAppServiceManage@0
  displayName: 'Swap deployment slots'
  inputs:
    azureSubscription: 'Resource Manager - Tailspin - Space Game'
    resourceGroupName: 'tailspin-space-game-rg'
    webAppName: '$(WebAppNameStaging)'
    sourceSlot: 'swap'
    targetSlot: 'production'
    action: 'Swap Slots'
```

### Test Plans

> testing esplorativo e manuale

### Repos

> repo illimitati, privati e su cloud. Oppure git pubblici

#### Migrazioni

* Da TFS
  * per preservare pacchetti e date commit, occorre avere TFS aggiornato alla release RTW
* Da GIT
  * basta selezionare importa codice da Git

### Artifacts

> Creare, ospitare e condividere pacchetti (NPM, Maven, Nuget)

* Package feed = Repository server. Es. NuGet, NPM, Maven, Docker
* Artifacts ha come alternative
  * Pubbliche: Nuget.ORG, NPMJS, DockerHub, Quay, Maven Central Repo
  * Private: MyGet, ACR, File Share
* SemVer: `Major.Minor.Patch[-Suffix]`
* Version pinning: richiedere una versione in base a una query
  * NuGet
    * usa parentesi quadre e tonde x indicare inclusione/esclusione
    * quando c'è una virgola, mi aspetto un intervallo tra due versioni dove il primo numero è la vers minore installabile e il secondo la maggiore
  * NPM
    * lo stesso package può appartenere a molti package come dipendenza => conflitti e molta duplicazione => automerge
    * `^` usato per fare merge sulla base della minor release
    * `~` usato per fare merge sulla base della patch
* 4 livelli di permessi: Owners, Contributors, Collaborators, Readers
* Tool come whiteSource e BlackDuck per vedere Vulnerabilità su Artifacts con componenti 3part

`Install-Package Newtonsoft.Json -Version 13.0.1`

## Azure Container Registry

* importante fare check dei pacchetti prima di fare push di un immagine
* importante fare check dei pacchetti contiuno con uno scheduled task. Le vulnerabilità vengono scoperte continuamente
* Insomma, fare check sempre.
* **Azure Defender for container registries** -> automazione di scan delle immagini e vulnerabilità. Ocio che funziona solo x Linux images, non Windows. Windows non ha difetti

## Docker

Per ottimizzare un docker, unire comandi RUN x avere meno layer

## Analizzatori statici

### SonarQube

* consente di tracciare il debito tecnico e spottare bug
* integrabile in DevOps (Board e Pipelines)

### Java

* Checkstyle PMD o FindBugs
* integrabili in DevOps

## Testing

* manuale - Azure Testing Plans
  * più costosi
* automatico - Azure Pipelines
  * unit tests - test veloci (~30") sul codice. Sintassi, correttezza, verifica con I/O. Sia su codice puro che su infrastrutturale
  * smoke test - test su singola componente (~15'). Correttezza, verifica I/O.
  * integration test - test multi-componente (~ore). Spesso schedulati di notte. Verifica interoperabilità tra componenti.
  * utili come documentazione, utili x validare la correttezza di un refactoring

> piramide del testing: alla base ci sono i test di unità e sopra quelli di UI. Copro quasi tutto coprendo unit test.
> spostare i test a sx: più vicini allo sviluppo

* fare test sensati, mirati, non su tutto, ma solo su nostro codice. Test veloci
* test con mock e stub x testare in modo isolato
* lint test x validare scrittura codice

Java: Checkstyle e JUnit
C#: XUnit e NUnit
UI: Selenium

`dotnet test Tailspin.SpaceGame.Web.Tests --configuration Release --no-build --logger trx`
esegue i test loggando il risultato in un xml in formato trx.

### Acceptance testing

Risponde alla domanda: una determinata versione del software è rilasciabile?

* blue/green deployment - parte utenza reindirizzata a nuova versione. Se tutto va bene, indirizza tutti alla nuova versione e decommissiona la precedente (deployment slot di ASP)
* canary release - gruppo di utenti selezionato x avere una nuova funzionalità (spesso tramite *feature flag*)
* A/B testing - test x validare efficacia di una funzionalità. 50% utenti hanno la funz, 50% non ce l'hanno. Con delle metriche (es. tasso di conversione) valido esperimento. (User Behavior Analytic su Application Insights)

### Altri test (non funzionali)

* stress test - importante testare l'infrastruttura per carichi improvvisi. Occorre verificare che dopo uno spike, torni al normale funzionamento.
* fault injection - con la chaos engineering si prova a mettere in crisi il sistema. Si testa disaster recovery, ungraceful shutdown, rallentamenti di rete
* security test - verifica vulnerabilità, sw automatici e analisi statica. Oppure tecnica del *red team* che prova a bucare il nostro SW.

### UI Testing

* vanno eseguiti in ambiente simile a produz (right)
* possono ess eseguiti nell'agente o nell'infra di test
* test plan = quando fare i test, dove, con che strumenti e cosa dovrà accadere al OK/KO

Strumenti

* Windows Application Driver (WinAppDriver) - UI testing app windows in WinForms o UWP
* Selenium - UI testing per web. Multi ling JS C# .NET con NUnit
* SpecFlow - BDD integrabile con Selenium - Gherkin = parser di test in ling naturale
* Cucumber - BDD integrabile con Selenium

Selenium
si basa sui locators (selettori)
In macos si installa un driver in Edge: spctl --add Tailspin.SpaceGame.Web.UITests/bin/Release/net5.0/msedgedriver
Si avvia con `dotnet test --configuration Release Tailspin.SpaceGame.Web.UITests`

* id attribute
* name attribute
* XPath expression
* Link text or partial link text
* Tag name, such as body or h1
* CSS class name
* CSS selector

* IWebDriver - implementata da ChromeDriver, FirefoxDriver, EdgeDriver, SafariDriver
* IJavaScriptExecutor - implementata da ChromeDriver, FirefoxDriver, EdgeDriver, SafariDriver
* IJavaScriptExecutor implementa `ExecuteScript`

```csharp
private IWebElement FindElement(By locator, IWebElement parent = null, int timeoutSeconds = 10)
{
  // WebDriverWait enables us to wait for the specified condition to be true
  // within a given time period.
  return new WebDriverWait(driver, TimeSpan.FromSeconds(timeoutSeconds))
    .Until(c => {
      IWebElement element = null;
      // If a parent was provided, find its child element.
      if (parent != null)
      {
        element = parent.FindElement(locator);
      }
      // Otherwise, locate the element from the root of the DOM.
      else
      {
        element = driver.FindElement(locator);
      }
      // Return true after the element is displayed and is able to receive user input.
      return (element != null && element.Displayed && element.Enabled) ? element : null;
    });
}
```

### Apache JMeter

Test di carico vs stress test = test di carico simula condizioni realistiche x misurare performance, stress test misura condizioni straordinarie per vedere se il sistema rimane vivo e mediamente stabile (ovviamente è lento)

Apache JMeter lancia thread paralleli di richieste e genera un report in xml:

`apache-jmeter-5.4.1/bin/./jmeter -n -t LoadTest.jmx -o Results.xml`

Formati supportati da ADO: CTest, JUnit (including PHPUnit), NUnit 2, NUnit 3, Visual Studio Test (TRX), and xUnit 2

X JMeter si può trasformare JMtere in JUnit XML tramite una XSLT (trasforaz XML)

### Code Coverage

% di codice coperto da unit test.

Tool x coverage:

* Coverlet x C# (nuget coverlet.msbuild
* JaCoCo e Cobertura formati x Java (in DevOps)
* [Report Generator](https://github.com/danielpalme/ReportGenerator) converte i vari formati tra loro e in html. Azure Pipelines usa questo tool internamente

Vengono installati come tool nuget (local vs globali). Cosa sono? Tipo utility npm.

dotnet new tool-manifest # crea un file manifest dove installare tool locali
dotnet tool install dotnet-reportgenerator-globaltool

dotnet test --no-build \
  --configuration Release \
  /p:CollectCoverage=true \
  /p:CoverletOutputFormat=cobertura \
  /p:CoverletOutput=./TestResults/Coverage/
  
dotnet tool run reportgenerator \
  -reports:./Tailspin.SpaceGame.Web.Tests/TestResults/Coverage/coverage.cobertura.xml \
  -targetdir:./CodeCoverage \
  -reporttypes:HtmlInline_AzurePipelines

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

* Raccoglie dati da applicazioni cloud e on-prem, OS di VM, Azure (risorse, sottoscriz e tenant, AD), altre fonti.
* I dati raccolti sono log (evento testuale, data e metadati) o metriche (data e info numerica, salvate su TSDB)
* Li mette a disposizione x altri servizi Azure
* Possono essere estesi:
  * abilitazione diagnostica (es. opt-in su Azure SQL x avere più metriche)
  * aggiunta di un Agente di Log Analytics su una VM
  * invio dati custom invocando un API REST via SDK
* Log e metriche interrogabili via Kusto, anche per visualizzazione su Dashboard
* monitoraggio di risorse Azure by design
* monitoraggio di pod su Kube con *Azure Monitor container insights*
* monitoraggio di VM con agente, con *Azure Monitor VM insights* dashboard che riunisce tutte le VM in un'unica schermata

Oggi c'è un agente unico x Azure Monitor come prodotto unico log+metriche.

Un tempo c'erano vari agenti (oggi considerati legacy) a seconda delle funzionalità:

* WAD/LAD - agente di diagnostica, per le metriche
* Telegraf - agente basato su InfluxDb per Linux sempre per metriche. Serve a Linux x gestire autoscale o alert
* Dependency agent - Lin/Win agente per dipendenze e tracce applicative, richiede agente Log Analytics
* Log Analytics Agent - Lin/Win agente di raccolta log

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
* `where`, `count`, `extend`, `summarize`, `take 10`, `top 10 by Xyz`
* Tipi base come `datetime(2018-11-01)`
* | where success == "False"

### Log Analytics

* Permette di analizzare e fare query su dati raccolti da varie fonti
* Fonti sono Azure Monitor, App Insights, Az Security Center, API, Powershell
* È un hub centrale di raccolta e permette varie azioni tra cui:
  * invocazione di funzioni: Logic App, Alert, Script o WebHook API, oppure connettori custom come ITSMC (integrazione con sistemi di incident da IT come ServiceNow, Provance, Cherwell, System Center Service Manager)
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
* Altre figherie: Snapshot Debugger, Log Analytics, Search, Smart Detection
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
  * Continuous Export su storage
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

#### Azure Defender Plan

* serivizio cross service try and buy, pay-as-u-go
* ti mette in sicurezza VM, SQL, WebApp, DB, storage, AKS, ACR, KV, ARM, DNS
* ti da uno score di sicurezza sulla compliance. Dashboard
* fa assessment continuo
* accesso JIT alle VM

### Azure Sentinel

* raccoglie da varie fonti anche esterne, multi-cloud, on-prem (utile x unificare)
* analizza con IA le minacce distinguendo veri e falsi positivi. Piattaforma **investigativa** e di **rilevazione**
* consente di allertare con Logic App in caso di incidente (playbook)
* aiuta a classificare gli incidenti in: positivi, falsi positivi e i dubbi
* crea una mappa "investigation map" che collega gli elementi e aiuta con drill down
* raccoglie dati da Azure Monitor Logs (prima crea il workspace!)
* unico ambiente centralizzato dove correlare metriche di performance, sicurezza, e problemi

#### Dashboard

* query multi-risorsa (app insights x production e x test) - max 100 risorse
* maximum integration and flexibility
* RBAC
* autorefresh
* full screen
* (--) no max flexibility or customization, no drill down, no interactivity
* (--) max 30days dati

#### Azure Monitor Workbooks

* integration with monitor and logs
* interactivity, filtering and drill down
* import/export
* (--) no autorefresh, limiti query e richieste
* (--) no risorse multiple
* (--) no personalizzazioni per utente, ma solo a livello di workspace

#### App Insights Workbooks

* documenti interattivi e collaborativi x indagare aspetti di metriche e log (tipo post-mortem di un incidente)
* (--) no autorefresh
* (--) no dettaglio delle dashboard

#### PowerBI

* super interattivo, zoom, cross-filtri, drilldown
* molte fonti dati
* performance xk dati cachati in un cubo
* (--) non supporta log e dati da Azure RM
* (--) refresh dati max ogni 3h

#### Grafana

* Open Source tool
* ottima interattività, zoom, filtro
* grande ecosistema di fonti dati (tra cui Grafana Azure Monitor Plugin)
* (--) non supporta log e dati da Azure RM

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

Kudu è piattaforma di **Source Control Management (SCM)**.
Si accede a `https://<app name>.scm.azurewebsites.net` con credenziali di deployment.
Oppure si accede da Azure con *Development Tools* > *Advanced Tools*.
Da Kudu, poi si può scaricare lo zip dei file da Windows, e da Linux lo zip dei log di docker

Alternative x Blob Storage:

Si accede ai log su Blob e si aprono in excel. Come accedere? Da portal azure, da cli o da Azure Storage Explorer.

#### Logging su Application Insights

Aggiungi codice per raccogliere in un logger centrale. AI funge anche da bidone x le metriche.

## VS App Center

Prodotto che consente di compilare app per IoS, Android, ecc. Consente CI/CD, UI testing, raccolta metriche utilizzo app.
Il portale PaaS è ubicato a [appcenter.ms](https://appcenter.ms).
È vantaggioso perché permette di integrare e migliorare i processi aziendali, efficienza e qualità.

Microsoft Test & Feedback extension - estensione x raccogliere feedback e crash sull'applicaz

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

## Git

`git config --global user.name "Mona Lisa"`
`git config --global user.email "email@example.com"`
caching della pwd -> gh auth login o GCM (git credential manager)

`git remote -v` elenca i remote (repo) e i permessi (fetch e push)

origin - repo su github

upstream - nome convenzionale del fork di un origin

`git remote add upstream https://github.com/MicrosoftDocs/mslearn-tailspin-spacegame-web.git` aggiunge un upstream alla repo attuale

`git pull origin main` indica da che remote vogliamo fare pull di un branch
`git push origin code-workflow` indica su che remote vogliamo fare push di un branch

`git blame` -> in alcuni ambti diventa git praise x evitare dispregiativo

`git cherry-pick` -> applicare commit su + branch anche indipendenti

`git --no-pager log --oneline` -> vedere log inline

`git revert --no-edit HEAD` -> annullare ultimo commit committando l'opposto

### Rules for commit messages

* Don’t end your commit message with a period.
* Keep your commit messages to 50 characters or less. Add extra detail in the extended description window if necessary. This is located just below the subject line.
* Use active voice. For example, "add" instead of "added" and "merge" instead of "merged".
* Think of your commit as expressing intent to introduce a change.

## GitHub

GitHub (GH) è una suite per VCS e CI/CD. 50M utenti.

Concetti chiave: Issues, notifiche, branch, commit, PR, label, actions, pages (sito statico)
**pulse** è una sezione che riepiloga cos'è successo su una issue

Cloning vs forking: clonare significa scaricare in locale una repo remota e committare su questa. Forkare significa copiare nel proprio gh la repo, poi la si clona e si lavora liberamente su questa copia. Se poi si vuole ricongiungere il fork alla repo originale, si può fare PR.

**GH Codespaces** - IDE as a Service (VS CODE a 0.18 $/h)

**GitHub-Flavored Markdown (GFM)** - markdown esteso con figherie di Git come cross-ref link, a PR, issue, snippet di codice, commenti, ecc.

* [x] First task
* [x] Second task
* [ ] Third task

In buona sostanza, conviene usare **Git LFS** (Large File Storage) per file pesanti.

### File speciali su GH

* README.md - mostrato nella folder corrente
* .github/ISSUE_TEMPLATE.md - precompilazione in ISSUE
* .github/PULL_REQUEST_TEMPLATE.md - precompilazione in PR
* CONTRIBUTING.md - mostrato quando uno fa PR
* LICENSE.md - licenza del prog
* FUNDING.yml - mostra una lista di sponsor
* SECURITY.md - pratiche di sicurezza
* CODEOWNERS - utenti aggiunti automaticamente in PR
* CITATION.cff - mostra come citare un software

* Utenti organizzati in Team gerarchici a matrioska x riflettere struttura organizzazione (nested teams)
* RBAC.
* Possibilità di sync da profili AD dove i team sono gruppi
* organization = meglio averne solo una, sempre

### Permessi su GH

* Repository permissions
  * Read - RO
  * Triage - catalogatori di issue, cose così, ma non modifica codice
  * Write - dev
  * Maintain - PM - permessi alti ma non distruttivi. Si sa cosa possono fare i PM
  * Admin - tutto
* Team permissions
  * Member - normali ruoli, eredita dai team superiori
  * Mantainer - gestisce team
* Organization permissions
  * Owner - tutto, gestire membri
  * Member - gestire repo e team
  * Billing manager - gestire informazioni billing
* Enterprise permissions
  * stesse delle organization, ma l'owner può gestire organizations
  * può decidere delle default policy per le organization
    * per grandi organiz: read (di default solo lettura sulle repo)
    * per piccole organiz: write (read/pull/push)

### Migrazioni verso GH

* Si vuole solo il codice o anche lo storico dei commit, le issue ecc?
* Da SubVersion, Mercurial, TFVC: **GH Migrator**
* Ocio ai file binari di grandi dimensioni => Git LFS
* Aggiungi in gitignore file da escludere (file che cambiano sempre, file pesanti non importanti, file sensibili, file di config, override personali)

### Ruoli GH

* owner - gestisce organization (sicurezza, utenze, billing, script custom, grant dei permessi, auth, migrazioni)
* admin - gestisce team
* team mantainer - gestisce team

### Autenticazioni

* Username/Password
* PAT (utile x software e cli)
* Chiavi SSH (generazione chiavi ank protette, compliant con SAML SSO)
* Deploy Key (chiavi SSH, ma solo x una repo)

Opzioni Auth:

* MFA/2FA
* SAML SSO (vari IdP AD FS, AAD, Okta, OneLogin, PingOne...)
* autenticazione di directory su FS con LDAP (integrato con AD, Oracle, OpenLDAP, OpenDirectory...)

### Sicurezza in GH

* nel file SECURITY.md i white hat (hacker etici) fanno disclosure delle vulnerabilità
* dependabot che fa scan continuo dipendenze e crea PR se trova pacchetti vulnerabili
* secret analysis, ti avverte se committi credenziali o segreti
* tool di analisi statica continua
* codeQL x estendere analisi statica con query personali sul codice

### GH Search

* ricerca globale
* ricerca contestuale (in this repo)
* Simpatico sistema di Label colorate ed estensibili oltre a [quelle di default](https://docs.github.com/en/issues/using-labels-and-milestones-to-track-work/managing-labels#about-default-labels)
* tag di ricerca
  * is:repo is:issue is:pr is:open is:closed
  * stars:>1000 created:<=2016-04-29 topics:5..10
  * `-` o NOT per escludere
  * language:javascript author:nat assignee:@me org:github
  * in:comments linked:pr
  * sort:author-date

### Sintassi globale x auto link

* menzioni si fanno con `/cc @user` x aggiungere utente in carbon copy
* Duplicate of `#8` fa il link alla issue
  * commit che inizia con "close", "closes", "fixed", "fix"... `#issue_id` va a chiudere issue in automatico
* GH-26 -> link alla PR 26
* jlord/sheetsee.js#26 github/linguist#4039 una specie di namespacing x repo
* a5c3785ed8d6a35868bc169f07e40e889087fd2e sha del commit -> prime 7 cifre a5c3785
* ulteriori autolink configurabili

### Permessi repo

Visibilità:

* private - solo io o solo chi decido io (es. team web)
* internal - progetto innersource, visibile a un'organizzazione
* public - visibile a tutti

Permessi (singolo o team):

* Read - RO
* Triage - catalogatori di issue, cose così, ma non modifica codice
* Write - dev
* Maintain - PM - permessi alti ma non distruttivi. Si sa cosa possono fare i PM
* Admin - tutto

### Cose da misurare secondo GH

* Measure process, not output
  * Code review turnaround time
  * Pull request size
  * Work in progress
  * Time to open
* Measure against targets and not absolutes
* Measure teams and not individuals
  * Number of unique contributors to a project
  * Number of projects reusing code
  * Number of cross-team @mentions

### GH Actions

Le GH **Actions** sono soluz di CI/CD come le Pipeline di DevOps. Integrano IaC e facilitano lo sviluppo. Si basano su un perverso sistema di credito a minuti mensili. Sono file Yaml eseguibili: Soliti sistemi con trigger e seq di azioni (Node.JS o Python). Parole chiave: on, job, step, runsOn, uses, matrix, artifact.

> Evento => Workflow => Job => Step => Action

* Evento - qualsiasi evento scatenante o eventi multipli o eventi condizionali (if x = y):
  * una PR, o altri eventi di GH
  * un evento esterno
  * una schedulazione (POSIX CRON `*/15 * * * *`)
  * workflow_dispatch = trigger manuale
  * repository_dispatch = trigger API REST
* Workflow - è la pipeline. Può essere di build, test, package, release, tutto insieme...
  * può ess disabilitato o il run cancellato
  * può ess templatizzato a livello di organization x riutilizzo
* Job - unità di esecuzione su una macchina self-hosted o GH-hosted
* Step - task individuale eseguito in un job costituito da almeno una action
* Action - singola istruzione o funzione.
  * può ess di 2 tipi:
    * container action (+ linguaggi, sono solo su linux)
    * JS action (solo JavaScript, ma multi-piattaforma)
  * può venire da:
    * action di base
    * action dal marketplace
    * scrittura action custom

* matrix = matrice di compilazione: in una stessa pipeline posso fare azioni su + OS in + versioni. Viene fatto il prod cartesiano dei due insiemi. Posso usare var custom come `${{ matrix.node-version }}`. `os: [ubuntu-latest, windows-2016] node-version: [8.x, 10.x, 12.x]` => 6 run
* artifact = docker o pacchetto semi-lavorato, che può ess download/upload-ato in varie fasi di actions
* integrazione con i flussi di lavoro - si può aggiungere uno step tipo: "PR reviewed", "aggiunta etichetta alla issue".
  * in questi casi, va sempre settata la env `GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}`
* variabili introdotte da `$`. Utili x non cablare
* Negli step/job si possono usare clausole `if: condizione`
* `run: cmd` esegue il comando `cmd`
* `with: ...` setta dei parametri per esempio in una action
* debug attivabile:
  * ACTIONS_RUNNER_DEBUG = true -> debug di RUN
  * ACTIONS_STEP_DEBUG = true -> debug di STEP
* log visualizzabili
  * da GH interfaccia web
  * da API con permessi su repo: `GET /repos/{owner}/{repo}/actions/runs/{run_id}/logs`

> **Ocio**! I minuti delle actions sono un credito.
>
> * Usando 1 minuto di Actions su Linux consumo 1 minuto di credito.
> * Usando 1 minuto di Actions su Windows consumo 2 minuti di credito.
> * Usando 1 minuto di Actions su Mac consumo 10 minuti di credito.

Il sistema di archiviazione ha una fatturazione a sé sempre su base mensile.

```yml
name: Node.js CI

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:

    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [10.x, 12.x]

    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}
    - run: npm ci
    - run: npm run build --if-present
    - run: npm test
    - name: Run build script
      run: ./.github/scripts/build.sh
      shell: bash
    - name: Cache NPM dependencies
      uses: actions/cache@v2
      with:
        path: ~/.npm
        key: ${{ runner.os }}-npm-cache-${{ hashFiles('**/package-lock.json') }}
        restore-keys: |
          ${{ runner.os }}-npm-cache-
          
```

```yml
name: Share data between jobs
on: push
jobs:
  job_1:
    name: Upload File
    runs-on: ubuntu-latest
    steps:
      - run: echo "Hello World" > file.txt
      - uses: actions/upload-artifact@v2
        with:
          name: file
          path: file.txt

  job_2:
    name: Download File
    runs-on: ubuntu-latest
    needs: job_1
    steps:
      - uses: actions/download-artifact@v2
        with:
          name: file
      - run: cat file.txt
```

```yml
# Custom Action Definition
name: "Hello Actions"
description: "Greet someone"
author: "octocat@github.com"

inputs:
    MY_NAME:
    description: "Who to greet"
    required: true
    default: "World"

runs:
    uses: "docker"
    image: "Dockerfile"

branding:
    icon: "mic"
    color: "purple"
```

### App GH

GH espone API solide su cui sono sviluppate molte app

* autorizzate con OAuth (chiedono la tua autorizzaz e ti impersonano)
* app gh (sono utenti a sé stanti che svolgono servizi - bot)
  * usano meccanismi di webhook x agire a certe azioni es. new PR
  * possono essere azionate in polling ma è + sconveniente
  * con firewall aziendale si possono usare servizi di tunneling come [Smee](https://smee.io/)
  * Probot - framework che semplifica gestione di webhook. Su questo sono state sviluppate
    * Pull - tiene un fork aggiornato
    * Delete Merged Branch - elimina branch mergiati
    * Release Drafter - genera change log sulle PR

### GH Script

In una GH Action, posso inserire un GH script che interagisce con GH API facendo azioni su issue, PR, repo ecc usando [octokit/rest.js](https://octokit.github.io/rest.js/v18).

* Questo script è già preconfigurato e autenticato. Basta scrivere cosa si vuole fare in JS.
* Basta scrivere `github.issues.createComment({...})`.
* Ci sono altre variabili come context, io e core. Aggiungono funzionalità, es http, sessione utente, I/O e altro.
* Esempio di Action GH con script

```yml
name: Learning GitHub Script

# Questo è il trigger:
on:
  issues:
    types: [opened]
jobs:
  # Nome job: comment
  comment:
    runs-on: ubuntu-latest
    steps:
      # Questo step commenta una issue
      - uses: actions/github-script@0.8.0
        with:
          github-token: ${{secrets.GITHUB_TOKEN}}
          script: |
            github.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: "🎉 You've created this issue comment using GitHub Script!!!"
            })
      # Questo serve a eseguire uno script su un altra directory
      - uses: actions/github-script@v2
        with:
          script: |
            const path = require('path')
            const scriptPath = path.resolve('./path/to/script.js')
            console.log(require(scriptPath)({context}))
```

### GH Boards

* un project in GH ha delle board e segue delle iterazioni (sprint) a cui sono associate delle release.
* Board segue Kanban: todo inprog done
* Milestone tracking x individuare raggiungimento obbiettivi di progetto

### Branch policy

* quando servono i long-lived branch? Quando va gestita retrocompatibilità, es. occorre tenere la release 1.0 x confronto. Allora, settala RO.
* se serve apportare un fix su + branch scollegati da tempo, non si deve mergiare, ma fare cherry-pick.

### Release in GH

* quando si rilascia conviene scrivere change log (automatizzabili con GH app), semver in un comodo form di GH
* in questo form si possono trascinare asset tipo gli eseguibili, zip del sorgente, ecc.
* verranno notificate le persone iscritte alla repo

### Licenze GH

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

### GH Packages

* repo privata che supporta: Maven/Gradle, NPM, NuGet, RubyGems
* totalmente integrato con GH Actions e codespaces
* comodo x progetti multi tech xk basta dare permessi a una repo e si possono scaricare Packages da + registry
* integra **GH Container Registry**, un Docker registry con RBAC si trova in [ghcr.io](https://ghcr.io)
* auth con username + PAT + endpoint di packages
* PAT può essere preso da interfaccia utente
* Per installare un pacchetto o collegarsi a Packages, basta usare i tool di NPM/Maven/NuGet con endpoint packages

### GH CLI

* `gh auth login`
* `gh repo clone [url]`
* `gh alias set / gh api` - estendere la cli con cmd custom
* `gh issue close`
* `gh pr comment`
* `gh run list` - per vedere workflow delle GH actions

> [Altri riferimenti](https://cli.github.com/manual/)

### GH Secrets

* un posto dove gestire i segreti manualmente
* NON devono essere scritti direttamente in GH Actions
* `creds: ${{ secrets.AZURE_CREDENTIALS }}`

### GH Deploy & Release

* verifica condizionale di label nelle PR per verificare se fare o no deploy
* In fase di CD si fa spesso IaC, si creano/distruggono risorse cloud
* artifacts hanno retention di 90 giorni ma è configurabile
* si possono cancellare artifacts, ma non reversibilmente
* nelle repo pubbliche spesso si impostano "required reviewers" e "wait timer"

Li farò in futuro:

* [GHA](https://lab.github.com/githubtraining/getting-started-with-github-apps)
* [SSE](https://lab.github.com/githubtraining/security-strategy-essentials)
* [GHS](https://lab.github.com/githubtraining/github-actions:-using-github-script)
* [RBW](https://lab.github.com/githubtraining/create-a-release-based-workflow)
* [GHCI](https://lab.github.com/githubtraining/github-actions:-continuous-integration)
* [GHCD](https://lab.github.com/githubtraining/github-actions:-continuous-delivery-with-azure)
* [GHCR](https://lab.github.com/githubtraining/github-actions:-publish-to-github-packages)
* [ISF](https://lab.github.com/githubtraining/innersource-fundamentals)

## Jenkins

* soluzione x CI/CD
* può ess installato in VM. Serve la porta 8080 aperta

## Bamboo

* soluzione x CI/CD

## AAD

Azure Active Directory - soluzione cloud-based per gestire identità interne ed esterne a un'ente/azienda

Consente di:

* accedere a risorse interne (es. app aziendali) ed esterne (es. MS 365, azure)
* tenere sicure identità e informazioni
* garantire accesso condizionale
* fornire SSO aziendale (MFA attivabile)

Gerarchia:

* Tenant = organizzazione. Ha un dominio xyz.onmicrosoft.com che può ess personalizzato in xyz.com
* Gruppo = livello di accesso condiviso comune
* Identità = Utente/App/Servizio
  * AAD role o RBAC
    * admin - gestione licenze, utenti
      * può usare `New-AzureADUser` o `az ad user create`
      * può usare `Remove-AzureADUser` o `az ad user delete`
      * può usare `New-AzureADMSInvitation` utile per essere automatizzato con bulk di mail
        * delete logica x 30 gg poi fisica
  * associaz app - user in 3 modi
    * direct associat
    * group associat
    * rule associat (es. sulla base di una regola: tutti quelli che lavorano in sede a udine)
Il tenant ha uno score di sicurezza da 1 a 233 (come mai 233?!)

### Licenze AAD

* AAD Free - gestione accessi, forget pwd, SSO
* pay as you go - funzionalità specifiche come AAD B2C
* Office365 app - free + landing page personalizzate (login/logout)
* AAD Premium P1 - gruppi dinamici, forget pwd self-service, MS Identity Manager
* AAD Premium P2 - cm sopra + AD Identity Protection + Privileged Identity Management (PIM) = liv aggiuntivo di sicurezza sugli admin

Categorie Utenti:

* membri - possono tutto in base ai permessi nel loro tenant
* guest - possono visualizzare e gestire solo loro risorse, x utenti esterni e garantire la possibilità di lavorare ank senza accesso al tenant

### Servizi AAD

* AAD B2B - utente guest riceve invito in tenant. Accede al tenant x una certa app cloud o locale (meglio attivare MFA) possiamo collaborare cn utenti esterni. Meglio rispetto alla federation (più complessa e persistente partnership. Qui non serve che ci sia un reparto IT che gestisca gli account altrui)
* AAD B2C - utente qualsiasi può registrarsi a un app e accedere. C'è un form di registraz, MFA. Monitoraggio aggiuntivo dei DDOS (può essere pay as you go)
* AD DS - Domain Services - x estendere un dominio a VM senza domain controller
* Gestione delle App - SSO su molte app, marketplace + app aziendali + app esterne. Gestione criteri di accesso (es. MFA) + controllo accessi. App locali in AAD con AAD Proxy
* AAD Identity Protection (Premium P2) - gestione criteri e rischi su identità. Possono esserci correzioni di rischi es. utente reimposta pwd

### Migrazione a AAD

1. Creare basi sicure
   * max 2 global admin
   * assegnare degli admin
   * PIM x monitorare admin (se P2)
   * configurare reimpost password
   * configurare password blacklist (password01, admin)
   * configurare non riutilizzo pwd
   * configurare pwd cloud senza richiesta di modifica x evitare incrementalità tipo patatina01 patatina02...
   * MFA condizionale (P1)
   * configurare AADIP (P2)
2. utenti, divice e sync
   * installa AAD connect (sync tra AD locale e AAD)
   * installa AAD connect health - statistiche integrità e sync (P1)
   * password hash sync
   * attiva writeback password - se scrivi su Azure, si scrive anche in locale (P1)
   * configuraz gruppi e utenti
   * pianificare gestione dispositivi (BYOD? cell aziendali o personali?)
   * accesso con login social con AD B2B
   * configurare metodi passwordless (authenticator) (P1)
3. app
   * identifica app usate
   * imposta app SaaS da AD (veloce)
   * imposta AAD proxy per app locali
4. monitorare accessi, admin e cicli di vita
   * PIM x monitorare admin (se P2)
   * configura gruppi dinamici (P1)
   * associa app a gruppi
   * configura \[de\]provisioning automatico degli utenti

## AD

AD invece si basa su Kerberos e NTLM. È solo per reti locali, stampanti, NAS...
Invce del Tenant, ci sono: Foreste, domini, unità organizzative

### identità ibrida

collega AD e AAD. Come sincronizzare le utenze ibride? Cioè, come dare SSO sia su cloud che risorse aziendali?

1. sincronizzazione dell'HASH della password tra AD e AAD
   * (-) hash salvati su cloud
2. Auth pass-through - mi collego a AAD, questo mi fa da proxy verso AD
3. Federated Auth - Active Directory Federation Services ADFS server
   * (+) MFA
   * (+) Smart Card

Ocio che x GDPR tutti i dati stiano in europa.
Alcuni dati di AAD B2B e B2C di confiugrazione o mail inviate agli utenti stanno in USA.
MFA si basa su provider di SMS e chiamate USA

## Costi

total cost of ownership = vero costo di qualcosa (sommerso) es. elettricità + hardware + personale + licenze sw + datacenter...

[TCO calculator](https://azure.microsoft.com/pricing/tco/calculator) fornisce stima TCO su soluz Azure e home-made. Proiezione su N anni. Strumento da CFO che fa capire che Azure è figo.

Sottoscrizioni Azure

* free
* pay as you go - collegata Carta Credito o Debito
* member offers

### Contratti con Azure

* Enterprise Agreement - azienda si impegna a spendere TOT in 3 anni. Microsoft fa scontoni e fa anche il caffè
* Dal Web - come i comuni mortali
* CSP - appoggiandosi a un intermediario

Uno dei fattori sottovalutati del costo è la zona Azure. Una zona costa meno ma magari è distante. Più zone possono causare rallentamenti e spesa x banda.

Cos'altro possiamo fare?

* usare Azure Advisor
* mettere limiti di spesa, alert sui budget
* usare Azure Reservations x sconti su VM
* usare Azure Cost Management + Billing
* usare zone sfigate su Azure
* usare tag x analizzare centri di costo / utilizzatori
* fare pulizia di inutilizzato, ridimensionare, spegnere
* go PaaS (spendi quanto usi)
* gestione attenta alle licenze (Azure Hybrid)

## App Configuration

Servizio di gestione centralizzato che consente gestione segreti, feature flag, versionamento config ecc.

## AZSK

Azure DevOps Security Kit - tool non ufficiale che verrà rimpiazzato da Azure Tenant Security scanner (AzTS).

Utile per garantire la **Continuous Assurance (CA)**: Runbook lanciato periodicamente per valutare eventuali drift di configurazioni DevOps.

## AKS

Sonde:

* **livenessProbe** - ping periodico per vedere se pod è vivo. Se non è configurato, do x scontato sia vivo. Se non risponde, mando un Restart
* **readinessProbe** - viene fatto x capire se il pod può accettare richieste. Se non può setto Failure, altrimenti Success. Se Failure, escludo il pod dagli IP del service
* **startupProbe** - viene chiesto se l'applicativo nel pod si è avviato. Se c'è questa sonda, le altre sono disabilitate
* nel caso di servizi di rete, API, applicativi che rispondono a porte WEB, di solito si configura la readiness. Perché se sta servendo altre richieste, viene escluso dal pool applicativo. Es. protezione un po' rudimentale da DDOS.

* per collegarlo ad ACI usare uno yaml apposito e tiller con HELM
* per collegarlo in AAD, occorre creare cluster, poi una System assigned managed-identity e poi un RBAC binding
* **Azure Defender for Kubernetes** - monitor dei log del cluster e notifica in caso di vulnerabilità

## DSC

PowerShell DSC tool **dichiarativo** per powershell per la gestione delle installazioni nelle VM

* Linguaggio dichiarativo serve a gestire deploy **idempotenti** (puoi eseguirli quante volte vuoi e non fare danni)
* Linguaggio dichiarativo serve a gestire i fallimenti con politiche di retry
* Script DSC viene compilato in un file .mof
* Si basa sul fatto che in Win c'è un LCM (Local Config Manager) in WMF (windows management framework) che verifica desiderata e attuale e attua modifiche differenziali
* Funziona in push o in pull ogni 15min (meglio soprattutto per ambienti con ridondanza e scalabilità)
* Tutti i SO Windows, Linux ma non debian o ubuntu 18.04

* per usare segreti usare PSCredential e non scriverli su script
* si basa su 3 data block parametrici innestati:
  * Configuration nome_config
  * Node nodi a cui mandare (uno, molti, tutti *)
  * Feature/Config/File da installare
* poi si lancia il comando di compilazione in file mof

Vari requirement di versioni su Win (dipende da WinRM e perciò no win server 2008 o win 7)
+

* Port: Only TCP 443 is required for outbound internet access.
* Global URL: *.azure-automation.net
* Global URL of US Gov Virginia: *.azure-automation.us
* Agent service: [https://.agentsvc.azure-automation.net](https://.agentsvc.azure-automation.net)

* In PS: @(elem1, elem2) = array
* In PS: @{ key1=value1 key2=value2 } = obj

```ps1
Configuration Create_Share
{
   Import-DscResource -Module xSmbShare
   # A node describes the VM to be configured

   Node $NodeName
   {
      # A node definition contains one or more resource blocks
      # A resource block describes the resource to be configured on the node
      xSmbShare MySMBShare
      {
          Ensure      = "Present"
          Name        = "MyFileShare"
          Path        = "C:\Shared"
          ReadAccess  = "User1"
          FullAccess  = "User2"
          Description = "This is an updated description for this share"
      }
   }
}

Create_Share -OutputPath C:\temp\
```

### Comandi DSC

* Installazione `Install-Module 'PSDscResources' -Verbose`
* Importare modulo `Import-DscResource -ModuleName 'PSDscResources'`
* Visualizzare tutte le feature personalizzabili `Get-DscResource | select Name,Module,Properties`
* Esegue i file mof nella cartella D:\ in push mode: `Start-DscConfiguration -path D:\`
* ottiene info da un nodo `Get-DscConfiguration e Get-DscConfigurationStatus`

## Azure Automation State Configuration

* Compila script DSC, li scrive, importa, li gestisce, li assegna a un nodo
* si basa su DSC, centralizza artifacts DSC
* pull server centrale da cui le VM prendono le config
* integrazione con Azure Monitor x vedere la compliance dei nodi

## Azure Governance

* Lock delle risorse critiche in DontDelete e ReadOnly (No eliminaz e modifica)
* Cost management e gestione subscription/mgmt group, spesso con i tag
* Policy, assignment x gestire regole di subscription e creare standard di conformità. Le policy NON sono retroattive. Le iniziative lo sono.
* Blueprint x avviare iniziative cross-subscription (alcune già pronte tipo ISO 27001)
* [Azure Cloud Adoption Framework](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework) è una guida metodologica per migrare dolcemente al cloud Azure
* RBAC utile per gestire granularità permessi

## Incident reporting and reviewing

* no-blame culture, non cercare colpevoli
* in un sistema complesso ci sono interazioni => errori imprevedibili, a volte catastrofi a volte near-miss
* importante fare retrospective-postmortem-postincident x opportunità di apprendere
* va fatta entro 24-36h xk l'essere umano dimentica
* concentrarsi su cosa sapevamo e cosa sappiamo ora, su cosa va storto nei processi
* prima cosa da fare, raccogliere i dati, poi conversare e sentire le varie campane

Errori frequenti

* incolpare le persone (umani sono parte del sistema, spesso l'errrore umano è un sintomo di un problema) Vedi incidenti dei B17
* ragionamento controfattuale (non aiuta ragionare su come sarebbe andata se solo...)
* linguaggio normativo - le persone si sono comportate male inappropriatamente, le persone si sono comportate bene, hanno fatto il possibile - sono giudizi, non fanno bene alla discussione e fanno perdere di vista i fatti
* raggionamento meccanicistico - il sistema avrebbe funzionato, stava funzionando se nessuno interveniva. No, un sistema ha bisogno di interventi

Concretamente

* utile querare su Microsoft Graph API per avere messaggi delle chat e dei canali di teams x ricostruire cronologia
* utile DevOps board e wiki x raccogliere le info
* utili le dashboard di Azure, possono ess esportate
* utile fare query in Kusto su Log Analytics

```kql
AzureActivity
| where CategoryValue == 'Administrative'
| where OperationNameValue endswith "write" or OperationNameValue endswith "delete"
| project TimeGenerated, Level, ResourceGroup, ResourceId, OperationName, OperationNameValue, ActivityStatus, Caller
| order by TimeGenerated nulls first
```

Pratiche Comuni

* avere un facilitatore neutrale che espone i fatti
* meeting max 60-90min
* pre-interviste utili x sondare se ci sono conflitti e avere già dati preventivi
* non farle per incidenti piccoli
* non chiedere perché ma come e cosa. Capiamo cosa è successo, come funzionano le cose ([Etsy debriefing guide](https://extfiles.etsy.com/DebriefingFacilitationGuide.pdf))
* concentrarsi anche su cos è andato bene, su come è stato rimediato

## Identity

* Service Principal
  * app registration su AAD dove creo un'identità a un applicazione, come un identity proxy
  * posso associare permessi con IAM (RBAC) per interagire con altri servizi
  * serve sapere tenant_id client_id e autenticazione
  * serve solo con app on-premise o servizi che non hanno ancora Managed Identity in Azure
* Managed Identity
  * prima si chiamava MSI
  * serve a dare un'identità ai servizi Azure
  * è free e inclusa in AAD
  * sgrava l'utente dal fardello di autenticare, ruotare credenziali ecc.
  * Azure Instance Metadata Service - REST API che usa MI per gestire identità
  * intergabili con KV
  * possono essere user-assigned o system-assigned
  * `az vm identity assign`, `show` x associare una vm a una system-assigned MI o vedere
  * `az identity create`, `delete` o `list` x gestire le user-assigned MI
