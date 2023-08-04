---
title: Ricerca e indicizzazione
description: Scopri come creare file di PDF ricercabili da documenti scansionati
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8095
thumbnail: KT-8095.jpg
exl-id: a22230b5-1ff2-4870-84da-f06a904c99e1
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '1364'
ht-degree: 1%

---

# Ricerca e indicizzazione

![Banner Hero per casi di utilizzo](assets/UseCaseSearchingHero.jpg)

Le organizzazioni devono spesso digitalizzare i documenti cartacei e i file scansionati. Considera questo [scenario](https://docs.google.com/document/d/11jZdVQAw-3fyE3Y-sIqFFTlZ4m02LsCC/edit). Uno studio legale ha migliaia di contratti legali che ha scansionato per creare file digitali. Vogliono determinare se uno di questi contratti legali ha una particolare clausola o un supplemento che devono rivedere. L’accuratezza è necessaria ai fini della conformità. La soluzione consiste nell’effettuare l’inventario dei documenti digitali, rendere il testo ricercabile e creare un indice per trovare queste informazioni.

La creazione di archivi digitali per il recupero delle informazioni necessarie per le operazioni di editing o downstream rappresenta un problema per la maggior parte delle organizzazioni.

## Cosa puoi imparare

Questo tutorial pratico esplora come [!DNL Adobe Acrobat Services] Le funzionalità delle API possono essere facilmente utilizzate per archiviare e digitalizzare i documenti. Puoi esplorare queste funzionalità creando un’applicazione Express NodeJS, quindi integrando [!DNL Acrobat Services] API per l’archiviazione, la digitalizzazione e la trasformazione dei documenti.

Per seguire, hai bisogno di [Node.js](https://nodejs.org/) e una conoscenza di base di Node.js e [Sintassi ES6](https://www.w3schools.com/js/js_es6.asp).

## API e risorse pertinenti

* [API di PDF Services](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Codice progetto](https://github.com/agavitalis/AdobeDocumentServicesAPIs.git)

## Impostazione del progetto

Per prima cosa, imposta la struttura di cartelle per l’applicazione. È possibile recuperare il codice sorgente [qui](https://github.com/agavitalis/AdobeDocumentAPI.git).

## Struttura della directory

Crea una cartella denominata AdobeDocumentServicesAPIs e aprila in un editor di tua scelta. Creare un&#39;applicazione NodeJS di base con `npm init` comando che utilizza questa struttura di cartelle:

```
AdobeDocumentServicesAPIs
config
default.json
controllers
createPDFController.js
makeOCRController.js
searchController.js
models
document.js
output
.gitkeep
routes
web.js
services
upload.js
views
index.hbs
ocr.hbs
search.hbs
index.js
```

Si sta utilizzando MongoDB come database per questa applicazione. Pertanto, per configurare, inserire le configurazioni di database predefinite nella cartella config/, incollando lo snippet di codice seguente nel file default.json di questa cartella, quindi aggiungere l&#39;URL del database.

```
### config/default.json and config/dev.json
{ "DBHost": "YOUR_DB_URI" }
```

## Installazione del pacchetto

Ora, installa alcuni pacchetti utilizzando il comando npm install, come mostrato nel frammento di codice seguente:

```
{
    "name": "adobedocumentservicesapis",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "directories": {
    "test": "test"
    },
    "dependencies": {
    "body-parser": "^1.19.0",
    "config": "^3.3.6",
    "express": "^4.17.1",
    "hbs": "^4.1.1",
    "mongoose": "^5.12.1",
    "morgan": "^1.10.0",
    "multer": "^1.4.2",
    "path": "^0.12.7"
    },
    "devDependencies": {},
    "scripts": {
    "start": "set NODE_ENV=dev && node index.js"
    },
    "repository": {
    "type": "git",
    "url": "git+https://github.com/agavitalis/AdobeDocumentServicesAPIs.git"
    },
    "author": "Ogbonna Vitalis",
    "license": "ISC",
    "bugs": {
    "url": "https://github.com/agavitalis/AdobeDocumentServicesAPIs/issues"
    },
    "homepage": "https://github.com/agavitalis/AdobeDocumentServicesAPIs#readme"
}
```

```
###bash
npm install express mongoose config body-parser morgan multer hbs path pdf-parse
Ensure that the content of your package.json file is similar to this code snippet:
###package.json
{
```

Questi snippet di codice installano le dipendenze dell’applicazione, incluso il motore di creazione dei modelli Handlebars per la visualizzazione. Nel tag scripts, si configurano i parametri di runtime dell&#39;applicazione.

## Integrazione [!DNL Acrobat Services] API

[!DNL Acrobat Services] include tre API:

* API dei servizi Adobe PDF

* API di Adobe PDF Embed

* Adobe dell’API di Document Generation

Queste API automatizzano la generazione, la manipolazione e la trasformazione dei contenuti PDF attraverso un set di servizi Web basati su cloud.

Per ottenere le credenziali necessarie [registrare](https://www.adobe.com/go/dcsdks_credentials?ref=getStartedWithServicesSDK) e completarlo. L’API PDF Embed può essere utilizzata liberamente. L’API di PDF Services e l’API di Document Generation sono gratuiti per sei mesi. Al termine della versione di prova, puoi [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) a soli $ 0,05 per transazione documento. Paghi solo se la tua azienda cresce ed elabora più contratti.

![Schermata per la creazione delle credenziali](assets/searching_1.png)

Una volta completata la registrazione, nel PC viene scaricato un esempio di codice che contiene le credenziali API. Estrarre questo esempio di codice e inserire i file private.key e pdftools-api-credentials.json nella directory principale dell&#39;applicazione.

Installare [PDF Services Node.js SDK](https://www.npmjs.com/package/@adobe/documentservices-pdftools-node-sdk) eseguendo il comando ` npm install --save @adobe/documentservices-pdftools-node-sdk ` utilizzando il terminale nella directory principale dell&#39;applicazione.

## Creazione di un PDF

[!DNL Acrobat Services] supporta la creazione di PDF da documenti di Microsoft Office (Word, Excel e PowerPoint) e altri [formati di file supportati](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf) come .txt, .rtf, .bmp, .jpg, .gif, .tiff e .png.

Per creare documenti PDF dai formati di file supportati, usa questo modulo per caricare i documenti. Puoi accedere ai file HTML e CSS per il modulo su [GitHub](https://github.com/agavitalis/AdobeDocumentServicesAPIs.git).

![Schermata del modulo Web](assets/searching_2.png)

Ora aggiungi i seguenti snippet di codice al file controllers/createPDFController.js. Questo codice recupera il documento e lo trasforma in un PDF.

I file originali e il file trasformato vengono salvati in una cartella all’interno dell’applicazione.

```
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk');
/*
* GET / route to show the createPDF form.
*/
function createPDF(req, res) {
//catch any response on the url
let response = req.query.response
res.render('index', { response })
}
/*
* POST /createPDF to create a new PDF File.
*/
function createPDFPost(req, res) {
let filePath = req.file.path;
let fileName = req.file.filename;
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Create an ExecutionContext using credentials and create a new operation
instance.
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials),
createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();
// Set operation input from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile(filePath);
createPdfOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
createPdfOperation.execute(executionContext)
.then((result) => {
result.saveAsFile('output/createPDFFromDOCX.pdf')
//download the file
res.redirect('/?response=PDF Successfully created')
})
.catch(err => {
if (err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log('Exception encountered while executing operation',
err);
} else {
console.log('Exception encountered while executing operation',
err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
//export all the functions
module.exports = { createPDF, createPDFPost };
```

Questo frammento di codice richiede [PDF Services Node.js SDK](https://www.npmjs.com/package/@adobe/documentservices-pdftools-node-sdk). Utilizzare le funzioni seguenti:

* createPDF, che visualizza il modulo per il caricamento del documento

* createPDFPost, che trasforma il documento caricato in un PDF

I documenti PDF trasformati vengono salvati nella directory di output, mentre il file originale viene salvato nella directory uploads.

## Utilizzo del riconoscimento del testo

Il riconoscimento ottico dei caratteri (OCR) converte le immagini e i documenti acquisiti in file ricercabili. È possibile convertire [!DNL Acrobat Services] API, immagini e documenti scansionati su PDF ricercabili. Dopo aver eseguito un’operazione OCR, il file diventa modificabile e ricercabile. È possibile memorizzare il contenuto del file in un archivio dati per indicizzarlo e utilizzarlo in altri modi.

È importante ricordare che la ricerca e l&#39;indicizzazione dei documenti scansionati sono fondamentali per molte organizzazioni in cui la gestione dei file e l&#39;elaborazione delle informazioni sono essenziali. La funzione OCR elimina queste problematiche.

Per implementare questa funzione, è necessario progettare un modulo di caricamento simile a quello precedente. Questa volta, è possibile limitare il modulo ai file PDF, in quanto la funzione OCR può essere utilizzata solo sui documenti PDF.

Di seguito è riportato il modulo di caricamento per questo esempio:

![Schermata del modulo per caricare i file](assets/searching_3.png)

Per manipolare PDF caricato ed eseguire alcune operazioni OCR, aggiungi lo snippet di codice seguente al file controllers/makeOCRController.js. Questo codice implementa il processo OCR in un file caricato e quindi salva il file nel file system dell&#39;applicazione.

```
const fs = require('fs')
const pdf = require('pdf-parse');
const mongoose = require('mongoose');
const Document = require('../models/document');
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk');
/*
* GET /makeOCR route to show the makeOCR form.
*/
function makeOCR(req, res) {
//catch any response on the url
let response = req.query.response
res.render('ocr', { response })
}
/*
* POST /makeOCRPost to create a new PDF File.
*/
function makeOCRPost(req, res) {
let filePath = req.file.path;
let fileName = req.file.filename;
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Create an ExecutionContext using credentials and create a new operation
instance.
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials),
ocrOperation = PDFToolsSdk.OCR.Operation.createNew();
// Set operation input from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile(filePath);
ocrOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
ocrOperation.execute(executionContext)
.then(async (result) => {
let newFileName = `createPDFFromDOCX-${Math.random() * 171}.pdf`;
await result.saveAsFile(`output/${newFileName}`);
let documentContent = fs.readFileSync(
require("path").resolve("./") + `\\output\\${newFileName}`
);
pdf(documentContent)
.then(function (data) {
//Creates a new document
var newDocument = new Document({
documentName: fileName,
documentDescription: description,
documentContent: data.text,
url: require("path").resolve("./") + `\\output\\${newFileName}`
});
//Save it into the DB.
newDocument.save((err, docs) => {
if (err) {
res.send(err);
} else {
//If no errors, send it back to the client
res.redirect(
"/makeOCR?response=OCR Operation Successfully performed on
the PDF File"
);
}
});
})
.catch(function (error) {
// handle exceptions
console.log(error);
});
})
.catch(err => {
if (err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log('Exception encountered while executing operation',
err);
} else {
console.log('Exception encountered while executing operation',
err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
//export all the functions
module.exports = { makeOCR, makeOCRPost };
```

Hai bisogno di [!DNL Acrobat Services] Node SDK e i moduli mangoose, pdf-parse e fs e lo schema del modello di documento. Questi moduli sono necessari per salvare il contenuto del file trasformato in un database MongoDB.

Ora è possibile creare due funzioni: makeOCR per visualizzare il modulo caricato e makeOCRPost per elaborare il documento caricato. Salvare il modulo originale in un database, quindi salvare il modulo trasformato nella cartella di output dell&#39;applicazione.

Le credenziali fornite dall&#39;Adobe dal file pdftools-api-credentials.json vengono caricate in ogni caso prima della trasformazione del file.

>[!NOTE]
>
>La funzione OCR supporta solo i documenti PDF.

Inoltre, aggiungi lo snippet di codice seguente al file Modes/Document.js dell’applicazione.

Nel frammento di codice, definire un modello mangoose e quindi descrivere le proprietà del documento da salvare nel database. Inoltre, indicizza il campo documentContent per semplificare ed efficiente la ricerca dei testi.

```
const mongoose = require("mongoose");
const Schema = mongoose.Schema;
//Document schema definition
var DocumentSchema = new Schema(
{
documentName: { type: String, required: false },
documentDescription: { type: String, required: false },
documentContent: { type: String, required: false },
url: { type: String, required: false },
status: {
type: String,
enum : ["active","inactive"],
default: "active"
}
},
{ timestamps: true }
);
//for text search
DocumentSchema.index({
documentContent: "text",
});
//Exports the DocumentSchema for use elsewhere.
module.exports = mongoose.model("document", DocumentSchema);
```

## Ricerca di testi

Ora è possibile implementare una semplice funzione di ricerca per consentire agli utenti di eseguire alcune semplici ricerche di testo. È inoltre possibile aggiungere la funzionalità di download per consentire il download dei file PDF.

Questa funzionalità richiede un modulo semplice e schede per visualizzare i risultati della ricerca. Puoi trovare le progettazioni per il modulo e le schede nel [GitHub](https://github.com/agavitalis/AdobeDocumentServicesAPIs.git).

La schermata seguente illustra la funzione di ricerca e i risultati della ricerca. Potete scaricare qualsiasi risultato della ricerca.

![Schermata delle funzioni di ricerca](assets/searching_4.png)

Per implementare la funzione di ricerca, crea un file searchController.js all&#39;interno della cartella del controller dell&#39;applicazione e incolla lo snippet di codice di seguito:

```
const fs = require('fs')
const mongoose = require('mongoose');
const Document = require('../models/document');
/*
* GET / route to show the search form.
*/
function search(req, res) {
//catch any response on the url
let response = req.query.response
res.render('search', { response })
}
/*
* POST /searchPost to search the contents of your saved file.
*/
function searchPost(req, res) {
let searchString = req.body.searchString;
Document.aggregate([
{ $match: { $text: { $search: searchString } } },
{ $sort: { score: { $meta: "textScore" } } },
])
.then(function (documents) {
res.render('search', { documents })
})
.catch(function (error) {
let response = error
res.render('search', { response })
});
}
//export all the functions
module.exports = { search, searchPost, downloadPDF };
```

Ora è disponibile una funzione di download che consente di scaricare i documenti restituiti dalla ricerca di un utente.

## Download di documenti

L’implementazione di una funzione di download è simile a quella già eseguita. Aggiungere il seguente frammento di codice dopo la funzione searchPost nel file controllers/earchController.js:

```
/*
* POST /downloadPDF To Download PDF Documents.
*/
async function downloadPDF(req, res) {
console.log("here")
let documentId = req.params.documentId
let document = await Document.findOne({_id:documentId});
res.download(download.link);
}
```

## Fasi seguenti

In questo tutorial pratico, hai integrato [!DNL Acrobat Services] API in un&#39;applicazione Node.js e ha utilizzato anche l&#39;API per implementare una trasformazione del documento che converte i file in PDF. È stata aggiunta una funzione OCR che consente di ricercare immagini e file scansionati. Quindi, hai salvato i file in una cartella in modo che possano essere scaricati.

Successivamente, è stata aggiunta una funzione di ricerca per cercare i documenti convertiti in testo mediante OCR. Infine, è stata implementata una funzione di download che consente di scaricare facilmente tali file. L&#39;applicazione completata consente a un&#39;azienda legale di individuare ed elaborare più facilmente un testo specifico.

Utilizzo [!DNL Acrobat Services] per la trasformazione dei documenti è altamente consigliato a causa della sua robustezza e facilità d&#39;uso rispetto ad altri servizi. Puoi creare rapidamente un account per iniziare a usufruire delle funzionalità di [!DNL Acrobat Services] API per la trasformazione e la gestione dei documenti.

Ora che hai una buona conoscenza di come utilizzare [!DNL Acrobat Services] Con le API, puoi migliorare le tue competenze con pratica. È possibile clonare il repository utilizzato in questo tutorial e sperimentare alcune delle abilità che hai appena imparato. Ancora meglio, puoi tentare di ricostruire questa applicazione esplorando le possibilità illimitate di [!DNL Acrobat Services] API.

Sei pronto ad abilitare la condivisione e la revisione dei documenti nella tua app? Registrati per [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
account sviluppatore. Prova gratis per sei mesi, poi [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) per soli \$0,05 per transazione documento in base alla crescita dell&#39;azienda.
