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
TQID: https://experienceleague.adobe.com/CV-KH0fg1Tjnr7eCmNaFMtttWO1QUCyjfyIfwg1Vqm0
product_v2: id: acdc2bde-2937-4877-90d9-031dd66278c9
feature_v2: id: b1809bd0-a86b-4991-8083-2e3b517fc3b8id: c4d07275-6387-4756-8bf7-681e581ffd27
subfeature_v2: id: c6f72a9c-54c4-4933-93c9-d7c656ff1f14
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2: id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
source-git-commit: 0110d2606056220c4236fe2f0e3afbfc112746e7
workflow-type: tm+mt
source-wordcount: 524
ht-degree: 2%

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

1. Esegui il comando seguente:

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.exportpdf.ExportPDFToDOCX`

Il PDF viene creato nella directory src/main/resources.

**.Net**

1. Aprite un prompt dei comandi.

1. Cambia le directory nella directory del codice di esempio.

   Ad esempio, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. Cambia nuovamente le directory nella directory ExportPDFtoDocx.

1. Esegui il comando seguente:

   `dotnet run ExportPDFToDocx.csproj`

Il tuo PDF viene creato nella stessa directory.

**Nodo.js**

1. Aprite un prompt dei comandi.

1. Cambia le directory nella directory del codice di esempio.

   Ad esempio, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. Esegui il comando seguente:

   `node src/ocr/ocr-pdf.js`

Il PDF viene creato nel percorso indicato nell&#39;output, che per impostazione predefinita è la directory pdfServicesSdkResult.

## Pensieri finali

Ora dovresti avere un esempio che funzioni e che possa essere importato nelle tue applicazioni esistenti per avviare una prova di concetto. In ciascuna delle directory di esempio, puoi visualizzare un altro esempio per esportare i file PDF in formato immagine. Gli stessi passaggi precedenti consentono di eseguire anche quel campione. Per passare a un altro formato, è possibile aggiornare il codice al nuovo formato desiderato:

SupportedTargetFormats.PPTX

E il risultato di destinazione:

output/exportPdfOutput.PPTX

In un altro formato.

## Risorse e passaggi successivi

* Per ulteriore assistenza e supporto, visita il forum della community [[!DNL Adobe Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&sort=latest_replies&filter=all)

* API dei servizi PDF [Documentazione](https://www.adobe.com/go/pdftoolsapi_doc)

* [Domande frequenti](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197) per le domande sull&#39;API di PDF Services

* [Contattaci](https://www.adobe.com/go/pdftoolsapi_requestform) per domande su licenze e prezzi
