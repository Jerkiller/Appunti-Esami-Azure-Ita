# Interaction design

## 1. Cos'è l'interaction design

Interaction design con ogni cosa HCI solo con pc e dispositivi. Ma ora il confine si sta assottigliando
Multidisciplinare, team da 15 persone, psicologi, informatici, ecc a volte costoso

obbiettivo dell'Int. des.: aiutare le persone a comunicare e interagire
Comprendere utenti-non esiste una taglia unica es. Vecchi col brondi nn x forza
Accessibilita e inclusivita
Deficit sensorialincognitivi fisici
Deficit permanenti temporanei situazionalip

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


Ricerca e design sono attivita caotiche, prevedono spreco, vicoli ciechi, false ipotesi prima di capire un problema e risplverlo

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
- Focalizzarsi su utenti, sul loro obiettivi, sui comportamenti, contesto, caratteristiche, decidere xon utenti in testa.
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
Nessuno lo sa, a volte abbiamo bisogno di cose che nemmeno sogniamo. Quindi si esplora spazio dei problemi. I designer e i dev a volte si rispecchiano in quello che fanno, ma non è x forza quello di cui ha bisogno un utente.

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
- è qualcosa di familiare per riconoscibilità - apprezzata dagli utenti (easy to learn)
- a volte si rompe per forza di cose (es. cestino sopra la scrivania)
- esperienza desktop, esperienza ecommerce, posta, social
- metafora di UI - mazzo di carte (card navigabili dei luoghi o swipe di tinder)
- la metafora diventa linguaggio comune (metaforico) nel tempo se funziona e non ci si accorge più come un paio di occhiali

5 tipi di interazioni fondamentali
- Dare istruzioni (menu, CLI, GUI - rapido efficiente, molte opzioni da valutare, utile per ripetizione, o operaz massive)
- Conversare (chatbot, llm, si/no, assistente vocale - familiare e piacevole, a volte diventa unilaterale come i sistemi dei telefono premi 1 se...)
- Manipolare (zoom, dragndrop, rotaz... - forma naturale, metafora di un'esperienza fisica comune e appagante, poca ansia, apprendibile, memorabile, a prova di errore. Scala male su numerosità - a quel punto si usano i comandi e le istruz astratte)
- Esplorare (implica una visualizz 3D (o 2D) - muoversi in uno spazio, anche un dataset numerico può diventare un luogo fisico)
- Rispondere (notifiche, domande, info da foto/QR - il sistema manda degli interrupt all'utente)

Tipi di interaz diverse: Costi diversi, diversi modi di interagire (es. dare comandi sia via menu e click, sia via CLI...), da adattare agli utenti e ai contesti