---
title: Creazione e modifica di report
description: Scopri come generare report PDF sul tuo sito Web per i clienti
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8093
thumbnail: KT-8093.jpg
exl-id: 2f2bf1c2-1b33-4eee-9fd2-5d0b77e6b0a9
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1292'
ht-degree: 0%

---

# Creazione e modifica di report

![Banner dell&#39;eroe del caso di utilizzo](assets/UseCaseReportHero.jpg)

Finanza, istruzione, marketing e altri settori utilizzano i PDF per condividere i dati con i clienti e gli stakeholder. I PDF semplificano la condivisione di documenti avanzati, con tabelle, elementi grafici e contenuti interattivi, in un formato visualizzabile da tutti. Le API [!DNL Adobe Acrobat Services] consentono a queste aziende di generare report PDF condivisibili da Microsoft Word, Microsoft Excel, elementi grafici e altri formati di documenti diversi.

Dì che [gestisci una società di monitoraggio dei social media](https://developer.adobe.com/document-services/use-cases/content-publishing/on-demand-report-creation). I clienti accedono a una parte del sito protetta da password per visualizzare l&#39;analisi della campagna. Spesso vogliono condividere queste statistiche con i loro dirigenti, azionisti, donatori o altri stakeholder. I documenti PDF scaricabili sono un ottimo modo per i tuoi clienti di condividere numeri, grafici e altro ancora.

L&#39;integrazione dell&#39;API [PDF Services](https://developer.adobe.com/document-services/apis/pdf-services) nel sito Web consente di generare report PDF in mobilità per ogni cliente. Puoi creare dei PDF e quindi combinarli in un unico rapporto pratico che i tuoi clienti potranno scaricare e trasmettere ai loro stakeholder.

## Cosa puoi imparare

In questo tutorial pratico, scoprite come utilizzare PDF Services SDK in un ambiente Node.js ed Express.js (con solo alcuni JavaScript, HTML e CSS) per aggiungere rapidamente e facilmente funzionalità orientate ai PDF a un sito Web esistente. Questo sito Web include una pagina in cui gli amministratori caricano i report, un&#39;area in cui i clienti visualizzano un elenco di report disponibili e selezionano i documenti da convertire in PDF e utili endpoint per scaricare i PDF generati dal sistema.

## API e risorse pertinenti

* [API dei servizi PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [API di incorporamento PDF](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

## Dashboard dei report delle campagne per i clienti

>[!NOTE]
>
>Questo tutorial non riguarda le procedure consigliate di Node.js o come proteggere le applicazioni Web. Alcune aree del sito Web sono accessibili al pubblico e la denominazione dei documenti potrebbe non essere compatibile con la produzione. Per discutere il miglior approccio possibile alla progettazione di un sistema di questo tipo, consultare gli architetti e gli ingegneri.

Qui, hai un&#39;applicazione web di base Express.js che ha un&#39;area rapporti clienti e una sezione amministratore. Questa applicazione può mostrare i report per le campagne sui social media. Ad esempio, può dimostrare quante volte si fa clic su un annuncio.

![Schermata di come ottenere report personalizzati](assets/report_1.png)

Puoi scaricare questo progetto dal [repository GitHub](https://github.com/afzaal-ahmad-zeeshan/express-adobe-pdf-tools).

Scopriamo ora come pubblicare i report.

## Caricamento dei report

Per semplificare, utilizzare solo il caricamento e l&#39;elaborazione basati su file system. In Express.js, puoi utilizzare il modulo fs per elencare tutti i file disponibili in una directory.

Nella stessa pagina, abilita l&#39;amministratore per caricare i file del report sul server per consentirne la visualizzazione ai clienti. Questi file possono essere in molti formati diversi, ad esempio Microsoft Word, Microsoft Excel, HTML e [altri formati di dati](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf), inclusi i file grafici. La pagina di amministrazione è simile alla seguente:

![Schermata delle funzionalità di amministrazione](assets/report_2.png)

>[!NOTE]
>
>Proteggi con password i tuoi URL o utilizza il pacchetto Passport di npm per proteggere l&#39;applicazione dietro il livello di autenticazione e autorizzazione.

Quando l&#39;amministratore seleziona e carica un file, questo viene spostato in un repository pubblico a cui possono accedere altri utenti. È possibile utilizzare lo stesso repository per pubblicare documenti dalla pagina di amministrazione ed elencare i report di marketing disponibili per i clienti. Questo codice è:

```
router.get('/', (req, res) => {
try {
let files = fs.readdirSync('./public/documents/raw') // read the files
res.status(200).render("reports", { page: 'reports', files: files });
} catch (error) {
res.status(500).render("crash", { error: error });
}
});
```

Questo codice elenca tutti i file ed esegue il rendering di una visualizzazione dell&#39;elenco dei file.

## Selezione dei report

Per quanto riguarda l’utente, è disponibile un modulo che consente ai clienti di selezionare i documenti da includere nel report della campagna sui social media. Per semplicità, nella pagina di esempio, mostra solo il nome del documento e una casella di controllo per selezionare il documento. I clienti possono selezionare uno o più report da combinare in un unico documento PDF.

Per un&#39;interfaccia utente più avanzata, è inoltre possibile visualizzare un&#39;anteprima del report qui.

![Schermata delle funzionalità del cliente](assets/report_3.png)

## Generazione di un report PDF

Utilizzare PDF Services SDK per creare i report PDF dai dati immessi. I dati (come mostrato nelle schermate in alto) possono provenire da diversi formati di dati come Microsoft Word, Microsoft Excel, HTML, grafica e altro ancora. Per iniziare, installare il pacchetto npm per PDF Services SDK.

```
$ npm install --save @adobe/documentservices-pdftools-node-sdk
```

Prima di iniziare, è necessario disporre di credenziali API, [libere da Adobe](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getcred). Usa il tuo account [!DNL Acrobat Services] [gratis per sei mesi, quindi paga in base al consumo](https://developer.adobe.com/document-services/pricing/main) a soli \$0,05 per ogni transazione documento.

Scarica il file di archivio ed estrai il file JSON per le credenziali e la chiave privata. Nel progetto di esempio, il file viene inserito nella directory src.

![Schermata della directory src](assets/report_4.png)

Dopo aver impostato le credenziali, è possibile scrivere l&#39;attività di conversione PDF. Per questa dimostrazione, è necessario eseguire due operazioni nell&#39;applicazione:

* Convertire documenti raw in file PDF

* Combina più file PDF in un unico report

La procedura generale è simile per l&#39;esecuzione di qualsiasi operazione. L&#39;unica differenza è il servizio che si utilizza. Nel codice seguente, convertite il documento raw in un file PDF:

```
async function createPdf(rawFile, outputPdf) {
try {
// configurations
const credentials = adobe.Credentials
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

Nel codice precedente, leggere le credenziali e creare il contesto di esecuzione. PDF Services SDK richiede il contesto di esecuzione per autenticare le richieste.

Quindi, eseguite l’operazione Crea PDF che converte i documenti raw in formato PDF. Utilizzare infine il parametro `outputPdf` per copiare il report PDF. Nell&#39;esempio di codice, questo codice si trova nel file src/helpers/pdf.js. Più avanti in questo tutorial, puoi importare il modulo PDF e chiamare questo metodo.

Come dimostrato nella sezione precedente, i clienti possono accedere alla pagina seguente per selezionare i report che desiderano convertire in PDF:

![Schermata delle funzionalità del cliente](assets/report_3.png)

Quando un cliente seleziona uno o più di questi report, viene creato il file PDF.

Per prima cosa, vediamo un singolo file PDF in azione. Quando l’utente seleziona un singolo report, devi solo convertirlo in PDF e fornire il collegamento di download.

```
try {
console.log(`[INFO] generating the report...`);
await pdf.createPdf(`./public/documents/raw/${reports}`, `./public/documents/processed/output.pdf`);
console.log(`[INFO] sending the report...`);
res.status(200).render("download", { page: 'reports', filename: 'output.pdf' });
} catch(error) {
console.log(`[ERROR] ${JSON.stringify(error)}`);
res.status(500).render("crash", { error: error });
}
```

Questo codice crea un report e condivide l’URL di download con il cliente. Di seguito è riportata la pagina Web di output:

![Schermata della schermata di download del cliente](assets/report_5.png)

Ecco il PDF di output:

![Schermata del report generico](assets/report_6.png)

I clienti possono selezionare più file per generare un report combinato. Quando il cliente seleziona più di un documento, si eseguono due operazioni: la prima crea un PDF parziale per ciascun documento e la seconda li combina in un unico report PDF.

```
async function combinePdf(pdfs, outputPdf) {
try {
// configurations
const credentials = adobe.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("./src/pdftools-api-credentials.json")
.build();
// Capture the credential from app and show create the context
const executionContext = adobe.ExecutionContext.create(credentials),
operation = adobe.CombineFiles.Operation.createNew();
// Pass the PDF content as input (stream)
for (let pdf of pdfs) {
const source = adobe.FileRef.createFromLocalFile(pdf);
operation.addInput(source);
}
// Async create the PDF
let result = await operation.execute(executionContext);
await result.saveAsFile(outputPdf);
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
```

Questo metodo è disponibile nel file src/helpers/pdf.js ed è esposto come parte dell’esportazione del modulo.

```
try {
console.log(`[INFO] creating a batch report...`);
// Create a batch report and send it back
let partials = [];
for (let index in reports) {
const name = `partial-${index}-${reports[index]}`;
await pdf.createPdf(`./public/documents/raw/${reports[index]}`, `./public/documents/processed/${name}`);
partials.push(`./public/documents/processed/${name.replace('docx', 'pdf').replace('xlsx', 'pdf')}`);
}
await pdf.combinePdf(partials, `./public/documents/processed/output.pdf`);
console.log(`[INFO] sending the combined report...`);
res.status(200).render("download", { page: 'reports', filename: 'output.pdf' });
} catch(error) {
console.log(`[ERROR] ${JSON.stringify(error)}`);
res.status(500).render("crash", { error: error });
}
```

Questo codice genera un report compilato per più documenti di input. L&#39;unica funzione aggiunta è il metodo `combinePdf` che accetta un elenco di nomi di percorso dei file PDF e restituisce un singolo PDF di output.

Ora, i clienti del tuo dashboard sui social media possono selezionare i report pertinenti dal proprio account e scaricarli come un pratico PDF. Questo dashboard consente ai responsabili e agli altri stakeholder di mostrare il successo delle campagne con dati, tabelle e grafici in un formato universalmente facile da aprire.

## Fasi seguenti

Questo tutorial pratico spiega come utilizzare l’API di PDF Services per aiutare i clienti a scaricare i report pertinenti come PDF facili da condividere. È stata creata un&#39;applicazione Node.js per illustrare le potenzialità dell&#39;API di PDF Services per i servizi di reporting e lettura di PDF. L’applicazione ha dimostrato come i clienti possono scaricare un singolo documento del report o combinare e unire più documenti in un unico report PDF.

Questa applicazione con tecnologia Adobe consente ai [clienti del dashboard per social media](https://developer.adobe.com/document-services/use-cases/content-publishing/on-demand-report-creation) di ottenere e condividere i report necessari, senza preoccuparsi se nel dispositivo sono installati Microsoft Office o altri software. Puoi utilizzare le stesse tecniche nella tua applicazione per aiutare i tuoi utenti a visualizzare, combinare e scaricare i documenti. In alternativa, consulta le altre API di Adobe per aggiungere firme e altro ancora.

Per iniziare, richiedi il tuo account gratuito [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html), quindi crea esperienze di reporting coinvolgenti per i tuoi dipendenti e clienti. Approfitta gratuitamente del tuo account per sei mesi, poi [paga in base al consumo](https://developer.adobe.com/document-services/pricing/main) man mano che le tue attività di marketing aumentano, a soli \$0,05 per transazione documento.
