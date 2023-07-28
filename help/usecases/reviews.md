---
title: Revisioni e approvazioni
description: Scopri come creare un flusso di lavoro per la revisione e l’approvazione di documenti per la collaborazione tra team
type: Tutorial
role: Developer
level: Intermediate
feature: Use Cases
thumbnail: KT-8094.jpg
jira: KT-8094
exl-id: d704620f-d06a-4714-9d09-3624ac0fcd3a
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '1623'
ht-degree: 0%

---

# Revisioni e approvazioni

![Banner Hero per casi di utilizzo](assets/UseCaseReviewsHero.jpg)

Durante la pandemia di COVID-19, molte imprese hanno dovuto collaborare in remoto tra loro, [condivisione e revisione di documenti digitali](https://www.adobe.io/apis/documentcloud/dcsdk/review-and-approval.html) presenta una serie di sfide per i team e le risorse interfunzionali.

Queste sfide includono la condivisione di documenti in diversi formati di file, la revisione e la creazione di commenti efficaci per il contenuto e la sincronizzazione con le modifiche più recenti. [!DNL Adobe Acrobat Services] Le API sono progettate per consentire agli sviluppatori di applicazioni di risolvere questi problemi per gli utenti.

## Cosa puoi imparare

Questa esercitazione pratica mostra come creare un flusso di lavoro per la revisione e l’approvazione di un documento in un’applicazione Web Node.js ed Express. Per seguire questo tutorial, avete bisogno di una certa esperienza con Node.js.

L&#39;applicazione dispone delle seguenti funzionalità:

* Convertire diversi tipi di file in PDF

* Abilita caricamenti file

* Possibilità per gli utenti di aggiungere commenti e annotazioni

* Visualizzare i PDF insieme ai commenti

* Abilitare i profili utente per identificare gli autori dei commenti

* Combina i file in un PDF finale che gli utenti possono scaricare

## API e risorse pertinenti

* [API di PDF Services](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [API PDF Embed](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Codice progetto](https://github.com/contentlab-io/adobe_reviews_and_approvals)

## Creazione di credenziali API di Adobe

Prima di avviare il codice, è necessario [crea credenziali](https://www.adobe.com/go/dcsdks_credentials) per Adobe PDF Embed API e Adobe PDF Services API. L’API PDF Embed può essere utilizzata liberamente. L’API di PDF Services può essere utilizzata gratuitamente per sei mesi, dopodiché puoi passare a un [piano a pagamento in base al consumo](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) a soli \$0,05 per transazione documento.

Quando crei le credenziali per l’API di PDF Services, seleziona **Creare un esempio di codice personalizzato** e selezionare Node.js per la lingua. Salva il file ZIP ed estrai pdftools-api-credentials.json e private.key nella directory principale del progetto Node.js Express.

## Impostazione di un progetto e delle dipendenze

Configura il tuo progetto Node.js ed Express in modo che serva file statici da una cartella denominata &quot;public&quot;. Puoi impostare il progetto in modi diversi, a seconda delle tue preferenze. Per iniziare rapidamente a utilizzare, è possibile [Generatore di app Express](https://expressjs.com/en/starter/generator.html). Oppure, se vuoi mantenere le cose semplici, puoi [inizia da zero](https://expressjs.com/en/starter/hello-world.html) e mantenere il codice in un unico file JavaScript. Nel progetto di esempio sopra collegato, stai utilizzando l&#39;approccio a un file e mantenendo tutto il tuo codice in index.js.

Copia `pdftools-api-credentials.json` e `private.key` file dall&#39;esempio di codice personalizzato alla directory principale del progetto. Inoltre, aggiungerli al file .gitignore, se presente, in modo che i file delle credenziali non vengano inviati accidentalmente a un repository.

Avanti, esegui `npm install @adobe/documentservices-pdftools-node-sdk` per installare Node.js SDK per PDF Services. Importa questo modulo e crea l’oggetto Credenziali API all’interno del codice (index.js nel progetto di esempio), dopo il resto delle importazioni delle dipendenze come segue:

```
  const PDFToolsSdk = require( "@adobe/documentservices-pdftools-node-sdk" );

  // Create Credentials
  const credentials =  PDFToolsSdk.Credentials
      .serviceAccountCredentialsBuilder()
      .fromFile( "pdftools-api-credentials.json" )
      .build();
```

Il codice di partenza dovrebbe essere simile al seguente:

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

Ora sei pronto per lavorare con [!DNL Acrobat Services] API.

## Conversione di un file in PDF

Per la prima parte del flusso di lavoro dei documenti, l’utente finale deve caricare i documenti da condividere. Per abilitare questa opzione, aggiungi una funzione di caricamento e consolida i diversi formati di file del documento in PDF per prepararli per il processo di revisione.

Inizia creando una funzione per convertire i documenti in PDF in base al [frammento di esempio per l’API di PDF Services](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html). In questo esempio vengono mostrati frammenti per molte altre funzioni vitali, tra cui il riconoscimento ottico dei caratteri (OCR), la protezione e la rimozione della password e la compressione.

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

Ora puoi utilizzare questa funzione per creare PDF da documenti caricati.

## Gestione dei caricamenti dei file

Per ricevere ed elaborare i documenti, il server deve quindi disporre di un endpoint per il caricamento dei file sul server Web.

Per prima cosa, crea una cartella all&#39;interno di una cartella di caricamento e denominala &quot;bozze&quot;. I file caricati e i file PDF convertiti vengono archiviati qui. Avanti, esegui `npm install express-fileupload` per installare il modulo Express-FileUpload e aggiungere il middleware ad Express nel codice:

```
const fileUpload = require( "express-fileupload" );
app.use( fileUpload() );
```

Ora aggiungi un `/upload `e salvare il file caricato all&#39;interno della cartella delle bozze utilizzando lo stesso nome di file. Quindi, chiamate la funzione che avete scritto in precedenza per creare un file PDF dello stesso documento, se non è già in formato PDF. Potete generare un nome file per il nuovo file PDF in base al nome del documento caricato originale:

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

Per caricare i file dall&#39;applicazione Web, crea un `index.html` pagina Web all&#39;interno della cartella uploads. Nella pagina, aggiungi un modulo di caricamento file che invii il file all&#39;endpoint /upload:

```
<form ref="uploadForm" 
      action="/upload"
      method="post" 
      encType="multipart/form-data">
      <input type="file" name="myFile" accept=".doc,.docx,.ppt,.pptx,.xls,.xlsx,.txt,.rtf,.bmp,.jpg,.gif,.tiff,.png">
      <input type="submit" value="Upload File" />
  </form>
```

![Schermata con funzionalità di caricamento file su pagina Web](assets/reviews_1.png)

Ora puoi caricare i documenti sul server Node.js. Il server salva il file all&#39;interno della cartella uploads/draft e crea una versione in formato PDF accanto ad esso.

È ora possibile incorporare i documenti caricati, quindi utilizza l’API PDF Embed per consentire agli utenti di aggiungere facilmente commenti e annotazioni ai documenti.

## Enumerazione dei file PDF

Poiché un flusso di lavoro tipico per documenti può coinvolgere più documenti, è necessario mostrare un elenco di documenti e collegarli a una nuova pagina di revisione dei documenti nell’app.

Innanzitutto, all&#39;interno del codice del server, aggiungi un endpoint /files che ottiene e restituisce un elenco di tutti i file PDF memorizzati nella cartella uploads/draft:

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

Aggiungi un `/download/:file` route che fornisce l&#39;accesso al file PDF caricato per l&#39;incorporamento nella pagina Web.

>[!NOTE]
>
>In un’applicazione di produzione è necessario aggiungere l’autenticazione e l’autorizzazione per garantire che la richiesta provenga da un utente valido e che l’utente sia autorizzato ad accedere al documento.

```
app.get( "/download/:file", function( req, res ){
    // Note: In production code, this should check authentication and user access permissions
    res.download( __dirname + "/uploads/drafts/" + req.params[ "file" ] );
});
```

Aggiornare la pagina index.html con un elemento elenco di file che viene compilato al momento del caricamento. Ogni elemento può essere collegato a una pagina Web draft.html e il nome del file viene passato alla pagina utilizzando i parametri della stringa di query.

>[!NOTE]
>
>Si utilizza jQuery per accodare ogni elemento, quindi è necessario caricare la libreria jQuery sulla pagina Web o accodare l&#39;elemento utilizzando un metodo diverso.

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

![Schermata di selezione di un file per la revisione](assets/reviews_2.png)

## Incorporamento di un PDF

È possibile incorporare e visualizzare i file PDF all&#39;interno dell&#39;applicazione Web.

Create una pagina Web denominata &quot;draft.html&quot; e aggiungete un elemento div alla pagina per il PDF incorporato:

```
  <div id="adobe-dc-view"></div>
```

Includi [!DNL Acrobat Services] libreria:

```
  <script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

All’interno di un tag script personalizzato, analizza il nome del file dai parametri della stringa di query in modo da sapere quale file incorporare nella pagina:

```
  <script type="text/javascript">
          let params = new URLSearchParams( window.location.search );
          let filename = params.get( "file" );
  </script>
```

Aggiungere un listener di eventi del documento per l&#39;evento adobe_dc_view_sdk.ready che carica il file PDF specificato in una vista incorporata all&#39;interno dell&#39;elemento div. Utilizza l’ID client dalle credenziali API di PDF Embed. Si desidera abilitare commenti e annotazioni, quindi incorporare la visualizzazione in modalità FULL_WINDOW e impostare l&#39;opzione showAnnotationsTools su true.

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

Per impostazione predefinita, in questa visualizzazione i commenti e le annotazioni vengono visualizzati come &quot;Guest&quot;. È possibile impostare il nome del revisore corrente per i commenti e le annotazioni registrando un callback del profilo utente nel codice di pagina nella visualizzazione PDF. Di seguito è riportato un profilo di esempio. In un’applicazione completa che include l’autenticazione dell’utente, le informazioni di profilo della sessione utente che ha effettuato l’accesso possono essere impostate in questo modo per identificare ogni commentatore del documento nel flusso di lavoro di revisione.

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

Il tuo profilo ti identifica come un utente specifico quando visualizzi e annota qualsiasi documento caricato utilizzando questa pagina Web.

## Salvataggio del feedback sul documento

Dopo che un utente ha aggiunto un commento a un documento, può fare clic su **Risparmia.** Per impostazione predefinita, fare clic su **Salva** scarica il file PDF aggiornato. Modificare questa azione per aggiornare il file PDF corrente sul server.

Aggiungi un `/save` endpoint al codice server che sovrascrive il file PDF nella cartella uploads/draft:

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

Registra una richiamata della vista PDF per l’API SAVE che carica il contenuto nell’endpoint /save. È possibile modificare il valore di autoSaveFrequency per consentire all&#39;applicazione di salvare automaticamente le modifiche in un timer e di includere ulteriori metadati nel file incorporato al completamento, se necessario.

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

Commenti e annotazioni sui documenti bozza ora vengono salvati sul server. È possibile [ulteriori informazioni su come eseguire le richiamate](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#callbacks-workflows) si adatta al flusso di lavoro. Ad esempio: [callback di stato](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#status-callback) per gestire i conflitti di file, se più persone desiderano rivedere e commentare contemporaneamente lo stesso documento.

Nel passaggio finale, combini tutti i documenti modificati in un unico file PDF utilizzando l’API di PDF Services.

## Combinazione di file PDF

Il codice combinato PDF è simile al codice di creazione PDF, ma utilizza l’operazione CombineFiles e aggiunge ciascun file come input.

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

## Download di final PDF in corso

Aggiungere un endpoint denominato /finalize che richiama la funzione per combinare tutti i file PDF all&#39;interno del `uploads/drafts` cartella in una `Final.pdf` file, quindi lo scarica.

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

Infine, aggiungere un collegamento nella pagina Web principale index.html a questo endpoint /finalize. Questo collegamento consente agli utenti di scaricare il risultato del flusso di lavoro del documento.

```
<a href="/finalize">Download final PDF</a>
```

![Schermata di download del documento finale](assets/reviews_3.png)

## Fasi seguenti

Questo tutorial pratico ha mostrato come [!DNL Acrobat Services] Le API integrano un [condivisione di documenti e flusso di lavoro di revisione](https://www.adobe.io/apis/documentcloud/dcsdk/review-and-approval.html) in un&#39;applicazione web. L&#39;applicazione consente ai dipendenti remoti di condividere file e collaborare con i colleghi del team, in modo particolarmente utile per dipendenti e appaltatori che lavorano da casa.

Puoi utilizzare queste tecniche per abilitare la collaborazione nella tua app o esplorare [Esempi SDK nodo servizi PDF](https://github.com/adobe/pdftools-node-sdk-samples) e [Esempi di API PDF Embed](https://github.com/adobe/pdf-embed-api-samples) su GitHub per trovare ispirazione su come utilizzare in altri modi le API di Adobe.

Sei pronto ad abilitare la condivisione e la revisione dei documenti nella tua app? Registrati [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) account sviluppatore. Accedi ad Adobe PDF Embed gratuitamente e goditi una versione di prova gratuita di sei mesi delle altre API. Al termine della versione di prova, puoi [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) per soli \$0,05 per transazione documento in base alla crescita dell&#39;azienda.
