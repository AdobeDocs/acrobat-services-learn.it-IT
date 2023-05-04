---
title: Uso dell’API dei servizi PDF per esportare PDF in Word, PowerPoint e altro
description: Scoprite come eseguire l’operazione di esportazione API di PDF Services utilizzando file di esempio per le lingue Node.js, Java e .Net
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-6674.jpg
kt: 6674
exl-id: 55f5b04e-0249-47d9-9131-2f9ec01db7e8
source-git-commit: aa5c88fb5725a3d1c50d5c6b73fce7add629b08d
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 5%

---

# Uso dell’API dei servizi PDF per esportare PDF in Word, PowerPoint e altro

![Crea immagine PDF Hero](assets/ExportPDF_hero.jpg)

L’API dei servizi Adobe PDF converte i file PDF in MS Office, testo e immagini utilizzando le API. Esistono molti casi d&#39;uso comuni per sbloccare i PDF esistenti per l&#39;elaborazione e l&#39;analisi dei contenuti e gli sviluppatori di API di PDF Services possono facilmente integrare questa funzionalità nei sistemi e nelle applicazioni esistenti. Converti i file PDF in MS Word per la modifica di contenuti, approvazioni e l&#39;invio successivo di firme per creare flussi di lavoro personalizzati per i contratti. Oppure esporta i contenuti PDF in formato MS Excel per calcoli di fatturazione e finanziari o analisi dei dati.

L’operazione di esportazione supporta le seguenti conversioni di file PDF:

* Da PDF a Microsoft Word (DOC, DOCX)
* Da PDF a Microsoft PowerPoint (PPTX)
* Da PDF a Microsoft Excel (XLSX)
* PDF al testo (RTF)
* PDF a immagine (JPEG, PNG)

In questa esercitazione, scopri le nozioni di base su come eseguire la tua prima operazione di esportazione API per PDF Services utilizzando file di esempio per le lingue Node.js, Java e .Net.

## Passaggio 1: Creare le credenziali e configurare l&#39;ambiente:

Utilizza le esercitazioni introduttive riportate di seguito per creare le credenziali API, scaricare file di esempio e configurare l’ambiente.

[Guida introduttiva all’API per servizi PDF e a Java](gettingstartedjava.md)

[Guida introduttiva all’API di PDF Services e a .Net](gettingstartednet.md)

[Guida introduttiva all’API dei servizi PDF e a Node.js](createpdffromhtml.md)

## Passaggio 2: Eseguire l’operazione di esportazione PDF utilizzando i file di esempio

**Java**

1. Aprite un prompt dei comandi.

1. Cambia le directory nella directory del codice di esempio.

   Ad esempio, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-java-samples

1. Eseguite il comando seguente:

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.exportpdf.ExportPDFToDOCX`

Il PDF viene creato nella directory src/main/resources.

**.Net**

1. Aprite un prompt dei comandi.

1. Cambia le directory nella directory del codice di esempio.

   Ad esempio, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. Modificate nuovamente le directory nella directory ExportPDFtoDocx.

1. Eseguite il comando seguente:

   `dotnet run ExportPDFToDocx.csproj`

Il PDF viene creato nella stessa directory.

**Node.js**

1. Aprite un prompt dei comandi.

1. Cambia le directory nella directory del codice di esempio.

   Ad esempio, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. Eseguite il comando seguente:

   `node src/ocr/ocr-pdf.js`

Il PDF viene creato nella posizione specificata nell&#39;output, che per impostazione predefinita è la directory pdfServicesSdkResult.

## Considerazioni finali

Ora è necessario disporre di un esempio funzionante che può essere importato nelle applicazioni esistenti per avviare una prova del concetto. In ognuna delle directory di esempio, è disponibile un altro esempio per esportare i file PDF in formato immagine. Gli stessi passaggi precedenti consentono di eseguire anche l’esempio. Per passare a un altro formato, potete aggiornare il codice nel nuovo formato desiderato:

SupportedTargetFormats.PPTX

E il risultato di destinazione:

output/exportPdfOutput.PPTX

In un altro formato.

## Risorse e passaggi successivi

* Per ulteriore assistenza e supporto, visitare il [[!DNL Adobe Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) forum della community

* API dei servizi PDF [Documentazione](https://www.adobe.com/go/pdftoolsapi_doc)

* [Domande frequenti](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) per domande sull’API di PDF Services

* [Contattaci](https://www.adobe.com/go/pdftoolsapi_requestform) per domande su licenze e prezzi
