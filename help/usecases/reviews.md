---
title: Revisioni e approvazioni
description: Scopri come creare un flusso di lavoro di revisione e approvazione dei documenti per la collaborazione tra team
type: Tutorial
role: Developer
level: Intermediate
thumbnail: KT-8094.jpg
jira: KT-8094
exl-id: d704620f-d06a-4714-9d09-3624ac0fcd3a
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1623'
ht-degree: 0%

---

# Revisioni e approvazioni

![Usa banner eroe caso](assets/UseCaseReviewsHero.jpg)

Durante la pandemia di COVID-19, si è resa necessaria la collaborazione remota tra team per molte aziende, [condivisione e revisione di documenti digitali](https://www.adobe.io/apis/documentcloud/dcsdk/review-and-approval.html) presenta una serie di sfide per i team e risorse interfunzionali.

Questi problemi includono la condivisione di documenti in diversi formati di file, la revisione e l&#39;inserimento di commenti efficaci sui contenuti e la sincronizzazione con le modifiche più recenti. [!DNL Adobe Acrobat Services] Le API sono progettate per consentire agli sviluppatori di applicazioni di risolvere questi problemi per i loro utenti.

## Cosa puoi imparare

Questa esercitazione pratica mostra come creare un flusso di lavoro di revisione e approvazione dei documenti in un&#39;applicazione Web Node.js e Express. Per seguire questa esercitazione, è necessario disporre di una certa esperienza con Node.js.

L&#39;applicazione dispone delle seguenti funzioni:

* Convertire diversi tipi di file in PDF

* Abilitare il caricamento dei file

* Offri agli utenti la possibilità di aggiungere commenti e annotazioni

* Visualizzare i PDF con questi commenti

* Abilitare i profili utente per identificare gli autori dei commenti

* Combinare più file in un PDF finale che gli utenti possono scaricare

## API e risorse pertinenti

* [API dei servizi PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [API di incorporamento PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Codice progetto](https://github.com/contentlab-io/adobe_reviews_and_approvals)

## Creazione di credenziali API di Adobe

Prima di avviare il codice, è necessario [creare le credenziali](https://www.adobe.com/go/dcsdks_credentials) per Adobe PDF Embed API e Adobe PDF Services API. L’API di incorporamento PDF è gratuita. L’API dei servizi PDF è gratuita per sei mesi, quindi puoi passare a un [piano tariffario prepagato](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) a soli \$0,05 per transazione documento.

Quando si creano le credenziali per l&#39;API dei servizi PDF, selezionare la proprietà **Creare un esempio di codice personalizzato** e selezionare Node.js per la lingua. Salva il file ZIP ed estrai pdftools-api-credentials.json e private.key nella directory principale del progetto Node.js Express.

## Impostazione di un progetto e dipendenze

Configura il progetto Node.js ed Express per gestire i file statici da una cartella denominata &quot;public&quot;. Potete impostare le modalità di progetto, a seconda delle preferenze. Per iniziare a lavorare rapidamente, è possibile utilizzare il metodo [Generatore di app Express](https://expressjs.com/en/starter/generator.html). O se vuoi semplificare le cose, puoi [ripartire da zero](https://expressjs.com/en/starter/hello-world.html) e mantenere il codice in un unico file JavaScript. Nel progetto di esempio collegato sopra, si utilizza l&#39;approccio &quot;one-file&quot; e si mantiene tutto il codice in index.js.

Copia il `pdftools-api-credentials.json` e `private.key` dall&#39;esempio di codice personalizzato alla directory principale del progetto. Inoltre, aggiungeteli al file .gitignore, se ne disponete, in modo che i file delle credenziali non vengano inviati accidentalmente a un repository.

Avanti, esegui `npm install @adobe/documentservices-pdftools-node-sdk` per installare Node.js SDK per i servizi PDF. Importa questo modulo e crea l&#39;oggetto credenziali API all&#39;interno del codice (index.js nel progetto di esempio), dopo che il resto della dipendenza viene importato come segue:

```
  const PDFToolsSdk = require( "@adobe/documentservices-pdftools-node-sdk" );

  // Create Credentials
  const credentials =  PDFToolsSdk.Credentials
      .serviceAccountCredentialsBuilder()
      .fromFile( "pdftools-api-credentials.json" )
      .build();
```

Il codice iniziale dovrebbe essere simile al seguente:

```
  
  const express = require( "express" );
  const PDFToolsSdk = require( "@adobe/documentservices-pdftools-node-sdk" );

  // Create Credentials
  const credentials =  PDFToolsSdk.Credentials
      .serviceAccountCredentialsBuilder()
      .fromFile( "pdftools-api-credentials.json" )
      .build();

  const app = express();

  app.use( express.static( "public" ) );

  app.listen( 8889, function() {
      console.log( "Server started on port", 8889 );
  } );
```

Ora puoi lavorare con [!DNL Acrobat Services] API.

## Conversione di un file in PDF

Per la prima parte del flusso di lavoro del documento, l’utente finale deve caricare i documenti da condividere. Per attivarla, è necessario aggiungere una funzione di caricamento e consolidare i diversi formati di file del documento in PDF per prepararli al processo di revisione.

Iniziare creando una funzione per convertire i documenti in PDF in base alla proprietà [snippet di esempio per l’API dei servizi PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html). Questo esempio mostra anche gli snippet per molte altre caratteristiche vitali, tra cui il riconoscimento ottico dei caratteri (OCR), la protezione e rimozione della password e la compressione.

```
function fileToPDF( filename, outputFilename, callback ) {
      // Create an ExecutionContext using credentials and create a new operation
  instance.
      const executionContext = PDFToolsSdk.ExecutionContext.create( credentials ),
          createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();

      // Set operation input from a source file.
      const input = PDFToolsSdk.FileRef.createFromLocalFile( filename );
      createPdfOperation.setInput( input );

      // Execute the operation and Save the result to the specified location.
      createPdfOperation.execute( executionContext )
          .then( result => {
              result.saveAsFile( outputFilename );
              callback( outputFilename );
          } );
  }
```

È ora possibile utilizzare questa funzione per creare PDF da documenti caricati.

## Gestione dei caricamenti di file

A questo punto, il server necessita di un endpoint di caricamento file sul server Web per ricevere ed elaborare i documenti.

Innanzitutto, create una cartella all&#39;interno di una cartella di caricamento e denominatela &quot;bozze&quot;. I file caricati e i file di PDF convertiti vengono memorizzati qui. Avanti, esegui `npm install express-fileupload` per installare il modulo Express-FileUpload e aggiungere il middleware a Express nel codice:

```
const fileUpload = require( "express-fileupload" );
app.use( fileUpload() );
```

A questo punto, aggiungere un `/upload `endpoint e salva il file caricato nella cartella bozze utilizzando lo stesso nome file. Quindi, chiamate la funzione che avete scritto in precedenza per creare un file PDF dello stesso documento se non è già in formato PDF. Potete generare un nome file per il nuovo file PDF in base al nome del documento caricato originale:

```
// Create a PDF file from an uploaded file
app.post( "/upload", ( req, res ) => {
    if( !req.files || Object.keys( req.files ).length === 0 ) {
        return res.status( 400 ).send( "No files were uploaded." );
    }
    
    // Create PDF from the uploaded file
    let file = req.files.myFile;
    file.mv( __dirname + "/uploads/drafts/" + file.name, ( err ) => {
        if( err ) {
            return res.status( 500 ).send( err );
        }
        if( file.name.endsWith( ".pdf" ) ) {
            res.redirect( "/" );
        }
        else {
            // Convert to PDF
            fileToPDF( __dirname + "/uploads/drafts/" + file.name, __dirname + "/uploads/drafts/" + file.name.replace( /\./g, "-" ) + ".pdf", ( file ) => {
                res.redirect( "/" );
            } );
        }
    });
} );
```

## Creazione di una pagina di caricamento

A questo punto, per caricare i file dall&#39;applicazione Web, creare un `index.html` pagina Web all&#39;interno della cartella uploads. Nella pagina, aggiungi un modulo di caricamento file che invia il file all&#39;endpoint /upload:

```
<form ref="uploadForm" 
      action="/upload"
      method="post" 
      encType="multipart/form-data">
      <input type="file" name="myFile" accept=".doc,.docx,.ppt,.pptx,.xls,.xlsx,.txt,.rtf,.bmp,.jpg,.gif,.tiff,.png">
      <input type="submit" value="Upload File" />
  </form>
```

![Screenshot delle funzionalità di caricamento dei file sulla pagina Web](assets/reviews_1.png)

Ora puoi caricare i documenti sul server Node.js. Il server salva il file all’interno della cartella uploads/draft e crea una versione in formato PDF.

A questo punto è possibile incorporare i documenti caricati, quindi utilizzare l&#39;API Incorpora PDF per consentire agli utenti di aggiungere facilmente commenti e annotazioni ai documenti.

## Enumerazione dei file PDF

Poiché un flusso di lavoro di un documento tipico può coinvolgere più documenti, è necessario visualizzare un elenco di documenti e collegare ciascuno di essi a una nuova pagina di revisione del documento nell&#39;app.

Innanzitutto, all&#39;interno del codice server, aggiungete un endpoint /files che ottiene e restituisce un elenco di tutti i file PDF memorizzati nella cartella uploads/draft:

```
const fs = require( "fs" );

app.get( "/files", ( req, res ) =\> {

fs.readdir( \_\_dirname + "/uploads/drafts/", ( err, files ) =\> {

if( err ) {

return res.status( 500 ).send( err );using

}

return res.json( files.filter( f =\> f.endsWith( ".pdf" ) ) );

} );

} );
```

Aggiungere un `/download/:file` che fornisce l’accesso al file PDF caricato per l’incorporamento nella pagina Web.

>[!NOTE]
>
>In un&#39;applicazione di produzione, è necessario aggiungere l&#39;autenticazione e l&#39;autorizzazione per garantire che la richiesta provenga da un utente valido e che l&#39;utente possa accedere al documento.

```
app.get( "/download/:file", function( req, res ){
    // Note: In production code, this should check authentication and user access permissions
    res.download( __dirname + "/uploads/drafts/" + req.params[ "file" ] );
});
```

Aggiornate la pagina index.html con un elemento dell&#39;elenco di file che si riempie in fase di caricamento. Ogni elemento può essere collegato a una pagina Web draft.html e il nome del file viene passato alla pagina utilizzando i parametri di stringa di query.

>[!NOTE]
>
>Per aggiungere ciascun elemento, utilizzate jQuery, quindi dovete caricare la libreria jQuery sulla pagina Web o aggiungere l&#39;elemento utilizzando un metodo diverso.

```
  <ul id="filelist">
      <li>Loading documents...</li>
  </ul>

  ...

  <script>
      // Load current files
      fetch( "/files" )
      .then( r => r.json() )
      .then( files => {
          if( files && files.length > 0 ) {
              $( "#filelist" ).empty();
              files.forEach( file => {
                  $( "#filelist" ).append( `<li><a
  href="/draft.html?file=${file}">${file}</a></li>` );
              })
          } else {
                  $("#filelist").append("<div>No documents found.</div>");
                }
      });
  </script>
```

![Screenshot della selezione di un file per la revisione](assets/reviews_2.png)

## Incorporamento di un PDF

È possibile incorporare e mostrare i file PDF nell&#39;applicazione Web.

Creare una pagina Web denominata &quot;draft.html&quot; e aggiungere un elemento div alla pagina per il PDF incorporato:

```
  <div id="adobe-dc-view"></div>
```

Includi [!DNL Acrobat Services] libreria:

```
  <script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

All&#39;interno di un tag script personalizzato, analizzare il nome del file dai parametri della stringa di query in modo da sapere quale file incorporare nella pagina:

```
  <script type="text/javascript">
          let params = new URLSearchParams( window.location.search );
          let filename = params.get( "file" );
  </script>
```

Aggiungete un listener di eventi del documento per l&#39;evento adobe_dc_view_sdk.ready che carica il file PDF specificato in una vista incorporata all&#39;interno dell&#39;elemento div. Utilizza l’ID client dalle credenziali API di incorporamento di PDF. Per abilitare commenti e annotazioni, incorporate la vista in modalità FULL_WINDOW e impostate l&#39;opzione showAnnotationsTools su true.

```
  document.addEventListener( "adobe_dc_view_sdk.ready", () => { 
      var adobeDCView = new AdobeDC.View( { 
          clientId: "YOUR CLIENT ID HERE",
          divId: "adobe-dc-view",
          locale: "en-US",
      } );
      adobeDCView.previewFile( {
          content: { location: { url: "download/" + filename } },
          metaData: { fileName: "Draft Version.pdf" }
      }, {
          embedMode: "FULL_WINDOW",
          showAnnotationTools: true,
          showPageControls: true
      } );
  });
```

## Creazione di un profilo utente

Per impostazione predefinita, i commenti e le annotazioni vengono visualizzati come &quot;Ospite&quot; in questa visualizzazione. È possibile impostare il nome del revisore corrente per i commenti e le annotazioni registrando un callback del profilo utente nel codice della pagina nella vista PDF. Di seguito è riportato un profilo di esempio. In un&#39;applicazione completa che include l&#39;autenticazione dell&#39;utente, le informazioni del profilo della sessione utente connesso possono essere impostate in questo modo per identificare ogni commentatore del documento nel flusso di lavoro di revisione.

```
  adobeDCView.registerCallback(
      AdobeDC.View.Enum.CallbackType.GET_USER_PROFILE_API,
      () => {
          return new Promise( ( resolve, reject ) => {
              resolve({
                  code: AdobeDC.View.Enum.ApiResponseCode.SUCCESS,
                  data: {
                      userProfile: {
                          name: "YOUR NAME",
                          firstName: "FIRST",
                          lastName: "LAST",
                          email: "document.editor@adobe.com"
                      }
                  }
              });
          });
      }
  );
```

Il tuo profilo ti identifica come utente specifico quando visualizzi e annota qualsiasi documento caricato utilizzando questa pagina Web.

## Salvataggio del feedback sul documento

Dopo che un utente ha commentato un documento, fa clic su **Salva.** Per impostazione predefinita, fate clic su **Salva** scarica il file PDF aggiornato. Modificate questa azione per aggiornare il file PDF corrente sul server.

Aggiungere un `/save` endpoint al codice server che sovrascrive il file PDF nella cartella uploads/draft:

```
  // Overwrite the PDF file with latest PDF changes and annotations
  app.post( "/save", ( req, res ) => {
      if( !req.files || Object.keys( req.files ).length === 0 ) {
          return res.status( 400 ).send( "No files were uploaded." );
      }

      let file = req.files.pdf;
      file.mv( __dirname + "/uploads/drafts/" + file.name, ( err ) => {
          if( err ) {
              return res.status( 500 ).send( err );
          }
          res.send( "File uploaded" );
      });
  } );
```

Registrate una chiamata alla vista PDF per l&#39;API SAVE che carica il contenuto nell&#39;endpoint /save. Potete modificare il valore autoSaveFrequency per consentire all&#39;applicazione di salvare automaticamente le modifiche in un timer e includere metadati aggiuntivi al file attualmente incorporato al termine, se desiderate.

```
  adobeDCView.registerCallback(
      AdobeDC.View.Enum.CallbackType.SAVE_API,
      ( metaData, content, options ) => {
          return new Promise( ( resolve, reject ) => {
              let formData = new FormData();
              formData.append( "pdf", new Blob( [ content ] ), "drafts/" + filename
  );
              fetch( "/save", {
                  method: "POST",
                  body: formData
              }).then( resp => {
                  resolve({
                      code: AdobeDC.View.Enum.ApiResponseCode.SUCCESS,
                      data: {
                          /* Updated file metadata after successful save operation */
                          metaData: Object.assign( metaData, {} )
                      }
                  });
              });
          });
      },
      {
          autoSaveFrequency: 0,
          enableFocusPolling: false,
          showSaveButton: true
      }
  );
```

I commenti e le annotazioni sui documenti bozza ora vengono salvati sul server. Puoi [scopri di più sulle funzioni di callback](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#callbacks-workflows) entra nel tuo flusso di lavoro. Ad esempio, [funzioni di callback dello stato](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#status-callback) gestire i conflitti di file se più persone desiderano rivedere e commentare lo stesso documento contemporaneamente.

Nel passaggio finale, si combinano tutti i documenti modificati in un unico file PDF utilizzando l&#39;API dei servizi PDF.

## Combinazione di file PDF

Il codice di combinazione PDF è simile al codice di creazione PDF, ma utilizza l&#39;operazione CombineFiles e aggiunge ogni file come input.

```
  function combineFilesToPDF( files, outputFilename, callback ) {
      // Create an ExecutionContext using credentials and create a new operation
  instance.
      const executionContext = PDFToolsSdk.ExecutionContext.create( credentials ),
          combineFilesOperation = PDFToolsSdk.CombineFiles.Operation.createNew();

      // Set operation inputs from source files.
      files.forEach( file => {
          const input = PDFToolsSdk.FileRef.createFromLocalFile( file );
          combineFilesOperation.addInput( input );
      } );

      // Execute the operation and Save the result to the specified location.
      combineFilesOperation.execute( executionContext )
          .then( result => {
              result.saveAsFile( outputFilename );
              callback( outputFilename );
          } );
 }
```

## Download del PDF finale

Aggiungete un endpoint denominato /finalize che chiama la funzione per combinare tutti i file PDF all&#39;interno del `uploads/drafts` in una cartella `Final.pdf` , quindi lo scarica.

```
  app.get( "/finalize", ( req, res ) => {
      fs.readdir( __dirname + "/uploads/drafts/", ( err, files ) => {
          if( err ) {
              return res.status( 500 ).send( err );
          }
          combineFilesToPDF(
              files.filter( f => f.endsWith( ".pdf" ) ).map( f => __dirname + 
  "/uploads/drafts/" + f ),
              __dirname + "/uploads/Final.pdf", ( file ) => {
              res.download( file );
          } );
      } );
  } );
```

Infine, aggiungete un collegamento nella pagina web main index.html a questo endpoint /finalize. Questo collegamento consente agli utenti di scaricare il risultato del flusso di lavoro del documento.

```
<a href="/finalize">Download final PDF</a>
```

![Screenshot dello scaricamento del documento finale](assets/reviews_3.png)

## Fasi seguenti

Questa esercitazione pratica mostra come [!DNL Acrobat Services] Le API integrano un [flusso di lavoro per la condivisione e la revisione di documenti](https://www.adobe.io/apis/documentcloud/dcsdk/review-and-approval.html) in un&#39;applicazione web. L&#39;applicazione consente ai lavoratori remoti di condividere file e collaborare con i propri colleghi di team, il che è particolarmente utile per i dipendenti e i collaboratori esterni che lavorano da casa.

Puoi utilizzare queste tecniche per abilitare la collaborazione nell’app o esplorare [Esempi di PDF Services Node SDK](https://github.com/adobe/pdftools-node-sdk-samples) e [Esempi di API di incorporamento di PDF](https://github.com/adobe/pdf-embed-api-samples) su GitHub per trovare ispirazione su come utilizzare le API di Adobe.

Vuoi abilitare la condivisione e la revisione di documenti nella tua app? Registrati [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) account sviluppatore. Accedi gratuitamente ad Adobe PDF Embed e prova gratuitamente per sei mesi le altre API. Dopo la versione di prova, puoi [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) a soli \$0,05 per transazione documento, man mano che la tua attività cresce.
