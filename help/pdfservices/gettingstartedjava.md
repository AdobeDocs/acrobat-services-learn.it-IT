---
title: Guida introduttiva all’API e al linguaggio Java di Adobe PDF Services
description: Gli sviluppatori possono iniziare in pochi minuti con i file di esempio pronti per l'esecuzione forniti per accedere a tutti i servizi Web disponibili
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6676
thumbnail: KT-6676.jpg
exl-id: 4a8f2119-c464-496b-bdc8-35dd387bef25
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 0%

---

# Guida introduttiva all’API e a Java di Adobe PDF Services

![Crea immagine PDF Hero](assets/GettingStartedJava_hero.jpg)

Gli sviluppatori possono iniziare in pochi minuti con i file di esempio pronti per l&#39;esecuzione forniti per accedere a tutti i servizi Web disponibili. Questa esercitazione descrive tutti i passaggi necessari per avviare l&#39;esecuzione degli esempi utilizzando l&#39;SDK Java di PDF Services:

## Passaggio 1: recupero delle credenziali e download dei file di esempio

Il primo passaggio consiste nel ottenere una credenziale (chiave API) per sbloccare l’utilizzo. [Registrati qui per la prova gratuita](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) e fai clic su &quot;Inizia&quot; per creare le nuove credenziali.

![Passo 1](assets/GettingStartedJava_step1.png)

È importante scegliere un &quot;Account personale&quot; per registrarsi per la versione di prova gratuita:

![Personale](assets/GettingStartedJava_personal.png)

Nel passaggio successivo si sceglierà il servizio API di PDF Services, quindi si aggiungeranno un nome e una descrizione per le credenziali.

È presente una casella di controllo per &quot;Creare codice di esempio personalizzato&quot;. Scegli questa opzione per aggiungere automaticamente le nuove credenziali ai file di esempio, in modo da salvare la procedura manuale per aggiungerle al progetto.

Quindi, scegli Java come lingua per ricevere gli esempi specifici di Java, quindi fai clic sul pulsante &quot;Crea credenziali&quot;.

![Credenziali](assets/GettingStartedJava_credentials.png)

Riceverai per il download un file .zip denominato PDFToolsSDK-JavaSamples.zip che può essere salvato nel file system locale.

## Passaggio 2: configurazione dell’ambiente Java

1. Installa [Java 8 o versione successiva](https://www.oracle.com/java/technologies/javase-downloads.html), se non lo hai ancora fatto.
1. Esegui `javac -version` per verificare l&#39;installazione.
1. Verificare che la cartella JDK bin sia inclusa nella variabile PATH (il metodo varia in base al sistema operativo).
1. Se non l&#39;hai già fatto, installa [Maven](https://maven.apache.org/install.html) utilizzando il tuo strumento preferito.

Gli esempi personalizzati forniscono tutto, dal codice di esempio pronto per l&#39;esecuzione, un file JSON delle credenziali incorporato e connessioni preconfigurate alle dipendenze.

1. Scarica [il progetto di esempio](https://github.com/adobe/pdftools-java-sdk-samples).
1. Crea il progetto di esempio con Maven: mvn clean install.
1. Verificare il codice di esempio nella riga di comando o nell&#39;IDE preferito.

## Pensieri finali

L’API di PDF Services consente di eliminare i processi manuali automatizzando i flussi di lavoro comuni e trasferendo il carico di elaborazione sul cloud. In un mondo in cui ogni browser tratta PDF in modo diverso, sfruttando l&#39;API Adobe PDF Embed insieme all&#39;API PDF Services, è possibile creare processi semplificati, affidabili e prevedibili che vengono eseguiti e visualizzati correttamente **ogni volta**, indipendentemente dalla piattaforma o dal dispositivo.

## Risorse e passaggi successivi

* Per ulteriore assistenza e supporto, visita il forum della community [[!DNL Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) di Adobe

* API dei servizi PDF [Documentazione](https://www.adobe.com/go/pdftoolsapi_doc)

* [Domande frequenti](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) per le domande sull&#39;API di PDF Services

* [Contattaci](https://www.adobe.com/go/pdftoolsapi_requestform) per domande su licenze e prezzi

* Articoli correlati

  [La nuova API di PDF Services offre ancora più funzionalità per i flussi di lavoro dei documenti](https://community.adobe.com/t5/document-services-apis/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [Versione di luglio di [!DNL Adobe Acrobat Services]: PDF Embed e PDF Services](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
