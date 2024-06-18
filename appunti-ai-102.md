# AI Challenge

> * [Link to the challenge](https://learn.microsoft.com/it-it/collections/ddkzh7oe0jpk?WT.mc_id=cloudskillschallenge_da09d3ca-a2bb-47dc-ba42-bea77b386a3d)
> * [Syllabus](https://learn.microsoft.com/it-it/credentials/certifications/resources/study-guides/ai-102#skills-measured-as-of-march-28-2024)
> * [video indexer](https://www.videoindexer.ai/)
> * [exam topics prime 15 pag](https://www.examtopics.com/exams/microsoft/ai-102/view/)

## 01. Intro

* AI = SW that exhibit human-like capabilities
  * Visual perception (is it a happy image)
  * Language (understand)
  * Speech (hear/speak/conversate)
  * Decision making (spikes/thresholds)
* AI built on top of ML (on data algorithms to train prediction models)
* ML built on data science (math+statistics)
* Deep learning - performant in unstructured data (lang, images), whereas alberi con boosting sfumato (XGBoost, LightGBM e CatBoost) for tabular data or GLM, decision trees...
* [REgister OAI Azure](https://aka.ms/oai/access) [Access OAI](https://oai.azure.com/)
* `az cognitiveservices account create -n MyOpenAIResource -g OAIResourceGroup -l eastus --kind OpenAI --sku s0 --subscription subscriptionID`
* ChatGPT chatbot sviluppato da OpenAI basato su modello GPT (Generative Preliminar Training)
* Modelli GPT 3.5, 3.5 turbo (chat optimised), 4, DALL-E. In OAI: models
* Prices vary according to selected model and tokens
* Deployments permette di rilasciare un modello personalizzato di ChatBot
* az cognitiveservices account deployment create \
   -g OAIResourceGroup \
   -n MyOpenAIResource \
   --deployment-name MyModel \
   --model-name gpt-35-turbo \
   --model-version "0301"  \
   --model-format OpenAI \
   --sku-name "Standard" \
   --sku-capacity 1
* si può dialogare con il modello tramite API REST, Python, C# o da Studio
* To test a model, we have playgrounds. Here I can use parameters like
  * temperature: model creativity (the lower, the more deterministic answers)
  * top p: probability of infrequent answers
  * penalties: penalize the **frequency** of a token in an answer, **presence** penalize the freq of a token in a conversation
  * max length and stop sequence - indicate the length of a possible answer and how to finish it
  * pre-post text: add text before after the model answer to allow user to continue the conversation or to take time 4 the model to think
* nel playground si possono scrivere esempi di conversazioni. si parla di modello few-shots (zero-shots è chatGPT puro)
* AzML - data - train model and deploy and use it
* Cognitive Services
  * Visual Perception
    * Image Analysis - get cathegories, tags
    * Video Analysis
    * Image Classification
    * Obj Detection
    * Face Analysis
    * OCR
    * Forms recognizer
 * Language
    * Lang Understanding
      * Answerting question
      * Text analysis
    * Translation
 * Speech
   * Hearing (speech to text)
   * Speaking (text to speak)
   * Speech translation
   * Speaker recognition
* Decision making
   * Anomaly detection
   * Content moderation (is text suitable for 18+)
   * Content personalisation (cart suggestion)
* Other
   * Metrics advisor (RCA)
   * Immersive reader (pics, insights)
   * Bot services - conversational interaction (mail, web chat, teams, oth)
   * Cognitive search - data, index enriched, search against index
* How to use cogn services? add instance of service in my subscription and I get my cognitive services resource (region)
  * res = multiservice (single endpoint, single access credentials, standard pricing, no free) / or / single service
  * training may require different resource (a cost for train, a cost for use)
  * endpoint URI of a resource
    * firewall to restrict some IPs or service endpoint (subnet) / private endpoint
    * REST (any lang, max personalisation, more complex) / SDK (create a client (endpoint, credentials) and use it, much sinpler)
  * 2 keys for key rotation (regeneration command rest) -> store key in KV (RBAC for service principal)
  * some services use tokens
  * cost - pay as you go - pay for transactions, how much do I use it?
  * metrics / logs (audit, response, trace) - diagnostics settings -> log anal workspace (az monitor), event hub, ext system, storage
  * on premise scenario
    * sometimes necessary (cars, factories) 
    * provisioning of cognitive services as containers from mcr.microsoft.com docker registry
    * 3 params required: Api key, billing URI, eula (must accept)
    * some internet connectivities to report consumptions to billing endpoint
  

## 02. Integrate Open AI and apps

* Deploy a specifc model in Open AI with some capabilities (e.g. Davinci 003)
* Endpoint ChatCompletion - I specify roles and a sample conversation to provide a context. (only GPT3.5turbo)
* Endpoint Embeddings - fFrom input to a vector
* Endpoint Completion
* pip install openai
* `# Add OpenAI library
from openai import AzureOpenAI

deployment_name = '<YOUR_DEPLOYMENT_NAME>' 

# Initialize the Azure OpenAI client
client = AzureOpenAI(
        azure_endpoint = '<YOUR_ENDPOINT_NAME>', 
        api_key='<YOUR_API_KEY>',  
        api_version="20xx-xx-xx" #  Target version of the API, such as 2024-02-15-preview
        )`

## 03. Prompt engineering

* play with top_p and temp, not both
* be clear (no ambiguity)
* add details, divide into sections '---'
* in chat completion, the System role is used to tell the assistant what to do and how: you're a librarian of star trek saga, you're a italian-> spanish translator, don't answer to anything, just translate.
* give a cronology to conversation or summarize periodically the conversation
* few-shots learning: give some examples (e.g. of classification)
* decompose the question in simpler shorter questions
* step by step approach of thoughts. Ask the mpdel a step by step approach of thoughts, to know which are the basis of reason, the sources.

## 04. Generate code

* codex models for writing code, now only gpt35-turbo or higher. Github copilot
* useful to generate code, optimize code (refactor), spot bugs and fix them, translate code dialect/language, or explain code
* and also... write unit test, write documentation/comments

## 05. Generate images

* Dall-E parameters in plyground vivid/natural, resolution 256*256 ... 1024*1024 1024*1792 standard hd
* API rest in post async con risposta di polling. param size, num imag e prompt

## 06. RAG vs opt

* RAG (Retrieval Augmented Generation) - sistema per fornire dati aggiuntivi
* RAG via REST o playground, sfrutta AI search ad esempio e GPT35turbo mette insieme le risposte
* md txt html pdf word ppt
* indice da mettere
* ocio ai token limitazioni

## 07. Responsible AI

* Basic prinicples
  * Fairness (No biases)
  * Reliability+Safety (No harm)
  * Privacy+Security
  * Inclusiveness (Everyone, disability)
  * Transparency
  * Accountability (Governance, ethics, legal standards)
* Approach in 4 steps mutuated by NIST AI Risk Framework: identify risks, measure them in the output, mitigate impact&presence, manage solution responsibly with a plan
* Identify Risks: generate inexact content, hallucinate, harmful, offensive content, illicit illegal content, biased, discriminatory content...
* Measure risks: measure possibility to happen and impact of risk. Can be subjective evaluation (e.g. make poison with cooking assistant).
  More useful: categorize the possible outputs and count them (harmful/non-harmful/degrees of harmfulness)
* Test and verify risk presence: try to spot identify risks using several prompts and try to extract what you want from the model (red teaming)
  Tests can be manual or automated and are useful to re-validate in case of changes to the model
* Mitigare risks: can be done at 4 levels:
  * Model - change model, train model: Gpt4 very large and creative, sometimes it's too risky
  * Security - filter prompts and response according to content filters (Azure Open AI already does), or avoid flooding/automated-bots
    * content filters use 4 categories: hate, sex, violence, self-harm. Every response is marked with a score per each category (safe-low-med-high)
  * Metaprompt and Grounding - work on the prompt to make it better. e.g. add specific context words, add RAG
  * UX - limit inputs, validate inputs, document risks and tutorial to guide the user to a good use
* Manage responsibly the solution
  * Before releasing: check that conformity is satisfied (both in company and sector). Legal notes, privacy, security, accessibility.
  * Consider a gradual release plan (more and more users)
  * Consider a plan to front the imprevisti and rollback plan
  * Implement features like: block harmful content, block malicious users/IPs, user feedback
  * Keep track of user telemetry with privacy
  * Document and share: what discovered must be documented and shared with stakeholders

## 08. AI Search
* Capacity AI Search con tier NON MODIFICABILE: F,B,S,L free, basic (2GB-15ind), standard S2 e S3 per tagli diversi, L ottimizzato x archiviaz, query più lente, indici grandiii.
* 1 RU = 1 partiz * 1 replica. repliche consentono di gestire + query simultanee. partiz ottimizzano indice e query
* **data source**: cosmos, azsql, blob semistrutturati, indice json diretto
* **set di competenze**: da 1 doc si poss estrarre varie info: lingua, sentiment, immg contenuto, pti chiave, luoghi menzionati, ecc. competenze custom
* **indexer**: processo lanciato all'avvio o periodicamente o on-demand x ricostruire indice (es aggiungi nuova competenza, rilanci)
* **indice** composto da key, searchable (by word), sortable, facetable (drill-down), filterable, retrievable /default yes
* document index è un JSON gerarchico es. doc0>image0>text0...
* i campi dell'indice possono essere usati cm input x competenze che come 1 pipeline vanno a generare altri campi in out
* search: fatta con SDK o REST API. sintassi query Lucene. modalità simple o full. La full supporta regex, o filtri complessi. seachFiels e select x dire che campi dell0index cercare e quali restituire
* query eseguita in vari passaggi: query analysis (create a subquery tree), lexical analysis (clean text, il/lo/la/e/...reimossi e parole derivate semplificate), document retrieval in inedx, assegnaz punteggio con TF/IDF
* Lucene può ess esteso con OData come $filter $orderby
* possibilità di querare AI search non solo per ricerca, ma anche x avere lista dei facet x GUI (es. lista autori su cui filtrare)
* query anche x autocompletam e suggerimento DocumentsOperationsExtensions. Suggest e Autocomplete
* possibilità di penalizzare o promuovere un certo aspetto in un punteggio di ricerca (TFIDF e' default), ma un es. è penalizzare file + datati
* possibilità di definire e cercare per sinonimi

## 09. Custom competence

* competenza: Custom.WebApiSkill
* custom comp. rispetta firma con array di documenti in input e array di doc in out. In out ci sono warnings e errors per ciascun documento. Inoltre ci sono dei chiave valore per ciascun doc input e output.
*  in una nuova skill, occorre definire URI dell'api, header, contesto di esecuz, input e output
*  utili le custom skill se voglio usare modulo Az ML per predire valori x integrare quello che manca o integrare con una Az Func.
*  Quando non usarle? Quando esistono già skill predefinite come sentiment analysis

## 10. Knowledge store with AI Search

* utile usare ADF alimentato con i JSON dell'indice, o PowerBI su un relazionale creato a partire dai dati dell'incide
* Microsoft.Skills.Util.ShaperSkill usata per creare un json con certi campi e certi valori basati su elementi dell'indice (proiezioni) mapping
* viene così creato un knowledgeStore materializzando quello che serve in oggetti (json), file (png, jpeg) e tabelle definendo cosa si vuole e come in un json della

## 11. arricchire dati di Lingua con AI Search

* competenza custom in cui estendo con AI Language features
* traduzione, sentiment, QA, estraz info, summarize, classification
* tutte le funz sia standard che personalizzabili se hanno l'icona ingranaggio verde e *
* [Language studio](https://language.cognitive.azure.com/)

## 12. Migliorare ricerca

* Lucene estende funz di ricerca:
  * fuzzy, di prossimità (3 parole a non + di 10 parole di distanza), regex, booleani, bool con raggruppamento, boosting di una parola^3, campo specifico e jolly
* AI search usa algoritmo somiglianza BM25
  * scoring dato da questo algoritmo in base ai termini
  * scoring profile può ess definito x ponderare alcune categorie
* tokenizzazione, lemmatizzazione fatta nel creare indice. 50 diversi language analyzer in MS. Pattern analyzer specializzato x cercare una specifica regex (cod postale)
  * 3 step: filtri caratteri -> tokenizzatore -> filtri di token -> indicizzazione token
  * filtri caratteri come html_strip, mapping come TX -> Texas
  * tokenizer (13 diversi e svariati) di solito x lingua
  * filtri token rimuove spazi, apostrofi, possessivi, punti degli acronomi, token + lunghi di 50 caratt...
* possibile creare un custom analyzer definendo charFilters, tokenizers, tokenFilters. Viene invocato in REST e viene settato nella definiz dell0'indice
* supporto multilingua. Si possono impostare + analyzer in varie lingue in contemporanea, in modo che in ricerca limito la query alla ricerca sul campo in tale lingua e ottengo score migliore
* posso aggiungere nuove lingue usando set di competenze traduzioni dove uso descr_en come input e sparo fuori descr_jp come out.
* funzioni geospaziali: geo.distance, geo.intersects (la prima x distanza tra 2 punti, la seconda per intersez punto-poligono utile per filtrare ordinare ricerca. poligoni in senso antiorario e chiusi (primo=ultimo pt). distance lt 5 = dist < 5km.

## 13. AML Custom Skill

* Microsoft.Skills.Custom.AmlSkill
* Endpoint custom location.cloudapp.azure.com:443 in AKS (no ACI), nodi + potenti, migliori performance
* con modelli cm regressioni o classificazione, utile arricchire un indice con competenza custom x calcolare un campo

## 14. Push dati in indice

* indexers fanno pull dati x creare o arricchire indice.
* ADF può fare push di dati verso indice (sink). Approccio no-code con +100 data source
* creare indice a mano, andare su ADF fare nuova pipeline di ingest, mapping campi ed è fatta. Limite: solo tipi semplici supportati
* altro modo di fare push è con rest api. Push con api key su endpoint ***.search.windows.net
  * POST con azioni di tipo upload, merge uploadOrMerge, Delete (insert update upsert delete)
* usare SDK Azure.Search.Documents
  * suggerimento: mandare un batch di 100N elementi e ottimizzare N per ricevere risposta valida in meno tempo/MB
  * exponential backoff (codici 207 e 503) e threading

## 15. Sicurezza e gestione AI Search

* Sicurezza
   * approccio on-prem: company on prem - Express-route - az gateway app service con deny internet traffic e attaccato az congnitive service
   * traffico in transito in https (eventualm chiavi su vault)
   * auth di ricerca con query key (max 50) o admin key (max 2)
   * proteggere traffico in uscita (es. con app entra) vrs AzureCognitiveSearch (ip noti) o altre risorse
   * protegg dati a liv di documento: utente-gruppo-gruppo di docum indicizzati-docum. metere in query di ricerca "search.in" e taggarlo come security_field
* Performance
   * date da grandezza e compless indice
   * aggiungere log analytics al servizio di ricerca cognitive service in modo da misurare prestaz
   * le richieste lunghe vengono limitate (207 e 503) da verificare in log analytics
   * prova a fare 1 query con postman. sottrai tempo totale risposta 125ms da elapsed-time (negli response header) = 125-21 = 104ms totale tempo di roundtrip
   * ridurre dimens e compless indice. tolgo alcuni campi, alcuni li rendo non sortable, altri solo facetable...
   * scalare servizio di ricerca (grazie al...), fare + partizioni (max 12) x parallelizzare
   * migliorare le richerche, più puntuali e meno generiche, meno complesse
* Costi
   * alti.
   * saperli stimare col price calculator in base al num di dati, immg crackate, ecc.
   * ridurre banda, tenere tutto in unica area, anche il frontend.
   * usare budget e avvisi
* Affidabilità
   * 2/3+ repliche x SLA alti di query e di indicizzazione
   * backup indice delegato all'utente
* Monitoring
   * usare metriche LogAnalytics come docum processati, grandezza indice, ricerche al sec, conteggio exec skill, richieste throtthlate...
   * query custom KQL su AzureDiagnostics
   * creazione avvisi in base a metriche e soglie
   * 

## 16. Classificaz semantica

* 1K query di ricerca semantica gratuite (max 50 risultati)
* ordinamento migliore x classificaz semantica (comprensione linguaggio)
* riepilogo testuale - didascalie

## 17. ricerca vettoriale

* query vettoriali. I doc vengono indicizzati tramite vettori numerici non intelligibili
* record simili avranno distanza vettoriale simile

## 18.-19.-20.-21.-22. riconoscimento form

* training su modello di questionario/documento es. dichiaraz di successione, biglietti da visita, W2
* da dati scritti e stampati (pdf) a dati strutturati
* modello composto se ci sono + template diversi ank con campi diversi. vengono puntati altri modelli. (ottimo x il caso misto)
* modello generale se non c'è una struttura mai
* modelli specifici o opzioni x la selezione se ci sono caselle da annerire
* utile per import massivo
* integrazione SDK Py Java JS C# REST avendo endpoint+apikey
* azure ai document image studio
* training personalizzato con json e immg, meglio se alta qualità. alemno 5-6 esempi
* custom skillset - competenza personalizzata x sfruttare cose custom nell'analisi di un form (ACI,AKS,FUN) Microsoft.Skills.Custom.WebApiSkill URI+I/O

## 23. Copilot Studio

* permette di creare chatbot (aka power virtual agent)
* diversi ambienti in base a target o location (es. Cina Germania x privacy diversa)
* definire fino a 1K nodi argomenti. Un arg: frasi trigger e poi nodi di conversazione (domande/risposte/info)
* possibili variabili/parametri ed entità
* pubblicaz bot x usarlo in teams, slack, facebook...
* test del bot live o alla pubblicazione

## 24. Argomenti Copilot

* nodi argomenti: info/domadna/condiz/variab/goto_argom/scheda interatt/azione/avanzate
* domanda chiusa (risposta multipla - campo identify x specificare su che dominio) e aperta (data ora/int/str) -> risposta salvata in variabile
* varie opz domanda: ripetiz, opzionale, escalation, ecc
* immagine o video in risposta (es. tutorial)
* schede adattive indipend dalla piattaforma, carousel o elenco con piu schede
* risposte rapide
* invoca azione scatena flusso power automate (es manda mail)
* flusso condizionale (if a è uguale a b, ... ramo) tutte le altre opzioni... ramo
* aggiunta pagine web https x istruire il bot. il bot propone una serie di temi da includere nei nodi di un argomento
* se non viene riconosciuto l'intento dell'utente, si può gestire un argom di sistema riassegna che passa la palla al bot oppure un argomento preimpostato di fallback, in cui per es. viene fatta escalation
* stato degli argomenti (si possono abilitare/disab)
* errori dei nodi / argomenti: es. cancello una variabile usata altrove, errori generici...

## 25. Entità e variabili

* entità tipizzate, regex, tipo oggetti
* variabili, visibilità in tutto il bot se iniziano con "bot."

## 26. Migliorare i bot

* flusso di MS power automate con parametri in input dal bot e output da flow (es. citta -> meteo -> info tempo)
* escalation verso multichannel di Dynamics 365 o Salesforce, Genesys, ZenDesk, ServiceNow...
* generative AI su siti web e sharepoint
  * risp fallback con AI
  * argomenti con AI per variare testo
  * copilot x generaz nuovi argomenti

## 27. Progettaz copiloti

* uso interno aziendale dei copiloti x sostituire intranet faq
* farsi domande tipo: dove migliorabile? dove cercano i clienti risposte? ...
* definire possibili scenari e azioni (nelle varie fasi di processo prevendita, pagam, acquisto, spediz, postvendita...)
* punti chiave di un copilota
  * deve conoscere cliente e capire le esigenze
  * dare contenuti efficaci e aggiornati
  * assistenza puntuale non generica
  * azioni intraprese o scalabilità verso umani
  * frasi brevi, tono positivo e pro-attivo, colloquiale. evita assertività(imperativi) o formalità
  * rispettare i pronomi io(bot)/noi (azienda)
* qualità copilota: cooperativo, objecitve-oriented, veloce, a turni, sincero, educato
* tipi argomenti
  * informativi
    * cos'e? perché? quand'è?
  * attività
    * vorrei... come posso...?
  * risoluz probl
    * quando ... ricevo mess di err. Qualcosa non funz.
* creare albero nodi di conversazioni. tenerlo corto o accorciarlo xk all'utente non piace rispondere a 10 domande
* dare obb a un argomento. es. arg. "reso" -> il cliente è autonomo x la restituzione
* valutare efficacia nel tempo: KPI come abbandono, completamento, tempo attività, num escalation, argomenti abbandonati...

## Image analysis

* captioning srv
* tags, add cathegory
* obj: it's a car at (88,102,200,400) top-left and width height
* adult content recognition
* celebrity recognition

## Video analysis

* Face
* OCR
* speech (create transcription)
* captioning/labels (key topics extract)
* sentiment analysis
* content moderation (adult/not)
* scene segmentation
* concepts, brands, project recognition

## Image classification

* train data with labels and images
* class label based on global image
* multi-label (1 immag ha + label) multi-class (+ label possibili, ma 1 immag ha 1 sola label)
* facial analysis (no more age/sex)
  * where it is, info about it (masks?, glasses?), detection, verification
  * detection = unique ID for every face (cached for 24h) useful to count people enter building and leave building
  * recognition - person group with some pics of ppl
* OCR read-api from images/pdf (small-large) books, magazines
  * 2000 pag, 500MB (4MB in free), 50x50-10Kx10K
* image analysis api can do ocr, but limited.

## Lang understanding

* learned
  * CLU utterence recognize pre-built entities or trained ones (train-test-deploy-review cycle)
  * custom named entity / data+labels as json
  * custom txt classification (single/multi label)
* pre-built
  * summary
  * PII detection (IP; mail , name...)
* KPIs
  * recall - dataset con 6 documenti con la parola pizza. la ricerca mi da 3 giusti e 1 errato. recall = 3/6 = 0.5
  * precision - come prima, ma è 3/4 = 0.75
* question answering
  * big knowledgebase - I want an endpoint to answer questions on it.
  * a bot can call this endpoint in NL
  * confidence of answer = %. gGimme 3 most confident answers
* text analisys
  * lang detection
  * keyphrase extraction
  * sentiment analysis (positive .9)
  * named entity recognition
  * entity linking
* trnanslation
  * lang detection
  * one-to-many translation
  * transliteration (alphabets)
  


https://www.youtube.com/watch?v=I7fdWafTcPY&ab_channel=JohnSavill%27sTechnicalTraining
1.27.38


## Answers

* export model to gzip, set app version to v1.1, run container with model mounted # https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-container-howto
* dispatch service useful to route user across many tasks/bots according to utterance
* create a service for ocr+sentiment analysis with single endpoint? PUT + CognitiveService (update = PATCH)
* on prem anomaly detection: (pull anomaly detection image implicit in build) - create custom dockerfile - build it + push to az cont registry - deploy a docker run script
* cognitive services billing = contoso.cognitiveservices.azure.com , sent analysis image mcr.microsoft.com/azure-cognitive-services/textanalytics/sentimen
* no free tier for computer vision. to add caption to images, custom vision prediction
* regenerateKey => a new query key is generated for rotation
* form recognizer limits for CUSTOM model training: 50MB, images and pdf
* form recognizer -> power bi (table projection)
* CMK encryption requires AzKV. increase index size and degrades query performance
* throttling di search: indice troppo grande? non aumentare indice, ma scala di tier ++ schei. evita CMK. puoi aumentare repliche x parallel.
* app in vm+vnet must use cogn service without internet (on prem) -> PRIVATE endpoint/service endpoint + az private link. cambiare settings su CS x accedere a vnet
* x limitare auth ad app, usare az roles
* mp3 to text - GetCompressedFormat(MP3) + speechRecognizer
* inclusiveness = bot+direct line speech. app auth con speech in entra id ha bisogno di pvt endpoint e dominio custom
* QnA - deploy QuognitiveService n Appservice to perform queries. QnA fornisce risp statiche su dati statici, sempre la stessa risp a una domanda
* recognize spoken lang: SpeechtoText with AutoDetectSourceLanguageConfig. recognize multiple langs = cogn.text analytics/recognition/general. useful when video stream with audio must be analyzed
* troubleshooting speech to text. errori di inserimento: aggiungi voci in background, err in eliminaz aggiungi voci sovrapposte. err di riconoscimento, train modello con + parole di nomi persone o prodotti.
* process 50K images OCR + text analytics -> cogserv s0
* describeImageInStreamAsync x leggere x non vedenti
* OAI via arm tpl: capacity = parametro che indica quanti k-tpm (1000 richieste al minuto servire). app-OAI: auth via api key e endpoint + deployment name in sdk
* bot with emulator framework x local dev
* az video indexer (one drive file -> download link). to adapt transcript, correct it directly or upload an adaptation text (some key words, some phrases). avoid special chars


## Snippets


```csharp
// Add phraselist feature to programmatically add phrases to bots https://docs.microsoft.com/en-us/azure/cognitive-services/luis/client-libraries-rest-api
var phraselistId = await client.Features.AddPhraseListAsync(appId, versionId, new PhraselistCreateObject
{
EnabledForAllModels = false,
IsExchangeable = true,
Name = "QuantityPhraselist",
Phrases = "few,more,extra"
});

private static async Task AnalyzeReceipt(
FormRecognizerClient recognizerClient, string receiptUri)
{
RecognizedFormCollection receipts = await recognizerClient.StartRecognizeReceiptsFromUri(new Uri(receiptUrl)).WaitForCompletionAsync();
```


https://learn.microsoft.com/it-it/azure/ai-services/

https://learn.microsoft.com/it-it/azure/ai-services/luis/

https://learn.microsoft.com/it-it/azure/ai-services/speech-service/
https://learn.microsoft.com/it-it/azure/ai-services/translator/
https://learn.microsoft.com/it-it/azure/ai-services/language-service/
https://learn.microsoft.com/it-it/azure/ai-services/luis/
