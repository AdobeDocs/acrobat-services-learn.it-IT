---
title: Flussi di lavoro degli accordi in Node.js
description: "[!DNL Adobe Acrobat Services] Le API incorporano facilmente funzionalità di PDF nelle applicazioni web"
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-7473.jpg
kt: 7473
keywords: In primo piano
exl-id: 44a03420-e963-472b-aeb8-290422c8d767
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '2182'
ht-degree: 1%

---

# Flussi di lavoro degli accordi in Node.js

![Usa banner eroe caso](assets/UseCaseAgreementHero.jpg)

Molte applicazioni e processi aziendali richiedono documentazione, come proposte e accordi. I documenti di PDF garantiscono una maggiore sicurezza e una minore modificabilità dei file. Inoltre, offrono un supporto per la firma digitale che consente ai clienti di completare i propri documenti in modo semplice e veloce. [!DNL Adobe Acrobat Services] Le API incorporano facilmente funzionalità di PDF nelle applicazioni web.

## Cosa puoi imparare

In questa esercitazione pratica, scopri come aggiungere servizi PDF a un’applicazione Node.js per digitalizzare un processo di accordo.

## API e risorse pertinenti

* [API dei servizi PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [API di incorporamento PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [API Adobe Sign](https://www.adobe.io/apis/documentcloud/sign.html)

* [Codice progetto](https://github.com/adobe/pdftools-node-sdk-samples)

## Configurazione [!DNL Adobe Acrobat Services]

Per iniziare, configura le credenziali da utilizzare [!DNL Adobe Acrobat Services]. Registrare un account e utilizzare il metodo [Node.js QuickStart](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#node-js) per verificare che le credenziali funzionino prima di integrare la funzionalità in un&#39;applicazione più grande.

Innanzitutto, richiedi un account sviluppatore di Adobe. Quindi, sul [Introduzione](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html?ref=getStartedWithServicesSDK) , selezionare la proprietà *Introduzione* in Crea nuove credenziali. Puoi registrarti per la versione di prova gratuita che offre 1.000 transazioni con documenti utilizzabili nell&#39;arco di sei mesi.

![Immagine della creazione di nuove credenziali](assets/AWNjs_1.png)

Nella seguente pagina Crea nuove credenziali, viene richiesto di scegliere tra l’API per l’incorporamento di PDF e l’API per i servizi di PDF.

Seleziona *API dei servizi PDF*.

Immettete un nome per l&#39;applicazione e selezionate la casella con etichetta *Creare un esempio di codice personalizzato*. Selezionando questa casella, le credenziali vengono incorporate automaticamente nell&#39;esempio di codice. Se lasciate questa casella deselezionata, dovete aggiungere manualmente le credenziali all&#39;applicazione.

Seleziona *Node.js* per il tipo di applicazione e fare clic su *Crea credenziali*.

Qualche istante dopo, un file .zip inizia a essere scaricato con un progetto di esempio, comprese le tue credenziali. Il pacchetto Node.js per [!DNL Acrobat Services] è già incluso nel codice di progetto di esempio.

![Immagine della selezione delle credenziali API dei servizi PDF](assets/AWNjs_2.png)

## Configurazione manuale del progetto di esempio

Se scegliete di non scaricare un progetto di esempio dalla pagina Crea nuove credenziali, potete anche impostare il progetto manualmente.

Scarica il codice (senza credenziali incorporate) da [GitHub](https://github.com/adobe/pdftools-node-sdk-samples). Se utilizzate questa versione del codice, dovete aggiungere le credenziali al file pdftools-api-credentials.json prima di utilizzare:

```
{
  "client_credentials": {
    "client_id": "<client_id>",
    "client_secret": "<client_secret>"
  },
  "service_account_credentials": {
    "organization_id": "<organization_id>",
    "account_id": "<technical_account_id>",
    "private_key_file": "<private_key_file_path>"
  }
}
```

Per l&#39;applicazione, è necessario copiare il file della chiave privata e i file delle credenziali nell&#39;origine dell&#39;applicazione.

È necessario installare il pacchetto Node.js per [!DNL Acrobat Services]. Per installare il pacchetto, utilizza il comando seguente:

```
npm install --save @adobe/documentservices-pdftools-node-sdk
```

## Impostazione della registrazione

Gli esempi qui usano Express per il framework applicativo. Utilizzano anche log4js per la registrazione delle applicazioni. Con log4js, puoi facilmente indirizzare l’accesso alla console o a un file:

```
const log4js = require('log4js');
const logger = log4js.getLogger();
log4js.configure( {
    appenders: { fileAppender: { type:'file', filename: './logs/applicationlog.txt'}},
    categories: { default: {appenders: ['fileAppender'], level:'info'}}
});
 
logger.level = 'info';
logger.info('Application started')
```

Il codice riportato sopra scrive i dati registrati in un file in ./logs/applicationlog.txt. Se invece desideri che scriva nella console, puoi commentare la chiamata a log4js.configure.

## Conversione di file Word in PDF

Accordi e proposte sono spesso scritti in un’applicazione per la produttività, come Microsoft Word. Per accettare documenti in questo formato e convertire il documento in PDF, è possibile aggiungere funzionalità all&#39;applicazione. Vediamo come caricare e salvare un documento in un&#39;applicazione Express e salvarlo nel file system.

Nel HTML dell&#39;applicazione, aggiungete un elemento file e un pulsante per avviare il caricamento:

```
<input type="file" name="source" id="source" />
<button onclick="upload()" >Upload</button>
```

Nella pagina JavaScript, caricate il file in modo asincrono utilizzando la funzione di recupero:

```
function upload()
{
  let formData = new FormData();
  var selectedFile = document.getElementById('source').files[0];
  formData.append("source", selectedFile);
  fetch('documentUpload', {method:"POST", body:formData});
}
```

Scegli una cartella in cui accettare i file caricati. L&#39;applicazione richiede un percorso a questa cartella. Trovare il percorso assoluto utilizzando un percorso relativo unito a \_\_dirname:

```
const uploadFolder = path.join(__dirname, "../uploads");
```

Poiché il file viene inviato tramite post, devi rispondere a un messaggio post sul lato server:

```
router.post('/', (req, res, next) => {
  console.log('uploading')
  if(!req.files || Object.keys(req.files).length === 0) {
  return res.status(400).send('No files were uploaded');
  }
    
  const uploadPath = path.join(uploadFolder, req.files.source.name);
  var buffer = req.files.source.data;
  var result = {"success":true};
  fs.writeFile(uploadPath, buffer, 'binary', (err)=> {
    if(err) {
      result.success = false;
    }
    res.json(result);
  });       
});
```

Una volta eseguita questa funzione, il file viene salvato nella cartella di caricamento delle applicazioni ed è disponibile per ulteriori elaborazioni.

Quindi, convertite il file dal formato nativo a PDF. Il codice di esempio scaricato in precedenza contiene uno script di nome `create-pdf-from-docx.js` per la conversione di un documento in PDF. La funzione seguente, `convertDocumentToPDF`, accetta un documento caricato e lo converte in un PDF in una cartella diversa:

```
function convertDocumentToPDF(sourcePath, destinationPath)
{    
  try {   
    const credentials = PDFToolsSDK.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdftools-api-credentials.json")
    .build();
 
    const executionContext = 
      PDFToolsSDK.ExecutionContext.create(credentials),
    createPdfOperation = PDFToolsSDK.CreatePDF.Operation.createNew();
 
    const docxReadableStream = fs.createReadStream(sourcePath);
    const input = PDFToolsSDK.FileRef.createFromStream(
      docxReadableStream, 
      PDFToolsSDK.CreatePDF.SupportedSourceFormat.docx);
    createPdfOperation.setInput(input);
 
    createPdfOperation.execute(executionContext)
    .then(result => result.saveAsFile(destinationPath))
    .catch(err => {        
      logger.erorr('Exception encountered while executing operation');        
    })
  }
  catch(err) {        
    logger.error(err);
  }
}
```

È possibile notare un pattern generale con il codice:

Il codice crea un oggetto credenziali e un contesto di esecuzione, inizializza alcune operazioni, quindi esegue l&#39;operazione con il contesto di esecuzione. Questo pattern è visibile in tutto il codice di esempio.

Effettuando alcune aggiunte alla funzione di caricamento in modo che chiami questa funzione, i documenti Word caricati dagli utenti vengono ora convertiti automaticamente in PDF.

Il codice seguente crea il percorso di destinazione per il PDF convertito e avvia la conversione:

```
const documentFolder = path.join(__dirname, "../docs");
var extPosition = req.files.source.name.lastIndexOf('.') - 1;
if(extPosition < 0 ) {
  extPosition = req.files.source.name.length
}
const destinationName = path.join(documentFolder,  
  req.files.source.name.substring(0, extPosition) + '.pdf');
console.log(destinationName);
 
logger.info('converting to ${destinationName}')
  convertDocumentToPDF(uploadPath, destinationName);
```

## Conversione di altri tipi di file in PDF

Il document toolkit converte in PDF altri formati, ad esempio static HTML, un altro tipo di documento comune. Il toolkit accetta i documenti di HTML compilati come file .zip con tutte le risorse a cui fa riferimento il documento (file CSS, immagini e altri file) nello stesso file .zip. Il documento di HTML stesso deve essere denominato index.html e inserito nella cartella principale del file .zip.

Per convertire un file .zip contenente HTML, utilizzate il codice seguente:

```
//Create an HTML to PDF operation and provide the source file to it
htmlToPDFOperation = PDFToolsSdk.CreatePDF.Operation.createNew();     
const input = PDFToolsSdk.FileRef.createFromLocalFile(sourceZipFile);
htmlToPDFOperation.setInput(input);
 
// custom function for setting options
setCustomOptions(htmlToPDFOperation);
 
// Execute the operation and Save the result to the specified location.
htmlToPDFOperation.execute(executionContext)
  .then(result => result.saveAsFile(destinationPdfFile))
  .catch(err => {
    logger.error('Exception encountered while executing operation');
});
```

La funzione `setCustomOptions` specifica altre impostazioni di PDF, ad esempio le dimensioni della pagina. Qui, la funzione imposta le dimensioni della pagina su 11,5 x 11 pollici:

```
const setCustomOptions = (htmlToPDFOperation) => {    
  const pageLayout = new PDFToolsSdk.CreatePDF.options.PageLayout();
  pageLayout.setPageSize(11.5, 8);

  const htmlToPdfOptions = 
    new PDFToolsSdk.CreatePDF.options.html.CreatePDFFromHtmlOptions.Builder()
    .includesHeaderFooter(true)
    .withPageLayout(pageLayout)
    .build();
  htmlToPDFOperation.setOptions(htmlToPdfOptions);
};
```

Quando si apre un documento HTML contenente alcuni termini, nel browser viene visualizzato quanto segue:

![Immagine di termini informatici](assets/AWNjs_3.png)

L&#39;origine di questo documento è composta da un file CSS e da un file HTML:

![Immagine del file CSS e HTML](assets/AWNjs_4.png)

Dopo aver elaborato il file HTML, lo stesso testo è disponibile in formato PDF:

![File PDF dei termini del computer](assets/AWNjs_5.png)

## Aggiunta di pagine

Un&#39;altra operazione comune con i file PDF consiste nell&#39;aggiungere alla fine delle pagine che possono contenere del testo standard, ad esempio un elenco di termini. Il kit di strumenti del documento può combinare più documenti PDF in un unico documento. Se disponi di un elenco di percorsi di documento (qui in `sourceFileList`), è possibile aggiungere i riferimenti dei file di ogni file a un oggetto per un&#39;operazione di combinazione.

Quando l&#39;operazione di combinazione viene eseguita, viene fornito un singolo file con una parte del contenuto sorgente. È possibile utilizzare `saveAsFile` sull&#39;oggetto per mantenere il file nell&#39;archiviazione.

```
const executionContext = PDFToolsSDK.ExecutionContext.create(credentials);
var combineOperation = PDFToolsSDK.CombineFiles.Operation.createNew();
 
sourceFileList.forEach(f => {
  var combinedSource = PDFToolsSDK.FileRef.createFromLocalFile(f);
  console.log(f);
  combineOperation.addInput(combinedSource);
});
    
 
combineOperation.execute(executionContext)
  .then(result=>result.saveAsFile(destinationFile))
  .catch(err => {
    logger.error(err.message);
});    
```

## Visualizzazione dei documenti PDF

Hai eseguito diverse operazioni sui file PDF, ma in ultima analisi, l&#39;utente deve visualizzare i documenti. Potete incorporare il documento in una pagina Web utilizzando l’API di incorporamento PDF di Adobe.

Nella pagina che visualizza il PDF, aggiungete un `<div />` per contenere il documento e assegnarvi un ID. A breve userai questo ID. Nella pagina Web, includere un `<script />` riferimento alla libreria JavaScript di Adobe:

```
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

L&#39;ultimo bit di codice necessario è una funzione che visualizza il documento una volta caricata l&#39;API JavaScript di Adobe PDF Embed. Quando si riceve la notifica che lo script viene caricato mediante un evento adobe_dc_view\_sdk.ready, creare un nuovo oggetto AdobeDC.View. Questo oggetto richiede l&#39;ID client e l&#39;ID dell&#39;elemento creato in precedenza. Trova il tuo ID client nel [Console Adobe Developer](https://console.adobe.io/). Quando visualizzate le impostazioni dell’applicazione creata durante la generazione delle credenziali precedenti, l’ID client viene visualizzato.

![Immagine della chiave client API](assets/AWNjs_6.png)

## Altre opzioni di PDF

Il [Dimostrazione delle API di Adobe PDF Embed](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf) consente di visualizzare in anteprima le varie opzioni per incorporare i documenti PDF.

![Immagine delle opzioni di incorporamento di PDF ](assets/AWNjs_7.png)

Potete attivare e disattivare varie opzioni e vedere immediatamente il rendering. Quando trovate una combinazione che vi piace, fate clic sul *\&lt;/\> Genera codice* per generare il codice HTML effettivo utilizzando queste opzioni.

![Immagine di Anteprima codice](assets/AWNjs_8.png)

## Aggiunta di firme digitali e protezione

Una volta che un documento è pronto, puoi aggiungere firme digitali per l’approvazione utilizzando Adobe Sign. Questa funzionalità funziona in modo leggermente diverso rispetto alla funzionalità utilizzata finora. Per le firme digitali, un’applicazione deve essere configurata per utilizzare OAuth per l’autenticazione dell’utente.

Il primo passo nella configurazione dell&#39;applicazione consiste nel [registrare la domanda](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/gstarted/create_app.md) per utilizzare OAuth per Adobe Sign. Una volta effettuato l’accesso, accedi alla schermata per creare le applicazioni facendo clic su *Account*, quindi aprire il *API Adobe Sign* e fai clic su *Applicazioni API* per aprire l&#39;elenco delle applicazioni registrate.

![Immagine del primo passaggio della registrazione dell&#39;applicazione](assets/AWNjs_9.png)

Per creare una nuova voce dell&#39;applicazione, fate clic sull&#39;icona più (+) nell&#39;angolo in alto a destra.

![Immagine dell&#39;icona più nell&#39;angolo superiore destro dello schermo](assets/AWNjs_10.png)

Nella finestra che si apre, immettete un nome applicazione e un nome visualizzato. Seleziona *Cliente* per il dominio, quindi fai clic su *Salva*.

![Immagine di dove immettere il nome dell&#39;applicazione e il nome di visualizzazione](assets/AWNjs_11.png)

Dopo aver creato l&#39;applicazione, potete selezionarla dall&#39;elenco e fare clic su *Configurare OAuth per l’applicazione*. Selezionate le opzioni desiderate. Nell’URL di reindirizzamento, immettete l’URL dell’applicazione. È possibile immettere più URL qui. Per l&#39;applicazione che state testando, il valore è:

```
http://localhost:3000/signed-in 
```

Il processo di utilizzo di OAuth per ottenere un token è standard. L&#39;applicazione indirizza un utente a un URL per l&#39;accesso. Una volta effettuato l&#39;accesso, l&#39;utente viene reindirizzato all&#39;applicazione con ulteriori informazioni nei parametri di query della pagina.

Per l&#39;URL di accesso, l&#39;applicazione deve passare l&#39;ID client, l&#39;URL di reindirizzamento e un elenco degli ambiti necessari.

Il pattern dell&#39;URL è simile al seguente:

```
https://secure.adobesign.com/public/oauth?
  redirect_uri=&
  response_type=code&
  client_id=&
  scope=
```

All&#39;utente viene richiesto di accedere al proprio ID per Adobe Sign. Dopo aver effettuato l’accesso, viene chiesto se devono fornire autorizzazioni all’applicazione.

![Immagine della schermata di conferma dell’accesso](assets/AWNjs_12.png)

Se l&#39;utente fa clic su *Consenti accesso* nell&#39;URL di reindirizzamento, un parametro di query denominato code passa il codice di autorizzazione:

https://YourServer.com/?code=**\&lt;authorization_code>**\&amp;api_access_point=https://api.adobesign.com&amp;web_access_point=https://secure.adobesign.com

L&#39;invio di questo codice al server Adobe Sign insieme all&#39;ID client e al segreto client fornisce un token di accesso per accedere al servizio. Salvare i valori nei parametri `api_access_point` e `web_access_point`. Questi valori vengono utilizzati per ulteriori richieste.

```
var requestURL = ' ${api_access_point}oauth/token?code=${code}'
  +'&client_id=${client_id}'
  +'&client_secret=${client_secret}&'
  +'redirect_uri=${redirect_url}&'
  +'grant_type=authorization_code';
request.post(requestURL, {form: { }
}, (err,response,body)=>{                
    var token_response = JSON.parse(body)
    var access_token = token_response.access_token;
    console.log(access_token);
});
```

Quando un documento richiede una firma, il documento deve prima essere caricato. L&#39;applicazione può caricare il documento sul `api_access_point` valore ricevuto durante la richiesta del token OAUTH. Il punto finale è `/api/rest/v6/transientDocuments`. I dati di richiesta sono i seguenti:

```
POST /api/rest/v6/transientDocuments HTTP/1.1
Host: api.na1.adobesign.com
Authorization: Bearer MvyABjNotARealTokenHkYyi
Content-Type: multipart/form-data
Content-Disposition: form-data; name=";File"; filename="MyPDF.pdf"
<PDF CONTENT>
```

All&#39;interno dell&#39;applicazione, create la richiesta con il codice seguente:

```
var uploadRequest = {
  'method': 'POST',
  'url': '${oauthParameters.signin_domain}/api/rest/v6/transientDocuments',
  'headers': {
    'Authorization': 'Bearer  ${auth_token}'
  },
  formData: {
    'File': {
      'value': fs.createReadStream(documentPath),
      'options': {
        'filename': fileName,
        'contentType': null
      }
    }
  }
};
 
request(uploadRequest, (error, response) => {
  if (error) throw new Error(error);
  var jsonResponse = JSON.parse(response.body);
  var transientDocumentId = jsonResponse.transientDocumentId;
  logger.info('transientDocumentId:', transientDocumentId)
});
```

La richiesta restituisce un `transientID` valore. Il documento è stato caricato ma non è ancora stato inviato. Per inviare il documento, utilizzare il metodo `transientID` per richiedere di inviare il documento.

Iniziare creando un oggetto JSON contenente le informazioni per il documento da firmare. Nell&#39;esempio seguente, la variabile `transientDocumentId` contiene l&#39;ID del codice precedente e `agreementDescription` contiene il testo che descrive l’accordo che deve essere firmato. Le persone da firmare sono elencate in `participantSetsInfo` per indirizzo e-mail e ruolo.

```
var requestBody = {
  "fileInfos":[
    {"transientDocumentId":transientDocumentId}],
    "name":agreementDescription,
    "participantSetsInfo":[
      {"memberInfos":[{"email":"user@domain.com"}],
       "order":1,"role":"SIGNER"}
    ],
    "signatureType":"ESIGN","state":"IN_PROCESS"
};
```

L’invio di questa richiesta Web crea la richiesta di firma e restituisce un oggetto JSON con un ID per la richiesta di accordo:

```
request(requestBody, function (error, response) {
  if (error) throw new Error(error);
  var JSONResponse = JSON.parse(response.body);
  var requestId = JSONResponse.id;
});
```

Se i firmatari dimenticano di firmare e hanno bisogno di un altro messaggio e-mail di notifica, invialo nuovamente utilizzando l’ID ricevuto in precedenza. L’unica differenza è che devi anche aggiungere gli ID dei partecipanti delle parti. Puoi ottenere gli ID dei partecipanti inviando una richiesta di GET a `/agreements/{agreementID}/members`.

Per richiedere di inviare il promemoria, create prima un oggetto JSON che descriva la richiesta. L&#39;oggetto minimo richiede un elenco degli ID dei partecipanti e uno stato per il promemoria (&quot;ACTIVE&quot;, &quot;COMPLETE&quot; o &quot;CANCELED&quot;).

Facoltativamente, la richiesta può contenere ulteriori informazioni, ad esempio un valore per &quot;nota&quot; che verrà visualizzato all&#39;utente. Oppure, un ritardo (in ore) di aspettare fino all&#39;invio del promemoria (in `firstReminderDelay`) e una frequenza di promemoria (nel campo &quot;frequency&quot;), che accetta valori quali DAILY_UNTIL_SIGNED, EVERY_THIRD_DAY_UNTIL_SIGNED o WEEKLY_UNTIL_SIGNED.

```
var requestBody = {
  //participantList is an array of participant ID strings
  "recipientParticipantIds":participantList
  ,"status":"ACTIVE",
  "note":"This is a reminder to sign out important agreement."
}
 
var reminderRequest = {
  'method': 'POST',
  'url': '${oauthParameters.signin_domain}/api/rest/v6/agreements/${agreementID}/reminders',
  'headers': {
    'Authorization': `Bearer ${access_token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(requestBody)
 
};

request(reminderRequest, function (error, response) {
});
```

Ed è tutto ciò che serve per inviare una richiesta di promemoria.

![Immagine del modulo Web](assets/AWNjs_13.png)

## Creazione di moduli Web

È inoltre possibile utilizzare l’API Adobe Sign per creare moduli Web. I moduli Web consentono di incorporare un modulo in una pagina Web o di collegarlo direttamente. Una volta creato, un modulo Web viene visualizzato anche tra i moduli Web della console Adobe Sign. È possibile creare moduli Web con stato BOZZA per la creazione incrementale, lo stato AUTHORING per la modifica dei campi modulo Web e lo stato ATTIVO per ospitare immediatamente il modulo.

![Immagine del modulo Web nella schermata Gestisci di Adobe Sign](assets/AWNjs_14.png)

Per creare un modulo Web, utilizzare il modulo `transientDocumentId`. Stabilire un titolo per il modulo e uno stato per inizializzarlo.

```
var requestBody = {
  "fileInfos": [
    {
      "transientDocumentId": transientDocumentId
    }
  ],
  "name": webFormTitle,
  "state": status,
  "widgetParticipantSetInfo": {
    "memberInfos": [ { "email": "" } ],
    "role": "SIGNER"
  }
}
```

```
var createWebFormRequest = {
  'method': 'POST',
  'url': `${oauthParameters.signin_domain}/api/rest/v6/widgets`,
  'headers': {
    'Authorization': `Bearer ${access_token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(requestBody)
}
```

```
request(createWebFormRequest, function (error, response) {
  var jsonResp = JSON.parse(response.body);
  var webFormID = jsonResp.id;
});
```

Ora puoi incorporare o collegare il tuo documento.

## Fasi seguenti

Come puoi vedere dalle Guide rapide e dal codice fornito, è facile implementare processi di approvazione dei documenti digitali e di PDF utilizzando Node con il [!DNL Adobe Acrobat Services] API. Le API di Adobe si integrano perfettamente nelle applicazioni client esistenti.

Per scoprire gli ambiti richiesti per una chiamata o per vedere come viene creata la chiamata, è possibile creare chiamate di esempio dal metodo [Documentazione delle API di riposo](https://secure.na4.adobesign.com/public/docs/restapi/v6). Il [QuickStart](https://github.com/adobe/pdftools-node-sdk-samples) dimostrare anche altre funzionalità e altri formati di file [!DNL Adobe Acrobat Services] processi API.

Puoi aggiungere numerose funzionalità di PDF alle tue applicazioni, consentendo agli utenti di visualizzare e firmare i documenti in modo semplice e veloce e molto altro ancora. Per iniziare, consultate [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/) oggi.
