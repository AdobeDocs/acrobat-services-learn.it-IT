---
title: Accelera il processo di vendita
description: Scopri come accelerare le vendite integrando le esperienze documentali con [!DNL Adobe Acrobat Services]
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-10222.jpg
kt: 10222
exl-id: 9430748f-9e2a-405f-acac-94b08ad7a5e3
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '1755'
ht-degree: 0%

---

# Accelera il processo di vendita

![Usa banner eroe caso](assets/UseCaseAccelerateSalesHero.jpg)

Dai white paper ai contratti e agli accordi, sono necessari numerosi documenti durante il percorso di acquisto. In questa esercitazione, scopri come [[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/) è in grado di integrare le esperienze documentali lungo l&#39;intero percorso per accelerare le vendite.

## Generare accordi e ordini di vendita dai dati

Contratti di vendita, contratti e altri documenti possono variare notevolmente in base a criteri specifici. Ad esempio, un contratto di vendita può includere solo alcuni termini basati su un criterio univoco, ad esempio trovarsi in un paese o in uno stato specifico o includere determinati prodotti come parte dell&#39;accordo. La creazione manuale di questi documenti o il mantenimento di diverse varianti di modello può aumentare notevolmente i costi legali associati alla revisione manuale delle modifiche.

[Adobe API di generazione documenti](https://developer.adobe.com/document-services/apis/doc-generation/) ti consente di prendere dati dal tuo CRM o da un altro sistema di dati per generare dinamicamente documenti di vendita basati su tali dati.

## Ottieni credenziali

Inizia registrandoti per ottenere le credenziali gratuite di Adobe PDF Services:

1. Naviga [qui](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html) per registrare le credenziali.
1. Accedi con il tuo Adobe ID.
1. Imposta il nome della tua credenziale (ad esempio, Demo degli accordi di vendita).

   ![Screenshot dell&#39;impostazione del nome della credenziale](assets/accsales_1.png)

1. Scegliete una lingua per scaricare il codice di esempio (ad esempio, Node.js).
1. Verifica di accettare **[!UICONTROL termini per sviluppatori]**.
1. Seleziona **[!UICONTROL Creare le credenziali]**.
Un file viene scaricato sul computer con un file ZIP contenente i file di esempio pdfservices-api-credentials.json e private.key per l&#39;autenticazione.

   ![Screenshot delle credenziali](assets/accsales_2.png)

1. Seleziona **[!UICONTROL Scarica il componente aggiuntivo Microsoft Word]** oppure andare a [AppSource](https://appsource.microsoft.com/en-cy/product/office/WA200002654) da installare.

   >[!NOTE]
   >
   >L’installazione del componente aggiuntivo Word richiede l’autorizzazione per installare i componenti aggiuntivi in Microsoft 365. Se non disponi dell’autorizzazione, contatta l’amministratore di Microsoft 365.

## I tuoi dati

Se state estraendo dati da un sistema di dati specifico, dovete generare i dati come dati JSON o il vostro schema. Questo scenario utilizza il seguente set di dati di esempio precreato:

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

Questo scenario utilizza un documento Ordine di vendita, che può essere scaricato [qui](https://github.com/benvanderberg/adobe-document-generation-samples/blob/main/SalesOrder/Exercise/SalesOrder_Base.docx?raw=true).

![Screenshot del documento di esempio degli ordini di vendita](assets/accsales_3.png)

1. Apri il *SalesOrder.docx* documento di esempio in Microsoft Word.
1. Se il plug-in Generazione documento è installato, seleziona **[!UICONTROL Generazione documento]** nella barra multifunzione. Se la barra multifunzione non contiene la Generazione di documenti, seguire queste istruzioni.
1. Seleziona **[!UICONTROL Introduzione]**.
1. Copia i dati di esempio JSON scritti sopra nel *Dati JSON* campo.

   ![Screenshot della copia dei dati JSON](assets/accsales_4.png)

Quindi, passate al pannello Tag generazione documento per inserire i tag nel documento.

1. Selezionate il testo da sostituire (ad esempio, *RAGIONE SOCIALE*).
1. Nel *Tag Generazione documento* pannello, cerca &quot;name&quot;.
1. Nell’elenco dei tag, seleziona il nome in Azienda.
1. Seleziona **[!UICONTROL Inserisci testo]**.

   ![Screenshot dell&#39;inserimento del tag](assets/accsales_5.png)

   Questo processo inserisce un tag denominato {{company.name}} perché il tag si trova sotto il percorso in JSON.

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

Ripetete queste azioni per alcuni dei tag aggiuntivi nel documento, ad esempio INDIRIZZO, CITTÀ, STATO, ZIP e così via.

## Anteprima del documento generato

Direttamente in Microsoft Word, potete visualizzare in anteprima il documento generato in base ai dati JSON di esempio.

1. Nel *Tag Generazione documento* pannello, seleziona **[!UICONTROL Genera documento]**. La prima volta che ti verrà richiesto di accedere con il tuo Adobe ID. Seleziona **[!UICONTROL Accedi]** e completa le richieste di accesso con le tue credenziali.

   ![Screenshot di come visualizzare in anteprima il documento generato](assets/accsales_6.png)

1. Seleziona **[!UICONTROL Visualizza documento]**.

   ![Screenshot del pulsante Visualizza documento](assets/accsales_7.png)

1. Si apre una finestra del browser che consente di visualizzare in anteprima i risultati del documento.

   ![Screenshot del documento nella finestra del browser](assets/accsales_8.png)

Potete vedere i tag nel documento che sono stati sostituiti con i dati dei dati di esempio originali.

![Screenshot dei tag sostituiti con dati](assets/accsales_9.png)

## Aggiungere una tabella al modello

In questo scenario successivo, aggiungete un elenco di prodotti a una tabella del documento.

1. Inserite il cursore nel punto in cui deve essere posizionata la tabella.
1. Nel *Tag Generazione documento* pannello, seleziona **[!UICONTROL Avanzate]**.
1. Espandi **[!UICONTROL Tabelle ed elenchi]**.
1. Nel *Record di tabella* , seleziona *referencesOrder*, che è un array che elenca tutti gli elementi del prodotto.
1. Nel campo Seleziona record colonna, digitare per includere *resoconto* e *totalPaymentDue.price* campo.
1. Seleziona **[!UICONTROL Inserisci tabella]**.

   ![Screenshot dell&#39;inserimento di una tabella](assets/accsales_10.png)

Modificate la tabella per adattarla a stili, dimensioni e altri parametri, come fareste con qualsiasi altra tabella in Microsoft Word.

## Aggiungere il calcolo numerico

I calcoli numerici consentono di calcolare somme e altri calcoli basati su una raccolta di dati, ad esempio un array. In questo scenario, aggiungere un campo per calcolare il subtotale.

1. Selezionate la proprietà *$0,00* accanto al titolo subtotale.
1. Nel *[!UICONTROL Tag Generazione documento]* pannello, espandere **[!UICONTROL Calcoli numerici]**.
1. Sotto *[!UICONTROL Selezionare il tipo di calcolo]*, scegli **[!UICONTROL Aggregazione]**.
1. Sotto *[!UICONTROL Selezionare il testo]*, scegli **[!UICONTROL Somma]**.
1. Sotto *[!UICONTROL Selezionare i record]*, scegli **[!UICONTROL ReferencesOrder]**.
1. In *[!UICONTROL Selezionare l’elemento per eseguire l’aggregazione]**, scegli **[!UICONTROL totalPaymentsDue.price]**.
1. Seleziona **[!UICONTROL Inserisci calcolo]**.

Questo processo inserisce un tag di calcolo che fornisce la somma dei valori. Calcoli più avanzati possono essere fatti utilizzando i calcoli JSONata. Esempio:

* Subtotale: `${{expr($sum(referencesOrder.totalPaymentDue.price))}}`
Calcola la somma di referencesOrder.totalPaymentDue.price.

* Imposta sulle vendite: `${{expr($sum(referencesOrder.totalPaymentDue.price)*0.08)}}`
Calcola il prezzo e si moltiplica per 8% per calcolare l&#39;imposta.

* Totale dovuto: `${{expr($sum(referencesOrder.totalPaymentDue.price)*1.08)}}`
Calcola il prezzo e moltiplica per 1,08 per calcolare il totale parziale + imposta.

## Aggiungere termini condizionali

Le sezioni condizionali consentono di includere solo una frase o un paragrafo quando viene soddisfatta una determinata condizione. In questo scenario, solo una sezione viene inclusa se corrisponde a un determinato stato.

1. Nel documento, trova la sezione denominata *INFORMATIVE SULLA PRIVACY IN CALIFORNIA*.
1. Selezionate la sezione con il cursore.

   ![Screenshot della selezione](assets/accsales_11.png)

1. Nel *[!UICONTROL Tag Generazione documento]*, seleziona **[!UICONTROL Avanzate]**.
1. Espandi **[!UICONTROL Contenuto condizionale]**.
1. Nel *[!UICONTROL Selezionare i record]* campo, ricerca e selezione **[!UICONTROL customer.address.state]**.
1. Nel *[!UICONTROL Seleziona, operatore]* , seleziona **=**.
1. Nel *[!UICONTROL Campo Valore]*, testo *CA*.
1. Seleziona **[!UICONTROL Inserisci condizione]**.

La sezione California viene visualizzata nel documento generato solo se customer.address.state = CA.

Quindi seleziona la sezione relativa agli INFORMATIVI SULLA PRIVACY DI WASHINGTON e ripeti i passaggi precedenti, sostituendo il valore CA con WA.

## Aggiungere un’immagine dinamica

L&#39;API di generazione dei documenti consente di inserire le immagini in modo dinamico dai dati. Ciò è utile quando si hanno diversi marchi e si desidera modificare loghi, immagini di ritratti o immagini per renderli più pertinenti per un determinato settore.

Le immagini possono essere passate da un URL nei dati o nel contenuto base64. Questo esempio utilizza un URL.

1. Posizionate il cursore nel punto in cui desiderate includere un’immagine.
1. Nel *[!UICONTROL Tag Generazione documento]* pannello, seleziona **[!UICONTROL Avanzate]**.
1. Espandi **[!UICONTROL Immagini]**.
1. Nel *[!UICONTROL Selezionare i tag]* campo, scegli **[!UICONTROL logo]**.
1. Nel *[!UICONTROL Testo alternativo opzionale]* , fornire una descrizione (ad esempio, logo). In questo processo viene inserito un segnaposto immagine simile al seguente:

   ![Screenshot dell&#39;immagine segnaposto](assets/accsales_12.png)

Tuttavia, potete impostare dinamicamente l’immagine su un’immagine già presente nel layout, in modo da:

1. Fate clic con il pulsante destro del mouse sull’immagine segnaposto inserita.

   ![Screenshot dell&#39;immagine segnaposto](assets/accsales_13.png)

1. Seleziona **[!UICONTROL Modifica testo Alt]**.
1. Nel pannello, copiate il testo che ha questo aspetto:
   `{ "location-path": "logo", "image-props": { "alt-text": "Logo" }}`
1. Selezionate un’altra immagine nel documento che desiderate rendere dinamica.

   ![Screenshot della nuova immagine nel documento](assets/accsales_14.png)

1. Fai clic con il pulsante destro del mouse sull’immagine e seleziona **[!UICONTROL Modifica testo Alt]**.
1. Incollate il valore nel pannello.

Questo processo sostituisce l’immagine con un’immagine presente nella variabile logo nei dati.

## Aggiungere tag per Acrobat Sign

Adobe Acrobat Sign consente di acquisire firme elettroniche sui documenti. Acrobat Sign consente di trascinare facilmente i campi nell’interfaccia Web, ma è anche possibile controllare la firma e il posizionamento di altri campi utilizzando un tag di testo. Con Adobe tag Generazione documento, puoi inserire facilmente questi campi tag di testo.

1. Individuare il punto in cui è richiesta la firma nel documento di esempio.
1. Inserire il cursore nel punto in cui è necessaria la firma.
1. Nel *[!UICONTROL Adobe tag Generazione documento]* pannello, seleziona **[!UICONTROL Adobe Sign]**.
1. Nel *[!UICONTROL Specificare il numero di destinatari]* , impostare il numero di destinatari (in questo esempio, uno).
1. Nel *[!UICONTROL Destinatari]* , seleziona **[!UICONTROL Signer-1]**.
1. Nel *[!UICONTROL Campo]* testo, selezione **[!UICONTROL Firma]**.
1. Seleziona **[!UICONTROL Inserisci tag di testo Adobe Sign]**.

Un tag viene inserito nel documento.

![Screenshot del tag della firma nel documento](assets/accsales_15.png)

Acrobat Sign fornisce diversi altri tipi di campi, ad esempio campi data.
1. Nel *Campo* testo, selezione **[!UICONTROL Data]**.
1. Spostate il cursore sopra la posizione Date nel documento.
1. Seleziona **[!UICONTROL Inserisci tag di testo Adobe Sign]**.

![Screenshot del tag di data nel documento](assets/accsales_16.png)

## Genera il tuo accordo

A questo punto è stato aggiunto un tag al documento e il documento è pronto per essere inviato. In questa sezione viene illustrato come generare un documento utilizzando gli esempi API di generazione del documento per Node.js, che tuttavia funzioneranno in qualsiasi lingua.

Apri pdfservices-node-sdk-samples-master scaricato quando hai registrato le credenziali. I file pdfservices-api-credentials.json e private.key devono essere inclusi in questi file.

1. Apri un Terminale per installare le dipendenze utilizzando l’installazione npm.
1. Copia il file data.json di esempio nella cartella delle risorse.
1. Copia il modello Word nella cartella delle risorse.
1. Creare un nuovo file nella directory principale della cartella samples denominata generate-salesOrder.js.

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
1. Sostituisci `<INSERT DOCX>` con il nome del file DOCX.
1. Per eseguirlo, utilizza Terminale per eseguire il nodo generate-salesOrder.js.

Il file di output deve trovarsi nella cartella /output con il documento generato correttamente.

## Altre opzioni

Una volta generato il documento, è possibile eseguire altre operazioni, ad esempio:

* Protezione dei documenti tramite password
* Comprimi PDF se sono presenti immagini di grandi dimensioni
* Acquisire firme elettroniche nel documento

Per ulteriori informazioni su alcune delle altre azioni disponibili, consultate gli script nella cartella /src nei file di esempio. Per ulteriori informazioni, consultare la documentazione delle diverse azioni.

## Ulteriori esempi di utilizzo

[!DNL Adobe Acrobat Services] può aiutarti a semplificare molte parti di un ciclo di vendita con flussi di lavoro basati su documenti digitali:

* Usa l&#39;API Incorpora di Adobe PDF per incorporare white paper e altri contenuti sui siti web, misurando e raccogliendo allo stesso tempo analisi sull&#39;audience
* Usa Acrobat Sign per acquisire le firme elettroniche negli accordi generati
* Estrarre i dati degli accordi dai documenti di PDF utilizzando l’API Adobe PDF Extract

## Apprendimento ulteriore

Sei interessato a saperne di più? Dai un&#39;occhiata ad alcuni altri modi di usare [!DNL Adobe Acrobat Services]:

* Scopri di più da [documentazione](https://developer.adobe.com/document-services/docs/overview/)
* Altre esercitazioni su Adobe Experience League
* Utilizzare gli script di esempio nella cartella /src per vedere come sfruttare PDF
* Segui [Blog tecnico Adobe](https://medium.com/adobetech/tagged/adobe-document-cloud) per suggerimenti e trucchi più recenti
* Abbonati a [Clip carta (streaming dal vivo mensile)](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF) informazioni sull&#39;automazione mediante [!DNL Adobe Acrobat Services]. =======
* Scopri di più da [documentazione](https://developer.adobe.com/document-services/docs/overview/)
* Altre esercitazioni su Adobe Experience League
* Utilizzare gli script di esempio nella cartella /src per vedere come sfruttare PDF
* Segui [Blog tecnico Adobe](https://medium.com/adobetech/tagged/adobe-document-cloud) per suggerimenti e trucchi più recenti
* Abbonati a [Clip carta (streaming dal vivo mensile)](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF) informazioni sull&#39;automazione mediante [!DNL Adobe Acrobat Services]
