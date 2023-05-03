---
title: Guida introduttiva alle API di Adobe PDF Services e .Net
description: Gli sviluppatori possono iniziare in pochi minuti con i file di esempio pronti per l'esecuzione forniti per accedere a tutti i servizi Web disponibili
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-6675.jpg
kt: 6675
keywords: In primo piano
exl-id: 22c59c75-fd99-4467-a6f6-917fb246469a
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '524'
ht-degree: 0%

---

# Guida introduttiva all’API dei servizi Adobe PDF e a .Net

![Crea immagine PDF Hero](assets/GettingStartedJava_hero.jpg)

Gli sviluppatori possono iniziare in pochi minuti con i file di esempio pronti per l&#39;esecuzione forniti per accedere a tutti i servizi Web disponibili. Questa esercitazione illustra tutti i passaggi necessari per iniziare a eseguire gli esempi utilizzando il kit di sviluppo software .Net (PDF Services):

## Passaggio 1: Acquisizione delle credenziali e download dei file di esempio

Il primo passo consiste nell&#39;ottenere una credenziale (chiave API) per sbloccare l&#39;uso. [Registrati per la versione di prova gratuita](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) e fai clic su &quot;Inizia&quot; per creare le tue nuove credenziali.

![Passaggio 1](assets/GettingStartedJava_step1.png)

È importante scegliere un &#39;Account personale&#39; per registrarsi alla versione di prova gratuita:

![Personale](assets/GettingStartedJava_personal.png)

Nel passaggio successivo, scegli il servizio API dei servizi di PDF, quindi aggiungi un nome e una descrizione per le tue credenziali.

È disponibile una casella di controllo per &quot;Creare un esempio di codice personalizzato&quot;. Scegliete questa opzione per aggiungere automaticamente le nuove credenziali ai file di esempio, in modo da salvare il passaggio manuale dell’aggiunta al progetto.

Quindi scegli Node.js come lingua per ricevere gli esempi specifici di Node.js e fai clic sul pulsante &quot;Crea credenziali&quot;.

![Credenziali](assets/GettingStartedJava_credentials.png)

Riceverai un file .zip da scaricare denominato PDFToolsSDK-.NetSamples.zip che può essere salvato nel file system locale.

## Passaggio 2: Configurazione dell&#39;ambiente .Net ed esecuzione del codice di esempio

1. Scarica e installa [.Net SDK](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/install)
1. Estrai il file scaricato **[!UICONTROL PDFToolsSDK-.NetSamples.zip]** e decomprimi il contenuto
1. cd nella directory principale samples **[!UICONTROL adobe-DC.PDFTools.SDK.NET.Samples]**
1. Dalla directory principale degli esempi, esegui `dotnet build`

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>build dotnet

   Ora è possibile eseguire i file di esempio.

   Questi passaggi finali mostrano come eseguire il primo esempio con l’operazione Crea PDF da Word:

1. Dalla directory principale di esempio alla cartella CreatePDFFromDocx, cd CreatePDFFromDocx/

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>cd CreatePDFFromDocx/

1. esecuzione `dotnet run CreatePDFFromDocx.csproj`

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples\CreatePDFFromDocx>esecuzione dotnet CreatePDFFromDocx.csproj

Il PDF verrà creato nella posizione indicata nell’output, che per impostazione predefinita corrisponde alla stessa cartella.

## Considerazioni finali

L&#39;API dei servizi PDF può aiutarti a eliminare i processi manuali automatizzando i flussi di lavoro comuni e spostando il carico di elaborazione sul cloud. In un mondo in cui ogni browser tratta la PDF in modo diverso, sfruttando l’API di incorporamento di Adobe PDF insieme all’API dei servizi di PDF, puoi creare processi ottimizzati, affidabili e prevedibili che vengono eseguiti e visualizzati correttamente **ogni volta** indipendentemente dalla piattaforma o dal dispositivo.

## Risorse e passaggi successivi

* Per ulteriore assistenza e supporto, visitare il [[!DNL Adobe Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) forum della community

* API dei servizi PDF [Documentazione](https://www.adobe.com/go/pdftoolsapi_doc)

* [Domande frequenti](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) per domande sull’API di PDF Services

* [Contattaci](https://www.adobe.com/go/pdftoolsapi_requestform) per domande su licenze e prezzi

* Articoli correlati

   [La nuova API dei servizi PDF offre ulteriori funzionalità per i flussi di lavoro basati su documenti](https://community.adobe.com/t5/document-services-apis/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

   [Versione di luglio [!DNL Adobe Acrobat Services]: Servizi di incorporazione e PDF per PDF](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
