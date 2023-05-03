---
title: Ricerca e indicizzazione
description: Scoprite come creare file di PDF ricercabili da documenti acquisiti mediante scansione
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8095.jpg
kt: 8095
exl-id: a22230b5-1ff2-4870-84da-f06a904c99e1
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '1364'
ht-degree: 1%

---

# Ricerca e indicizzazione

![Usa banner eroe caso](assets/UseCaseSearchingHero.jpg)

Spesso le organizzazioni devono digitalizzare i propri documenti cartacei e i file digitalizzati. Considera questo [scenario](https://docs.google.com/document/d/11jZdVQAw-3fyE3Y-sIqFFTlZ4m02LsCC/edit). Uno studio legale ha migliaia di contratti legali che ha scansionato per creare file digitali. Essi intendono stabilire se uno di tali contratti giuridici contenga una clausola particolare o un complemento e devono quindi procedere a una revisione. La precisione è necessaria ai fini della conformità. La soluzione consiste nel fare un inventario dei documenti digitali, rendere il testo ricercabile e creare un indice per trovare queste informazioni.

La creazione di archivi digitali per recuperare le informazioni per le operazioni di editing o downstream è un vero e proprio incubo per la maggior parte delle organizzazioni.

## Cosa puoi imparare

Questa esercitazione pratica esplora come [!DNL Adobe Acrobat Services] Le funzionalità delle API e possono essere facilmente utilizzate per archiviare e digitalizzare i documenti. Esplora queste funzionalità creando un&#39;applicazione Express NodeJS, quindi integrando [!DNL Acrobat Services] API per l&#39;archiviazione, la digitalizzazione e la trasformazione dei documenti.

Per seguire, devi [Node.js](https://nodejs.org/) installato e una conoscenza di base di Node.js e [Sintassi ES6](https://www.w3schools.com/js/js_es6.asp).

## API e risorse pertinenti

* [API dei servizi PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Codice progetto](https://github.com/agavitalis/AdobeDocumentServicesAPIs.git)

## Impostazione del progetto

Innanzitutto, impostate la struttura delle cartelle per l&#39;applicazione. Potete recuperare il codice sorgente [qui](https://github.com/agavitalis/AdobeDocumentAPI.git).

## Struttura della directory

Create una cartella denominata AdobeDocumentServicesAPI e apritela in un editor di vostra scelta. Creare un&#39;applicazione NodeJS di base con il metodo `npm init` mediante questa struttura di cartelle:

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

MongoDB è utilizzato come database per questa applicazione. Pertanto, per configurare, posizionate le configurazioni di database predefinite nella cartella config/, incollando lo snippet di codice seguente nel file default.json di questa cartella, quindi aggiungete l&#39;URL del database.

```
### config/default.json and config/dev.json
{ "DBHost": "YOUR_DB_URI" }
```

## Installazione del pacchetto

Ora, installa alcuni pacchetti utilizzando il comando di installazione npm come illustrato nello snippet di codice riportato di seguito:

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

Questi snippet di codice installano le dipendenze dell&#39;applicazione, compreso il motore di creazione modelli Handlebar per la visualizzazione. Nel tag script, configurate i parametri di runtime dell&#39;applicazione.

## Integrazione [!DNL Acrobat Services] API

[!DNL Acrobat Services] include tre API:

* API dei servizi Adobe PDF

* API di incorporamento di Adobe PDF

* Adobe API di generazione documenti

Queste API automatizzano la generazione, la manipolazione e la trasformazione dei contenuti PDF attraverso un insieme di servizi web basati su cloud.

Per ottenere le credenziali necessarie [registrare](https://www.adobe.com/go/dcsdks_credentials?ref=getStartedWithServicesSDK) e completare il flusso di lavoro. L’API di incorporamento PDF è gratuita. Le API per i servizi di PDF e le API per la generazione di documenti sono gratuite per sei mesi. Al termine della versione di prova, puoi [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) a soli 0,05 dollari per transazione documentale. Paghi solo quando la tua azienda cresce ed elabora più contratti.

![Screenshot della creazione delle credenziali](assets/searching_1.png)

Una volta completata la registrazione, nel PC viene scaricato un esempio di codice contenente le credenziali API. Estrarre questo esempio di codice e inserire i file private.key e pdftools-api-credentials.json nella directory principale dell&#39;applicazione.

Installazione [PDF Services Node.js SDK](https://www.npmjs.com/package/@adobe/documentservices-pdftools-node-sdk) eseguendo la ` npm install --save @adobe/documentservices-pdftools-node-sdk ` utilizzando il terminale nella directory principale dell&#39;applicazione.

## Creazione di un PDF

[!DNL Acrobat Services] supporta la creazione di PDF da documenti Microsoft Office (Word, Excel e PowerPoint) e altri [formati di file supportati](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf) come .txt, .rtf, .bmp, .jpg, .gif, .tiff e .png.

Per creare documenti PDF dai formati di file supportati, utilizza questo modulo per caricare i documenti. È possibile accedere ai file HTML e CSS per il modulo in [GitHub](https://github.com/agavitalis/AdobeDocumentServicesAPIs.git).

![Screenshot del modulo Web](assets/searching_2.png)

Ora, aggiungete i seguenti snippet di codice al file controllers/createPDFController.js. Questo codice recupera il documento e lo trasforma in un PDF.

I file originali e il file trasformato vengono salvati in una cartella all&#39;interno dell&#39;applicazione.

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

Questo snippet di codice richiede [PDF Services Node.js SDK](https://www.npmjs.com/package/@adobe/documentservices-pdftools-node-sdk). Utilizzare le funzioni seguenti:

* createPDF, che visualizza il modulo di caricamento del documento

* createPDFPost, che trasforma il documento caricato in un PDF

I documenti PDF trasformati vengono salvati nella directory di output, mentre il file originale viene salvato nella directory di caricamento.

## Uso del riconoscimento del testo

Il riconoscimento ottico dei caratteri (OCR) converte le immagini e i documenti acquisiti mediante scansione in file ricercabili. Puoi convertire [!DNL Acrobat Services] API, immagini e documenti scansionati a PDF ricercabili. Dopo aver eseguito un’operazione OCR, il file diventa modificabile e ricercabile. Potete memorizzare il contenuto del file in un archivio dati per indicizzarlo e altri usi.

Ricorda che la ricerca e l&#39;indicizzazione dei documenti acquisiti da scanner sono fondamentali per molte organizzazioni in cui la gestione dei file e l&#39;elaborazione delle informazioni sono essenziali. La funzione OCR elimina queste sfide.

Per implementare questa funzione, è necessario progettare un modulo di caricamento simile a quello precedente. Questa volta, il modulo viene limitato ai file di PDF, in quanto è possibile utilizzare la funzione OCR solo nei documenti di PDF.

Di seguito è riportato il modulo di caricamento per questo esempio:

![Screenshot del modulo per caricare i file](assets/searching_3.png)

Ora, per manipolare il PDF caricato ed eseguire alcune operazioni OCR, aggiungi lo snippet di codice riportato di seguito al file controller/makeOCRController.js. Questo codice implementa il processo OCR su un file caricato e quindi salva il file nel file system dell&#39;applicazione.

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

È necessario il [!DNL Acrobat Services] Node SDK e i moduli mangusta, pdf-parse e fs e lo schema del modello di documento. Questi moduli sono necessari per salvare il contenuto del file trasformato in un database MongoDB.

Ora create due funzioni: makeOCR per visualizzare il modulo caricato, quindi makeOCRPost per elaborare il documento caricato. Salvare il modulo originale in un database, quindi salvare il modulo trasformato nella cartella di output dell&#39;applicazione.

Le credenziali fornite dall&#39;Adobe dal file pdftools-api-credentials.json vengono caricate in ogni caso prima di trasformare il file.

>[!NOTE]
>
>La funzione OCR supporta solo i documenti PDF.

Aggiungete inoltre lo snippet di codice seguente al file Modes/Document.js dell&#39;applicazione.

Nello snippet di codice, definite un modello di mangusta e quindi descrivete le proprietà del documento da salvare nel database. Inoltre, indicizzate il campo documentContent per semplificare e rendere efficiente la ricerca dei testi.

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

Ora è possibile implementare una semplice funzione di ricerca per consentire agli utenti di eseguire semplici ricerche di testo. Potete anche aggiungere funzionalità di download per abilitare il download dei file PDF.

Questa funzionalità richiede un modulo semplice e schede per visualizzare il risultato della ricerca. Puoi trovare le progettazioni per il modulo e le schede su [GitHub](https://github.com/agavitalis/AdobeDocumentServicesAPIs.git).

La schermata seguente illustra la funzione di ricerca e i risultati della ricerca. Potete scaricare i risultati della ricerca.

![Screenshot delle funzioni di ricerca](assets/searching_4.png)

Per implementare la funzione di ricerca, create un file searchController.js all&#39;interno della cartella controller dell&#39;applicazione e incollate lo snippet di codice seguente:

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

Ora implementa una funzione di download per abilitare il download dei documenti restituiti dalla ricerca di un utente.

## Download dei documenti

L’implementazione di una funzione di download è simile a quella già eseguita. Aggiungere il seguente snippet di codice dopo la funzione searchPost nel file controllers/earchController.js:

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

In questa esercitazione pratica, hai integrato [!DNL Acrobat Services] Le API in un&#39;applicazione Node.js e l&#39;API utilizzata per implementare una trasformazione del documento che converte i file in PDF. È stata aggiunta una funzione OCR che rende le immagini e i file acquisiti mediante scansione ricercabili. I file sono stati quindi salvati in una cartella in modo che possano essere scaricati.

A questo punto è stata aggiunta una funzione di ricerca per cercare i documenti convertiti in testo mediante OCR. Infine, è stata implementata una funzione di download per consentire il facile scaricamento di tali file. L&#39;applicazione completata rende molto più semplice per un&#39;azienda legale individuare ed elaborare testi specifici.

Utilizzo [!DNL Acrobat Services] per la trasformazione dei documenti è altamente consigliato per la sua robustezza e facilità di utilizzo rispetto ad altri servizi. Puoi creare rapidamente un account per iniziare a utilizzare le funzionalità di [!DNL Acrobat Services] API per la trasformazione e la gestione dei documenti.

Ora che hai una profonda conoscenza di come utilizzarlo [!DNL Acrobat Services] API, puoi migliorare le tue competenze con la pratica. Puoi clonare il repository utilizzato in questo tutorial e sperimentare con alcune delle abilità appena apprese. Ancora meglio, puoi tentare di ricreare questa applicazione esplorando le possibilità illimitate di [!DNL Acrobat Services] API.

Vuoi abilitare la condivisione e la revisione di documenti nella tua app? Registrati per [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
account sviluppatore. Prova gratuita di sei mesi, quindi [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) a soli \$0,05 per transazione documento, man mano che la tua attività cresce.
