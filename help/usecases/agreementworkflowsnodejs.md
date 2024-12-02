---
title: Flussi di lavoro per accordi in Node.js
description: Le API [!DNL Adobe Acrobat Services] integrano facilmente le funzionalità PDF nelle applicazioni Web
feature: Use Cases
role: Developer
level: Beginner
type: Tutorial
jira: KT-7473
thumbnail: KT-7473.jpg
keywords: In evidenza
exl-id: 44a03420-e963-472b-aeb8-290422c8d767
source-git-commit: f8a31b8f98d99bf1f3787e0f0f19cc9f26e24d8d
workflow-type: tm+mt
source-wordcount: '2094'
ht-degree: 0%

---

# Flussi di lavoro degli accordi in Node.js

![Banner dell&#39;eroe del caso di utilizzo](assets/UseCaseAgreementHero.jpg)

Molte applicazioni e processi aziendali richiedono documentazione come proposte e accordi. Con i documenti PDF i file risultano più sicuri e meno modificabili. Offre inoltre il supporto per la firma digitale, per consentire ai clienti di completare i documenti in modo semplice e veloce. Le API [!DNL Adobe Acrobat Services] integrano facilmente le funzionalità PDF nelle applicazioni Web.

## Cosa puoi imparare

In questo tutorial pratico, scopri come aggiungere servizi PDF a un’applicazione Node.js per digitalizzare un processo di accordo.

## API e risorse pertinenti

* [API dei servizi PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [API di incorporamento PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [API Adobe Sign](https://www.adobe.io/apis/documentcloud/sign.html)

* [Codice progetto](https://github.com/adobe/pdftools-node-sdk-samples)

## Configurazione di [!DNL Adobe Acrobat Services]

Per iniziare, configurare le credenziali per l&#39;utilizzo di [!DNL Adobe Acrobat Services]. Registra un account e utilizza [Node.js Quickstart](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#node-js) per verificare il funzionamento delle credenziali prima di integrare la funzionalità in un&#39;applicazione di grandi dimensioni.

Per prima cosa, ottieni un account sviluppatore Adobe. Quindi, nella pagina [Introduzione](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html?ref=getStartedWithServicesSDK), seleziona l&#39;opzione *Introduzione* in Crea nuove credenziali. Puoi registrarti per la versione di prova gratuita che fornisce 1.000 transazioni di documenti che possono essere utilizzati in sei mesi.

![Immagine della creazione di nuove credenziali](assets/AWNjs_1.png)

Nella pagina Creazione di nuove credenziali seguente viene richiesto di scegliere tra l&#39;API PDF Embed e l&#39;API PDF Services.

Selezionare *API Servizi PDF*.

Immettere un nome per l&#39;applicazione e selezionare la casella *Crea codice personalizzato di esempio*. Selezionando questa casella, le credenziali vengono incorporate automaticamente nell&#39;esempio di codice. Se si lascia deselezionata questa casella, è necessario aggiungere manualmente le credenziali all&#39;applicazione.

Seleziona *Node.js* per il tipo di applicazione e fai clic su *Crea credenziali*.

Pochi istanti dopo, un file .zip inizia a essere scaricato con un progetto di esempio che include le tue credenziali. Il pacchetto Node.js per [!DNL Acrobat Services] è già incluso nel codice del progetto di esempio.

![Immagine della selezione delle credenziali API di PDF Services](assets/AWNjs_2.png)

## Configurazione manuale del progetto di esempio

Se scegliete di non scaricare un progetto di esempio dalla pagina Crea nuove credenziali, potete anche impostare il progetto manualmente.

Scarica il codice (senza credenziali incorporate) da [GitHub](https://github.com/adobe/pdftools-node-sdk-samples). Se si utilizza questa versione del codice, è necessario aggiungere le credenziali al file pdftools-api-credentials.json prima di utilizzare:

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

Per la propria applicazione, è necessario copiare il file della chiave privata e i file delle credenziali nell&#39;origine dell&#39;applicazione.

È necessario installare il pacchetto Node.js per [!DNL Acrobat Services]. Per installare il pacchetto, utilizza il comando seguente:

```
npm install --save @adobe/documentservices-pdftools-node-sdk
```

## Impostazione della registrazione

In questi esempi viene utilizzato Express per il framework dell&#39;applicazione. e log4js per la registrazione delle applicazioni. Con log4js, potete indirizzare facilmente l&#39;accesso alla console o a un file:

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

Il codice riportato sopra scrive i dati registrati in un file in ./logs/applicationlog.txt. Se invece desideri che scriva sulla console, puoi commentare la chiamata su log4js.configure.

## Conversione di file Word in PDF

Gli accordi e le proposte vengono spesso scritti in un’applicazione di produttività, come Microsoft Word. Per accettare documenti in questo formato e convertire il documento in PDF, è possibile aggiungere funzionalità per l&#39;applicazione. Diamo un&#39;occhiata a come caricare e salvare un documento in un&#39;applicazione Express e salvarlo nel file system.

Nel HTML dell’applicazione, aggiungi un elemento file e un pulsante per avviare il caricamento:

```
<input type="file" name="source" id="source" />
<button onclick="upload()" >Upload</button>
```

In JavaScript della pagina, caricare il file in modo asincrono utilizzando la funzione di recupero:

```
function upload()
{
  let formData = new FormData();
  var selectedFile = document.getElementById('source').files[0];
  formData.append("source", selectedFile);
  fetch('documentUpload', {method:"POST", body:formData});
}
```

Scegli una cartella per accettare i file caricati. L&#39;applicazione richiede un percorso per questa cartella. Trovare il percorso assoluto utilizzando un percorso relativo unito a \_\_dirname:

```
const uploadFolder = path.join(__dirname, "../uploads");
```

Poiché il file viene inviato tramite post, è necessario rispondere a un messaggio di post sul lato server:

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

Dopo l&#39;esecuzione di questa funzione, il file viene salvato nella cartella di caricamento delle applicazioni ed è disponibile per ulteriori elaborazioni.

Quindi, converti il file dal suo formato nativo in PDF. Il codice di esempio scaricato in precedenza contiene uno script denominato `create-pdf-from-docx.js` per la conversione di un documento in PDF. La funzione seguente, `convertDocumentToPDF`, accetta un documento caricato e lo converte in un PDF in un&#39;altra cartella:

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

Con il codice è possibile osservare uno schema generale:

Il codice crea un oggetto credenziali e un contesto di esecuzione, inizializza alcune operazioni, quindi esegue l&#39;operazione con il contesto di esecuzione. Potete vedere questo pattern in tutto il codice di esempio.

Aggiungendo alcune aggiunte alla funzione di caricamento in modo da richiamarla, i documenti di Word caricati dagli utenti vengono ora automaticamente convertiti in PDF.

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

Document Toolkit converte altri formati in PDF, ad esempio static HTML, un altro tipo di documento comune. Il toolkit accetta documenti HTML inseriti in un pacchetto come file .zip contenente tutte le risorse a cui fa riferimento il documento (file CSS, immagini e altri file) nello stesso file .zip. Il documento HTML stesso deve essere denominato index.html e inserito nella directory principale del file .zip.

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

La funzione `setCustomOptions` specifica altre impostazioni di PDF, ad esempio le dimensioni della pagina. Qui puoi vedere come la funzione imposta le dimensioni di pagina su 11,5 x 11 pollici:

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

All’apertura di un documento HTML che contiene alcuni termini, nel browser viene visualizzato quanto segue:

![Immagine dei termini per il computer](assets/AWNjs_3.png)

L&#39;origine di questo documento è composta da un file CSS e da un file HTML:

![Immagine del file CSS e HTML](assets/AWNjs_4.png)

Dopo l’elaborazione del file HTML, il testo in formato PDF sarà lo stesso:

![File PDF delle Condizioni per l&#39;utilizzo del computer](assets/AWNjs_5.png)

## Aggiunta di pagine

Un’altra operazione comune con i file PDF è aggiungere alla fine delle pagine che possono contenere testo standard, ad esempio un elenco di termini. Il toolkit per documenti può combinare più documenti PDF in un unico documento. Se si dispone di un elenco di percorsi di documento (qui in `sourceFileList`), è possibile aggiungere i riferimenti dei file di ogni file a un oggetto per un&#39;operazione di combinazione.

Quando viene eseguita l’operazione di combinazione, viene fornito un singolo file con il contenuto di origine. È possibile utilizzare `saveAsFile` sull&#39;oggetto per rendere permanente il file nell&#39;archiviazione.

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

## Visualizzazione di documenti PDF

Hai eseguito diverse operazioni sui file PDF, ma alla fine, l&#39;utente deve visualizzare i documenti. Puoi incorporare il documento in una pagina Web utilizzando l’API PDF Embed di Adobe.

Nella pagina che visualizza il PDF, aggiungi un elemento `<div />` per contenere il documento e assegnargli un ID. Questo ID verrà utilizzato a breve. Nella pagina Web, includi un riferimento `<script />` alla libreria JavaScript di Adobe:

```
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

L’ultimo bit di codice necessario è una funzione che visualizza il documento una volta caricato l’API JavaScript di Adobe PDF Embed. Quando ricevete una notifica che avvisa che lo script è stato caricato tramite un evento adobe_dc_view\_sdk.ready, create un nuovo oggetto AdobeDC.View. Questo oggetto richiede l&#39;ID client e l&#39;ID dell&#39;elemento creato in precedenza. Trova il tuo ID client in [Adobe Developer Console](https://console.adobe.io/). Quando si visualizzano le impostazioni per l&#39;applicazione creata durante la generazione delle credenziali precedenti, viene visualizzato l&#39;ID client.

![Immagine della chiave del client API](assets/AWNjs_6.png)

## Altre opzioni PDF

La demo [API Adobe PDF Embed](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf) ti consente di visualizzare in anteprima le varie altre opzioni per incorporare i documenti PDF.

![Immagine delle opzioni di incorporamento di PDF ](assets/AWNjs_7.png)

Puoi attivare e disattivare varie opzioni e vedere immediatamente come vengono eseguite. Quando trovi una combinazione che ti piace, fai clic sul pulsante *\&lt;/\> Genera codice* per generare il codice HTML effettivo utilizzando tali opzioni.

![Immagine dell&#39;anteprima del codice](assets/AWNjs_8.png)

## Aggiunta di firme digitali e sicurezza

Una volta che il documento è pronto, è possibile aggiungervi firme digitali per l’approvazione utilizzando Adobe Sign. Questa funzionalità funziona in modo leggermente diverso rispetto a quella utilizzata finora. Per le firme digitali, un’applicazione deve essere configurata per utilizzare OAuth per l’autenticazione dell’utente.

Il primo passaggio della configurazione dell&#39;applicazione è [registrare l&#39;applicazione](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/gstarted/create_app.md) per utilizzare OAuth per Adobe Sign. Una volta effettuato l&#39;accesso, accedi alla schermata per la creazione di applicazioni facendo clic su *Account*, quindi apri la sezione *API Adobe Sign* e fai clic su *Applicazioni API* per aprire l&#39;elenco delle applicazioni registrate.

![Immagine del primo passaggio della registrazione dell&#39;applicazione](assets/AWNjs_9.png)

Per creare una nuova voce dell’applicazione, fai clic sull’icona più in alto a destra.

![Immagine dell&#39;icona più nell&#39;angolo superiore destro dello schermo](assets/AWNjs_10.png)

Nella finestra visualizzata, immetti il nome dell’applicazione e il nome visualizzato. Seleziona *Cliente* per il dominio, quindi fai clic su *Salva*.

![Immagine della posizione in cui immettere il nome dell&#39;applicazione e il nome visualizzato](assets/AWNjs_11.png)

Dopo aver creato l’applicazione, puoi selezionarla dall’elenco e fare clic su *Configura OAuth per l’applicazione*. Seleziona le opzioni. Nell’URL di reindirizzamento, immetti l’URL dell’applicazione. Qui puoi inserire più URL. Per l&#39;applicazione che state testando, il valore è:

```
http://localhost:3000/signed-in 
```

Il processo di utilizzo di OAuth per ottenere un token è standard. L’applicazione indirizza un utente a un URL per l’accesso. Dopo che l&#39;utente ha effettuato correttamente l&#39;accesso,
vengono reindirizzati all&#39;applicazione con informazioni aggiuntive nei parametri di query della pagina.

Per l’URL di accesso, l’applicazione deve passare l’ID client, l’URL di reindirizzamento e un elenco degli ambiti necessari.

Il modello per l&#39;URL è simile al seguente:

```
https://secure.adobesign.com/public/oauth?
  redirect_uri=&
  response_type=code&
  client_id=&
  scope=
```

All’utente viene richiesto di accedere al proprio ID per Adobe Sign. Dopo aver effettuato l’accesso, viene richiesto se fornire le autorizzazioni all’applicazione.

![Immagine della schermata di conferma dell&#39;accesso](assets/AWNjs_12.png)

Se l&#39;utente fa clic su *Consenti accesso* nell&#39;URL di reindirizzamento, un parametro di query denominato codice passa il codice di autorizzazione:

https://YourServer.com/?code=**\&lt;codice_autorizzazione\>**\&amp;api_access_point=https://api.adobesign.com&amp;web_access_point=https://secure.adobesign.com

La pubblicazione di questo codice sul server Adobe Sign insieme all’ID client e al segreto client fornisce un token di accesso per accedere al servizio. Salvare i valori nei parametri `api_access_point` e `web_access_point`. Questi valori vengono utilizzati per ulteriori richieste.

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

Quando un documento richiede una firma, è necessario prima caricarlo. L’applicazione può caricare il documento nel valore `api_access_point` ricevuto durante la richiesta del token OAUTH. L&#39;endpoint è `/api/rest/v6/transientDocuments`. I dati della richiesta hanno un aspetto simile al seguente:

```
POST /api/rest/v6/transientDocuments HTTP/1.1
Host: api.na1.adobesign.com
Authorization: Bearer MvyABjNotARealTokenHkYyi
Content-Type: multipart/form-data
Content-Disposition: form-data; name=";File"; filename="MyPDF.pdf"
<PDF CONTENT>
```

All’interno dell’applicazione, genera la richiesta con il codice seguente:

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

La richiesta restituisce un valore `transientID`. Il documento è stato caricato, ma non è ancora stato inviato. Per inviare il documento, utilizzare `transientID` per richiedere l&#39;invio del documento.

Inizia creando un oggetto JSON contenente le informazioni per il documento da firmare. Di seguito, la variabile `transientDocumentId` contiene l’ID del codice precedente e `agreementDescription` il testo che descrive l’accordo che deve essere firmato. Le persone che dovranno firmare il documento sono elencate in `participantSetsInfo` in base all&#39;indirizzo e-mail e al ruolo.

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

Se i firmatari dimenticano di firmare e hanno bisogno di un’altra e-mail di notifica, invia nuovamente le notifiche utilizzando l’ID ricevuto in precedenza. L’unica differenza è che devi aggiungere anche gli ID partecipanti delle parti. È possibile ottenere gli ID dei partecipanti inviando una richiesta GET a `/agreements/{agreementID}/members`.

Per richiedere l’invio del promemoria, crea prima un oggetto JSON che descriva la richiesta. L’oggetto minimo richiede un elenco degli ID partecipanti e uno stato per il promemoria (&quot;ATTIVO&quot;, &quot;COMPLETO&quot; o &quot;ANNULLATO&quot;).

La richiesta può facoltativamente contenere informazioni aggiuntive, ad esempio un valore per &quot;nota&quot; che verrà visualizzato all&#39;utente. Oppure, un ritardo (in ore) nell&#39;attesa dell&#39;invio del promemoria (in `firstReminderDelay`) e una frequenza di promemoria (nel campo &quot;frequenza&quot;), che accetta valori quali DAILY_Until_SIGNED, EVERY_THIRD_DAY_Until_SIGNED o WEEKLY_Until_SIGNED.

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

È tutto ciò che serve per inviare una richiesta di promemoria.

![Immagine del modulo Web](assets/AWNjs_13.png)

## Creazione di moduli Web

Per creare moduli Web, puoi anche utilizzare l’API di Adobe Sign. I moduli Web consentono di incorporare un modulo in una pagina Web o di collegarlo direttamente a essa. Una volta creato, un modulo Web viene visualizzato anche tra i moduli Web nella console di Adobe Sign. È possibile creare moduli Web con stato BOZZA per la compilazione incrementale, stato AUTHORING per la modifica dei campi del modulo Web e stato ACTIVE per ospitare immediatamente il modulo.

![Immagine del modulo Web nella schermata Gestisci di Adobe Sign](assets/AWNjs_14.png)

Per creare un modulo Web, utilizza il modulo `transientDocumentId`. Stabilire un titolo per il modulo e uno stato per inizializzarlo.

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

Ora potete incorporare o collegare il vostro documento.

## Fasi seguenti

Come si può vedere dalla procedura di avvio rapido e dal codice fornito, è facile implementare i processi di approvazione dei documenti digitali e PDF utilizzando Nodo con le API [!DNL Adobe Acrobat Services]. Le API di Adobe si integrano perfettamente nelle applicazioni client esistenti.

Per individuare gli ambiti necessari per una chiamata o per vedere come viene compilata la chiamata, è possibile creare chiamate di esempio dalla [documentazione API REST](https://secure.na4.adobesign.com/public/docs/restapi/v6). In [Quickstart](https://github.com/adobe/pdftools-node-sdk-samples) sono inoltre disponibili altre funzionalità e formati di file per i processi delle API [!DNL Adobe Acrobat Services].

Puoi aggiungere numerose funzionalità di PDF alle tue applicazioni, consentendo agli utenti di visualizzare e firmare in modo facile e veloce i documenti e molto altro ancora. Per iniziare, controlla [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/) oggi.
