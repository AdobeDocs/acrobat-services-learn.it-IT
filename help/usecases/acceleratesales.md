---
title: Accelera il processo di vendita
description: Scopri come accelerare le vendite integrando le esperienze con i documenti con  [!DNL Adobe Acrobat Services]
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-10222
thumbnail: KT-10222.jpg
exl-id: 9430748f-9e2a-405f-acac-94b08ad7a5e3
source-git-commit: b7a20f30a2eb175053c7a25be0411f80dd88899f
workflow-type: tm+mt
source-wordcount: '1704'
ht-degree: 0%

---

# Velocizzare il processo di vendita

![Banner dell&#39;eroe del caso di utilizzo](assets/UseCaseAccelerateSalesHero.jpg)

Da white paper a contratti e accordi, sono necessari numerosi documenti per tutto il percorso di acquisto. In questo tutorial, scopri come [[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/) può integrare le esperienze dei documenti in questo percorso per accelerare le vendite.

## Genera accordi e ordini cliente dai dati

I contratti di vendita, i contratti e altri documenti possono variare notevolmente in base a criteri specifici. Ad esempio, un contratto di vendita può includere solo alcuni termini in base a criteri specifici, come l&#39;appartenenza a un determinato paese o stato o l&#39;inclusione di determinati prodotti come parte del contratto. La creazione manuale di questi documenti o la gestione di diverse varianti di modelli può aumentare notevolmente i costi legali associati alla revisione manuale delle modifiche.

[L&#39;API di Document Generation](https://developer.adobe.com/document-services/apis/doc-generation/) di Adobe consente di prelevare dati dal sistema CRM o da altri sistemi di dati per generare dinamicamente documenti di vendita basati su tali dati.

## Ottieni credenziali

Per iniziare, registra le credenziali gratuite dei servizi Adobe PDF:

1. Naviga [qui](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html) per registrare le tue credenziali.
1. Effettua l’accesso utilizzando il tuo Adobe ID.
1. Impostare il nome delle credenziali (ad esempio, Demo contratti di vendita).

   ![Schermata di impostazione del nome delle credenziali](assets/accsales_1.png)

1. Scegli una lingua per scaricare il codice di esempio (ad esempio, Node.js).
1. Verifica di accettare **[!UICONTROL condizioni per gli sviluppatori]**.
1. Selezionare **[!UICONTROL Crea credenziali]**.
Nel computer viene scaricato un file con un file ZIP contenente i file di esempio pdfservices-api-credentials.json e private.key per l&#39;autenticazione.

   ![Schermata delle credenziali](assets/accsales_2.png)

1. Seleziona **[!UICONTROL Scarica il componente aggiuntivo Microsoft Word]** o vai a [AppSource](https://appsource.microsoft.com/en-cy/product/office/WA200002654) per installarlo.

   >[!NOTE]
   >
   >L’installazione del componente aggiuntivo Word richiede l’autorizzazione per installare i componenti aggiuntivi in Microsoft 365. Se non disponi delle autorizzazioni necessarie, contatta il tuo amministratore Microsoft 365.

## I tuoi dati

Se si estraggono dati da un sistema di dati specifico, è necessario generare tali dati come dati JSON o generare uno schema personalizzato. In questo scenario viene utilizzato il seguente set di dati di esempio precreato:

```
{
    "salesOrder": {
        "comment": "Make sure to call 555-555-1234 when you arrive. The front door is broken."
    },
    "company": {
        "name":"Home Services Co.",
        "address": {
            "city": "Homestead",
            "state": "NY",
            "zip": "14623",
            "streetAddress": "123 Demohome Street"
        }
    },
    "customer": {
        "address": {
            "city": "Seattle",
            "state": "WA",
            "zip": "98052",
            "streetAddress": "20341 Whitworth Institute 405 N. Whitworth"
        },
        "email": "mailto:jane-doe@xyz.edu",
        "jobTitle": "Professor",
        "name": "Jane Doe",
        "telephone": "(425) 123-4567",
        "url": "http://www.janedoe.com"
    },
    "tax": {
        "state":"WA",
        "rate": 0.08
    },
    "referencesOrder": [
        {
            "description": "Carpet Cleaning Service - 3BR 2BA",
            "totalPaymentDue": {
                "price": 359.54
            },
            "orderedItem": {
                "description": "Carpet Cleaning Service"
            }
        },
        {
            "description": "Home Cleaning Service - 3BR 2BA",
            "totalPaymentDue": {
                "price": 299.99
            },
            "orderedItem": {
                "description": "House Cleaning Service"
            }
        }
    ]
}
```

## Aggiungere tag di base al documento

In questo scenario viene utilizzato un documento Ordine di vendita scaricabile [qui](https://github.com/benvanderberg/adobe-document-generation-samples/blob/main/SalesOrder/Exercise/SalesOrder_Base.docx?raw=true).

![Schermata del documento ordine cliente di esempio](assets/accsales_3.png)

1. Apri il documento di esempio *SalesOrder.docx* in Microsoft Word.
1. Se il plug-in Document Generation è installato, seleziona **[!UICONTROL Document Generation]** nella barra multifunzione. Se sulla barra multifunzione non è presente Document Generation, seguire le istruzioni riportate di seguito.
1. Seleziona **[!UICONTROL Introduzione]**.
1. Copia i dati di esempio JSON scritti in precedenza nel campo *Dati JSON*.

   ![Schermata di copia dei dati JSON](assets/accsales_4.png)

Quindi, passate al pannello Tag di Document Generation per inserire i tag nel documento.

1. Seleziona il testo da sostituire (ad esempio, *NOME AZIENDA*).
1. Nel pannello *Document Generation Tagger*, cercate &quot;name&quot;.
1. Nell’elenco dei tag, seleziona nome in azienda.
1. Selezionare **[!UICONTROL Inserisci testo]**.

   ![Schermata di inserimento del tag](assets/accsales_5.png)

   Questo processo inserisce un tag denominato `{{company.name}}` perché il tag si trova sotto il percorso nel JSON.

   ```
   {
   …
   "company": {
       "name":"Home Services Co.",
       …
   },
   …
   }
   ```

Ripetere queste azioni per alcuni dei tag aggiuntivi del documento, ad esempio INDIRIZZO, CITTÀ, STATO, CAP e così via.

## Visualizzare in anteprima il documento generato

Direttamente in Microsoft Word, puoi visualizzare in anteprima il documento generato in base ai dati JSON di esempio.

1. Nel pannello *Document Generation Tagger*, selezionate **[!UICONTROL Genera documento]**. La prima volta che ti viene richiesto di accedere con il tuo Adobe ID. Seleziona **[!UICONTROL Accedi]** e completa le richieste di accesso con le tue credenziali.

   ![Schermata di anteprima del documento generato](assets/accsales_6.png)

1. Selezionare **[!UICONTROL Visualizza documento]**.

   ![Schermata del pulsante Visualizza documento](assets/accsales_7.png)

1. Viene visualizzata una finestra del browser che consente di visualizzare in anteprima i risultati del documento.

   ![Schermata del documento nella finestra del browser](assets/accsales_8.png)

Nel documento sono visualizzati i tag sostituiti con i dati dei dati di esempio originali.

![Schermata dei tag sostituiti con dati](assets/accsales_9.png)

## Aggiungere una tabella al modello

In questo scenario successivo, aggiungere un elenco di prodotti a una tabella del documento.

1. Inserire il cursore nel punto in cui deve essere posizionata la tabella.
1. Nel pannello *Document Generation Tagger*, selezionate **[!UICONTROL Advanced]**.
1. Espandere **[!UICONTROL Tabelle ed elenchi]**.
1. Nel campo *Record di tabella*, seleziona *referencesOrder*, una matrice che elenca tutti gli elementi del prodotto.
1. Nel campo Seleziona record di colonna digitare per includere *descrizione* e *totalPaymentDue.price*.
1. Selezionare **[!UICONTROL Inserisci tabella]**.

   ![Schermata di inserimento tabella](assets/accsales_10.png)

Modificate la tabella per adattarla a stili, dimensioni e altri parametri come qualsiasi altra tabella di Microsoft Word.

## Aggiungi calcolo numerico

I calcoli numerici consentono di calcolare somme e altri calcoli in base a una raccolta di dati, ad esempio una matrice. In questo scenario aggiungere un campo per calcolare il subtotale.

1. Seleziona *$0.00* accanto al titolo del subtotale.
1. Nel pannello *[!UICONTROL Document Generation Tagger]*, espandere **[!UICONTROL Calcoli numerici]**.
1. In *[!UICONTROL Seleziona tipo di calcolo]*, scegli **[!UICONTROL Aggregazione]**.
1. In *[!UICONTROL Seleziona tipo]*, scegli **[!UICONTROL Somma]**.
1. In *[!UICONTROL Seleziona record]*, scegli **[!UICONTROL ReferencesOrder]**.
1. In *[!UICONTROL Selezionare l&#39;elemento da aggregare]**, scegliere **[!UICONTROL totalPaymentsDue.price]**.
1. Selezionare **[!UICONTROL Inserisci calcolo]**.

Questo processo inserisce un tag di calcolo che fornisce la somma dei valori. È possibile eseguire calcoli più avanzati utilizzando i calcoli JSONata. Esempio:

* Subtotale: `${{expr($sum(referencesOrder.totalPaymentDue.price))}}`
Calcola la somma di referencesOrder.totalPaymentDue.price.

* IVA: `${{expr($sum(referencesOrder.totalPaymentDue.price)*0.08)}}`
Calcola il prezzo e moltiplica per 8% per calcolare l&#39;imposta.

* Totale dovuto: `${{expr($sum(referencesOrder.totalPaymentDue.price)*1.08)}}`
Calcola il prezzo e moltiplica per 1,08 per calcolare il subtotale + l&#39;imposta.

## Aggiungi termini condizionali

Le sezioni condizionali consentono di includere una frase o un paragrafo solo quando viene soddisfatta una determinata condizione. In questo scenario, solo una sezione viene inclusa se corrisponde a un determinato stato.

1. Nel documento, individua la sezione *INFORMATIVE SULLA PRIVACY DELLA CALIFORNIA*.
1. Selezionare la sezione con il cursore.

   ![Schermata della selezione](assets/accsales_11.png)

1. In *[!UICONTROL Document Generation Tagger]*, seleziona **[!UICONTROL Advanced]**.
1. Espandere **[!UICONTROL Contenuto condizionale]**.
1. Nel campo *[!UICONTROL Seleziona record]*, cerca e seleziona **[!UICONTROL customer.address.state]**.
1. Nel campo *[!UICONTROL Seleziona operatore]*, selezionare **=**.
1. Nel campo *[!UICONTROL Valore]*, digitare *CA*.
1. Selezionare **[!UICONTROL Inserisci condizione]**.

La sezione California viene visualizzata nel documento generato solo se customer.address.state = CA.

Quindi, selezionare la sezione per WASHINGTON PRIVACY STATEMENTS e ripetere i passaggi precedenti, sostituendo il valore CA con WA.

## Aggiungere un’immagine dinamica

L’API Document Generation consente di inserire dinamicamente immagini dai dati. Ciò è utile quando sono presenti diversi marchi secondari e si desidera modificare loghi, immagini di ritratti o immagini per renderli più pertinenti per un determinato settore.

Le immagini possono essere passate da un URL nel contenuto data o base64. Questo esempio utilizza un URL.

1. Posizionare il cursore nel punto in cui si desidera includere un&#39;immagine.
1. Nel pannello *[!UICONTROL Document Generation Tagger]*, selezionate **[!UICONTROL Advanced]**.
1. Espandere **[!UICONTROL Immagini]**.
1. Nel campo *[!UICONTROL Seleziona tag]*, scegli il **[!UICONTROL logo]**.
1. Nel campo *[!UICONTROL Testo alternativo facoltativo]*, fornire una descrizione (ad esempio, logo). Questo processo inserisce un segnaposto immagine simile al seguente:

   ![Schermata dell&#39;immagine segnaposto](assets/accsales_12.png)

Tuttavia, potete impostare l’immagine in modo dinamico su un’immagine già presente nel layout, effettuando le seguenti operazioni:

1. Fare clic con il pulsante destro del mouse sull&#39;immagine segnaposto inserita.

   ![Schermata dell&#39;immagine segnaposto](assets/accsales_13.png)

1. Seleziona **[!UICONTROL Modifica testo alternativo]**.
1. Nel pannello, copia il testo che appare così:
   `{ "location-path": "logo", "image-props": { "alt-text": "Logo" }}`
1. Selezionate un’altra immagine nel documento che desiderate rendere dinamica.

   ![Schermata della nuova immagine nel documento](assets/accsales_14.png)

1. Fai clic con il pulsante destro del mouse sull&#39;immagine e seleziona **[!UICONTROL Modifica testo alternativo]**.
1. Incollate il valore nel pannello.

Questo processo sostituisce l&#39;immagine con un&#39;immagine che si trova nella variabile del logo nei dati.

## Aggiungere tag per Acrobat Sign

Adobe Acrobat Sign consente di acquisire firme elettroniche sui documenti. Acrobat Sign consente di trascinare facilmente i campi all’interno dell’interfaccia Web. Tuttavia, è possibile controllare la firma e l’inserimento di altri campi anche mediante un tag di testo. Con Adobe Document Generation Tagger potete facilmente inserire questi campi per tag di testo.

1. Individua il punto in cui è richiesta una firma nel documento di esempio.
1. Inserire il cursore dove è necessaria la firma.
1. Nel pannello *[!UICONTROL Adobe Document Generation Tagger]*, selezionate **[!UICONTROL Adobe Sign]**.
1. Nel campo *[!UICONTROL Specificare il numero di destinatari]*, impostare il numero di destinatari (in questo esempio si tratta di uno).
1. Nel campo *[!UICONTROL Destinatari]*, seleziona **[!UICONTROL Firmatario-1]**.
1. Nel tipo *[!UICONTROL Campo]*, selezionare **[!UICONTROL Firma]**.
1. Seleziona **[!UICONTROL Inserisci tag di testo Adobe Sign]**.

Nel documento viene inserito un tag.

![Schermata del tag di firma nel documento](assets/accsales_15.png)

In Acrobat Sign sono disponibili diversi altri tipi di campi, ad esempio i campi data.

1. Nel tipo *Campo*, selezionare **[!UICONTROL Data]**.
1. Sposta il cursore sopra la posizione della data nel documento.
1. Seleziona **[!UICONTROL Inserisci tag di testo Adobe Sign]**.

![Schermata del tag data nel documento](assets/accsales_16.png)

## Generare l’accordo

Ora hai aggiunto i tag al documento e sei pronto per iniziare. Questa sezione descrive come generare un documento utilizzando gli esempi di API di Document Generation per Node.js, ma funzionerà in qualsiasi lingua.

Apri il master pdfservices-node-sdk-samples-master scaricato al momento della registrazione delle credenziali. I file pdfservices-api-credentials.json e private.key devono essere inclusi in questi file.

1. Apri un terminale per installare le dipendenze tramite npm install.
1. Copia il file data.json di esempio nella cartella delle risorse.
1. Copiare il modello Word nella cartella delle risorse.
1. Create un nuovo file nella directory principale della cartella samples denominata generate-salesOrder.js.

```
const PDFServicesSdk = require('@adobe/pdfservices-node-sdk');
const fs = require('fs');
const path = require('path');

var dataFileName = path.join('resources', '<INSERT JSON FILE');
var outputFileName = path.join('output', 'salesOrder_'+Date.now()+".pdf");
var inputFileName = path.join('resources', '<INSERT DOCX>');

//Loads credentials from the file that you created.
const credentials =  PDFServicesSdk.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdfservices-api-credentials.json")
    .build();

// Setup input data for the document merge process
const jsonString = fs.readFileSync(dataFileName),
jsonDataForMerge = JSON.parse(jsonString);

// Create an ExecutionContext using credentials
const executionContext = PDFServicesSdk.ExecutionContext.create(credentials);

// Create a new DocumentMerge options instance
const documentMerge = PDFServicesSdk.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.PDF);

// Create a new operation instance using the options instance
const documentMergeOperation = documentMerge.Operation.createNew(options)

// Set operation input document template from a source file.
const input = PDFServicesSdk.FileRef.createFromLocalFile(inputFileName);
documentMergeOperation.setInput(input);

// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
.then(result => result.saveAsFile(outputFileName))
.catch(err => {
    if(err instanceof PDFServicesSdk.Error.ServiceApiError
        || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
        console.log('Exception encountered while executing operation', err);
    } else {
        console.log('Exception encountered while executing operation', err);
    }
});
```

1. Sostituisci `<INSERT JSON FILE>` con il nome del file JSON in /resources.
1. Sostituire `<INSERT DOCX>` con il nome del file DOCX.
1. Per eseguire, utilizzare Terminale per eseguire il nodo generate-salesOrder.js.

Il file di output deve trovarsi nella cartella /output con il documento generato correttamente.

## Altre opzioni

Una volta generato il documento, potete eseguire ulteriori azioni, ad esempio:

* Proteggi il documento con una password
* Comprimi PDF se sono presenti immagini di grandi dimensioni
* Acquisire firme elettroniche sul documento

Per ulteriori informazioni su alcune delle altre azioni disponibili, consultate gli script nella cartella /src nei file di esempio. Puoi anche imparare di più esaminando la documentazione delle diverse azioni.

## Casi d’uso aggiuntivi

[!DNL Adobe Acrobat Services] consente di semplificare molte parti di un ciclo di vendita con i flussi di lavoro dei documenti digitali:

* Utilizza l’API Adobe PDF Embed per incorporare white paper e altri contenuti nei siti Web e allo stesso tempo misurare e raccogliere analisi sulla visibilità
* Usa Acrobat Sign per acquisire le firme elettroniche sugli accordi generati
* Estrarre i dati degli accordi dai documenti PDF mediante l’API Adobe PDF Extract

## Ulteriori informazioni

Vuoi saperne di più? Scopri altri modi per utilizzare [!DNL Adobe Acrobat Services]:

* Ulteriori informazioni dalla [documentazione](https://developer.adobe.com/document-services/docs/overview/)
* Vedi altre esercitazioni su Adobe Experience League
* Utilizza gli script di esempio nella cartella /src per scoprire come utilizzare PDF
* Segui [Blog Adobe](https://medium.com/adobetech/tagged/adobe-document-cloud) per suggerimenti e trucchi più recenti
* Abbonati a [Paper Clips (lo streaming dal vivo mensile)](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF) per informazioni sull&#39;automazione tramite [!DNL Adobe Acrobat Services].
=======
* Ulteriori informazioni dalla [documentazione](https://developer.adobe.com/document-services/docs/overview/)
* Vedi altre esercitazioni su Adobe Experience League
* Utilizza gli script di esempio nella cartella /src per scoprire come utilizzare PDF
* Segui [Blog Adobe](https://medium.com/adobetech/tagged/adobe-document-cloud) per suggerimenti e trucchi più recenti
* Abbonati a [Paper Clips (lo streaming dal vivo mensile)](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF) per informazioni sull&#39;automazione tramite [!DNL Adobe Acrobat Services]
