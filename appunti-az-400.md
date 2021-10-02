# AZ400

[Pdf del corso](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3VP8d)


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
