---
title: Guida introduttiva all'API dei servizi Adobe PDF e a .Net
description: Gli sviluppatori possono iniziare in pochi minuti con i file di esempio pronti per l'esecuzione forniti per accedere a tutti i servizi Web disponibili
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6675
thumbnail: KT-6675.jpg
keywords: In evidenza
exl-id: 22c59c75-fd99-4467-a6f6-917fb246469a
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 0%

---

# Guida introduttiva all&#39;API di Adobe PDF Services e a .Net

![Crea immagine PDF Hero](assets/GettingStartedJava_hero.jpg)

Gli sviluppatori possono iniziare in pochi minuti con i file di esempio pronti per l&#39;esecuzione forniti per accedere a tutti i servizi Web disponibili. Questa esercitazione descrive tutti i passaggi necessari per avviare l&#39;esecuzione degli esempi utilizzando PDF Services .Net SDK:

## Passaggio 1: recupero delle credenziali e download dei file di esempio

Il primo passaggio consiste nel ottenere una credenziale (chiave API) per sbloccare l’utilizzo. [Registrati qui per la prova gratuita](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) e fai clic su &quot;Inizia&quot; per creare le nuove credenziali.

![Passo 1](assets/GettingStartedJava_step1.png)

È importante scegliere un &quot;Account personale&quot; per registrarsi per la versione di prova gratuita:

![Personale](assets/GettingStartedJava_personal.png)

Nel passaggio successivo si sceglierà il servizio API di PDF Services, quindi si aggiungeranno un nome e una descrizione per le credenziali.

È presente una casella di controllo per &quot;Creare codice di esempio personalizzato&quot;. Scegli questa opzione per aggiungere automaticamente le nuove credenziali ai file di esempio, in modo da salvare la procedura manuale per aggiungerle al progetto.

Quindi, scegli Node.js come lingua per ricevere gli esempi specifici di Node.js e fai clic sul pulsante &quot;Crea credenziali&quot;.

![Credenziali](assets/GettingStartedJava_credentials.png)

Riceverai per il download un file .zip denominato PDFToolsSDK-.NetSamples.zip che può essere salvato nel file system locale.

## Passaggio 2: configurare l&#39;ambiente .Net ed eseguire il codice di esempio

1. Scarica e installa [.Net SDK](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/install)
1. Estrai **[!UICONTROL PDFToolsSDK-.NetSamples.zip]** scaricato e decomprimi il contenuto
1. cd nella directory radice degli esempi **[!UICONTROL adobe-DC.PDFTools.SDK.NET.Samples]**
1. Dalla directory radice degli esempi, eseguire `dotnet build`

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>versione dotnet

   Ora sei pronto per eseguire i file di esempio.

   Questi passaggi finali mostrano come eseguire il primo esempio con l&#39;operazione Crea PDF da Word:

1. Dalla directory principale degli esempi passare alla cartella CreatePDFFromDocx, cd CreatePDFFromDocx/

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>cd CreatePDFFromDocx/

1. esegui `dotnet run CreatePDFFromDocx.csproj`

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples\CreatePDFFromDocx>dotnet run CreatePDFFromDocx.csproj

Il PDF verrà creato nella posizione indicata nell’output, che per impostazione predefinita è la stessa cartella.

## Pensieri finali

L’API di PDF Services consente di eliminare i processi manuali automatizzando i flussi di lavoro comuni e trasferendo il carico di elaborazione sul cloud. In un mondo in cui ogni browser tratta PDF in modo diverso, sfruttando l&#39;API Adobe PDF Embed insieme all&#39;API PDF Services, è possibile creare processi semplificati, affidabili e prevedibili che vengono eseguiti e visualizzati correttamente **ogni volta**, indipendentemente dalla piattaforma o dal dispositivo.

## Risorse e passaggi successivi

* Per ulteriore assistenza e supporto, visita il forum della community [[!DNL Adobe Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all)

* API dei servizi PDF [Documentazione](https://www.adobe.com/go/pdftoolsapi_doc)

* [Domande frequenti](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) per le domande sull&#39;API di PDF Services

* [Contattaci](https://www.adobe.com/go/pdftoolsapi_requestform) per domande su licenze e prezzi

* Articoli correlati

  [La nuova API di PDF Services offre ancora più funzionalità per i flussi di lavoro dei documenti](https://community.adobe.com/t5/document-services-apis/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [Versione di luglio di [!DNL Adobe Acrobat Services]: PDF Embed e PDF Services](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
