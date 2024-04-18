# AI Challenge

> [Link to the challenge](https://learn.microsoft.com/it-it/collections/ddkzh7oe0jpk?WT.mc_id=cloudskillschallenge_da09d3ca-a2bb-47dc-ba42-bea77b386a3d)

## 01. Intro

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
* **indice** composto da key, searchable, sortable, facetable, filterable, retrievable /default yes
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

## 12.

## 13.

## 14.

## 15.

## 16.

## 17.

## 18.

## 19.

## 20.

## 21.

## 22.

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

## 27.

