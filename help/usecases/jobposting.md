---
title: Registrazione processo
description: Scopri come sviluppare un'esperienza web uniforme e coerente per i candidati e i datori di lavoro
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8092
thumbnail: KT-8092.jpg
exl-id: 0e24c8fd-7fda-452c-96f9-1e7ab1e06922
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1447'
ht-degree: 0%

---

# Pubblicazione processo

![Banner dell&#39;eroe del caso di utilizzo](assets/UseCaseJobHero.jpg)

Quando si gestisce un sito Web con più utenti, è fondamentale progettare un&#39;esperienza che garantisca un&#39;esperienza uniforme per tutti.

Si immagini lo scenario seguente: si dispone di un sito Web che consente ai datori di lavoro di [caricare le pubblicazioni di lavoro](https://developer.adobe.com/document-services/use-cases/content-publishing/job-posting). Per le persone in cerca di lavoro, è comodo visualizzare facilmente tutti i documenti relativi a un post in un formato coerente. Tuttavia, per i datori di lavoro è conveniente allegare informazioni in qualsiasi formato di file. Per facilitare entrambi i tipi di utenti, puoi convertire automaticamente in PDF tutti i documenti caricati e incorporarli in linea nel messaggio.

## Cosa puoi imparare

Questa esercitazione pratica descrive un esempio di Node.js che utilizza [!DNL Adobe Acrobat Services] e il relativo [Node.js SDK](https://www.npmjs.com/package/@adobe/documentservices-pdftools-node-sdk) per aggiungere queste funzionalità a un sito di pubblicazione di job. In questo modo si crea un sito web più facile da utilizzare e più attraente sia per i datori di lavoro che per le persone in cerca di lavoro. Di seguito è riportato il [codice completo](https://github.com/contentlab-io/adobe_job_posting) del [progetto](https://github.com/contentlab-io/adobe_job_posting), per consentirti di seguire la lettura.

Per iniziare, configura una semplice applicazione Web Node.js basata su Express. [Express](https://expressjs.com/) è un framework minimalista di applicazioni Web che offre funzionalità come l&#39;instradamento e la creazione di modelli. Il codice per l&#39;applicazione è disponibile su [GitHub](https://github.com/contentlab-io/adobe_job_posting). Installare inoltre il [database PostgreSQL](https://www.postgresql.org/) e configurarlo per archiviare il PDF.

## API [!DNL Acrobat Services] rilevanti

* [API di incorporamento PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [API dei servizi PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

## Creazione delle credenziali Adobe API

Innanzitutto, è necessario [creare credenziali](https://www.adobe.com/go/dcsdks_credentials) per Adobe PDF Embed API (gratuito per l&#39;uso) e Adobe PDF Services API (gratuito per sei mesi poi [pay-as-you-go](https://developer.adobe.com/document-services/pricing/main) per soli \$0,05 per transazione documento). Quando si creano le credenziali per l’API di PDF Services, selezionare l’opzione &quot;Crea codice di esempio personalizzato&quot;. Salva il file ZIP ed estrai pdftools-api-credentials.json e private.key nella directory principale del progetto Node.js Express.

È inoltre necessaria una chiave API per l’API di incorporamento liberamente disponibile. Da [Progetti](https://developer.adobe.com/console/projects), seleziona il progetto che hai creato. Fai quindi clic su **Aggiungi al progetto** e seleziona **API**. Fare clic su **PDF Embed API**.

Specifica il dominio per l’API PDF Embed. La chiave API deve essere pubblica (trova nel codice eseguito dal browser). Specificando un dominio, assicurati che un altro utente in un dominio diverso non possa utilizzare la chiave API.

Impossibile utilizzare &quot;localhost&quot; come dominio. Specificare un dominio, ad esempio &quot;testing.local&quot;, e modificare il file degli host sul computer per reindirizzare il dominio a 127.0.0.1, che è il computer. Quindi, invece di testare l&#39;applicazione su localhost:3000, è possibile testarla su testing.local:3000. Al termine, trova la chiave API per l’API PDF Embed nella pagina del progetto.

## Aggiunta di un modulo di caricamento e di un gestore

Con un’applicazione Express funzionante e credenziali API, è inoltre necessario un modulo che consenta agli utenti di caricare i propri documenti sul sito Web. A tale scopo, modificare il modello index.jade.

Crea un campo di input per il nome del processo caricato e per un documento contenente ulteriori informazioni.

All’interno del blocco di contenuto del modello, aggiungi il modulo seguente:

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

Aggiungere quindi un gestore per la richiesta POST all&#39;azione /upload. Quindi, aggiungi una route per /upload al file routes/index.js. Puoi creare un nuovo file per questa route, ma dovrai aggiornare il file app.js per riflettere il nuovo file. All&#39;interno di questo gestore route è possibile accedere al nome specificato e al file caricato.

```
router.post('/upload', async function (req, res, next) {
    const name = req.body.name;
    const fileContents = req.files.attachment.data;

    // code to work with the uploaded document
  });
```

La funzione è asincrona, quindi è possibile utilizzare la parola chiave await nella funzione, utile quando si chiamano i metodi che eseguono le chiamate API.

![Schermata del sito Web per la pubblicazione dei processi](assets/jobs_1.png)

## Utilizzo dell’API di PDF Services

Prima di utilizzare l&#39;API PDF Services, è necessario aggiungere le seguenti importazioni all&#39;inizio del file route:

```
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk');
  const { Readable } = require('stream');
```

Direttamente sotto le importazioni, puoi caricare le credenziali API e creare un [contenuto di esecuzione](https://www.javascripttutorial.net/javascript-execution-context/). Poiché è possibile riutilizzare un contesto di esecuzione per operazioni diverse, è consigliabile eseguire questa operazione una sola volta.

```
  const credentials = PDFToolsSdk.Credentials
  .serviceAccountCredentialsBuilder()
  .fromFile("pdftools-api-credentials.json")
  .build();

  const executionContext = PDFToolsSdk.ExecutionContext.create(credentials);
```

Tornare ora alla scrittura del codice nel gestore richieste in corrispondenza del commento nel blocco `router.post`. Iniziate convertendo il documento in PDF.

```
  const createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();

  const input = PDFToolsSdk.FileRef.createFromStream(Readable.from(fileContents),
  req.files.attachment.mimetype);

  createPdfOperation.setInput(input);

  let result = await createPdfOperation.execute(executionContext);

  result.saveAsFile('output-pdf' + new Date().getTime() + '.pdf');
  return res.send('success!');
```

La maggior parte delle operazioni esegue gli stessi quattro passaggi. Inizializzare innanzitutto il tipo di operazione utilizzando il metodo createNew della classe appropriata. Creare quindi l&#39;input per l&#39;operazione, ovvero FileRef. Le operazioni successive possono ignorare questo passo perché il risultato di un&#39;operazione è anche un FileRef. Per questa operazione iniziale, creare un FileRef dai byte del file caricato. In terzo luogo, è necessario assegnare l&#39;input all&#39;operazione. Infine, l&#39;operazione viene eseguita con il contesto di esecuzione come parametro nel metodo Execute. Questo metodo restituisce una promessa in modo da poter attendere il risultato.

Il codice salva il PDF restituito in un file e invia una semplice risposta di &quot;operazione completata&quot; al browser. La parte &quot;Data&quot; del nome file garantisce un nome file univoco. SaveAsFile restituisce un errore se il file di destinazione esiste.

## Conversione di immagini in testo e compressione del PDF

A questo punto, utilizza il riconoscimento ottico dei caratteri (OCR) per convertire le immagini in testo e quindi comprimi il risultato. Questa operazione viene eseguita utilizzando le operazioni OCR e Comprimi PDF simili all’operazione Crea PDF. Aggiungere quanto segue al file di route, in `router.post`:

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

Esiste un’alternativa migliore rispetto al salvataggio del file su un disco rigido e alla restituzione di una risposta HTTP eccessivamente semplificata. Archivia invece PDF in un database e visualizza una pagina Web che incorpora PDF utilizzando l’API gratuita di PDF Embed di Adobe. In questo modo, l&#39;annuncio di lavoro o la brochure del datore di lavoro è visibile sul sito web per chi cerca lavoro da trovare e visualizzare, completo di loghi aziendali e altri elementi di progettazione.

## Memorizzazione del PDF in un database

Memorizzare i PDF in un database PostgreSQL. Scarica il pacchetto node-postgres per connettersi a Postgres in Node.js. Installare il pacchetto stream-buffer perché, a un certo punto, è necessario memorizzare il contenuto del PDF in un buffer e FileRef funziona solo con i flussi. Quindi, utilizza il pacchetto stream-buffer per scrivere il contenuto in un buffer.

```
npm install pg stream-buffers
```

Creare una tabella di database per le pubblicazioni di lavoro. Sono necessarie una colonna per un identificatore univoco, una colonna per un nome e una colonna per il PDF associato. È possibile creare una tabella di database dall&#39;interfaccia della riga di comando (CLI) Postgres:

```
CREATE TABLE job_postings (id TEXT PRIMARY KEY, name TEXT NOT NULL, attachment
BYTEA NOT NULL);
```

Torna ai file Node.js. Aggiungi alcune importazioni nella parte superiore del file:

```
  const { Client } = require('pg');
  const streamBuffers = require('stream-buffers');
```

Per memorizzare il PDF nella tabella del database, modificare la funzione di caricamento. Sostituisci le ultime due righe (saveAsFile e send) con questo frammento di codice:

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

Per scrivere il contenuto, creare un WritableStreamBuffer. Con l&#39;evento finish, è il momento di eseguire la query SQL. Il pacchetto node-postgres converte automaticamente il parametro Buffer nel formato BYTEA. La query reindirizza l&#39;utente a /job/{id}, un endpoint creato in seguito.

Per l’API PDF Embed, è inoltre necessario un endpoint che restituisca solo il contenuto PDF:

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

## Incorporamento delle PDF

Creare ora l&#39;endpoint /job/{id}, che esegue il rendering di un modello contenente il nome della pubblicazione del processo richiesta e un PDF incorporato.

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

Nella directory views/, crea un file job.jade con questo contenuto:

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

Il primo script è View SDK di Adobe, che semplifica l’incorporamento delle PDF. Il secondo script è un one-liner in linea che imposta il valore di window.embedUrl sull&#39;URL del PDF fornito dal gestore di route rapido. Creare personalmente il terzo script nel modo seguente:

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

Ora puoi testare l’intero processo di caricamento di un documento, reindirizzamento alla pagina /job/id e visualizzazione del PDF incorporato. Gli utenti eseguono gli stessi passaggi per aggiungere un annuncio di lavoro o un altro documento al sito Web.

![Schermata di prova di un documento PDF caricato](assets/jobs_2.png)

Per vedere un incorporamento in linea in azione, guardate questa [demo dal vivo](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/IN_LINE/Bodea%20Brochure.pdf).

## Fasi seguenti

Questa esercitazione pratica spiega come utilizzare Node.js con [!DNL Acrobat Services] per convertire in PDF un [annuncio di lavoro](https://developer.adobe.com/document-services/use-cases/content-publishing/job-posting) caricato in vari formati. Il PDF risultante è stato quindi incorporato in una pagina Web. Ora puoi aggiungere la stessa funzione al tuo sito Web, rendendo più facile per i datori di lavoro caricare descrizioni di lavori, brochure e altro ancora da trovare per chi è in cerca di lavoro. Queste funzionalità aiutano tutti a ottenere le informazioni necessarie per trovare il lavoro dei sogni.

[!DNL Acrobat Services] ti consente di aggiungere funzioni chiave di gestione dei documenti al tuo sito Web o app. Per ulteriori informazioni sulle funzionalità di queste API, consulta la seguente documentazione di avvio rapido:

* [API di incorporamento PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [API dei servizi PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

Per iniziare ad aggiungere funzionalità di gestione dei documenti di facile utilizzo al tuo sito Web, [iscriviti alla versione di prova gratuita](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html). L&#39;API di Adobe PDF Embed è sempre gratuita e l&#39;API dei servizi Adobe PDF è gratuita per sei mesi; quindi è di soli \$0,05 per transazione di documento e potrai [pagare in base alla tua disponibilità](https://developer.adobe.com/document-services/pricing/main) in base alla crescita della tua azienda.
