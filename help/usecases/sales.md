---
title: Gestione di proposte di vendita e contratti
description: Scopri come creare un flusso di lavoro efficiente per automatizzare e semplificare le proposte di vendita
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8099
thumbnail: KT-8099.jpg
exl-id: 219c70de-fec1-4946-b10e-8ab5812562ef
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '1306'
ht-degree: 0%

---

# Gestione delle proposte di vendita e dei contratti

![Banner dell&#39;eroe del caso di utilizzo](assets/UseCaseSalesHero.jpg)

Le proposte di vendita rappresentano il primo passo di un&#39;azienda verso l&#39;acquisizione di clienti. Come per tutto, le prime impressioni durano. La prima interazione con i clienti ha fissato le aspettative per la vostra attività. La tua proposta deve essere concisa, precisa e conveniente.

I contratti e le proposte contengono diversi tipi di dati all&#39;interno della struttura del documento. Contengono sia dati dinamici (nome del cliente, quantità di preventivi e così via) che dati statici (testo standard come le capacità stabili, i profili del team e i termini SOW standard). La creazione di documenti modello, ad esempio le proposte di vendita, spesso comporta attività monotone, ad esempio la sostituzione manuale dei dettagli del progetto in un modello standard. In questo tutorial, utilizzerai dati dinamici e flussi di lavoro per creare un processo efficiente per [la creazione di proposte di vendita](https://www.adobe.io/apis/documentcloud/dcsdk/sales-proposals-and-contracts.html).

## Cosa puoi imparare

In questo tutorial pratico, scopri come implementare flussi di lavoro e dati dinamici utilizzando diversi strumenti, i più importanti dei quali sono le API [!DNL Adobe Acrobat Services]. Queste API vengono utilizzate per rendere le proposte di vendita e i contratti più convenienti per te e per la tua azienda. In questo tutorial vengono illustrate le tecniche pratiche per la creazione, l’unione e la visualizzazione automatica dei documenti PDF. L&#39;esecuzione manuale di queste operazioni è lunga e ripetitiva. Sfruttando le API di [!DNL Acrobat Services], puoi ridurre il tempo dedicato a queste attività.

## API e risorse pertinenti

* [Microsoft Word](https://www.office.com/)

* [Nodo.js](https://nodejs.org/en/)

* [npm](https://www.npmjs.com/get-npm)

* [[!DNL Acrobat Services] API](https://www.adobe.io/apis/documentcloud/dcsdk/)

* [Adobe dell&#39;API di Document Generation](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [API Adobe Sign](https://www.adobe.io/apis/documentcloud/sign.html)

* [Adobe del tagger di generazione del documento](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

## Risoluzione del problema

Ora che hai gli strumenti installati, puoi iniziare a risolvere il problema. Le proposte hanno sia contenuti statici che contenuti dinamici specifici per ogni cliente. I colli di bottiglia si verificano perché entrambi i tipi di dati sono necessari ogni volta che si effettua una proposta. L&#39;immissione del testo statico richiede molto tempo e consente di automatizzarlo e gestire manualmente i dati dinamici di ciascun client.

Per prima cosa, crea un modulo di acquisizione dati in [Microsoft Forms](https://www.office.com/launch/forms?auth=1) (o nel tuo generatore di moduli preferito). Modulo per i dati dinamici dei client aggiunti a una proposta di vendita. Compila questo modulo con domande per ottenere i dettagli necessari dai clienti, ad esempio il nome della società, la data, l&#39;indirizzo, l&#39;ambito del progetto, i prezzi e altri commenti. Per creare il proprio modello, utilizzare [form](https://forms.office.com/Pages/ShareFormPage.aspx id=DQSIkWdsW0yxEjajBLZtrQAAAAAAAAAAN__rtiGj5UNElTR0pCQ09ZNkJRUlowSjVQWDNYUEg2RC4u&amp;sharetoken=1AJeMavBAzzxuISRKmUy). L’obiettivo è che i potenziali clienti compilino il modulo, per poi esportare le loro risposte come file JSON, che vengono passati alla parte successiva del flusso di lavoro.

Alcuni generatori di moduli consentono solo di esportare i dati come file CSV. Pertanto, potresti trovare utile [convertire](http://csvjson.com/csv2json) il file CSV generato in un file JSON.

I dati statici vengono riutilizzati in ogni proposta di vendita. Pertanto, è possibile utilizzare un modello di proposta di vendita in Microsoft Word per fornire il testo statico. Puoi utilizzare questo [modello](https://1drv.ms/w/s!AiqaN2pp7giKkmhVu2_2pId9MiPa?e=oeqoQ2), ma puoi creare un [modello di Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html) personalizzato.

Ora, hai bisogno di qualcosa che prenda sia i dati dinamici dei clienti nel formato JSON e il testo statico nel modello Microsoft Word per fare una proposta di vendita unica per un cliente. Le API [!DNL Acrobat Services] vengono utilizzate per unire i due elementi e generare un PDF che può essere firmato.

Per farlo funzionare, si utilizzano i tag. I tag sono stringhe di facile utilizzo che possono rappresentare numeri, parole, matrici o anche oggetti complessi. I tag fungono da segnaposto per i dati dinamici, che in questo caso sono dati client immessi nel modulo. Una volta inseriti i tag nel modello, puoi mappare i campi modulo dal file JSON al modello Word.

## Uso dei tag

Apri il modello di proposta di vendita e seleziona la scheda **Inserisci**. Nel gruppo **Componenti aggiuntivi**, selezionare **Ottieni componenti aggiuntivi**. Quindi, seleziona **Adobe componente aggiuntivo Document Generation** per aggiungerlo. Una volta aggiunto, viene visualizzato il tag Document Generation nella scheda **Home** del gruppo **Adobe**.

Nella scheda **Home** del gruppo **Adobe**, selezionate **Document Generation** per iniziare ad applicare i tag al documento. Viene visualizzato un video dimostrativo in un pannello sul lato destro della finestra.

![Schermata del componente aggiuntivo Document Tagger in Word](assets/sales_1.png)

Seleziona **Introduzione**. Viene quindi richiesto di fornire dati di esempio. Incolla o carica il file JSON di risposta del modulo come illustrato di seguito.

![Schermata per incollare il codice di esempio](assets/sales_2.png)

Seleziona **Genera tag** per ottenere un elenco di campi dal file JSON che hai incollato o caricato. I tag sono mostrati di seguito, sulla barra laterale destra.

![Schermata dei tag disponibili](assets/sales_3.png)

Dopo aver generato i tag, potete inserirli nel documento. I tag vengono aggiunti al documento nella posizione del cursore. Come mostrato sopra, è necessario aggiungere il tag **Ambito progetto** direttamente sotto il sottotitolo **Ambito progetto**. In questo modo, quando un client entra nell&#39;ambito del progetto nel modulo, la sua risposta va sotto il sottotitolo **Ambito progetto**, sostituendo il tag che hai appena aggiunto. Dopo aver aggiunto i tag, parte del documento dovrebbe essere simile all’immagine acquisita sullo schermo.

![Schermata per l&#39;aggiunta di tag al documento di Word](assets/sales_4.png)

## Utilizzo delle API

Vai alla [home page](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html) delle API [!DNL Acrobat Services]. Per iniziare a utilizzare le API [!DNL Acrobat Services], sono necessarie le credenziali per l&#39;applicazione. Scorri fino in fondo e seleziona **Avvia prova gratuita** per creare le credenziali. Puoi utilizzare questi servizi [gratis per sei mesi, quindi pagare in base al consumo](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) a soli $ 0,05 per ogni transazione documento, in modo da pagare solo ciò di cui hai bisogno.

Seleziona **API dei servizi PDF** come servizio preferito e compila gli altri dettagli come illustrato di seguito.

![Schermata di recupero delle credenziali](assets/sales_5.png)

Una volta create le credenziali, vengono forniti alcuni esempi di codice. Seleziona la lingua preferita (questo tutorial utilizza Node.js). Le credenziali API sono in un file zip. Estrai i file in PDFToolsSDK-Node.jsSamples.

Per iniziare, crea una cartella vuota denominata doc automatico\*\*.\*\* Nella cartella, eseguire il comando seguente per inizializzare un progetto Node.js: `npm init`. Assegna un nome al progetto &quot;documento automatico&quot;*.*

Nella cartella ./PDFToolsSDK-Node.jsSamples/adobe-dc-pdf-tools-sdk-node-samples, c&#39;è un file chiamato pdftools-api-credentials.json. Spostalo insieme a private.key nella cartella del documento automatico. Contiene le tue credenziali API. Inoltre, nella cartella del documento automatico, crea una sottocartella denominata &quot;resources&quot;. Contiene i dati formattati JSON ricevuti dai clienti ogni volta che generi una proposta di vendita. Nella stessa cartella, salva il modello di proposta di vendita da Microsoft Word.

Ora sei pronto a fare un po&#39; di magia! Poiché si utilizza Node.js in questo tutorial, è necessario installare Node.js [!DNL Acrobat Services] SDK. A tale scopo, nella cartella auto-doc, eseguire yarn add @adobe/documentservices-pdftools-node-sdk.

Ora crea un file denominato merge.js e incolla il codice seguente al suo interno.

```
javascript
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk'),
fs = require('fs');
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Setup input data for the document merge process
const jsonString = fs.readFileSync('resources/Proposal.json'),
jsonDataForMerge = JSON.parse(jsonString);
// Create an ExecutionContext using credentials
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials);
// Create a new DocumentMerge options instance
const documentMerge = PDFToolsSdk.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.PDF);
// Create a new operation instance using the options instance
const documentMergeOperation = documentMerge.Operation.createNew(options)
// Set operation input document template from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile('resources/Proposal.docx');
documentMergeOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
.then(result => result.saveAsFile('output/Proposal.pdf'))
.catch(err => {
if (err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log('Exception encountered while executing operation', err);
} else {
console.log('Exception encountered while executing operation', err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
```

Questo codice ottiene il file JSON dal modulo Microsoft con l&#39;aiuto dei tag creati con [!DNL Acrobat Services]. Unisce quindi i dati con il modello di proposta di vendita creato in Microsoft Word per generare un nuovo PDF. Il PDF viene salvato nel file appena creato./output.

Inoltre, il codice utilizza [Adobe Sign API](https://www.adobe.io/apis/documentcloud/sign.html) per richiedere a entrambe le società di firmare la proposta di vendita generata. Per una spiegazione dettagliata di questa API, leggi questo post del blog.

## Fasi seguenti

Hai iniziato con un processo inefficiente e noioso che richiedeva automazione. Si è passati dalla creazione manuale di documenti per ogni cliente alla creazione di un flusso di lavoro semplificato per automatizzare e semplificare [il processo delle proposte di vendita](https://www.adobe.io/apis/documentcloud/dcsdk/sales-proposals-and-contracts.html).

Con Microsoft Forms, i clienti ottengono dati critici che andranno nelle loro proposte uniche. È stato creato un modello di proposta di vendita in Microsoft Word per fornire il testo statico che non si desidera ricreare ogni volta. Hai quindi utilizzato [!DNL Acrobat Services] API per unire i dati del modulo e del modello per creare un PDF delle proposte di vendita per i tuoi clienti in modo più efficiente.

Questo tutorial pratico offre solo un’idea di ciò che è possibile fare con queste API. Per scoprire altre soluzioni, visita la pagina delle API [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html). Usa tutti questi strumenti gratuitamente per sei mesi. Quindi, paga solo $0,05 per transazione documento sul piano [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html), in modo da pagare solo quando il tuo team aggiunge più prospettive alla tua pipeline delle vendite.
