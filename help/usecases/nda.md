---
title: Creazione di un accordo di riservatezza
description: Scopri come creare un PDF NDA dinamico per la collaborazione
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8098
thumbnail: KT-8098.jpg
exl-id: f4ec0182-a46e-43aa-aea3-bf1d19f1a4ec
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '1164'
ht-degree: 3%

---

# Creazione di un accordo di riservatezza

![Banner Hero per casi di utilizzo](assets/UseCaseNDAHero.jpg)

Le organizzazioni collaborano con collaboratori esterni per creare i propri servizi e prodotti. Un accordo di non divulgazione (NDA) è una parte importante di queste collaborazioni. Esso vieta a tutte le parti di divulgare informazioni riservate che potrebbero danneggiare una delle due entità.

Il formato NDA più utilizzato è un documento PDF. Le organizzazioni preparano un accordo di riservatezza e lo inviano a tutte le parti. Quindi, una volta che tutti hanno firmato, avviano il contratto. In un team ad alta velocità, la creazione manuale di PDF rallenta il progresso.

## Cosa puoi imparare

Questo tutorial pratico spiega come creare un modello NDA Microsoft Word specializzato per la tua azienda. Componente aggiuntivo gratuito di Adobe per Microsoft Word, [Adobe di Tagger Document Generation](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo), inserisce &quot;tags&quot; per inserire i valori dinamici. Scopri come passare i dati JSON al modello e creare un PDF dinamico. Il PDF risultante può essere inviato tramite e-mail o mostrato ai tuoi collaboratori nel browser, a seconda delle esigenze e degli obiettivi aziendali. Per seguirmi, hai solo bisogno di una piccola esperienza con Node.js, JavaScript, Express.js, HTML e CSS.

## API e risorse pertinenti

