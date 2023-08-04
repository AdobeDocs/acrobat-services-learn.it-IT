---
title: Utilizzo dell'API di PDF Services per esportare PDF in Word, PowerPoint e altri formati
description: Scopri come eseguire l’operazione di esportazione delle API di PDF Services utilizzando file di esempio per i linguaggi Node.js, Java e .Net
feature: PDF Services API
role: Developer
level: Intermediate
type: Tutorial
jira: KT-6674
thumbnail: KT-6674.jpg
exl-id: 55f5b04e-0249-47d9-9131-2f9ec01db7e8
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 5%

---

# Utilizzo dell&#39;API di PDF Services per esportare PDF in Word, PowerPoint e altri formati

![Crea immagine PDF Hero](assets/ExportPDF_hero.jpg)

L’API di Adobe PDF Services converte i file PDF in MS Office, testo e immagini utilizzando le API. Esistono molti casi d&#39;uso comuni per sbloccare i PDF esistenti per la modifica e l&#39;analisi dei contenuti e con PDF Services gli sviluppatori di API possono integrare facilmente questa funzionalità nei sistemi e nelle applicazioni esistenti. Converti i file PDF in MS Word per modificare il contenuto, approvare e inviare successivamente per le firme, al fine di creare flussi di lavoro per contratti personalizzati. Oppure esporta il contenuto di PDF in formato MS Excel per i calcoli di fatture e finanziari o l&#39;analisi dei dati.

L’operazione di esportazione supporta le seguenti conversioni di file PDF:

* Da PDF a Microsoft Word (DOC, DOCX)
* Da PDF a Microsoft PowerPoint (PPTX)
* Da PDF a Microsoft Excel (XLSX)
* Da PDF a testo (RTF)
* Da PDF a immagine (JPEG, PNG)

In questo tutorial, scopri le nozioni di base su come eseguire la prima operazione di esportazione delle API di PDF Services utilizzando file di esempio per i linguaggi Node.js, Java e .Net.

## Passaggio 1: crea le tue credenziali e configura il tuo ambiente:

Utilizza le esercitazioni introduttive riportate di seguito per creare le credenziali API, scaricare file di esempio e configurare l’ambiente.

[Guida introduttiva all’API e a Java di PDF Services](gettingstartedjava.md)

[Guida introduttiva all&#39;API di PDF Services e a .Net](gettingstartednet.md)

[Guida introduttiva all’API di PDF Services e a Node.js](createpdffromhtml.md)

## Passaggio 2: esegui l’operazione di esportazione dei PDF utilizzando i file di esempio

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

1. Cambia nuovamente le directory nella directory ExportPDFtoDocx.

1. Eseguite il comando seguente:

   `dotnet run ExportPDFToDocx.csproj`

Il tuo PDF viene creato nella stessa directory.

**Node.js**

1. Aprite un prompt dei comandi.

1. Cambia le directory nella directory del codice di esempio.

   Ad esempio, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. Eseguite il comando seguente:

   `node src/ocr/ocr-pdf.js`

Il PDF viene creato nel percorso indicato nell&#39;output, che per impostazione predefinita è la directory pdfServicesSdkResult.

## Pensieri finali

Ora dovresti avere un esempio che funzioni e che possa essere importato nelle tue applicazioni esistenti per avviare una prova di concetto. In ciascuna delle directory di esempio, puoi visualizzare un altro esempio per esportare i file PDF in formato immagine. Gli stessi passaggi precedenti consentono di eseguire anche quel campione. Per passare a un altro formato, è possibile aggiornare il codice al nuovo formato desiderato:

SupportedTargetFormats.PPTX

E il risultato di destinazione:

output/exportPdfOutput.PPTX

In un altro formato.

## Risorse e passaggi successivi

* Per ulteriore assistenza e supporto, visita [[!DNL Adobe Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) forum della community

* API di PDF Services [Documentazione](https://www.adobe.com/go/pdftoolsapi_doc)

* [DOMANDE FREQUENTI](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) per domande sulle API di PDF Services

* [Contattaci](https://www.adobe.com/go/pdftoolsapi_requestform) per domande su licenze e prezzi
