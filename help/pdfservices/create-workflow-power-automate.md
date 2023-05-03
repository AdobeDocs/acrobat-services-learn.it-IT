---
title: Crea il tuo primo flusso di lavoro in Microsoft Power Automate
description: Scopri come utilizzare il connettore Adobe PDF Services in Microsoft Power Automate
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-10379.jpg
kt: 10379
exl-id: 095b705f-c380-42cc-9329-44ef7de655ee
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '1999'
ht-degree: 2%

---


# Crea il tuo primo flusso in Microsoft Power Automate

Scopri come creare il tuo primo flusso [Microsoft Power Automate](https://flow.microsoft.com) utilizzando il metodo [Servizi Adobe PDF](https://us.flow.microsoft.com/it-it/connectors/shared_adobepdftools/adobe-pdf-services/) connettore.

In questa esercitazione pratica imparerai come:

* Convertire documenti Word in PDF
* Combinare più documenti PDF in un unico PDF
* Protect un documento PDF con una password

## Preparazione

### Cosa ti serve

* **Credenziali di prova o di produzione per i servizi Adobe PDF**
Scopri come ottenere e configurare le credenziali in Microsoft Power Automate [qui](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfservices/getting-credentials-power-automate.html).
* **Microsoft Power Automate con connettori Premium**
Scopri come controllare il livello di licenza per Power Automate [qui](https://docs.microsoft.com/en-us/power-platform/admin/power-automate-licensing/types).
* **OneDrive**
In questo tutorial viene utilizzato il connettore di archiviazione OneDrive, ma qualsiasi connettore di archiviazione può essere sostituito.

### File di esempio

Ce ne sono due [file di esempio](assets/sample-assets.zip) che devi decomprimere e caricare su OneDrive:

* WordDocument01.docx
* WordDocument02.docx

### Recupero delle credenziali

Per completare questa esercitazione, sono necessarie le credenziali già configurate in Microsoft Power Automate per i servizi Adobe PDF. Se non hai completato questo passaggio, consulta la [istruzioni qui](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfservices/getting-credentials-power-automate.html).

## Parte 1: Creare un nuovo flusso e convertire Word in PDF

### Creare il flusso

In questa parte, viene creato un nuovo flusso [Microsoft Power Automate](https://flow.microsoft.com) con un flusso immediato, aggiungi parametri, scarica i file da OneDrive e convertili in PDF.

1. Passa a [Microsoft Power Automate](https://flow.microsoft.com) e accedi con le tue credenziali.
1. Nella barra laterale, seleziona **[!UICONTROL Crea]**.

   ![Pulsante Crea](assets/createButtonPowerAutomate.png)

1. Seleziona **[!UICONTROL Flusso istantaneo]**.
1. Assegna un nome al tuo flusso.
1. Sotto *Scegli come attivare questo flusso*, seleziona **[!UICONTROL Attivare manualmente un flusso]**.
1. Seleziona **[!UICONTROL Crea]**.

### Ottenere il contenuto dei file

Quindi, ottenere il contenuto dei file di esempio.

>[!PREREQUISITES]
>
>Se non hai caricato il [file di esempio](assets/sample-assets.zip) in OneDrive, decomprimeteli e caricateli.


1. In [Power Automate](https://flow.microsoft.com), seleziona **[!UICONTROL + Nuovo passaggio]**.
1. Cerca *OneDrive* nella barra di ricerca.
1. Scegli il tuo account OneDrive personale o di lavoro selezionando **[!UICONTROL OneDrive for business]** o **[!UICONTROL OneDrive]**.
1. Cerca *Ottenere il contenuto del file* nella barra di ricerca.
1. Nel **[!UICONTROL File]** selezionare l&#39;icona Cartella per passare alla proprietà *WordDocument01.docx* in OneDrive.

   ![Ottieni i contenuti dei file Azione OneDrive in Microsoft Power Automate](assets/getFileContentOneDrive.png)

### Converti file in PDF

Ora che hai il contenuto del file, puoi convertire il documento in PDF.

1. In [Power Automate](https://flow.microsoft.com), seleziona **[!UICONTROL + Nuovo passaggio]**.
1. Cerca *Servizi Adobe PDF* nella barra di ricerca.
1. Seleziona **[!UICONTROL Servizi Adobe PDF]**.
1. Cerca *Converti da Word a PDF* nella barra di ricerca.
1. In **[!UICONTROL Nome file]**, assegna al file il nome desiderato ma deve terminare con *.docx*. Questa estensione è necessaria per convertire i documenti da Word a PDF.
1. Posiziona il cursore nel **[!UICONTROL Contenuto file]** campo.
1. Uso del metodo **[!UICONTROL Contenuti dinamici]** pannello, seleziona **[!UICONTROL Contenuto dei file]**.

   ![Convertire un’azione da Word a PDF in Microsoft Power Automate](assets/convertWordToPDFActionPowerAutomate.png)

### Salvare il file in OneDrive

Una volta generato il documento, salva nuovamente il file in OneDrive.

1. In [Microsoft Power Automate](https://flow.microsoft.com), seleziona **[!UICONTROL + Nuovo passaggio]**.
1. Cerca *OneDrive* nella barra di ricerca.
1. Scegli il tuo account OneDrive personale o di lavoro selezionando **[!UICONTROL OneDrive for business]** o **[!UICONTROL OneDrive]**.
1. Cerca *Ottenere il contenuto del file* nella barra di ricerca.
1. Cerca *Crea file* nella barra di ricerca.
1. Seleziona **[!UICONTROL Crea file]**.
1. Nel **[!UICONTROL Percorso cartella]** selezionare l&#39;icona della cartella per specificare dove salvare il file in OneDrive.
1. In **[!UICONTROL Nome file]**, assegna al file il nome desiderato ma deve terminare con *.docx*. Questa estensione è necessaria per convertire i documenti da Word a PDF.
1. Nel **[!UICONTROL Contenuto file]** campo, uso **[!UICONTROL Contenuti dinamici]** per inserire la variabile PDF File Content.

### Prova flusso

1. In alto a sinistra, seleziona **[!UICONTROL Senza titolo]** per rinominare il flusso.
1. Seleziona **[!UICONTROL Salva]**.
1. Seleziona **[!UICONTROL Test]**.
1. Seleziona **[!UICONTROL Manualmente]** e poi **[!UICONTROL Salva e prova]**.
1. Seleziona **[!UICONTROL Continua]**.
1. Seleziona **[!UICONTROL Flusso di esecuzione]**.

Nella cartella OneDrive, viene visualizzato il PDF convertito.

![Documento PDF convertito selezionato in OneDrive](assets/selectedGeneratedFileInOneDrive.png)

## Parte 2: Generare un documento dinamico da un modello

Questa parte successiva si basa sulla parte 1 e utilizza la proprietà *Genera documento da Word* per unire dinamicamente i dati nel documento.

### Rivedere il modello di documento

Apri *WordDocument02_.docx* dai file di esempio in OneDrive. Il documento Word contiene diversi tag di testo che rappresentano i luoghi in cui i dati vengono inseriti nel documento.

### Aggiungere parametri al trigger

Per inviare i dati dinamici nel documento, dovete creare alcuni parametri affinché il trigger richieda i valori.

1. Quando modificate il flusso, selezionate **[!UICONTROL Attivare manualmente un flusso]** per espandere l&#39;azione.
1. Seleziona **[!UICONTROL Aggiungere un input]**.
1. Seleziona **[!UICONTROL Testo]**.
1. Assegnare un nome al campo *Nome*.

Ripeti i passaggi da 2 a 4 per aggiungere i seguenti campi:

* Cognome
* Salario

![Attivazione in Power Automate con i campi dei parametri](assets/triggerParametersInPowerAutomate.png)

### Ottenere il contenuto di un modello

Per generare un documento, devi prima ottenere il contenuto del file del modello Word.

1. In Power Automate, seleziona + **[!UICONTROL Nuovo passaggio]**.
1. Cerca *OneDrive* nella barra di ricerca.
1. Scegli il tuo account OneDrive personale o di lavoro selezionando **[!UICONTROL OneDrive for business]** o **[!UICONTROL OneDrive]**.
1. Cerca *Ottenere il contenuto del file* nella barra di ricerca.
1. Nel **[!UICONTROL File]** selezionare l&#39;icona Cartella per passare alla proprietà *WordDocument02.docx* in OneDrive.

![Ottieni l&#39;azione sui contenuti dei file da OneDrive in Microsoft Power Automate](assets/getFileContentAction02.png)

### Genera documento da modello

1. In Power Automate, seleziona **[!UICONTROL + Nuovo passaggio]**.
1. Cerca *Servizi Adobe PDF* nella barra di ricerca.
1. Seleziona **[!UICONTROL Servizi Adobe PDF]**.
1. Selezionate la proprietà **[!UICONTROL Generare un documento da un modello Word]** azione .
1. Nel **[!UICONTROL Nome file modello]** , assegnare al file il nome desiderato, ma deve terminare con *.docx*.

#### Unione dati

Uso del metodo *Generare un documento da un modello Word* , potete unire i dati nel documento da una qualsiasi delle diverse variabili presenti in precedenza nel flusso utilizzando il contenuto dinamico.

Copia i dati JSON seguenti nel **Unisci dati** campo:

```
{
    "FirstName": "",
    "LastName": "",
    "Salary": ""
}
```

1. Posiziona il cursore nel campo tra le due virgolette per il *FirstName* valore.
1. Uso del metodo **[!UICONTROL Contenuto dinamico]** , inserire la proprietà *Nome* dall’azione di flusso attivata manualmente.

   ![Generare un documento con tag dati in JSON](assets/generateDocumentJSONAction.png)

1. Ripetere i punti 7-8 per il metodo **[!UICONTROL LastName]** e **[!UICONTROL Salario]** campi.
1. Nel **[!UICONTROL Contenuto file modello]** , utilizzare il metodo **[!UICONTROL Contenuti dinamici]** per inserire la proprietà **[!UICONTROL Contenuto dei file]** dal metodo *Ottenere il contenuto del file* passo.

![Generare un documento da un modello Word in Power Automate con tutti i valori completati](assets/generateDocumentJSONActionCompleted.png)

>[!TIP]
>
>Il *Generare un documento da un modello Word* utilizza l’API di generazione dei documenti di Adobe. Per ulteriori informazioni sulla creazione dei modelli, consultate le risorse seguenti:
>
>* [Ulteriori informazioni sulla generazione di documenti di Adobe](https://developer.adobe.com/document-services/apis/doc-generation/)
>* [Adobe tag Generazione documento per Microsoft Word](https://appsource.microsoft.com/en-US/product/office/WA200002654)
>* [Adobe documentazione API Generazione documento](https://developer.adobe.com/document-services/docs/overview/document-generation-api/)


### Salvare il file in OneDrive

Una volta generato il documento, puoi salvarlo nuovamente in OneDrive.

1. In Power Automate, seleziona **+ [!UICONTROL Nuovo passaggio]**.
1. Cerca *OneDrive* nella barra di ricerca.
1. Scegli il tuo account OneDrive personale o di lavoro selezionando **[!UICONTROL OneDrive for business]** o **[!UICONTROL OneDrive]**.
1. Cerca *Crea file* nella barra di ricerca.
1. Seleziona **[!UICONTROL Crea file]**.
1. Nel **[!UICONTROL Percorso cartella]** selezionare l&#39;icona della cartella per specificare dove salvare il file in OneDrive.
1. Nel **[!UICONTROL Nome file]** , impostare il nome del file. Poiché l’output è un PDF, il nome del file deve terminare con l’estensione .pdf.
1. Utilizzare la proprietà **[!UICONTROL Contenuti dinamici]** per inserire la variabile PDF File Content nella **[!UICONTROL Contenuto file]** campo.

### Prova flusso

![Esegui lo schermo di flusso in Microsoft Power Automate che richiede gli input](assets/runFlowParameters.png)

1. Seleziona **[!UICONTROL Salva]**.
1. Seleziona **[!UICONTROL Test]**.
1. Seleziona **[!UICONTROL Manualmente]** e poi **[!UICONTROL Salva e prova]**.
1. Seleziona **[!UICONTROL Continua]**.
1. Immettere i valori per *Nome*, *Cognome* e *Salario*.
1. Seleziona **[!UICONTROL Flusso di esecuzione]**.

Nella cartella OneDrive viene visualizzato un PDF generato dal documento Word. Quando aprite il documento PDF in OneDrive, i dati vengono uniti nelle posizioni dei tag di testo.


## Parte 3: Combina PDF in uno

Ora che hai generato e convertito un documento Word in un PDF, la parte successiva consiste nel combinare più documenti PDF.

>[!NOTE]
>
>Nelle azioni precedenti, una copia del documento è stata salvata come file in OneDrive. Per utilizzare strumenti quali Unisci PDF, non è necessario salvare il file in OneDrive. Potete invece passare l’output direttamente da un’azione a quella successiva, il che è meglio che salvare in OneDrive dopo ogni azione. Tuttavia, a scopo dimostrativo, questi file vengono salvati in OneDrive.

### Passaggio Aggiungi PDF unione

1. Quando modificate il flusso, selezionate **[!UICONTROL + Passaggio successivo]** per aggiungere un’azione alla fine del flusso.
1. Cerca *Servizi Adobe PDF* nella barra di ricerca.
1. Seleziona **[!UICONTROL Servizi Adobe PDF]**.
1. Selezionate la proprietà **[!UICONTROL Unisci PDF]** azione.
1. Nel **[!UICONTROL Nome file Merge PDF]** , immettere il nome file desiderato (ad esempio,*CombinedDocument.pdf*).
1. Nel **[!UICONTROL Contenuto file -1]** , utilizzare il metodo **[!UICONTROL Contenuti dinamici]** per inserire la proprietà *Contenuto file PDF* dal metodo **[!UICONTROL Converti da Word a PDF]** passo.
1. Per aggiungere il documento successivo, seleziona **+ [!UICONTROL aggiungi nuova voce]**.
1. Nel **[!UICONTROL Contenuto file - 2]** , utilizzare il metodo **[!UICONTROL Contenuti dinamici]** per inserire la proprietà **[!UICONTROL Contenuto file di output]** dal metodo *Generare un documento da un modello Word* passo.

![Azione Unisci PDF in Microsoft Power Automate](assets/mergePDFAction.png)

### Salva PDF unito in OneDrive

Una volta combinato il documento, puoi salvarlo nuovamente in OneDrive.

1. In Power Automate, seleziona **+ [!UICONTROL Nuovo passaggio]**.
1. Cerca *OneDrive* nella barra di ricerca.
1. Scegli il tuo account OneDrive personale o di lavoro selezionando **[!UICONTROL OneDrive for business]** o **[!UICONTROL OneDrive]**.
1. Cerca *Crea file* nella barra di ricerca.
1. Seleziona **[!UICONTROL Crea file]**.
1. Nel **[!UICONTROL Percorso cartella]** selezionare l&#39;icona della cartella per specificare dove salvare il file in OneDrive.
1. Nel **[!UICONTROL Nome file]** , impostare il nome del file. Poiché l’output è un PDF, il nome del file deve terminare con .pdf.
1. Nel **[!UICONTROL Contenuto file]** campo, uso **[!UICONTROL Contenuti dinamici]** per inserire la proprietà *Contenuto file PDF* dal metodo **[!UICONTROL Unisci PDF]** passo.

   ![Panoramica sul flusso in Microsoft Power Automate](assets/flowOverviewSavedMergedDocument.png)

### Prova flusso

1. Seleziona **[!UICONTROL Salva]**.
1. Seleziona **[!UICONTROL Test]**.
1. Seleziona **[!UICONTROL Manualmente]** e poi **[!UICONTROL Salva e prova]**.
1. Seleziona **[!UICONTROL Continua]**.
1. Immettere i valori per *Nome*, *Cognome* e *Salario*.
1. Seleziona **[!UICONTROL Flusso di esecuzione]**.

Nella cartella OneDrive viene visualizzato il PDF combinato con le pagine del primo e del secondo documento.

## Parte 4: documento Protect PDF

Dopo aver generato il documento, potete proteggerlo dalla modifica inserendo un passaggio aggiuntivo prima di salvarlo in OneDrive.

### Proteggere i PDF

1. Durante la modifica del flusso in Power Automate, seleziona **+** tra il **[!UICONTROL Unisci PDF]** e la proprietà **[!UICONTROL Crea file 3]** azione.

   ![Simbolo più tra due azioni per aggiungere una nuova azione](assets/addActionToProtect.png)

1. Seleziona **[!UICONTROL Aggiungere un’azione]**.
1. Cerca *Servizi Adobe PDF* nella barra di ricerca.
1. Seleziona **[!UICONTROL Servizi Adobe PDF]**.
1. Selezionate la proprietà **[!UICONTROL Protect PDF da visualizzazione]** azione.
1. Nel **[!UICONTROL Nome file]** , imposta il nome sul nome desiderato, purché termini con l&#39;estensione .pdf.
1. Impostare la proprietà **[!UICONTROL Password]** alla password specificata per aprire il documento.
1. Nel **[!UICONTROL Contenuto file]** , utilizzare il metodo **[!UICONTROL Contenuto dinamico]** per inserire la proprietà *Contenuto file PDF* dal metodo **[!UICONTROL Unisci PDF]** passo.

### Aggiorna il salvataggio in OneDrive

Una volta protetto il documento, puoi salvarlo nuovamente in OneDrive. In questo esempio, si sta aggiornando la **Crea file 3** con un nuovo *Contenuto file* valore.

1. Selezionate il cursore nel **[!UICONTROL Contenuto file]** nel **[!UICONTROL Crea file 3]** azione.
1. Utilizzare la proprietà **[!UICONTROL Contenuto dinamico]** per inserire la proprietà *Contenuto file PDF* dal metodo **Protect PDF da visualizzazione** passo.

### Prova flusso

1. Seleziona **[!UICONTROL Salva]**.
1. Seleziona **[!UICONTROL Test]**.
1. Seleziona **[!UICONTROL Manualmente]** e poi **[!UICONTROL Salva e prova]**.
1. Seleziona **[!UICONTROL Continua]**.
1. Immettere i valori per *Nome*, *Cognome* e *Salario*.
1. Seleziona **[!UICONTROL Flusso di esecuzione]**.

Nella cartella OneDrive viene visualizzato il PDF combinato che richiede di immettere una password per visualizzare il documento.

## Passaggi successivi

In questa esercitazione, è stato convertito un documento Word in un PDF, generato un documento in base ai dati, unito i documenti e protetto con una password. Per ulteriori informazioni, esplora alcune delle altre azioni disponibili nel connettore Adobe PDF Services in Microsoft Power Automate:

* Visualizza i modelli predefiniti disponibili in Microsoft Power Automate.
* Impara da [articoli](https://medium.com/adobetech/tagged/microsoft-power-automate) sul blog di Adobe Tech.
* Revisione [documentazione](https://developer.adobe.com/document-services/docs/overview/document-generation-api/) ad Adobe, API per la generazione di documenti.
