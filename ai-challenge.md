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
* 
