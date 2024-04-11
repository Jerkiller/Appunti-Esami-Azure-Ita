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
