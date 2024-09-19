---
title: Gestione dei flussi di lavoro per documenti finanziari in Java
description: "[!DNL Adobe Acrobat Services] fornisce tutti gli strumenti, i servizi e le funzionalità necessari per elaborare ed estrarre dati dai documenti finanziari PDF"
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-7482
thumbnail: KT-7482.jpg
exl-id: 3bdc2610-d497-4a54-afc0-8b8baa234960
source-git-commit: ad13c28a0c218fc0027afc02445e5ed532c2340d
workflow-type: tm+mt
source-wordcount: '1204'
ht-degree: 0%

---

# Gestione dei flussi di lavoro dei documenti finanziari in Java

![Banner dell&#39;eroe del caso di utilizzo](assets/UseCaseFinancialHero.jpg)

Il settore finanziario utilizza ampiamente i file PDF per lo scambio di dati, in quanto consente di mantenere il formato, la progettazione e la struttura dei documenti. Questo robusto formato consente agli analisti e ai consulenti finanziari di aiutare i clienti a prendere decisioni consapevoli.

Il formato PDF, tuttavia, può risultare difficile da elaborare e automatizzare, soprattutto quando si combinano più origini dati, un caso d&#39;uso comune nel settore finanziario. La creazione di una soluzione personalizzata per l&#39;elaborazione dei documenti PDF è un&#39;opzione possibile, ma non è necessario investire troppo tempo e denaro in software e infrastrutture. [!DNL Adobe Acrobat Services] fornisce tutti gli strumenti, i servizi e le funzionalità necessari per elaborare ed estrarre dati dai documenti PDF.

## Cosa puoi imparare

In questo tutorial pratico, scopri come utilizzare le API [!DNL Adobe Acrobat Services] per le applicazioni [!DNL Java Spring Boot]. Si crea un&#39;app MVC (Model-View-Controller) che estrae il contenuto dai documenti PDF, lo converte in altri formati di dati, ad esempio Excel, combina più PDF e protegge le risorse tramite password. Questo tutorial spiega come elaborare i documenti PDF e mostrarli sui tuoi siti Web utilizzando l&#39;Adobe [PDF Embed API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

## API e risorse pertinenti

* [API dei servizi PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [API di incorporamento PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Esempi di progetto](https://github.com/adobe/pdftools-java-sdk-samples)

## Configurazione

[!DNL Adobe Acrobat Services] utilizza un sistema di autenticazione per controllare l&#39;accesso alle risorse. Per accedere ai servizi, è necessario richiedere una chiave API da Adobe per la tua organizzazione o applicazione. Se disponi di una chiave API, continua alla sezione successiva. Per creare una nuova chiave API, visita [Guida introduttiva](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) nel sito [!DNL Acrobat Services]. Puoi creare una chiave utilizzando la versione di prova gratuita che fornisce 1.000 transazioni di documenti che possono essere utilizzate per un massimo di sei mesi.

Per seguire questo tutorial, sono necessari due set di chiavi API:

* Servizi Adobe PDF: utilizzati per elaborare il documento PDF

* API di Adobe PDF Embed

Dopo aver creato le credenziali, copiare le credenziali API di PDF Services e la chiave privata nell&#39;applicazione [!DNL Spring Boot] all&#39;interno della sezione delle risorse. Ulteriori informazioni sulle [librerie e dipendenze Maven e Gradle](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=services) nel sito Web [!DNL Adobe Acrobat Services]. Prima di procedere, assicurati di configurare tutti i pacchetti e le librerie necessari.

![Schermata del percorso della directory per le credenziali API di PDF Services](assets/FAWJ_1.png)

Per configurare i servizi di registrazione, visitare la [documentazione di Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=services) e scorrere fino alla sezione Registrazione.

>[!NOTE]
>
> Nell’ambiente di produzione, non salvare le chiavi private nel controllo della versione. Utilizzare sempre un archivio segreto o un servizio di inserimento chiavi per impedire l&#39;utilizzo non autorizzato delle credenziali.

Ora che l&#39;applicazione [!DNL Spring Boot] è configurata, è possibile procedere con l&#39;elaborazione dei PDF e la generazione di report per i clienti.

## Invio dei dati del report

Per utilizzare l&#39;API di Adobe PDF Services, configurare prima un `ExecutionContext` che utilizzi le credenziali specificate. Poiché si dispone delle credenziali all&#39;interno dell&#39;applicazione, è possibile leggerle dal file e creare il contesto nel modo seguente:

```
Credentials credentials = Credentials.serviceAccountCredentialsBuilder()
    .fromFile(AUTH_FILE_PATH)
    .build();

ExecutionContext executionContext = ExecutionContext.create(credentials);
```

Quindi, ottieni il contesto per elaborare i documenti PDF. Ecco le azioni che puoi eseguire:

* Convertire i documenti PDF (in Excel, Word o tipo di grafica)

* Creazione di documenti PDF (da HTML, Excel, Word e altri)

* Combinare più documenti PDF

* Protect e rimuovere la protezione dei documenti PDF (è necessario disporre della password)

* Ottimizzazione dei documenti PDF per la distribuzione in rete

Tutti questi esempi sono disponibili nel repository [GitHub samples](https://github.com/adobe/pdfservices-java-sdk-samples/tree/master/src/main/java/com/adobe/pdfservices/operation/samples).

Successivamente, in [!DNL Spring Boot], è possibile ottenere un file utilizzando il percorso String o il flusso in cui viene caricato il file. Ogni operazione eseguita deve essere inizializzata e deve essere impostato un percorso del file di input. Per questo tutorial, utilizzi i report di PDF disponibili pubblicamente da [Blackrock](https://www.blackrock.com/us/individual/products/investment-funds). Puoi utilizzare qualsiasi altra fonte, compresi i tuoi report.

Iniziate acquisendo l&#39;oggetto FileRef dal file. Per semplicità, concentratevi sui file in base al percorso String. Di seguito, viene creata un&#39;operazione per convertire un file nel percorso da PDF a Excel:

```
ExecutionContext executionContext = ExecutionContext.create(credentials);
ExportPDFOperation exportOperation = ExportPDFOperation.createNew(ExportPDFTargetFormat.XLSX);

// Create the input source
FileRef inputPdf = FileRef.createFromLocalFile(INPUT_PDF);
exportOperation.setInput(inputPdf);
```

Dopo questo passaggio, il programma è pronto per eseguire la prima operazione su PDF. Quindi, si esegue l&#39;operazione e si ottiene il risultato nel foglio Excel:

```
try {
    FileRef output = exportOperation.execute(executionContext);
    output.saveAs(OUTPUT_EXCEL);
} catch (ServiceApiException e) {
    e.printStackTrace();
}
```

In questo scenario viene gestito un solo file PDF. Potete anche iniziare con più file di PDF e combinarli in un unico file. L&#39;utilizzo di più file è comune nel reporting dei dati finanziari, in quanto è necessario elaborare i fondi provenienti da più origini per fornire un report completo.

## Generazione del report

[!DNL Adobe Acrobat Services] non supporta l&#39;elaborazione di documenti di Excel preconfigurati, ma è comunque possibile utilizzare framework e librerie della community per elaborare il contenuto.

Ad esempio, è possibile utilizzare [Apache POI](https://poi.apache.org/) per elaborare Excel (o altri documenti di Microsoft) nell&#39;app [!DNL Java Spring Boot] oppure eseguire altre attività manuali o automatizzate nel file di Excel.

In questo esempio, iniziando dai documenti PDF, si estrae il valore patrimoniale netto per i tre fondi e li si mostra in una tabella. È possibile richiamare anche altre informazioni, ad esempio grafici e tabelle, in base alle proprie esigenze e ai dati disponibili. È anche possibile importare dati da altre origini.

Dopo aver generato il report, in questo esempio in formato Excel, è possibile utilizzare le operazioni di Adobe PDF Services per convertire nuovamente il report in un documento PDF e proteggerlo.

Per convertire il report dal formato Excel a un documento PDF, eseguire la seguente operazione:

```
ExecutionContext executionContext = ExecutionContext.create(credentials);
CreatePDFOperation exportOperation = CreatePDFOperation.createNew();

// Create the input source
FileRef inputPdf = FileRef.createFromLocalFile(INPUT_EXCEL);
exportOperation.setInput(inputPdf);

try {
    FileRef output = exportOperation.execute(executionContext);
    output.saveAs(OUTPUT_PDF);
} catch (ServiceApiException e) {
    e.printStackTrace();
}
```

>[!TIP]
>
> Per evitare di dover ricreare l&#39;oggetto ogni volta che viene effettuata una richiesta, utilizzare Spring&#39;s dependency injection per inserire l&#39;oggetto `ExecutionContext`.

Questo codice genera un documento PDF dal report in formato Excel.

Prima di consegnare questo PDF ai tuoi clienti, puoi proteggerlo con una password. Creare un&#39;altra operazione che gestisca automaticamente questa protezione, ProtectPDFOperation, quindi utilizzare ProtectPDFOptions per aggiungere la password al documento.

```
ProtectPDFOptions options = ProtectPDFOptions.passwordProtectOptionsBuilder()
                    .setUserPassword("p@55w0rd")
                    .setEncryptionAlgorithm(EncryptionAlgorithm.AES_256)
                    .build();
ProtectPDFOperation operation = ProtectPDFOperation.createNew(options);
```

Specificare quindi l&#39;input ed eseguire l&#39;operazione. Il file risultante deve avere una password per impedirne l&#39;accesso non autorizzato.

## Visualizzazione del report

Ora che il report PDF è stato generato, puoi visualizzarlo sul sito Web utilizzando l’API Adobe PDF Embed. Questa API JavaScript consente agli sviluppatori Web di caricare ed eseguire il rendering dei documenti PDF in modo nativo all&#39;interno del browser Web.

>[!NOTE]
>
> A questo punto è necessario il secondo token di credenziali, l&#39;ID client.

Nell&#39;applicazione [!DNL Spring Boot], aggiungere il seguente frammento di HTML in cui si desidera eseguire il rendering del report PDF:

```
<div id="pdf-viewer"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
    document.addEventListener("adobe_dc_view_sdk.ready", function()
    {
        var adobeDCView = new AdobeDC.View({ clientId: "<your-client-id-here>", divId: "pdf-viewer" });
        adobeDCView.previewFile(
        {
            content: {
                location: {
                    url: "<your-document.pdf>"
                }
            },
            metaData: {
                fileName: "<document-name.pdf>"
            }
        });
    });
</script>
```

Questo script carica il documento PDF e consente agli utenti di annotare e aggiungere commenti ai documenti. Ecco la vista di questa API di incorporamento come mostrato in Firefox:

![Schermata di un documento PDF in Firefox](assets/FAWJ_2.png)

L’API PDF Embed fornisce tutti gli strumenti necessari per visualizzare in anteprima il PDF e per annotare il report.

## Fasi seguenti

In questa esercitazione pratica sono state esplorate le API [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/) e si è discusso su come utilizzare questi servizi per elaborare i dati PDF e generare report per le decisioni finanziarie. È stato illustrato come integrare le API nei sistemi utilizzando [!DNL Java Spring Boot] come framework di esempio, per dimostrare quanto sia facile elaborare rapidamente i documenti PDF.

Esplora [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/) e scopri cosa possono fare i servizi Adobe PDF per la tua azienda. Per ulteriori informazioni sulle altre funzionalità disponibili nell&#39;SDK, consulta il [repository GitHub](https://github.com/adobe/pdftools-java-sdk-samples) per gli esempi e scopri come l&#39;[API PDF Embed](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) può aiutarti a visualizzare rapidamente i PDF nelle tue applicazioni.

Per combinare e manipolare facilmente i documenti, creando utili report PDF per i clienti finanziari, iniziate subito registrandovi per il vostro [account sviluppatore Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/) gratuito.
