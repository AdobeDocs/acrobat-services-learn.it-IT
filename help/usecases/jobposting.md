---
title: Distribuzione
description: Scopri come sviluppare un'esperienza web uniforme e uniforme per i candidati e i datori di lavoro
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8092.jpg
kt: 8092
exl-id: 0e24c8fd-7fda-452c-96f9-1e7ab1e06922
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '1527'
ht-degree: 0%

---

# Distribuzione delle mansioni

![Usa banner eroe caso](assets/UseCaseJobHero.jpg)

Quando si gestisce un sito web con più utenti, è fondamentale progettare un&#39;esperienza che garantisca un&#39;esperienza uniforme per tutti.

Immaginate il seguente scenario: hai un sito Web che consente ai datori di lavoro di [caricamento di post di lavoro](https://www.adobe.io/apis/documentcloud/dcsdk/job-posting.html). Per chi è in cerca di lavoro, è utile visualizzare facilmente tutti i documenti relativi a un post in un formato uniforme. Tuttavia, per i datori di lavoro è conveniente allegare le informazioni in qualsiasi formato di file si trovino. Per offrire la massima praticità a entrambi i tipi di utenti, puoi convertire automaticamente tutti i documenti caricati in PDF e incorporarli in linea nella pubblicazione.

## Cosa puoi imparare

Questa esercitazione pratica illustra un esempio di Node.js che utilizza [!DNL Adobe Acrobat Services] e le sue [Node.js SDK](https://www.npmjs.com/package/@adobe/documentservices-pdftools-node-sdk) per aggiungere queste funzionalità a un sito di registrazione dei processi. Questo crea un sito web più facile da usare e più attraente per datori di lavoro e in cerca di lavoro. Ecco il [completo](https://github.com/contentlab-io/adobe_job_posting) [codice progetto](https://github.com/contentlab-io/adobe_job_posting), nel caso in cui si desideri seguirli durante la lettura.

Per iniziare, configura una semplice applicazione web Node.js basata su Express. [Espresso](https://expressjs.com/) è un framework minimalista per applicazioni web che offre funzionalità come l&#39;indirizzamento e la creazione di modelli. Il codice dell&#39;applicazione è disponibile su [GitHub](https://github.com/contentlab-io/adobe_job_posting). Inoltre, installa il [Database PostgreSQL](https://www.postgresql.org/) e impostarlo per memorizzare il PDF.

## Pertinente [!DNL Acrobat Services] API

* [API di incorporamento PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [API dei servizi PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

## Creazione di credenziali API di Adobe

Per prima cosa, devi [creare le credenziali](https://www.adobe.com/go/dcsdks_credentials) per Adobe PDF Embed API (gratuito) e Adobe PDF Services API (gratis per sei mesi [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) a soli \$0,05 per transazione documento). Quando si creano le credenziali per l&#39;API dei servizi PDF, selezionare l&#39;opzione &quot;Crea un esempio di codice personalizzato&quot;. Salva il file ZIP ed estrai pdftools-api-credentials.json e private.key nella directory principale del progetto Node.js Express.

È inoltre necessaria una chiave API per l’API Embed liberamente disponibile. Da [Progetti](https://console.adobe.io/projects), accedi al progetto che hai creato. Quindi fai clic su **Aggiungi al progetto** e seleziona **API**. Infine, fai clic su **API di incorporamento PDF**.

Specificate il dominio per l&#39;API di incorporamento PDF. La chiave API deve essere pubblica (trovarla nel codice eseguito dal browser). Specificando un dominio, assicuratevi che altri utenti di un dominio diverso non possano utilizzare la chiave API.

Non è possibile utilizzare &quot;localhost&quot; come dominio. Specificate un dominio, ad esempio &quot;testing.local&quot;, e modificate il file host sul computer per reindirizzare il dominio a 127.0.0.1, che è il computer. Quindi, anziché provare l&#39;applicazione su localhost:3000, potete provarla su testing.local:3000. Al termine, trova la chiave API per l’API di incorporamento PDF nella pagina del progetto.

## Aggiunta di un modulo di caricamento e di un gestore

Con un’applicazione Express funzionante e credenziali API, è inoltre necessario un modulo che consenta agli utenti di caricare i propri documenti sul sito Web. Modificare il modello index.jade a questo scopo.

Creare un campo di input per il nome della pubblicazione dei processi caricati e per un documento contenente ulteriori informazioni.

All’interno del blocco di contenuti del modello, aggiungi il seguente modulo:

```
extends layout

block content
  h1= title

  form(action="/upload", enctype="multipart/form-data", method="POST")
    label Job posting name:&nbsp;
    input(type="text", name="name", required="required")
    br
    br
    label Describing document:&nbsp;
    input(type="file", name="attachment", required="required")
    br
    br
    input(type="submit", value="Submit job posting")
```

Quindi, aggiungere un gestore per la richiesta POST all&#39;azione /upload. Quindi, aggiungete un percorso per /upload al file routes/index.js. Puoi creare un nuovo file per questo percorso, ma dovrai aggiornare il file app.js per riflettere il nuovo file. All&#39;interno di questo gestore di percorsi, è possibile accedere al nome specificato e al file caricato.

```
router.post('/upload', async function (req, res, next) {
    const name = req.body.name;
    const fileContents = req.files.attachment.data;

    // code to work with the uploaded document
  });
```

La funzione è asincrona e consente di utilizzare la parola chiave await nella funzione, utile per chiamare i metodi che eseguono le chiamate API.

![Screenshot del sito Web di job posting](assets/jobs_1.png)

## Uso dell&#39;API dei servizi PDF

Prima di utilizzare l’API Servizi PDF, è necessario aggiungere le seguenti importazioni nella parte superiore del file route:

```
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk');
  const { Readable } = require('stream');
```

Direttamente sotto le importazioni, puoi caricare le credenziali API e creare un [contenuto dell&#39;esecuzione](https://www.javascripttutorial.net/javascript-execution-context/). Poiché è possibile riutilizzare un contesto di esecuzione per operazioni diverse, è opportuno eseguirlo una sola volta.

```
  const credentials = PDFToolsSdk.Credentials
  .serviceAccountCredentialsBuilder()
  .fromFile("pdftools-api-credentials.json")
  .build();

  const executionContext = PDFToolsSdk.ExecutionContext.create(credentials);
```

A questo punto, tornare alla scrittura del codice nel gestore di richieste al commento nel `router.post` blocco. Iniziate convertendo il documento in PDF.

```
  const createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();

  const input = PDFToolsSdk.FileRef.createFromStream(Readable.from(fileContents),
  req.files.attachment.mimetype);

  createPdfOperation.setInput(input);

  let result = await createPdfOperation.execute(executionContext);

  result.saveAsFile('output-pdf' + new Date().getTime() + '.pdf');
  return res.send('success!');
```

La maggior parte delle operazioni avviene con quattro passi. Inizializzare il tipo di operazione utilizzando il metodo createNew della classe appropriata. Quindi, creare l&#39;input per l&#39;operazione, ovvero FileRef. Le operazioni successive possono saltare questo passaggio perché il risultato di un&#39;operazione è anche un fileRef. Per questa operazione iniziale, creare un fileRef dai byte del file caricato. Infine, è necessario assegnare l&#39;input all&#39;operazione. Infine, viene eseguita l&#39;operazione, con il contesto di esecuzione come parametro nel metodo execute. Questo metodo restituisce una Promessa in modo da poter attendere il risultato.

Il codice salva il PDF restituito in un file e invia una semplice risposta &quot;success&quot; al browser. La parte &quot;Date&quot; del nome del file garantisce un nome file univoco. Il file saveAsFile restituisce un errore se il file di destinazione esiste.

## Conversione di immagini in testo e compressione del PDF

A questo punto, utilizzate il riconoscimento ottico dei caratteri (OCR) per convertire le immagini in testo e quindi comprimere il risultato. A tale scopo, utilizzate le operazioni OCR e CompressPDF simili a quelle CreatePDF. Aggiungete quanto segue al file route, in `router.post`:

```
  const name = req.body.name;
  const fileContents = req.files.attachment.data;

  const createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();
  const input = PDFToolsSdk.FileRef.createFromStream(Readable.from(fileContents),
  req.files.attachment.mimetype);
  createPdfOperation.setInput(input);

  let result = await createPdfOperation.execute(executionContext);

  const ocrOperation = PDFToolsSdk.OCR.Operation.createNew();
  ocrOperation.setInput(result);
  result = await ocrOperation.execute(executionContext);

  const compressPdfOperation = PDFToolsSdk.CompressPDF.Operation.createNew();
  compressPdfOperation.setInput(result);
  result = await compressPdfOperation.execute(executionContext);

  result.saveAsFile('output-pdf' + new Date().getTime() + '.pdf');
  return res.send('success!');
```

È necessario eseguire questa operazione una sola volta perché il risultato è un FileRef, che il codice può passare a setInput.

Esiste un&#39;alternativa migliore del salvataggio del file su un disco rigido e la restituzione di una risposta HTTP eccessivamente semplificata. Archiviate invece il PDF in un database e visualizzate una pagina Web che incorpora il PDF utilizzando l&#39;API di incorporamento di PDF gratuita di Adobe. In questo modo, la postazione o la brochure del lavoro del datore di lavoro è visibile sul sito web per chi cerca lavoro da trovare e visualizzare, completo di loghi aziendali e altri elementi di design.

## Memorizzazione del PDF in un database

Memorizzare i PDF in un database PostgreSQL. Scarica il pacchetto node-postgres per connettersi a Postgres in Node.js. Installate il pacchetto di streaming-buffer perché, ad un certo punto, dovete memorizzare il contenuto del PDF in un buffer e FileRef funziona solo con i flussi. Quindi, utilizzate il pacchetto di buffer di streaming per scrivere il contenuto in un buffer.

```
npm install pg stream-buffers
```

Ora create una tabella di database per la pubblicazione dei processi. È necessaria una colonna per un identificatore univoco, una colonna per un nome e una colonna per il PDF associato. Potete creare una tabella di database dall’interfaccia della riga di comando Postgres (CLI):

```
CREATE TABLE job_postings (id TEXT PRIMARY KEY, name TEXT NOT NULL, attachment
BYTEA NOT NULL);
```

Tornate ai file Node.js. Aggiungere alcune importazioni nella parte superiore del file:

```
  const { Client } = require('pg');
  const streamBuffers = require('stream-buffers');
```

Per memorizzare il PDF nella tabella di database, modificate la funzione di caricamento. Sostituire le ultime due righe (saveAsFile e send) con questo snippet di codice:

```
  const pgClient = new Client();
  pgClient.connect();

  const id = Math.random().toString(36).substr(2, 6); // not securely random at all,
  but serves the purpose for this demo

  const writableStream = new streamBuffers.WritableStreamBuffer();
  writableStream.on("finish", async () => {    
    await pgClient.query("INSERT INTO job_postings VALUES ($1, $2, $3)", [
      id,
      name,
      writableStream.getContents()
    ]);
    res.redirect(`/job/${id}`);
  })
  result.writeToStream(writableStream);
```

Per scrivere il contenuto, creare un componente WritableStreamBuffer. Con l&#39;evento finish, è il momento di eseguire la query SQL. Il pacchetto node-postgres converte automaticamente il parametro Buffer nel formato BYTEA. La query reindirizza l&#39;utente a /job/{id}, un endpoint creato in seguito.

Per l&#39;API di incorporamento PDF, è necessario anche un endpoint che restituisca solo il contenuto del PDF:

```
  router.get('/pdf/:id', async function (req, res, next) {
    const id = req.params.id;
 
    const pgClient = new Client();
    pgClient.connect();

  const pgResult = await pgClient.query("SELECT attachment FROM job_postings WHERE id
  = $1", [id]);
  const buffer = pgResult.rows[0].attachment;
  res.type('pdf');
    return res.send(buffer);
  });
```

## Incorporamento del PDF

A questo punto, create l&#39;endpoint /job/{id}, che esegue il rendering di un modello contenente il nome della pubblicazione dei processi richiesta e un PDF incorporato.

```
router.get('/job/:id', async function(req, res, next) {
    const id = req.params.id;

    const pgClient = new Client();
    pgClient.connect();

    const pgResult = await pgClient.query("SELECT name FROM job_postings WHERE id =
  $1", [id]);
    const name = pgResult.rows[0].name;

    res.render('job', { pdf_url: `/pdf/${id}`, name });
  });
```

Nella directory views/, creare un file job.jade con questo contenuto:

```
  extends layout

  block content
    h1= name
    div(id='adobe-dc-view')
    script(src='https://documentcloud.adobe.com/view-sdk/main.js')
    script.
      window.embedUrl = "!{pdf_url}";
    script(src='/javascripts/embed-pdf.js')
```

Il primo script è View SDK di Adobe, che semplifica l&#39;incorporamento del PDF. Il secondo script è un one-liner in linea che imposta il valore di window.embedUrl sull&#39;URL del PDF fornito dal gestore di percorsi Express. Create personalmente il terzo script nel modo seguente:

```
  document.addEventListener("adobe_dc_view_sdk.ready", function () {
    var adobeDCView = new AdobeDC.View({ clientId: "YOUR API KEY HERE", divId:
   "adobe-dc-view" });
    adobeDCView.previewFile({
      content: { location: { url: '//' + window.location.host + window.embedUrl }
         },
      metaData: { fileName: "Job posting" }
    });
  });
```

Ora puoi provare l’intero processo di caricamento di un documento, reindirizzarlo alla pagina /job/id e visualizzare il PDF incorporato. Gli utenti seguono gli stessi passaggi per aggiungere un documento di lavoro o un altro documento al sito Web.

![Screenshot della prova di un documento PDF caricato](assets/jobs_2.png)

Per vedere un incorporamento in linea in azione, consultate questa sezione [demo live](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/IN_LINE/Bodea%20Brochure.pdf).

## Fasi seguenti

Questa esercitazione pratica spiega come utilizzare Node.js con [!DNL Acrobat Services] per convertire un file caricato [registrazione del lavoro](https://www.adobe.io/apis/documentcloud/dcsdk/job-posting.html) in vari formati su un PDF. Il PDF risultante è stato poi incorporato in una pagina Web. Ora puoi aggiungere la stessa funzione al tuo sito web, semplificando il caricamento delle descrizioni dei lavori, delle brochure e altro ancora per chi è in cerca di lavoro. Queste funzionalità aiutano tutti a ottenere le informazioni necessarie per trovare il lavoro da sogno.

[!DNL Acrobat Services] ti consente di aggiungere funzioni chiave di gestione dei documenti al tuo sito web o alla tua app. Per approfondire le possibilità offerte da queste API, consulta la seguente documentazione di avvio rapido:

* [API di incorporamento PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [API dei servizi PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

Per iniziare ad aggiungere al tuo sito web funzionalità intuitive per la gestione dei documenti, [richiedi la versione di prova gratuita](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html). L&#39;API di incorporamento di Adobe PDF è sempre gratuita e l&#39;API dei servizi Adobe PDF è gratuita per sei mesi, quindi è solo \$0,05 per transazione del documento, così puoi [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) man mano che il tuo business cresce.
