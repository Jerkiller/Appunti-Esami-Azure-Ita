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

X impostare FS logging via CLI:
`az webapp log config --application-logging true --level verbose --name <app-name> --resource-group <RG-name>`
Impostando a false, nn si disattiva, ma si imposta a error only.
X vedere log: `az webapp log show --name <app-name> --resource-group <resource-group-name>`

### Application Insights

Aggiungi codice per raccogliere in un logger centrale. AI funge anche da bidone x le metriche.
