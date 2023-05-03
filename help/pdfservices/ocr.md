---
title: Uso dell’API dei servizi Adobe PDF per i file PDF OCR
description: Grazie all’OCR (Optical Character Recognition) è possibile sbloccare i PDF scansionati per estrarre il testo e creare file ricercabili
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-6677.jpg
kt: 6677
keywords: Eroe
exl-id: 61a9a2d1-94c3-41c2-8f90-a56a938ef245
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 4%

---

# Uso dell’API dei servizi Adobe PDF per i file PDF OCR

![Crea immagine PDF Hero](assets/OCR_hero.jpg)

Con l’OCR (Optical Character Recognition) potete sbloccare i PDF scansionati per estrarre il testo e creare file ricercabili. Grazie alle nostre avanzate API basate su cloud, integra l&#39;OCR in qualsiasi flusso di lavoro basato su documenti per offrire la soluzione perfetta per l&#39;archiviazione, la copia del testo e la creazione di indici di documenti ricercabili. Create archivi ricercabili da archivi di PDF scansionati per sbloccare informazioni importanti e risparmiare tempo grazie alla possibilità di eseguire ricerche rapide. Oppure applica l’OCR ai tuoi PDF dalle scansioni caricate per consentirne la modifica per i flussi di lavoro di onboarding.

Gli sviluppatori possono iniziare in pochi minuti con i file di esempio pronti per l’esecuzione forniti per l’OCR.

In questa esercitazione sono illustrate le nozioni di base su come eseguire la prima operazione OCR API di PDF Services utilizzando file di esempio per le lingue Node.js, Java e .Net.

## Passaggio 1: Creare le credenziali e configurare l&#39;ambiente

Utilizza le esercitazioni introduttive riportate di seguito per creare le credenziali API, scaricare file di esempio e configurare l’ambiente.

[Guida introduttiva all’API per servizi PDF e a Java](gettingstartedjava.md)
[Guida introduttiva all’API di PDF Services e a .Net](gettingstartednet.md)
[Guida introduttiva all’API dei servizi PDF e a Node.js](createpdffromhtml.md)

## Eseguire l&#39;esempio OCR fornito nei file di esempio

L’operazione OCR consente di utilizzare impostazioni internazionali in inglese per impostazione predefinita, ma fornisce anche supporto per tedesco, francese, danese e [altre lingue](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#ocr-with-explicit-language). L&#39;impostazione predefinita è impostazioni internazionali en-us.

Quando si passano le opzioni con l&#39;operazione OCR, comprese impostazioni internazionali specifiche, il metodo accetta anche il parametro &#39;type&#39; che dispone di due opzioni:

* SEARCHABLE_IMAGE: Consente di modificare l’immagine originale durante il processo di ottimizzazione (ad esempio, di convertirla in desktop) prima di posizionare sopra di essa un livello di testo invisibile. Questo tipo rimuove gli artefatti indesiderati e in alcuni scenari può risultare più leggibile.

* SEARCHABLE_IMAGE_EXACT: Assicura che il testo sia ricercabile e selezionabile. Questa opzione mantiene l’immagine originale e vi posiziona sopra un livello di testo invisibile. Consigliato per i casi che richiedono la massima fedeltà all’immagine originale.

**Java**

1. Aprite un prompt dei comandi.

1. Cambia le directory nella directory del codice di esempio.

   Ad esempio, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-java-samples>.

1. Eseguite il comando seguente:

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.ocrpdf.OcrPDF`

Il PDF verrà creato nella directory src/main/resources.

**.Net**

1. Aprite un prompt dei comandi.

1. Cambia le directory nella directory del codice di esempio.

   Ad esempio, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. Cambia nuovamente le directory nella directory OcrPDF.

1. Eseguite il comando seguente:

   `dotnet run OcrPDF.csproj`

Il PDF verrà creato nella stessa directory.

**Node.js**

1. Aprite un prompt dei comandi.

1. Cambia le directory nella directory del codice di esempio.

   Ad esempio, C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. Eseguite il comando seguente:

   `node src/ocr/ocr-pdf.js`

Il PDF verrà creato nella posizione specificata nell&#39;output, che per impostazione predefinita è la directory di output.

## Considerazioni finali

Con questi semplici passaggi utilizzando i file di esempio, è necessario avere un esempio funzionante su cui basarsi. Oltre all&#39;esempio OCR utilizzato in questa esercitazione, è disponibile un altro esempio di OCR mediante le opzioni di testo e impostazioni internazionali supportate descritte in precedenza.

Da qui puoi semplicemente sostituire i file di input e di output che si trovano nel campione per utilizzare il tuo PDF e finalizzare la dimostrazione di concetto per il tuo caso d&#39;uso.

![Proof of Concept](assets/OCR_poc.png)

## Risorse e passaggi successivi

* Per ulteriore assistenza e supporto, visita l&#39;Adobe [[!DNL Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) forum della community

* API dei servizi PDF [Documentazione](https://www.adobe.com/go/pdftoolsapi_doc)

* [Domande frequenti](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) per domande sull’API di PDF Services

* [Contattaci](https://www.adobe.com/go/pdftoolsapi_requestform) per domande su licenze e prezzi
