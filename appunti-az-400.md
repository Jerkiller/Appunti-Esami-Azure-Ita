# AZ400

[Pdf del corso](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3VP8d)


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
