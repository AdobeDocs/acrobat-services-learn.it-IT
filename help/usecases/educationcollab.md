---
title: Collaborazione tra studenti e docenti
description: Scopri come creare una piattaforma di apprendimento online che consenta a docenti e studenti di condividere facilmente le risorse in PDF
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8091.jpg
jira: KT-8091
exl-id: 570a635c-e539-4afc-a475-ecf576415217
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1485'
ht-degree: 0%

---

# Collaborazione tra studenti e docenti

![Usa banner eroe caso](assets/UseCaseStudentHero.jpg)

Gli istituti didattici utilizzano documenti PDF per condividere materiale didattico con gli studenti. I PDF offrono ai docenti un formato di documento intercambiabile.

Integrazione [API dei servizi Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html) e [API di incorporamento di Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) in un&#39;app offre a docenti e studenti un&#39;unica piattaforma su cui insegnare e imparare. Ad esempio, la tua app può consentire agli studenti di porre domande sui loro compiti e sulle schede di report e di collaborare alle assegnazioni di gruppo.

Esiste un SDK ufficiale per le applicazioni Node.js per accedere alle API di PDF Services. Questo consente di convertire in PDF documenti quali Microsoft Word o Microsoft Excel. Inoltre, puoi eseguire operazioni più avanzate, ad esempio combinando più report, ridisponendo le pagine e proteggendo i PDF. Per ulteriori informazioni, consultate [documentazione del prodotto](https://www.adobe.io/apis/documentcloud/dcsdk/).

## Cosa puoi imparare

In questa esercitazione pratica, scopri come creare una piattaforma di apprendimento online che [consente a docenti e studenti di condividere facilmente le risorse](https://www.adobe.io/apis/documentcloud/dcsdk/student-teacher-collaboration.html) in PDF. Questa esercitazione utilizza un metodo [portale di apprendimento](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers) creato utilizzando il runtime JavaScript Node.js (Node.js) e i servizi PDF.

Il portale di apprendimento ha le seguenti caratteristiche:

* Consenti ai docenti di caricare risorse

* Consente agli studenti di selezionare più documenti da convertire in PDF

* Consente la conversione di documenti in PDF

* Fornisce un’anteprima PDF per gli studenti in un browser Web e consente loro di annotare i documenti senza software aggiuntivo

* Consente agli studenti di lasciare i commenti e scaricarli nei computer

Scopri come [!DNL Adobe Acrobat Services] offri ai tuoi studenti un&#39;esperienza ricca di PDF. [!DNL Acrobat Services] Le API si integrano perfettamente nelle applicazioni esistenti, consentendo agli studenti di caricare, convertire e visualizzare i file, quindi di creare e salvare commenti, il tutto all&#39;interno della configurazione corrente.

## API e risorse pertinenti

* [API di incorporamento PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [API dei servizi PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Codice progetto](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers)

## Caricamento di risorse nel portale di apprendimento

Nella sezione insegnanti del portale di apprendimento, gli insegnanti possono caricare documenti quali compiti e test. I documenti possono essere in qualsiasi formato, ad esempio Microsoft Word, Microsoft Excel, HTML, vari formati di immagine e così via.

![Screenshot della sezione insegnanti del portale di apprendimento](assets/edu_1.png)

I documenti caricati vengono archiviati e presentati agli studenti quando aprono la pagina Web.

Per informazioni su come l&#39;applicazione carica i file, vedere la [codice progetto](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers).

## Conversione di documenti in PDF

Gli studenti possono convertire uno o più documenti di qualsiasi tipo in PDF, ad esempio Microsoft Word, Excel e PowerPoint, nonché altri tipi di file di testo e immagini più diffusi. Il portale di apprendimento utilizza PDF Services per eseguire la conversione dei file in PDF.

Per creare un portale di apprendimento personalizzato, devi prima creare le tue credenziali. [Registrati](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) per utilizzare gratuitamente le API dei servizi PDF per sei mesi e fino a 1.000 transazioni di documenti. Dopodiché, [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) a soli \$0,05 per transazione documento mentre la classe aumenta le assegnazioni.

Quando uno studente seleziona un documento dal dashboard, visualizza quanto segue:

![Screenshot della sezione studenti del portale di apprendimento](assets/edu_2.png)

Lo studente seleziona semplicemente i documenti per la conversione e fa clic **Scarica report**.

Il portale di apprendimento converte i documenti in PDF e visualizza una pagina di rapporto, insieme a un’anteprima del file PDF.

Di seguito è riportato il codice di esempio per questo passaggio:

```
async function createPdf(rawFile, outputPdf) {
    try {
            // configurations
            const credentials =  adobe.Credentials
            .serviceAccountCredentialsBuilder()
            .fromFile("./src/pdftools-api-credentials.json")
            .build();
 
            // Capture the credential from app and show create the context
            const executionContext = adobe.ExecutionContext.create(credentials),
            operation = adobe.CreatePDF.Operation.createNew();
 
            // Pass the content as input (stream)
            const input = adobe.FileRef.createFromLocalFile(rawFile);
            operation.setInput(input);
 
            // Async create the PDF
            let result = await operation.execute(executionContext);
            await result.saveAsFile(outputPdf);
    } catch (err) {
            console.log('Exception encountered while executing operation', err);
    }
}
```

Il codice di esempio chiama il metodo `createPdf` all&#39;interno del gestore di percorsi Express per generare il PDF.

Per scoprire come viene chiamato questo metodo, vedere [il codice del progetto](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers/blob/master/src/helpers/pdf.js).

## Anteprima delle risorse di apprendimento

L&#39;interfaccia utente utilizza l&#39;API PDF Embed per eseguire il rendering dei PDF in un browser Web. Questa API è disponibile gratuitamente.

L’API per l’incorporamento di PDF utilizza una credenziale diversa dall’API dei servizi di PDF, quindi è necessario [creare una credenziale](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
prima di utilizzarlo. Potrai quindi utilizzare PDF Embed completamente gratuito.

Assicurati di immettere l’URL del sito Web corretto nel token. In caso contrario, potrebbe non essere possibile eseguire il rendering dei PDF con il token.

L&#39;interfaccia utente utilizza il metodo [Handlebars](https://handlebarsjs.com/) linguaggio dei modelli. Viene visualizzato il PDF in un browser Web.

Di seguito è riportato il codice per questo passaggio:

```
<div id="adobe-dc-view" style="height: 750px; width: 700px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
    document.addEventListener("adobe_dc_view_sdk.ready", function () {
        var adobeDCView = new AdobeDC.View({ clientId: "<your-credentials-here>", divId: "adobe-dc-view" });
        adobeDCView.previewFile(
            {
                content: {
                    location: { url: "<file-url>" }
                },
                    metaData: { fileName: "<file-name>" }
            },
           );
    });
</script>
 
<p>Material has been generated, <a href="/students/download/{{filename}}" target="_blank">click here</a> to download it.
</p>
```

Questo codice visualizza l&#39;output del PDF e il collegamento per scaricare il report del PDF, come illustrato di seguito nell&#39;acquisizione delle schermate:

![Screenshot dell’anteprima di student PDF](assets/edu_3.png)

Gli studenti devono poter scaricare il report o lavorare sul materiale qui.

## Annotazione dei documenti PDF

Una piattaforma di apprendimento dovrebbe supportare annotazioni, commenti e discussioni di base nei PDF. L’API di incorporamento di PDF fornisce tutte queste funzioni. Attiva il supporto per le annotazioni mediante `showAnnotationTools`, consentendo a docenti e studenti di commentare i documenti e archiviare i commenti come parte del PDF.

Per attivare le annotazioni nei documenti PDF, è sufficiente passare l’argomento `showAnnotationTools` : fedele alla proprietà `previewFile` metodo. Viene visualizzato lo strumento annotazioni nell’anteprima PDF. Accedete a questo strumento dal menu con i tre punti nell’angolo superiore destro dell’anteprima.

![Screenshot degli strumenti di commento in PDF](assets/edu_4.png)

Nei documenti caricati dagli insegnanti, gli studenti possono evidenziare il testo, aggiungere commenti e così via.

![Screenshot dell’aggiunta di un commento in PDF](assets/edu_5.png)

Nella schermata di cui sopra, l’utente è etichettato come &quot;Ospite&quot;, ma puoi configurare i profili per gli utenti, ad esempio studenti e docenti.

Quando uno studente applica un’annotazione, l’API Incorpora PDF visualizza un **Salva** sul banner superiore. Il salvataggio aggiunge le annotazioni al file. Prova a fare clic **Salva** per vedere come viene salvato il file con l’annotazione incorporata nel rapporto.

Gli studenti possono utilizzare annotazioni per fare domande o condividere i loro commenti sul materiale di apprendimento.

## Monitoraggio dell&#39;utilizzo dei documenti

È importante per gli insegnanti e le scuole vedere come gli studenti utilizzano le piattaforme online. Questo aiuta i docenti a sostenere gli studenti con risorse che li aiutano a svolgere meglio i loro compiti. L’API di incorporamento di PDF si integra con l’analisi che puoi utilizzare per misurare tutti gli eventi in corso, ad esempio quando gli utenti aprono, leggono e chiudono i documenti. Con l&#39;API dei servizi PDF, i docenti possono anche disattivare la stampa, il download e la modifica dei file per contribuire a mantenere l&#39;integrità accademica.

Se si dispone di un [Adobe Analytics](https://www.adobe.io/apis/experiencecloud/analytics.html) , è possibile utilizzare la sua [integrazione preconfigurata](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfembed/controlpdfexperience.html?lang=en#adobe-analytics). In caso contrario, utilizza le funzioni di callback per integrare i servizi di PDF con altri fornitori di analisi, ad esempio [Google](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfembed/controlpdfexperience.html?lang=en#google-analytics).

Per abilitare la misurazione degli eventi del documento, associare i gestori di eventi mediante `registerCallback` con l&#39;Adobe dell&#39;istanza della vista DC. Potete visualizzare le metriche di base, ad esempio l’apertura di un documento o la lettura di una pagina, sulla console. Puoi anche salvare le metriche in un registro o pubblicarle in altri archivi di analisi.

Di seguito è riportato il codice di esempio per associare i gestori di eventi:

```
adobeDCView.registerCallback(
    AdobeDC.View.Enum.CallbackType.EVENT_LISTENER,
    function(event) {
           console.log(event);
    },
    {
           enablePDFAnalytics: true
    }
);
```

Gli insegnanti possono vedere quanti studenti hanno visto il compito, quanti hanno esaminato tutte le pagine dei loro appunti e altri dettagli preziosi.

Di seguito è riportata una schermata di acquisizione della console del browser Web:

![Screenshot della console del browser Web](assets/edu_6.png)

Questa schermata mostra che lo studente ha aperto il file assegnazione, ha letto la prima pagina, o non ha scorrere le pagine aggiuntive o il documento aveva una sola pagina, quindi ha scaricato il file. Puoi raccogliere queste metriche per eseguire analisi e studiare il comportamento dei tuoi studenti.

Inoltre, [Adobe Analytics](https://business.adobe.com/products/analytics/adobe-analytics.html) è integrato con l&#39;API di incorporamento di PDF, quindi se disponi di un abbonamento alla suite Adobe Analytics, puoi pubblicare le metriche nell&#39;abbonamento. Per pubblicare le metriche in Adobe Analytics, è sufficiente passare l&#39;ID della suite alla funzione di costruzione API di PDF Embed. (Tenete presente che dovete utilizzare le credenziali API di incorporamento PDF, non le credenziali API dei servizi PDF).

Di seguito è riportato un esempio di codice che mostra come passare l&#39;ID della suite alla funzione di costruzione API di incorporamento di PDF:

```
var adobeDCView = new AdobeDC.View({
    clientId: "<your-adobe-dc-credential>",
    divId: "<#element>"
    reportSuiteId: <your-id-here>,
}); 
```

## Fasi seguenti

Questa esercitazione pratica ha esaminato come utilizzare l’API PDF Services e l’API PDF Embed per creare un portale di apprendimento, facilitando l’utilizzo efficace [collaborazione tra studenti e docenti](https://www.adobe.io/apis/documentcloud/dcsdk/student-teacher-collaboration.html). Utilizzando questo portale, gli insegnanti possono caricare materiale didattico in qualsiasi formato e convertirlo in PDF utilizzando l’API dei servizi PDF. Gli studenti possono quindi visualizzare in anteprima questi PDF utilizzando l&#39;API di incorporamento di PDF.

Ora che sai come annotare i report PDF, archiviare le annotazioni e monitorare l&#39;uso dei report PDF, puoi iniziare a implementare queste soluzioni nei tuoi progetti.

È possibile utilizzare [!DNL Adobe Acrobat Services] API per creare esperienze di PDF interattive intuitive sul tuo sito web. Approfitta dell&#39;API gratuita di Adobe PDF Services per sei mesi e non [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) (tramite AWS o un accordo diretto) solo per \$0,05 per transazione documento. Usa Adobe PDF Incorpora gratuitamente senza limiti di tempo. Crea un account gratuito per [inizia](https://www.adobe.com/go/dcsdks_credentials) oggi.
