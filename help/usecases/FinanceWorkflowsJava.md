---
title: Gestione dei flussi di lavoro dei documenti finanziari in Java
description: "[!DNL Adobe Acrobat Services] offre tutti gli strumenti, i servizi e le funzionalità necessari per elaborare ed estrarre dati da documenti finanziari PDF"
type: Tutorial
role: Developer
level: Intermediate
thumbnail: KT-7482.jpg
jria: KT-7482
exl-id: 3bdc2610-d497-4a54-afc0-8b8baa234960
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1315'
ht-degree: 0%

---

# Gestione dei flussi di lavoro basati su documenti finanziari in Java

![Usa banner eroe caso](assets/UseCaseFinancialHero.jpg)

Il settore finanziario utilizza ampiamente i file PDF per lo scambio di dati, in quanto consente di mantenere il formato, la progettazione e la struttura dei documenti. Questo formato robusto consente ad analisti e consulenti finanziari di aiutare i clienti a prendere decisioni informate.

Il formato di PDF, tuttavia, può essere difficile da elaborare e automatizzare, soprattutto quando si combinano più fonti di dati, un caso d&#39;uso comune nel settore finanziario. La creazione di una soluzione personalizzata per elaborare i documenti PDF è un&#39;opzione, ma non è necessario investire troppo tempo e denaro in software e infrastrutture. [!DNL Adobe Acrobat Services] fornisce tutti gli strumenti, i servizi e le funzionalità necessari per elaborare ed estrarre i dati dai documenti PDF.

## Cosa puoi imparare

In questa esercitazione pratica, scopri come utilizzare [!DNL Adobe Acrobat Services] API per [!DNL Java Spring Boot] applicazioni. È possibile creare un&#39;app MVC (Model-View-Controller) che estrae contenuti dai documenti PDF, li converte in altri formati di dati come Excel, combina più PDF e protegge le risorse con una password. Questa esercitazione spiega come elaborare i documenti di PDF e mostrarli sui siti Web utilizzando l&#39;Adobe [API di incorporamento PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

## API e risorse pertinenti

