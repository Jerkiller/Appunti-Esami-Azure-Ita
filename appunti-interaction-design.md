# Interaction design

## Table of contents
- [1. Cos'è l'interaction design](#1-cosè-linteraction-design)
- [2. Processo di int des](#2-processo-di-int-des)
- [3. Concettualizzare l'interazione](#3-concettualizzare-linterazione)
- [4. Cognizione](#4-cognizione)


## 1. Cos'è l'interaction design

Interaction design con ogni cosa / HCI solo con pc e dispositivi. Ma ora il confine si sta assottigliando
Multidisciplinare, team da 15 persone, psicologi, informatici, ecc a volte costoso

obbiettivo dell'Int. des.: aiutare le persone a comunicare e interagire
Comprendere utenti-non esiste una taglia unica es. Vecchi col brondi nn x forza
Accessibilita e inclusivita

Deficit
- sensoriali cognitivi fisici
- permanenti temporanei situazionali

Usabilita in relaz con ux. Usabile piacevole efficace utile
> 6 dim Usabilità:
> Efficace efficiente sicuro utile apprendibile memorabile
- Sicurezza: di tutti gli errori possibili, quali possono accadere? Es. Confirm dialog
- Apprend quanto ci metto a imparare a usarlo? Quanto sono disposto a metterci?
- Memorab dopo mesi che non lo uso, mi ricordo? Es. Usare icone

**Flusso** -> stato in cui non ti accorgi che il tempo vola, tipo social
**Microinterazioni** - aiutano a migliorare ux es. Suono del cestino, manopola con rotazione perfetta. Non ci si stanca mai

**Dark pattern** - usati x persuadere o ingannare utente x fargli compiere un acquisto o un altro fine. Es. Se non deselezioni ti installo mcafee. Oppure disiscrizione da mail impossibile.
Meglio piccole spinte piacevoli e delicate. Es. Nel carrello ti propongo una vetrina di ymal. Come corsia casse al supermercato

Alcuni **principi euristici** globalmente sensati (ma localmente discutibili).
- **Visibilita** - funzionalita esplicite non come i rubinetti autogrill
- **Feedback all'utente** - no turbo lag
- **Vincoli** - poka yoke, disabilito opzioni non valide, forzo utente a non fare errori
- **Findability** e navigabilita
- **Coerenza** - facile in sistemi piccoli, ma su grande... a volte conviene romperla
- **Affordance** - dare indizi su come si usi un oggetto es maniglia o mouse che si fa cliccare
- **Semplicita** - nielsen dice di elimnare ogni elemento della gui. Se funziona lo stesso, rimuovilo.

I principi a volte si contraddicono, va trovato compromesso. Es. Rompere coerenza a volte aiuta. Oppure semplicita si ma anche estetica


Ricerca e design sono attivita caotiche, prevedono spreco, vicoli ciechi, false ipotesi prima di capire un problema e rispolverarlo

---

## 2. Processo di int des

Doppio rombo (pensiero divergente e convergente 2 volte)
`Discover <> define - Develop <> deliver`

Va esplorato **spazio dei problemi**
Va fatto con l'utente. Il P.O. a volte se ne dimentica, il marketing crea aspettative troppo alte. Si combattono coinvolgendo gli utenti dell'inizio. Si guadagna ownership, l'utente sente di aver contribuito.
A volte va cercato equilibrio xk troppi coinvolg utenti causa rework o altri eff collaterali
**Codesign** o design collaborativo o partecipativo
Altri modi di coinvolgere è chiedere feedback afrermarket es recensioni o E.R.S. (error reporting system) come quelli di windows

Focalizzarsi su utenti e non sulla tecnologia.
3 pilastri
- Focalizzarsi su utenti, sui loro obiettivi, sui comportamenti, contesto, caratteristiche, decidere con utenti in testa.
- Misurare e definire obiettivi
- Iterare. Mai al primo colpo. Il design va di trial and error

Ciclo di vita di int des:
- Scoperta requisiti
- Progettare alternative
- Prototipazione
- Validazione

Altri approcci:
- Google design sprint - settimane super full in cui ci si da dentro x risolvere un problema, si esplorano e protitipano soluzioni. Ogni giorno ha un obb specifico
- Research in the wild ritw

Chi sono gli utenti?
Domanda non banale.
Es. Cellulari ci sono utenti come i giovani genitori o i controllaschermo. E quali sono gli stakeholder? Utile analisi e molto ampio cerchio 

Quali bisogni e requisiti?
Nessuno lo sa, a volte abbiamo bisogno di cose che nemmeno sognamo. Quindi si esplora spazio dei problemi. I designer e i dev a volte si rispecchiano in quello che fanno, ma non è x forza quello di cui ha bisogno un utente.

Da dove viene creativita?
Non è misticismo. È spesso rielaboraz di design ed esperienze passate. È fecondazione incrociata tra vari campi. Non ci si deve limitare all'idea che funzionicchia.

Come si scelgono alternative? 
Facendo provare prototipi e avere feedback, validando fattibilita costo, fcendo a/b test o test multivariati
**Usability engineering**: stabilisci, misura e criteri accettabilita sw scientificam. Due soluzioni possono essere differentem usabili.
Int design si integra bene con agile, XP o altre robe come kanban: team di lavoro, iterazioni, feedback stakeholder...

## 3. Concettualizzare l'interazione

Come spiegare un prodotto? Con un PoC - forza il designer a formulare (come si interagirà, se è fattibile, come l'utente apprenderà)
* Il PoC ha molte incognite e le accetta
* Utile raccogliere idee varie, segnare da dove vengono, ricerche che supportino o diano forma alle idee
* Utile a presentare anche esternamente (es. a finance o marketing)
* Costo di sviluppo inferiore

Quali le assunzioni/presupposti e supposizioni?
- Presupposto - si da x scontato, ma rich indagini ulteriori
- Supposizione - diamo per vero, ma abbiamo dubbi
Scriverle, discuterle, testarle. (es. TV 3D - presupposti mancati doppio schermo, utenti non disposti a usare occhiali)

Discussione sulle idee
- Ci sono problemi su prodotti esistenti?
- Perché? Quali le evidenze?
- Come un design può superarli?

Assunzioni e presupposti definiscono uno spazio di design.
La soluzione proposta è un modello concettuale che poi può diventare PoC.
Discutere insieme forma un **terreno comune**, un linguaggio. Stimola l'apertura mentale. Orienta il team all'utente e alla soluz di problemi.

**Design concept** - un insieme di idee, immagini, documenti per un design. Es. un manifestino che illustra il design delle luci nel tappeto x guidare alle scale invece dell'ascensore

Modello concettuale
- un modello è la rappresentaz. semplificata di qualcosa (processo/sistema) - serve a descrivere
- modello concettuale di design: descriz di come un sistema è organizzato e agisce
- Cosa l'utente può fare? Cosa vede? ecc.
- un modello concettuale è una mappa di entità e relaz.: metafore e analogie, concetti di dominio e no, relazioni tra concetti (es. contenuto in), relaz concetto-UX
I team discutono i modelli concettuali. Quelli più semplici e ovvi funzionano meglio.
- un e-commerce ha il modello concettuale basato su customer exp in centro commerciale (metafore carrello cassa, concetto di prodotto acquisto pagamento ecc)
- si riutilizzano spesso soliti mod. concettuali (pattern come form di inserimento, navigazione, ecc), a volte nuovi modelli subentrano (WWW, foglio di calcolo, desktop digitale)
- Xerox star: nasce l'esper. desktop pensata x chi odiava l'informatica, pesante utilizzo di metafore da ufficio cartaceo. Cartelle, cestino, documento, taglia incolla. Nuovi concetti come la stampante introdotti. Azione di Drag n drop riprendeva lo spostamento fisico di oggetti.

Metafora/analogia
- usata da sempre nella didattica (es. metafora del gioco x evoluz)
- spiega in modo semplice/familiare qualcosa di difficile e concettuale
- spesso si basa sull'intuizione e non serve spiegarla
- è qualcosa di familiare per riconoscibilità - apprezzata dagli utenti (easy to learn)
- a volte si rompe per forza di cose (es. cestino sopra la scrivania)
- esperienza desktop, esperienza ecommerce, posta, social
- metafora di UI - mazzo di carte (card navigabili dei luoghi o swipe di tinder)
- la metafora diventa linguaggio comune (metaforico) nel tempo se funziona e non ci si accorge più come un paio di occhiali
- quelle buone rimangono nel tempo (es. macchina da scrivere - anche se pochi ora l'hanno usata)

5 tipi di interazioni fondamentali
- Dare istruzioni (menu, CLI, GUI - rapido efficiente, molte opzioni da valutare, utile per ripetizione, o operaz massive)
- Conversare (chatbot, llm, si/no, assistente vocale - familiare e piacevole, a volte diventa unilaterale come i sistemi dei telefono premi 1 se...)
- Manipolare (zoom, dragndrop, rotaz... - forma naturale, metafora di un'esperienza fisica comune e appagante, poca ansia, apprendibile, memorabile, a prova di errore. Scala male su numerosità - a quel punto si usano i comandi e le istruz astratte)
- Esplorare (implica una visualizz 3D (o 2D) - muoversi in uno spazio, anche un dataset numerico può diventare un luogo fisico)
- Rispondere (notifiche, domande, info da foto/QR - il sistema manda degli interrupt all'utente)

Tipi di interaz diverse: Costi diversi, diversi modi di interagire (es. dare comandi sia via menu e click, sia via CLI...), da adattare agli utenti e ai contesti: sono un modo x pensare a come sostenere al meglio le attività dell'utente.

Avere il controllo - ci piace, ci serve
Nuovi sistemi hanno il controllo (es. AI) o retroazioni automatizzate. Bisogna chiedersi quale il limite.
Es. Fidarsi del GPS anche se assurdo? Sistema segnalazione malore che monitora costantemente - troppo controllo
> Interfacce utente a intervento. Sistemi autonomi in cui però è facile inserirsi. Potenza dell'AI va imbrigliata

Una visione può guidare l'interaction design. Come si immagina il futuro? es. superpoteri grazie a tech, percezione e cognizione aumentata. Es2. Siri nel 1987

## 4. Cognizione

Tipi di cognizione:
* Esperienziale - reagire ad eventi/stimoli/oggetti esterni - affine al pensiero veloce
* Riflessiva - dentro di noi - vicina al pensiero lento

### Processi cognitivi (spesso interdipendenti, non scindibili):

* **attenzione**
   * ci permette di non essere investiti dalle auto
   * è anche selezione di cosa concentrarsi
   * dipende dagli obb delle persone e dalle info circostanti
   * A volte si è attenti e si cerca specificità, a volte si vaga nel menu cercando qualcosa che stuzzichi
   * Ci sono heavy e light multitasker. gli heavy tendono ad avere soglia più bassa dell'attenzione, si distraggono ma fanno buon uso di questa dote. Distrarsi non è sempre negativo: alcuni compiti richiedono distrazione su tanti fronti
   * multitasking generalmente piu dispendioso xk effort x riprendere dal context switching: app di messaggistica rallentano la lettura di un testo fino al 50%, a volte MT porta a fare errori
   * sale operatorie con sempre piu schermi - tecniche come allarmi colorati o suoni per cose critiche
   * telefono alla guida scatena processi cognitivi che distraggono facilmente (es. immaginare faccia di chi parla) - modalità aereo x auto?
   * luoghi di lavoro - vanno studiate interfacce ad hoc pensando al MT
   > UI: no bloating, diversificare importanza con ordine, spaziature, stili, modalità
* **percezione**
   * con 5 sensi, vista predominante su udito e su altri
   > UI: distinguibilità e percepibilità icone, testo, separatori evidenti o spazi, distinguibilità suoni, feedback tattili usati parsimoniosamente
* **memoria**
  * funziona come non vorremmo a volte, ricordiamo cose poco utili
  * l'info viene filtrata, codificata e memorizzata
  * più attenzione -> più ricordo, più rielaboraz (appunti, discussione, esercizi) -> più ricordo
  * contesto di codifica: ci ricordiamo del medico quando è in divisa, quando è in abiti civili stentiamo a riconoscerlo
  * smartphone è protesi mnemonica: scatto foto e presto meno attenzione, internet a portata di mano. Ricordo dove/come reperire qualcosa e non la cosa stessa.
  * PIM - gestione file (imm/video/audio) personali. Le persone preferiscono colocare in cartelle che usare categorie, metadati
  * Regola di miller: ricordiamo circa 7 piu o meno 2 elementi. Generalm non si applica a design xk l'iutente non ha necess di ricordare gli elementi della GUI, ma riconsocerli
  * Password - poco memorabili. MFA a domande complessi da ricordare, alto carico mnemonico - PWD manager vincono, approccio passwordless
  * come progetttare un social in modo che aiuti a dimenticare una relaz finita?
  * sense cam per malati di alzheimer - foto della giornata ogni 30s x ricordare cosa successo. Memoria triplica
  > UI: ridurre burden evitando procedure lunghe, UI x riconoscimento anziché ricordo, modi di classificare info con cartelle
* **apprendimento** (interdipend con memoria)
  * intenzionale - mi metto, leggo manuale, studio, pratico, mi esercito. Sforzo e noia
  * incidentale - esco, imparo la strada, facendo imparo, provo. Più gradevole
  * UI: meglio interfacce che incoraggiano l'esplorazione, in apprendimento GUI, spingere verso scelte obbligate il discente
* leggere, parlare, ascoltare (elaborare il **linguaggio**)
  * soggettivo preferire leggere o ascoltare
    * ascolto amato - bambini e storie, adulti audiolibri, ma meno permanente
    * leggere è più veloce, si può tornare indietro, più rigoroso (non bene x dislessici)
    * il parlato è più sgrammaticato
    * UI: curare dimens testuale, attenzione agli speech based, interfacce tattili
* risolvere probl, pianificare, decidere, **ragionare**
  * azioni riflessive quando si ha tempo e info adeguata (es. analisi costi-benefici, quante proteine, allergeni) 
  * azioni istintive con poco tempo, sovraccarico di informazioni - spesso euristiche semplici di scelta (confezione bella, costo basso, marca conosciuta)
  * evoluzione ci ha portati a essere dei pessimi decisori veloci (fuggi o combatti)
  * si ama sempre meno il rischio - si delega a app di recensioni, open day, ecc paralysis by analysis
  * UI: fornire più info e corrette x chi deve scegliere, salvare preferenze utente
 
### Framework congnitivi (3 esterni e 3 interni)
> possono spiegare l'interaz. utente e aiutare a progettare interfacce migliori

* **Modelli mentali** - costruzione interna (soggettiva) usato per semplificare una situazione un aspetto del mondo, una tecnologia (es. cos'è la rete wifi, cos'è l'AI)
  * il tecnico di rete ha un modello mentale avanzato del wifi, l'utente medio ne ha uno semplice, gli consente di fare previsioni e il funzionamento di base
  * comune è usare modelli mentali errati (es. premere due volte ai semafori - di più è prima), a volte modelli basati su analogie inappropriate o superstizione
  * es. termostato o forno imposti dei setpoint. Non è che se metti il forno a 300 °C si riscalda piu in fretta
  * UI: istruzioni chiare e facili - supporto, tutorial - affordance che rendono naturale l'interazione (es. swipe, click o sleezione)
  * UI: trasparenza della UI - voglio una cosa xyz, chiedo all'interfaccia xyz e non devo impostare cento cose, mettere 8 password, perdere tempo dove non voglio (es. conferenza con il pubblico e il power point non si apre)
* **Golfo della valutazione / Golfo dell'esecuzione**
  * due componenti: utente e sistema (mondo) tra di loro due golfi da attraversare (sforzo cognitivo)
  * golfo dell'esecuzione - come uso il sistema? come interagisco?
  * golfo della valutazione - com'è lo stato del sistema? come interpreto?
  * progetto un sistema in modo da facilitare l'attraversamento dei golfi, l'utente impara anche lui.
* **Elaborazione delle informazioni**
  * spesso usate analogie x descrivere la mente - cervello ~ computer, processore, rete telefonica, rete neurale
  * analogia con processore: informazioni passano una pipeline di fasi (coding comparing matching executing)
  * modello elab informazioni - base x fare previsioni sulle prestazioni: T di risposta e colli di bottiglia
  * ne derivò modello del processore umano (albori HCI - anni 80)
  * oggi si preferisce studiare processi cognitivi in relazione all'ambiente esterno (studio in the wild)

* **Cognizione distribuita**
  * Non tutta la cognizione è dentro di noi: abbiamo riferimenti (alla casa rossa giro a dx), strumenti/artefatti (telefono, calcolatrice), interazioni (chiedo/parlo), verifichiamo direttamente (oggi c'è il sole? guardo la finestra), apro il frigo x scegliere che mangiare
  * l'informazione si sposta continuamente dall'interno all'esterno e viaggia in diversi mezzi (es. sistema di bordo della cabina di pilotaggio, o assistenza sanitaria (DiCoT framework Distributed Cognition 4 Teamwork - x lavorare meglio in gruppo))
  * struttura la UI con priorità verso l'importante, con ridondanza del sistema
* **Rappresentazione esterna**
  * Usiamo rappresentazioni esterne con sempre più strumenti, calcolatrici, penne, appunti, diagrammi, mappe
  * Rapp. Esterna: come interagiamo con sti strumenti (ormai spesso indispensabili)? Qual è il mezzo migliore?
  * Vantaggi esternalizzazione:
    * Riduzione *carico mnemonico*: ricordare costa caro (agenda compleanni, rubrica, postit, lista spesa, pila di cose da leggere)
    * *Offload computazionale*: operaz a mente, carta e penna o calcolatrice, con num arabi o romani
    * *Annotazione e tracing cognitivo*: modificare qualcosa x rispecchiare la realtà (spuntare lista spesa, ordinare oggetti)
  * UI: aiutiamo con interfacce a ridurre carico mnemonico, diamo supporti alternativi (esternalizzazioni diverse), riduciamo il carico computazionale, mostriamo le info rilevanti prima, dei riassunti. Ridurre il carico cognitivo
* **Interazione incarnata**
  * artefatti e tecnologie che ci indicano come sono correlati al mondo esterno ci dicono come vanno usati (es. libro aperto sul tavolo perché devo leggerlo)
  * trarre significato dall'interazione con strumenti fisici (anche cucchiai e tazze)
  * tutto è risultato di esperienza sensoriale/corporea --> senso del mondo (astratto e concreto)
  * anche interazione con le cose è mediata dal corpo (trovare i limiti, i problemi o nuove idee)
  * visualizzazione circuiti, marking nel ballo (ballare con brevissimi movimenti riassunti), servono a ripercorrere esperienza fisica, per migliorarla, ricordarla, ecc

## 5. Interazione sociale

* **Umano** - è animale **sociale** da sempre, scambio continuo di informazioni in gruppi (famiglia, lavoro, amici)
* Dai primi cellulari e social network si è passati a tecnologie sociali - si sono evolute x interagire insieme, ora è sempre connesso
* Social computing - lavorare collaborare globalmente anche da remoto e con spazi condivisi (lavagne calendari...)
* farsi domande orienta le nuove tecnologie. Es. che etichetta comportamentale? le persone parlano sempre a turno?
* I piu giovani preferiscono decidere all'ultimo minuto cosa fare - frustrante se prenoto con 1 sett di anticipo un biglietto
* troppo utilizzo dello smartphone a scapito di socializzazione, attenzione, ecc.
* alcune tch incoragggiano l'uso sociale (open the magic door di assistenti vocali tipo libro game a voce. Esempio deboluccio IMO)
* vedersi e parlare o mandarsi un messaggio? vedersi incoraggia la sincerità, la gestualità è insostituibile, i tempi, l'improvvisazione, i cambi argomento. Via messaggio è tutto diverso

### Conversazione e dialogo
* ci sono regole non scritte: apertura / svolgimento / chiusura
* messaggi e mail sono molto diverse dal parlato (spesso più dirette e frammentate, si saltano i saluti)
* studio interessante - aperture e chiusure più informali (hey, hello) nelle mail ottengono piu probabilità di risposta
* regole della conversazione - il parlante sceglie il successivo, un altro può decidere di iniziare, oppure il parlante continua / a cascata. se non avviene la 1 si passa alla 2 e così via
* spesso si dichiara quanto si vuole parlare - all'inizio (dirò 3 cose, se mi date 1 min,) - alla fine (concludendo... per riassumere, tutto chiaro?)
* back-channelling usato x dire quando si vuole lasciare o prendere parola (mmmm uh-uh) orientamento corpo (allontanarsi, avvicinarsi), sguardo, gesti
* frasi in coppie di adiacenza (aspettativa e risposta)
* meccanismi di riparazione - chiedo, rispondo, chiedo conferma ripentendo, correggo spiegandomi meglio
* a volte regole rotte anche intenzionalmente (chi continua a parlare) o in interrogazione lo studente non risponde (imbarazzo)
* fratture nella conversazione si riparano ascoltando e intervenendo es. che cosa? non ho capito
* nelle mail le fratture si riparano piu soft (intendevi x? la mail forse è andata in spam, promemoria rispondimi)
* ambigue anche le mail. Es. "..." la sospensione scaricano sul destinatario l'onere di interpretare invece della respons del mittente di chiarire
* imitazione - nella conversazione spesso si imita, sorride - sorrido, sbadiglia, sbadiglio, gratta testa, anch'io. Questo porta a empatia

### conversare da remoto

* primi esperimenti video window (agevolava chiacchiera naturale)
* diffusione app di video conf (zoom, meeting owl, skype, teams...), migliorata qualità video
* robot di telepresenza
  * vedono, si spostano, interagiscono, ascoltano e parlano
  * es. bambino in ospedale segue lezioni o invalido visita un museo
  * Beam+ modello commerciale
* facebook spaces - idea di usare un avatar in ambiente 3D per facilitare conversazione - attualmente limitato (un po' cartoonesco), molte persona hanno motion sickness, ma l'idea è che a tendere se migliora potrebbe diventare una dipendenza

### Copresenza

Stare insieme nello stesso luogo: agevolato da tecnologie cm multitouch o kinetic
* le persone comunicano (aggiornano o comandano) tramite gesti e strumenti, si coordinano.
  * linguaggi sintatt e semantici specifici: direttore d'orchestra, personale terra aeroporto
  * linguaggi universali (ciao, sventolare un oggetto)
* le persone hanno consapevolezza dello spazio
  * consap. periferica: concentrati su qualcosa, ci si accorge anche di altro (si origlia, coda dell'occhio)
  * consap. situazionale: consap. di quanto succede per comprendere come info eventi e azioni influiranno.
  * si lavora in team e se è coeso, si creano strategie di comunicazione e coordinamento (es. due responsabili di ambiti diversi che si coordinano quasi involontariamente)
* I bambini giocano insieme (es. sabbia AR o lego) coordinandosi e comunicando spesso
* interfacce che si condividono funzionano:
  * lavagne, schermi, tavoli multitouch - schemi preesistenti adattati al digitale xk funzionano bene, risultano naturali
* A volte, alcuni sono più partecipi di altri (timidezza, incertezza lingua, autismo...) 
  * un dispositivo fisico (es. lavagna digitale) invoglia un confronto paritario dove anche i timidi si esprimono
  * Reflect Table: tavolo con led in base a quanto si parla - alcuni hanno feedback retroattivo di limitarsi
* Strumenti utili ad evitare l'isolamento
  * Babble sistema IBM con biglie di chi è in conversazione. CHi no pian piano esce dal cerchio 
  * Sococo - ufficio virtuale per remote working o sedi distanti con piantina virtuale
  * breaktime barometer - indicatore di chi era in pausa caffe comune e disposto a parlare. Alcuni socializzavano di più, altri evitavano i momenti di affollamento e si isolavano di piu 
  * opinionizer - x rompere il ghiaccio alle conferenze, honeypot di uno schermo dove ognuno poteva scrivere commenti e mettere avatar cartoon. Intorno a sto schermo si creava un capannello di gente e spesso si parlava

### Coinvolgimento sociale
* i social hanno dato vita a molte iniziative di collaborazione sociale o escalation virali
  * dalle sfide di retweet di selfie pubblicitarie
  * risposta all'emergenza e iniziative sociali (es. volontariato)
  * community specifiche ampliate a dismisura
  * giornalismo diffuso (ognuno twitta la notizia del maltempo), i TG si rifanno ai post
  * miglioramento servizi comuni
  * citizen science (es. plant net o air quality)
  * fortnite - gioco comunitario
  * griefbot - ma eticamente?

## 6. Interazione emotiva

## 7. Interfacce

## 8. Raccolta dei dati

## 9. Analisi, interpretazione e presentazione dei dati

## 10. Dati su grande scala

## 11. Scoperta dei requisiti

## 12. Design, prototipazione e costruzione

## 13. Interaction design in pratica

## 14. Introduzione alla validazione

## 15. Studi di valutazione

## 16. Valutazione: ispezioni, analytics e modelli
