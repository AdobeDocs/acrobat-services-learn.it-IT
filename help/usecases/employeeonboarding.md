---
title: Modernizzazione della formazione dei dipendenti
description: Scopri come modernizzare l’inserimento dei dipendenti con [!DNL Adobe Acrobat Services] API
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-10203.jpg
jira: KT-10203
exl-id: 0186b3ee-4915-4edd-8c05-1cbf65648239
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1514'
ht-degree: 1%

---

# Modernizzazione dell&#39;inserimento dei dipendenti

![Usa banner eroe caso](assets/usecaseemployeeonboardinghero.jpg)

In una grande organizzazione, l&#39;inserimento dei dipendenti può essere un processo grande e lento. Tipicamente c&#39;è un mix di documentazione personalizzata con materiale boilerplate che deve essere presentato e firmato da un nuovo dipendente. Questo mix di materiale personalizzato e confezionato richiede più passaggi, sottraendo tempo prezioso alle persone coinvolte nel processo. [!DNL Adobe Acrobat Services] e Acrobat Sign può modernizzare e automatizzare questo approccio, liberando il personale delle risorse umane da dedicare a attività più importanti. Vediamo come ciò viene raggiunto.

## Cosa sono [!DNL Adobe Acrobat Services]?

[[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/homepage) sono un insieme di API relative all’utilizzo dei documenti (e non solo dei PDF). In linea di massima, questa gamma di servizi si articola in tre categorie principali:

* I primi sono i [Servizi PDF](https://developer.adobe.com/document-services/apis/pdf-services/) set di strumenti. Si tratta di metodi di utilità per lavorare con PDF e altri documenti. I servizi includono operazioni quali la conversione da e verso il PDF, l&#39;esecuzione dell&#39;OCR e l&#39;ottimizzazione, l&#39;unione e la divisione dei PDF e così via. È il kit di strumenti delle funzionalità di elaborazione dei documenti.
* [API PDF Extract](https://developer.adobe.com/document-services/apis/pdf-extract/) utilizza tecniche IA/ML avanzate per analizzare un PDF e restituire un&#39;incredibile quantità di dettagli sui contenuti. Questo include il testo, lo stile e le informazioni sulla posizione, e può anche restituire dati tabulari in formato CSV/XLS e recuperare le immagini.
* Infine, [API per la generazione di documenti](https://developer.adobe.com/document-services/apis/doc-generation/) consente agli sviluppatori di utilizzare Microsoft Word come &quot;modello&quot;, di combinare i propri dati (provenienti da qualsiasi origine) e di generare documenti dinamici personalizzati (PDF e Word).

Gli sviluppatori possono [registrarsi](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html) e prova tutti questi servizi con una versione di prova gratuita. Il [!DNL Acrobat Services] La piattaforma utilizza un&#39;API REST, ma supporta anche SDK per Node, Java, .NET e Python (Extract solo in questo momento).

Anche se non è un&#39;API, gli sviluppatori possono anche utilizzare il [API di incorporamento PDF](https://developer.adobe.com/document-services/apis/pdf-embed/), che offre un&#39;esperienza di visualizzazione uniforme e flessibile dei documenti nelle pagine Web.

## Che cos’è Acrobat Sign?

[Acrobat Sign](https://www.adobe.com/it/sign.html) è leader mondiale nei servizi di firma elettronica. Puoi inviare documenti per la firma utilizzando diversi flussi di lavoro, comprese più firme. Acrobat Sign supporta anche i flussi di lavoro che richiedono firme e informazioni aggiuntive. Tutte queste funzionalità sono supportate da una dashboard avanzata con un sistema di authoring flessibile.

Come con [!DNL Acrobat Services], Acrobat Sign ha un [prova gratuita](https://www.adobe.com/sign.html#sign_free_trial) che consente agli sviluppatori di testare il processo di firma sia tramite il dashboard che tramite un&#39;API REST intuitiva.

## Uno scenario iniziale

Prendiamo in considerazione uno scenario reale che dimostri come i servizi Adobi possano essere d&#39;aiuto. Quando un nuovo dipendente entra in un&#39;azienda, ha bisogno di informazioni personalizzate su misura per il suo ruolo. Inoltre, hanno bisogno di materiale anche per tutta l&#39;azienda. Infine, devono dimostrare l&#39;accettazione delle politiche aziendali firmando i documenti. Dividiamolo in passi concreti:

* In primo luogo, è necessaria una lettera di accompagnamento personalizzata che saluti il nuovo dipendente per nome. La lettera deve contenere informazioni sul nome, il ruolo, lo stipendio e la posizione del dipendente.
* La lettera personalizzata deve essere combinata con un PDF contenente informazioni di base a livello aziendale (ad esempio, politiche in materia di risorse umane, vantaggi, ecc.)
* Deve essere incluso un documento finale che richiede la firma e la data del dipendente.
* Tutte le informazioni sopra descritte devono essere presentate come un documento che viene inviato al dipendente per la firma.

Andiamo nei dettagli su come farlo.

## Generazione di documenti dinamici

Adobe [Generazione documento](https://developer.adobe.com/document-services/apis/doc-generation/) L&#39;API consente agli sviluppatori di creare documenti dinamici utilizzando Microsoft Word e un semplice linguaggio di modellazione come base per la generazione di PDF e documenti Word. Ecco un esempio di come funziona.

Iniziamo con un documento Word che ha valori codificati. Il documento può essere formattato nel modo desiderato, includendo elementi grafici, tabelle e così via. Ecco il documento iniziale.

![Screenshot del documento iniziale](assets/onboarding_1.png)

La generazione di documenti funziona aggiungendo &quot;token&quot; a un documento Word che viene sostituito con i dati. I token possono essere immessi manualmente, ma [Componente aggiuntivo Microsoft Word](https://developer.adobe.com/document-services/docs/overview/document-generation-api/wordaddin/) questo lo rende più facile da fare. L’apertura fornisce uno strumento che consente agli autori di definire tag, o set di dati, utilizzabili nel documento.

![Screenshot di Document Tagger](assets/onboarding_2.png)

Potete caricare le informazioni JSON da un file locale, copiarle nel testo JSON o selezionare per continuare con i dati iniziali. In questo modo è possibile definire i tag in modo ad hoc in base alle proprie esigenze specifiche. In questo esempio, è necessario solo un tag per nome, ruolo, stipendio e posizione. Questa operazione viene eseguita utilizzando il metodo **Crea tag** pulsante:

![Screenshot della definizione di un tag](assets/onboarding_3.png)

Dopo aver definito il primo tag, potete continuare a definirne il numero necessario:

![Screenshot dei tag definiti](assets/onboarding_4.png)

Con i tag definiti, selezionate il testo nel documento e sostituitelo con i tag, se necessario. In questo esempio, i tag vengono aggiunti per nome, ruolo e stipendio.

![Screenshot dei tag](assets/onboarding_5.png)

La generazione di documenti non supporta solo tag semplici ma anche espressioni logiche. Il secondo paragrafo del documento ha un testo che si applica solo alle persone in Louisiana. È possibile aggiungere un&#39;espressione condizionale passando alla scheda Avanzate del tag del documento e definendo una condizione. Di seguito viene definita una semplice condizione di uguaglianza, ma si noti che sono supportati anche i confronti numerici e altri tipi di confronto.

![Screenshot di Condition](assets/onboarding_6.png)

Questa proprietà può quindi essere inserita e racchiusa attorno al paragrafo:

![Screenshot di Condition in doc](assets/onboarding_7.png)

Per verificare il funzionamento, selezionare **Genera documento**. La prima volta che effettuate questa operazione, dovete accedere con un Adobe ID. Dopo l’accesso, viene presentato JSON predefinito che può essere modificato manualmente.

![Screenshot dei dati](assets/onboarding_8.png)

Viene generato un PDF che può essere visualizzato o scaricato.

![Screenshot di Generated PDF](assets/onboarding_9.png)

Mentre il tag documento consente di progettare e testare rapidamente, una volta completato e in produzione, potete utilizzare uno degli SDK per automatizzare questo processo. Mentre il codice effettivo differisce in base a esigenze specifiche, di seguito è riportato un esempio dell&#39;aspetto del codice in Node.js:

```js
 const PDFServicesSdk = require('@adobe/pdfservices-node-sdk');

const credentials =  PDFServicesSdk.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdfservices-api-credentials.json")
    .build();

// Data would be dynamic...
let data = {
    "name":"Raymond Camden",
    "role":"Lead Developer",
    "salary":9000,
    "location":"Louisiana"
}

// Create an ExecutionContext using credentials.
const executionContext = PDFServicesSdk.ExecutionContext.create(credentials);

// Create a new DocumentMerge options instance.
const documentMerge = PDFServicesSdk.DocumentMerge,
    documentMergeOptions = documentMerge.options,
    options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.PDF);

// Create a new operation instance using the options instance.
const documentMergeOperation = documentMerge.Operation.createNew(options);

// Set operation input document template from a source file.
const input = PDFServicesSdk.FileRef.createFromLocalFile('documentMergeTemplate.docx');
documentMergeOperation.setInput(input);

// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
    .then(result => result.saveAsFile('documentOutput.pdf'))
    .catch(err => {
        if(err instanceof PDFServicesSdk.Error.ServiceApiError
            || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
            console.log('Exception encountered while executing operation', err);
        } else {
            console.log('Exception encountered while executing operation', err);
        }
    });
```

In breve, il codice imposta le credenziali, crea un oggetto operation e imposta l&#39;input e le opzioni, quindi chiama l&#39;operazione. Infine, salva il risultato come PDF. (I risultati possono essere generati anche in Word.)

Generazione di documenti supporta casi di utilizzo molto più complessi, tra cui la possibilità di disporre di tabelle e immagini completamente dinamiche. Vedere la [documentazione](https://developer.adobe.com/document-services/docs/overview/document-generation-api/) per maggiori dettagli.

## Esecuzione di operazioni PDF

Il [API dei servizi PDF](https://developer.adobe.com/document-services/apis/pdf-services/) fornisce un ampio set di operazioni di utilità per lavorare con i PDF. Queste operazioni includono:

* Creazione di PDF dai documenti Office
* Esportazione di PDF in documenti Office
* Combinazione e suddivisione dei PDF
* Applicazione dell’OCR ai PDF
* Impostazione, rimozione e modifica della protezione dei PDF
* Eliminazione, inserimento, riordinamento e rotazione delle pagine
* Ottimizzazione dei PDF tramite compressione o linearizzazione
* Ottenere proprietà PDF

Per questo scenario, il risultato della chiamata Generazione documento deve essere unito a un PDF standard. Questa operazione è abbastanza semplice con gli SDK. Ecco un esempio di in Node.js:

```js
const PDFServicesSdk = require('@adobe/pdfservices-node-sdk');
 
// Initial setup, create credentials instance.
const credentials = PDFServicesSdk.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdfservices-api-credentials.json")
    .build();
 
// Create an ExecutionContext using credentials and create a new operation instance.
const executionContext = PDFServicesSdk.ExecutionContext.create(credentials),
    combineFilesOperation = PDFServicesSdk.CombineFiles.Operation.createNew();
 
// Set operation input from a source file.
const combineSource1 = PDFServicesSdk.FileRef.createFromLocalFile('documentOutput.pdf'),
      combineSource2 = PDFServicesSdk.FileRef.createFromLocalFile('standardCorporate.pdf');

combineFilesOperation.addInput(combineSource1);
combineFilesOperation.addInput(combineSource2);
 
// Execute the operation and Save the result to the specified location.
combineFilesOperation.execute(executionContext)
    .then(result => result.saveAsFile('combineFilesOutput.pdf'))
    .catch(err => {
        if (err instanceof PDFServicesSdk.Error.ServiceApiError
            || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
            console.log('Exception encountered while executing operation', err);
        } else {
            console.log('Exception encountered while executing operation', err);
        }
    });
```

Questo codice accetta i due PDF, li unisce e salva il risultato in un nuovo PDF. Semplice e facile! Vedere la [documenti](https://developer.adobe.com/document-services/docs/overview/pdf-services-api/) per fare alcuni esempi.

## Il processo di firma

Alla fine del processo di onboarding, il dipendente deve firmare un accordo in cui dichiara di aver letto e accettato tutte le politiche definite all&#39;interno. [Acrobat Sign](https://www.adobe.com/it/sign.html) supporta numerosi flussi di lavoro e integrazioni, tra cui una automatizzata tramite [API](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html). In linea di massima, la parte finale dello scenario può essere completata come segue:

Innanzitutto, progetta il documento che include il modulo che deve essere firmato. Esistono diversi modi per farlo, tra cui una grafica progettata nel dashboard degli utenti di Adobe Sign. Un&#39;altra opzione è quella di utilizzare il componente aggiuntivo Word Generazione documento per inserire i tag. Questo esempio richiede una firma e una data.

![Screenshot del documento con tag Sign](assets/onboarding_10.png)

Questo documento può essere salvato come PDF e, utilizzando lo stesso metodo descritto in precedenza, unito con tutti i documenti insieme. Questo processo crea un pacchetto uniforme che contiene un saluto personalizzato, la documentazione aziendale standard e una pagina finale adatta alla firma.

Il modello può essere caricato nel dashboard di Acrobat Sign e quindi utilizzato per i nuovi accordi. Utilizzando l&#39;API REST, questo documento può quindi essere inviato al potenziale dipendente per richiedere la propria firma.

![Screenshot del documento firmato](assets/onboarding_11.png)

## Sperimentalo da solo

Tutto ciò che viene descritto in questo articolo può essere testato subito. Il [!DNL Adobe Acrobat Services] API [prova gratuita](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html) attualmente offre 1.000 richieste gratuite per un periodo di sei mesi. Acrobat Sign [prova gratuita](https://www.adobe.com/sign.html#sign_free_trial) consente di inviare accordi con filigrana a scopo di prova.

Domande? Il [forum di supporto](https://community.adobe.com/t5/document-services-apis/ct-p/ct-Document-Cloud-SDK) viene monitorata dagli sviluppatori di Adobi e dal personale di supporto ogni giorno. Infine, per trovare più ispirazione, assicurati di trovare la prossima [Clip carta](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF) episodio. Ci sono incontri regolari in diretta con notizie, demo e conversazioni con i clienti.