* [API dei servizi PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [API di incorporamento PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Esempi di progetti](https://github.com/adobe/pdftools-java-sdk-samples)

## Configurazione

[!DNL Adobe Acrobat Services] utilizza un sistema di autenticazione per controllare l’accesso alle risorse. Per accedere ai servizi, è necessario richiedere una chiave API da Adobe per la propria organizzazione o applicazione. Se disponi di una chiave API, passa alla sezione successiva. Per creare una nuova chiave API, visita [Guida introduttiva](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) nel [!DNL Acrobat Services] sito. È possibile creare una chiave utilizzando la versione di prova gratuita, che offre 1.000 transazioni di documenti utilizzabili per un massimo di sei mesi.

Per seguire questa esercitazione, sono necessari due set di chiavi API:

* Adobe PDF Services - utilizzato per elaborare il documento PDF

* API di incorporamento di Adobe PDF

Dopo aver creato le credenziali, copia le credenziali API dei servizi di PDF e la chiave privata nel [!DNL Spring Boot] nella sezione risorse. Ulteriori informazioni sul [Librerie e dipendenze Maven e Gradle](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=services) sulla [!DNL Adobe Acrobat Services] sito web. Prima di procedere, assicurati di configurare tutti i pacchetti e le librerie necessari.

![Screenshot del percorso della directory per le credenziali API di PDF Services](assets/FAWJ_1.png)

Per configurare i servizi di registrazione, visita [documentazione di Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=services) e scorri fino alla sezione Archiviazione.

>[!NOTE]
>
> Nell’ambiente di produzione, non salvare le chiavi private nel controllo delle versioni. Utilizza sempre un archivio segreto o un servizio di iniezione delle chiavi per impedire l&#39;uso non autorizzato delle credenziali.

Ora che il [!DNL Spring Boot] l&#39;applicazione è configurata, puoi procedere con l&#39;elaborazione dei PDF e la generazione di report per i clienti.

## Invio dei dati del report

Per utilizzare l&#39;API dei servizi Adobe PDF, configura innanzitutto un `ExecutionContext` che utilizza le credenziali fornite. Poiché le credenziali sono disponibili nell&#39;applicazione, potete leggerle dal file e creare il contesto nel modo seguente:

```
Credentials credentials = Credentials.serviceAccountCredentialsBuilder()
    .fromFile(AUTH_FILE_PATH)
    .build();

ExecutionContext executionContext = ExecutionContext.create(credentials);
```

Ottieni quindi il contesto per elaborare i documenti di PDF. Di seguito sono riportate le azioni che puoi eseguire:

* Convertire i documenti PDF (in Excel, Word o grafica)

* Creare i documenti PDF (da HTML, Excel, Word e altro)

* Combinare più documenti PDF

* Protect e deproteggi i documenti PDF (è necessario disporre della password)

* Ottimizzare i documenti di PDF per la distribuzione in rete

Tutti questi campioni sono disponibili nel [Esempi di GitHub](https://github.com/adobe/pdfservices-java-sdk-samples/tree/master/src/main/java/com/adobe/pdfservices/operation/samples) archivio.

Avanti, in [!DNL Spring Boot], è possibile ottenere un file utilizzando il percorso String o lo streaming in cui viene caricato il file. Ogni operazione eseguita deve essere inizializzata e deve essere impostato un percorso di file di input. Per questa esercitazione, utilizzate i report PDF disponibili pubblicamente da [Blackrock](https://www.blackrock.com/us/individual/products/investment-funds). Puoi utilizzare qualsiasi altra fonte, inclusi i tuoi report.

Per prima cosa, acquisire il [FileRef](https://opensource.adobe.com/pdfservices-java-sdk-samples/apidocs/latest/com/adobe/pdfservices/operation/io/FileRef.html) dal file. Per semplicità, è possibile concentrarsi sui file in base al percorso String. Di seguito viene creata un&#39;operazione per convertire un file nel percorso da PDF a Excel:

```
ExecutionContext executionContext = ExecutionContext.create(credentials);
ExportPDFOperation exportOperation = ExportPDFOperation.createNew(ExportPDFTargetFormat.XLSX);

// Create the input source
FileRef inputPdf = FileRef.createFromLocalFile(INPUT_PDF);
exportOperation.setInput(inputPdf);
```

Dopo questo passaggio, il programma è pronto per eseguire la prima operazione sulla PDF. A questo punto, si esegue l&#39;operazione e si ottiene il risultato nel foglio Excel:

```
try {
    FileRef output = exportOperation.execute(executionContext);
    output.saveAs(OUTPUT_EXCEL);
} catch (ServiceApiException e) {
    e.printStackTrace();
}
```

Questo scenario gestisce un solo file PDF. Puoi anche iniziare con più file PDF e combinarli in un unico file. L’utilizzo di più file è comune nei report dei dati finanziari perché per fornire un report completo devi elaborare fondi provenienti da più fonti.

## Generazione del report

[!DNL Adobe Acrobat Services] non supporta l&#39;elaborazione di documenti Excel, ma è comunque possibile utilizzare framework e librerie della comunità per elaborare il contenuto.

Ad esempio, è possibile utilizzare il metodo [Apache POI](https://poi.apache.org/) per elaborare Excel (o altri documenti Microsoft) nel [!DNL Java Spring Boot] oppure eseguire altre attività manuali o automatizzate nel file Excel.

In questo esempio, iniziando dai documenti di PDF, si estrae il valore netto di risorse per i tre fondi e li si visualizza in una tabella. Puoi anche richiamare altre informazioni, come grafici e tabelle, in base alle tue esigenze e ai dati disponibili. Puoi anche importare dati da altre fonti.

Dopo aver generato il report, in questo esempio in formato Excel, puoi utilizzare le operazioni dei servizi Adobe PDF per riconvertirlo in un documento PDF e proteggerlo.

Per convertire il rapporto dal formato Excel a un documento PDF, utilizza la seguente operazione:

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
> Per evitare di dover ricreare l&#39;oggetto ogni volta che viene richiesta, utilizzare l&#39;iniezione di dipendenza di Spring per iniettare il `ExecutionContext` oggetto.

Questo codice genera un documento PDF dal report in formato Excel.

Prima di consegnare questo PDF ai tuoi clienti, puoi proteggerlo con una password. Create un&#39;altra operazione che gestisca questa protezione per voi, [ProtectPDFOperation](https://opensource.adobe.com/pdfservices-java-sdk-samples/apidocs/latest/com/adobe/pdfservices/operation/pdfops/ProtectPDFOperation.html), quindi utilizzare [ProtectPDFOptions](https://opensource.adobe.com/pdfservices-java-sdk-samples/apidocs/latest/com/adobe/pdfservices/operation/pdfops/options/protectpdf/package-summary.html) per aggiungere la password al documento.

```
ProtectPDFOptions options = ProtectPDFOptions.passwordProtectOptionsBuilder()
                    .setUserPassword("p@55w0rd")
                    .setEncryptionAlgorithm(EncryptionAlgorithm.AES_256)
                    .build();
ProtectPDFOperation operation = ProtectPDFOperation.createNew(options);
```

Quindi, specificate l&#39;input ed eseguite l&#39;operazione. Il file risultante deve avere una password per impedire l&#39;accesso non autorizzato.

## Visualizzazione del report

Ora che il report PDF è stato generato, puoi visualizzarlo sul sito Web utilizzando l’API di incorporamento di Adobe PDF. Questa API JavaScript consente agli sviluppatori Web di caricare ed eseguire il rendering dei documenti PDF in modo nativo all&#39;interno del browser Web.

>[!NOTE]
>
> A questo punto hai bisogno del secondo token di credenziali, l’ID client.

Nella [!DNL Spring Boot] aggiungere lo snippet di HTML seguente nel punto in cui si desidera eseguire il rendering del rapporto PDF:

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

Questo script carica il documento PDF e consente agli utenti di annotare e commentare i documenti. Di seguito è riportata la vista di questa API Incorpora, come mostrato in Firefox:

![Screenshot di un documento PDF in Firefox](assets/FAWJ_2.png)

L’API Incorpora PDF offre tutti gli strumenti necessari per visualizzare in anteprima il PDF e annotare il report.

## Fasi seguenti

Questa esercitazione pratica ha esplorato la [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/) API e ha discusso come utilizzare questi servizi per elaborare i dati dei PDF e generare report per le decisioni finanziarie. Ha dimostrato come integrare le API nei sistemi, utilizzando [!DNL Java Spring Boot] come esempio di framework, per mostrare quanto sia semplice elaborare rapidamente i documenti PDF.

Esplora [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/) e scopri cosa possono fare i servizi Adobe PDF per il tuo business. Per ulteriori informazioni sulle funzioni disponibili nell&#39;SDK, consultare il [repository GitHub](https://github.com/adobe/pdftools-java-sdk-samples) per i campioni e scopri come [API di incorporamento PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) consente di visualizzare rapidamente i PDF all&#39;interno delle applicazioni.

Per combinare e manipolare facilmente i documenti, creare report di PDF utili per i clienti finanziari, inizia registrandoti gratuitamente [Adobe account sviluppatore](https://www.adobe.io/apis/documentcloud/dcsdk/) oggi.
