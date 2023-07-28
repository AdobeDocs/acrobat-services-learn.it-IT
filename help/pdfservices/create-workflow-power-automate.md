---
title: Crea il tuo primo flusso di lavoro in Microsoft Power Automate
description: Scopri come utilizzare il connettore Adobe PDF Services in Microsoft Power Automate
type: Tutorial
role: Developer
level: Beginner
feature: PDF Services API
thumbnail: KT-10379.jpg
kt: 10379
exl-id: 095b705f-c380-42cc-9329-44ef7de655ee
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '1999'
ht-degree: 2%

---


# Crea il tuo primo flusso in Microsoft Power Automate

Scopri come creare il tuo primo flusso in [Microsoft Power Automate](https://flow.microsoft.com) utilizzando il [Servizi Adobe PDF](https://us.flow.microsoft.com/it-it/connectors/shared_adobepdftools/adobe-pdf-services/) connettore.

In questo tutorial pratico scopri come:

* Convertire documenti di Word in PDF
* Combinare documenti PDF in un unico PDF
* Protect un documento PDF con una password

## Preparazione

### Cosa serve

* **Credenziali di prova o produzione per Adobe PDF Services**
Ulteriori informazioni su come ottenere e configurare le credenziali in Microsoft Power Automate [qui](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfservices/getting-credentials-power-automate.html).
* **Microsoft Power Automate con connettori Premium**
Scopri come controllare il livello di licenza per Power Automate [qui](https://docs.microsoft.com/en-us/power-platform/admin/power-automate-licensing/types).
* **OneDrive**
In questo tutorial viene utilizzato il connettore di archiviazione OneDrive, ma è possibile sostituire qualsiasi connettore di archiviazione.

### File di esempio

Ce ne sono due [file di esempio](assets/sample-assets.zip) che devi decomprimere e caricare in OneDrive:

* WordDocument01.docx
* WordDocument02.docx

### Recupero delle credenziali

Per completare questa esercitazione, è necessario che le credenziali siano già configurate in Microsoft Power Automate per i servizi Adobe PDF. Se non hai completato questo passaggio, consulta la [istruzioni qui](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfservices/getting-credentials-power-automate.html).

## Parte 1: Creare un nuovo flusso e convertire Word in PDF

### Creare il flusso

In questa parte, viene creato un nuovo flusso in [Microsoft Power Automate](https://flow.microsoft.com) utilizzando un flusso istantaneo, aggiungi parametri, ottieni i tuoi file da OneDrive e convertili in PDF.

1. Passa a [Microsoft Power Automate](https://flow.microsoft.com) e accedi con le tue credenziali.
1. Nella barra laterale, seleziona **[!UICONTROL Crea]**.

   ![Pulsante Crea](assets/createButtonPowerAutomate.png)

1. Seleziona **[!UICONTROL Flusso istantaneo]**.
1. Assegna un nome al flusso.
1. Sotto *Scegli come attivare questo flusso*, seleziona **[!UICONTROL Attivare manualmente un flusso]**.
1. Seleziona **[!UICONTROL Crea]**.

### Ottieni il contenuto dei file

Quindi, ottieni il contenuto dei file di esempio.

>[!PREREQUISITES]
>
>Se non è stato caricato il [file di esempio](assets/sample-assets.zip) in OneDrive, decomprimerli e caricarli.


1. Ingresso [Power Automate](https://flow.microsoft.com), seleziona **[!UICONTROL + Nuovo passaggio]**.
1. Cerca *OneDrive* nella barra di ricerca.
1. Scegli il tuo account OneDrive personale o aziendale selezionando **[!UICONTROL OneDrive for Business]** oppure **[!UICONTROL OneDrive]**.
1. Cerca *Ottieni contenuto file* nella barra di ricerca.
1. Nella **[!UICONTROL File]** , seleziona l&#39;icona Cartella per accedere al *WordDocument01.docx* file in OneDrive.

   ![Azione Ottieni contenuto file OneDrive in Microsoft Power Automate](assets/getFileContentOneDrive.png)

### Converti file in PDF

Ora che hai il contenuto del file, puoi convertire il documento in PDF.

1. Ingresso [Power Automate](https://flow.microsoft.com), seleziona **[!UICONTROL + Nuovo passaggio]**.
1. Cerca *Servizi Adobe PDF* nella barra di ricerca.
1. Seleziona **[!UICONTROL Servizi Adobe PDF]**.
1. Cerca *Converti da Word a PDF* nella barra di ricerca.
1. Ingresso **[!UICONTROL Nome file]**, assegna al file il nome desiderato, ma deve terminare con *.docx*. Questa estensione è necessaria per la conversione di documenti da Word a PDF.
1. Posizionare il cursore nel **[!UICONTROL Contenuto del file]** campo.
1. Utilizzo di **[!UICONTROL Contenuto dinamico]** , seleziona **[!UICONTROL Contenuto del file]**.

   ![Azione Converti da Word a PDF in Microsoft Power Automate](assets/convertWordToPDFActionPowerAutomate.png)

### Salva il file in OneDrive

Una volta generato il documento, salva nuovamente il file in OneDrive.

1. Ingresso [Microsoft Power Automate](https://flow.microsoft.com), seleziona **[!UICONTROL + Nuovo passaggio]**.
1. Cerca *OneDrive* nella barra di ricerca.
1. Scegli il tuo account OneDrive personale o aziendale selezionando **[!UICONTROL OneDrive for Business]** oppure **[!UICONTROL OneDrive]**.
1. Cerca *Ottieni contenuto file* nella barra di ricerca.
1. Cerca *Crea file* nella barra di ricerca.
1. Seleziona **[!UICONTROL Crea file]**.
1. Nella **[!UICONTROL Percorso cartella]** , seleziona l’icona della cartella per specificare dove salvare il file in OneDrive.
1. Ingresso **[!UICONTROL Nome file]**, assegna al file il nome desiderato, ma deve terminare con *.docx*. Questa estensione è necessaria per la conversione di documenti da Word a PDF.
1. Nella **[!UICONTROL Contenuto del file]** campo, uso **[!UICONTROL Contenuto dinamico]** per inserire la variabile PDF File Content.

### Prova flusso

1. In alto a sinistra, seleziona **[!UICONTROL Senza titolo]** per rinominare il flusso.
1. Seleziona **[!UICONTROL Salva]**.
1. Seleziona **[!UICONTROL Test]**.
1. Seleziona **[!UICONTROL Manualmente]** e poi **[!UICONTROL Salva e prova]**.
1. Seleziona **[!UICONTROL Continua]**.
1. Seleziona **[!UICONTROL Esegui flusso]**.

Nella cartella OneDrive, ora dovresti vedere il PDF convertito.

![Documento PDF convertito selezionato in OneDrive](assets/selectedGeneratedFileInOneDrive.png)

## Parte 2: Generare un documento dinamico da un modello

Questa parte successiva si basa sulla Parte 1 e utilizza *Genera documento da Word* per unire dinamicamente i dati nel documento.

### Rivedere il modello di documento

Apri *WordDocument02_.docx* dai file di esempio in OneDrive. Il documento di Word contiene diversi tag di testo che rappresentano posizioni in cui i dati vengono inseriti nel documento.

### Aggiungi parametri al trigger

Per inviare dati dinamici nel documento, è necessario creare alcuni parametri affinché il trigger richieda i valori.

1. Quando modifichi il flusso, seleziona **[!UICONTROL Attivare manualmente un flusso]** per espandere l&#39;azione.
1. Seleziona **[!UICONTROL Aggiungi un input]**.
1. Seleziona **[!UICONTROL Testo]**.
1. Assegna un nome al campo *Nome*.

Ripeti i passaggi da 2 a 4 per aggiungere i seguenti campi:

* Cognome
* Stipendio

![Attivazione in Power Automate con campi parametro](assets/triggerParametersInPowerAutomate.png)

### Ottenere il contenuto di un modello

Per generare un documento, devi prima ottenere il contenuto del file del modello Word.

1. In Power Automate, seleziona + **[!UICONTROL Nuovo passaggio]**.
1. Cerca *OneDrive* nella barra di ricerca.
1. Scegli il tuo account OneDrive personale o aziendale selezionando **[!UICONTROL OneDrive for Business]** oppure **[!UICONTROL OneDrive]**.
1. Cerca *Ottieni contenuto file* nella barra di ricerca.
1. Nella **[!UICONTROL File]** , seleziona l&#39;icona Cartella per accedere al *WordDocument02.docx* file in OneDrive.

![Ottieni l’azione sul contenuto dei file da OneDrive in Microsoft Power Automate](assets/getFileContentAction02.png)

### Genera documento da modello

1. In Power Automate, seleziona **[!UICONTROL + Nuovo passaggio]**.
1. Cerca *Servizi Adobe PDF* nella barra di ricerca.
1. Seleziona **[!UICONTROL Servizi Adobe PDF]**.
1. Selezionare il **[!UICONTROL Genera documento da modello Word]** azione.
1. Nella **[!UICONTROL Nome file modello]** , assegna al file il nome desiderato, ma deve terminare con *.docx*.

#### Unisci dati

Utilizzo di *Genera documento da modello Word* azione, è possibile unire i dati nel documento da una qualsiasi delle diverse variabili precedentemente nel flusso utilizzando Contenuto dinamico.

Copia i dati JSON di seguito nella **Unisci dati** campo:

```
{
    "FirstName": "",
    "LastName": "",
    "Salary": ""
}
```

1. Posizionare il cursore nel campo tra le due virgolette per *FirstName* valore.
1. Utilizzo di **[!UICONTROL Contenuto dinamico]** , inserisci il *Nome* dall&#39;azione Attiva manualmente un flusso.

   ![Genera documento con tag dati in JSON](assets/generateDocumentJSONAction.png)

1. Ripetere i passaggi da 7 a 8 per **[!UICONTROL LastName]** e **[!UICONTROL Stipendio]** campi.
1. Nella **[!UICONTROL Contenuto file modello]** , utilizza il **[!UICONTROL Contenuto dinamico]** per inserire il **[!UICONTROL Contenuto del file]** valore dal *Ottieni contenuto file* passaggio.

![Azione Genera documento da modello Word in Power Automate con tutti i valori completati](assets/generateDocumentJSONActionCompleted.png)

>[!TIP]
>
>La *Genera documento da modello Word* L’azione utilizza l’API di Document Generation di Adobe. Per ulteriori informazioni sulla creazione dei modelli, di seguito sono riportate alcune risorse:
>
>* [Ulteriori informazioni sulla generazione di documenti di Adobe](https://developer.adobe.com/document-services/apis/doc-generation/)
>* [Adobe di Tagger Document Generation per Microsoft Word](https://appsource.microsoft.com/en-US/product/office/WA200002654)
>* [Adobe della documentazione dell’API di Document Generation](https://developer.adobe.com/document-services/docs/overview/document-generation-api/)

### Salva il file in OneDrive

Una volta generato il documento, puoi salvare nuovamente il file in OneDrive.

1. In Power Automate, seleziona **+ [!UICONTROL Nuovo passaggio]**.
1. Cerca *OneDrive* nella barra di ricerca.
1. Scegli il tuo account OneDrive personale o aziendale selezionando **[!UICONTROL OneDrive for Business]** oppure **[!UICONTROL OneDrive]**.
1. Cerca *Crea file* nella barra di ricerca.
1. Seleziona **[!UICONTROL Crea file]**.
1. Nella **[!UICONTROL Percorso cartella]** , seleziona l’icona della cartella per specificare dove salvare il file in OneDrive.
1. Nella **[!UICONTROL Nome file]** , imposta il nome del file. Poiché l’output è un PDF, il nome del file deve terminare con l’estensione .pdf.
1. Utilizzare la **[!UICONTROL Contenuto dinamico]** per inserire la variabile PDF File Content nel **[!UICONTROL Contenuto del file]** campo.

### Prova flusso

![Esegui la schermata di flusso in Microsoft Power Automate che richiede gli input](assets/runFlowParameters.png)

1. Seleziona **[!UICONTROL Salva]**.
1. Seleziona **[!UICONTROL Test]**.
1. Seleziona **[!UICONTROL Manualmente]** e poi **[!UICONTROL Salva e prova]**.
1. Seleziona **[!UICONTROL Continua]**.
1. Immetti i valori per *Nome*, *Cognome* e *Stipendio*.
1. Seleziona **[!UICONTROL Esegui flusso]**.

Nella cartella OneDrive, ora viene visualizzato un PDF generato dal documento Word. Quando apri il documento PDF in OneDrive, i dati vengono uniti nelle posizioni dei tag di testo.


## Parte 3: Combinare le PDF in un&#39;unica soluzione

Dopo aver generato e convertito un documento di Word in un PDF, la parte successiva consiste nel combinare più documenti di PDF insieme.

>[!NOTE]
>
>Nelle azioni precedenti, hai salvato una copia del documento come file in OneDrive. Per utilizzare strumenti come Unisci PDF, non è necessario salvare il file in OneDrive. Puoi passare l’output direttamente da un’azione all’altra, il che è meglio del salvataggio in OneDrive dopo ogni azione. Tuttavia, per scopi dimostrativi, devi salvare questi file in OneDrive.

### Passaggio Aggiungi Merge PDF

1. Quando modifichi il flusso, seleziona **[!UICONTROL + Passaggio successivo]** per aggiungere un’azione alla fine del flusso.
1. Cerca *Servizi Adobe PDF* nella barra di ricerca.
1. Seleziona **[!UICONTROL Servizi Adobe PDF]**.
1. Selezionare il **[!UICONTROL Unisci PDF]** azione.
1. Nella **[!UICONTROL Nome file di merge PDF]** , inserisci il nome del file desiderato (ad esempio,*CombinedDocument.pdf*).
1. Nella **[!UICONTROL Contenuto file -1]** , utilizza il **[!UICONTROL Contenuto dinamico]** per inserire il *Contenuto file PDF* valore dal **[!UICONTROL Converti da Word a PDF]** passaggio.
1. Per aggiungere il documento successivo, seleziona **+ [!UICONTROL aggiungi nuovo elemento]**.
1. Nella **[!UICONTROL Contenuto file - 2]** , utilizza il **[!UICONTROL Contenuto dinamico]** per inserire il **[!UICONTROL Contenuto file di output]** valore dal *Genera documento da modello Word* passaggio.

![Azione Unisci PDF in Microsoft Power Automate](assets/mergePDFAction.png)

### Salva PDF unito in OneDrive

Una volta combinato il documento, è possibile salvarlo nuovamente in OneDrive.

1. In Power Automate, seleziona **+ [!UICONTROL Nuovo passaggio]**.
1. Cerca *OneDrive* nella barra di ricerca.
1. Scegli il tuo account OneDrive personale o aziendale selezionando **[!UICONTROL OneDrive for Business]** oppure **[!UICONTROL OneDrive]**.
1. Cerca *Crea file* nella barra di ricerca.
1. Seleziona **[!UICONTROL Crea file]**.
1. Nella **[!UICONTROL Percorso cartella]** , seleziona l’icona della cartella per specificare dove salvare il file in OneDrive.
1. Nella **[!UICONTROL Nome file]** , imposta il nome del file. Poiché l’output è un PDF, il nome del file deve terminare con .pdf.
1. Nella **[!UICONTROL Contenuto del file]** campo, uso **[!UICONTROL Contenuto dinamico]** per inserire il *Contenuto file PDF* valore dal **[!UICONTROL Unisci PDF]** passaggio.

   ![Panoramica sul flusso in Microsoft Power Automate](assets/flowOverviewSavedMergedDocument.png)

### Prova flusso

1. Seleziona **[!UICONTROL Salva]**.
1. Seleziona **[!UICONTROL Test]**.
1. Seleziona **[!UICONTROL Manualmente]** e poi **[!UICONTROL Salva e prova]**.
1. Seleziona **[!UICONTROL Continua]**.
1. Immetti i valori per *Nome*, *Cognome* e *Stipendio*.
1. Seleziona **[!UICONTROL Esegui flusso]**.

Nella cartella OneDrive, puoi visualizzare il PDF combinato con le pagine del primo e del secondo documento.

## Parte 4: Documento Protect PDF

Dopo aver generato il documento, puoi proteggerlo dalle modifiche includendo un passaggio aggiuntivo prima di salvarlo in OneDrive.

### Proteggere i PDF

1. Durante la modifica del flusso in Power Automate, seleziona **+** nel mezzo **[!UICONTROL Unisci PDF]** azione e **[!UICONTROL Crea file 3]** azione.

   ![Simbolo più tra due azioni per aggiungere una nuova azione](assets/addActionToProtect.png)

1. Seleziona **[!UICONTROL Aggiungere un’azione]**.
1. Cerca *Servizi Adobe PDF* nella barra di ricerca.
1. Seleziona **[!UICONTROL Servizi Adobe PDF]**.
1. Selezionare il **[!UICONTROL Protect PDF dalla visualizzazione]** azione.
1. Nella **[!UICONTROL Nome file]** , imposta il nome desiderato, a condizione che termini con l’estensione .pdf.
1. Impostare la **[!UICONTROL Password]** per aprire il documento.
1. Nella **[!UICONTROL Contenuto del file]** , utilizza il **[!UICONTROL Contenuto dinamico]** per inserire il *Contenuto file PDF* valore dal **[!UICONTROL Unisci PDF]** passaggio.

### Aggiorna salvataggio in OneDrive

Una volta protetto il documento, puoi salvare di nuovo il file in OneDrive. In questo esempio si sta aggiornando il **Crea file 3** azione con una nuova *Contenuto del file* valore.

1. Selezionare il cursore nel **[!UICONTROL Contenuto del file]** nel campo **[!UICONTROL Crea file 3]** azione.
1. Utilizzare la **[!UICONTROL Contenuto dinamico]** per inserire il *Contenuto file PDF* valore dal **Protect PDF dalla visualizzazione** passaggio.

### Prova flusso

1. Seleziona **[!UICONTROL Salva]**.
1. Seleziona **[!UICONTROL Test]**.
1. Seleziona **[!UICONTROL Manualmente]** e poi **[!UICONTROL Salva e prova]**.
1. Seleziona **[!UICONTROL Continua]**.
1. Immetti i valori per *Nome*, *Cognome* e *Stipendio*.
1. Seleziona **[!UICONTROL Esegui flusso]**.

Nella cartella OneDrive, viene visualizzato il PDF combinato che ora richiede di immettere una password per visualizzare il documento.

## Passaggi successivi

In questo tutorial, è stato convertito un documento Word in PDF, è stato generato un documento basato su dati, sono stati uniti documenti e protetto con una password. Per ulteriori informazioni, esplora alcune delle altre azioni disponibili nel connettore Adobe PDF Services in Microsoft Power Automate:

* Visualizza i modelli predefiniti disponibili in Microsoft Power Automate.
* Impara da [articoli](https://medium.com/adobetech/tagged/microsoft-power-automate) sul blog tecnico Adobe.
* Revisione [documentazione](https://developer.adobe.com/document-services/docs/overview/document-generation-api/) ad Adobe l’API di Document Generation.