Con [!DNL Adobe Acrobat Services], puoi generare documenti PDF al volo utilizzando i dati dinamici. [!DNL Acrobat Services] offre una suite di strumenti di PDF, tra cui l’API di Document Generation di Adobe per automatizzare [Creazione di accordi di riservatezza](https://www.adobe.io/apis/documentcloud/dcsdk/nda-creation.html).

* [Adobe dell’API di Document Generation](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [API Adobe Sign](https://www.adobe.io/apis/documentcloud/sign.html)

* [Adobe di Tagger Document Generation](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

* [Codice progetto](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample)

* [[!DNL Acrobat Services] tasti](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getcred)

## Creazione del modello JSON

Il modello Microsoft Word dipende dal modello JSON, quindi devi prima crearlo. Per l’esercitazione, utilizza una struttura JSON di base che contiene i dettagli aziendali, come le informazioni di contatto.

```
{
"vendor": {
"companyName": "GlobalCorp",
"street": "123 Any Street",
"street2": "",
"city":"Anywhere",
"state":"CA",
"primaryContact": {
"firstName":"John",
"lastName":"Doe",
"email":"john-doe@example.com",
"phone":"123-456-7890"
}
},
"authorizedSigner": {
"firstName": "Sarah",
"lastName": "Rose",
"email": "sarah@example.com",
"phone":"555-555-1234"
}
}
```

Questa struttura viene utilizzata all&#39;interno di Microsoft Word per generare un modello. Questi dati possono provenire da qualsiasi origine dati, purché sia in formato JSON. Per semplicità, si creano più file all&#39;interno dell&#39;applicazione Node.js, ma il caso d&#39;uso potrebbe richiedere una connessione di database per richiamare le informazioni del fornitore.

## Creazione del modello Microsoft Word

Creare il modello NDA in un documento di Microsoft Word. L’API Adobe PDF Services prevede che il documento Microsoft Word contenga tag in cui il servizio può inserire valori da documenti JSON. Sebbene il modello sia lo stesso per tutte le richieste da Adobe, i dati dinamici in JSON cambiano. Questi tag consentono di creare documenti PDF per ogni fornitore in questo caso, utilizzando un singolo modello Microsoft Word e velocizzando il processo automatizzando la generazione di documenti NDA.

È possibile installare [componente aggiuntivo gratuito Document Generation Tagger](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo) in Microsoft Word. Se fai parte di un&#39;organizzazione, puoi richiedere all&#39;amministratore di Microsoft Office di installare il componente aggiuntivo gratuito per tutti.

Una volta installato il componente aggiuntivo, puoi trovarlo nella scheda Home nella categoria Adobe. Per aprire la scheda, seleziona **Document Generation**:

![Schermata del componente aggiuntivo Document Generation in Word](assets/nda_1.png)

All’interno della scheda, puoi caricare il documento JSON di esempio. Questo documento può essere un esempio perché viene utilizzato solo per creare un modello di Microsoft Word.

![Schermata con dati di esempio nel componente aggiuntivo Document Generation](assets/nda_2.png)

Seleziona **Genera tag** per visualizzare gli elementi che è possibile utilizzare all&#39;interno del modello. Di seguito sono riportate le proprietà estratte dalla struttura JSON, pronte per essere utilizzate nel modello:

![Schermata dei tag di testo nel componente aggiuntivo Document Generation](assets/nda_3.png)

Queste sono le funzioni della `authorizedSigner` campo. Gli altri campi vengono disposti a capo ed è possibile espandere la visualizzazione in Microsoft Word. Il componente aggiuntivo offre inoltre opzioni avanzate per i dati, ad esempio tabelle, elenchi, valori calcolati e altro ancora.

## Creazione dei tag

Crea un modello o importa un [modello esistente](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html#sample-blade) in Microsoft Word. Dopo aver configurato il documento, aggiungi tag a ciascun campo facendo clic sui token corrispondenti nel componente aggiuntivo.

Il seguente modello in un file di Microsoft Word:

![Schermata del modello di esempio](assets/nda_4.png)

Questo file contiene diversi tag. Quando si esegue il programma, questi campi vengono compilati con le informazioni sul fornitore.

Document Generation Tagger si integra con l’API di Adobe Sign. Grazie a questa integrazione, puoi creare automaticamente i tag di testo di Sign in modo che il documento generato possa essere inviato ad Adobe Sign per la firma.

## Generazione dell&#39;accordo di non divulgazione per i fornitori

Nell’applicazione di esempio, sono state preparate cartelle per l’input e l’output. Come accennato in precedenza, si utilizzano file JSON, in modo che ci siano due file per mostrare i fornitori disponibili nel sistema. I file vengono visualizzati all&#39;interno di un modulo che viene stampato sul browser:

```
<h1><b>NDA</b>: Generate for vendor.</h1>
<hr />
<p>Following ({{files.length}}) vendors are ready, select to generate NDA and deliver for signature:</p>
<form method="POST">
<ul>
{{#each files }}
<li><input type="checkbox" name="vendor" value="{{this}}" id="file-{{@index}}" /> <label for="file-{{@index}}">{{this}}</label></li>
{{/each}}
</ul>
<input type="submit" value="Create NDA" />
</form>
```

Questo codice genera la seguente interfaccia utente (UI) nel browser:

![Schermata dell’interfaccia utente Crea NDA](assets/nda_5.png)

Quando l’amministratore seleziona una persona, l’app utilizza i Servizi Adobe PDF per generare l’accordo di riservatezza in mobilità.

```
async function compileDocFile(json, inputFile, outputPdf) {
try {
// configurations
const credentials = adobe.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("./src/pdftools-api-credentials.json")
.build();
// Capture the credential from app and show create the context
const executionContext = adobe.ExecutionContext.create(credentials);
// create the operation
const documentMerge = adobe.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(json, documentMergeOptions.OutputFormat.PDF);
const operation = documentMerge.Operation.createNew(options);
// Pass the content as input (stream)
const input = adobe.FileRef.createFromLocalFile(inputFile);
operation.setInput(input);
// Async create the PDF
let result = await operation.execute(executionContext);
await result.saveAsFile(outputPdf);
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
```

Utilizzare questo codice all&#39;interno del router Express:

```
// Create one report and send it back
try {
console.log(`[INFO] generating the report...`);
const fileContent = fs.readFileSync(`./public/documents/raw/${vendor}`, 'utf-8');
const parsedObject = JSON.parse(fileContent);
await pdf.compileDocFile(parsedObject, `./public/documents/template/Adobe-NDA-Sample.docx`, `./public/documents/processed/output.pdf`);
console.log(`[INFO] sending the report...`);
res.status(200).render("preview", { page: 'nda', filename: 'output.pdf' });
} catch(error) {
console.log(`[ERROR] ${JSON.stringify(error)}`);
res.status(500).render("crash", { error: error });
}
```

Puoi visualizzare [il codice di esempio completo](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample) su GitHub.

Questo codice utilizza un documento JSON e il modello Microsoft Word nella chiamata API al [!DNL Adobe Acrobat Services] SDK Nella risposta, riceverai l&#39;output e lo salverai nel file system dell&#39;app. Puoi inoltrare il documento generato ai tuoi clienti via e-mail o mostrare loro un&#39;anteprima all&#39;interno del browser utilizzando il software gratuito [API di Adobe PDF Embed](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

Questa chiamata crea il seguente documento NDA:

![Schermata di anteprima del documento NDA](assets/nda_6.png)

[!DNL Adobe Acrobat Services] Le API inseriscono il contenuto per creare un documento PDF. Senza questi strumenti, potrebbe essere necessario scrivere il codice per elaborare i documenti di Office e lavorare con i formati di file raw PDF. Con l&#39;aiuto dei Servizi Adobe PDF, puoi eseguire tutti questi passaggi con una singola chiamata API.

Ora utilizzare [API ADOBE SIGN](https://www.adobe.io/apis/documentcloud/sign.html) chiedere la firma delle dichiarazioni di riservatezza e consegnare il documento finale firmato a tutte le parti. Adobe Sign invia una notifica [utilizzo di un webhook](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/webhooks.md). Ascoltando questo webhook, puoi recuperare lo stato dell’accordo di riservatezza.

Per una spiegazione più dettagliata del processo di Adobe Sign, [consulta la documentazione](https://www.adobe.io/apis/documentcloud/sign/docs.html) oppure leggete questo approfondito post di blog.

## Fasi seguenti

In questo tutorial pratico, l’Adobe Document Generation Tagger è stato utilizzato per generare dinamicamente documenti PDF utilizzando modelli di Microsoft Word e file di dati JSON. Il componente aggiuntivo ha contribuito a [crea automaticamente accordi di riservatezza](https://www.adobe.io/apis/documentcloud/dcsdk/nda-creation.html) personalizzato per ciascuna parte, quindi raccogli le firme tramite l’API Sign.

Puoi utilizzare queste tecniche per creare dinamicamente i tuoi accordi di riservatezza o altri documenti, lasciando al tuo team il tempo di concentrarsi sul lavoro produttivo. Esplora [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html) per trovare le API e gli SDK per la lingua e il runtime desiderati, in modo da poter aggiungere le funzioni PDF direttamente alle applicazioni per la creazione rapida di documenti PDF. [Introduzione](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) con una versione di prova gratuita per sei mesi
[pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) a soli $ 0,05 per transazione documento.
