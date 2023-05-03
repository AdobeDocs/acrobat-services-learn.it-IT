---
title: Gestione delle fatture
description: Scopri come generare automaticamente, proteggere tramite password e inviare le fatture ai clienti
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8145.jpg
kt: 8145
exl-id: 5871ef8d-be9c-459f-9660-e2c9230a6ceb
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '1427'
ht-degree: 1%

---

# Gestione delle fatture

![Usa banner eroe caso](assets/UseCaseInvoicesHero.jpg)

È fantastico quando le aziende sono in piena espansione, ma la produttività ne risente quando arriva il momento di preparare tutte queste fatture. La generazione manuale delle fatture richiede molto tempo, oltre al rischio di commettere un errore, di perdere denaro o di far arrabbiare un cliente con un importo non corretto.

Ad esempio, pensa a Danielle che lavora nel [ufficio contabilità](https://www.adobe.io/apis/documentcloud/dcsdk/invoices.html) [di una società di fornitura medica](https://www.adobe.io/apis/documentcloud/dcsdk/invoices.html). È la fine del mese, quindi estrae informazioni da diversi sistemi, verifica la sua accuratezza e formattazione delle fatture. Dopo tutto questo lavoro, finalmente è pronta a convertire i documenti in PDF (in modo che tutti possano visualizzarli senza acquistare un software specifico) e inviare ad ogni cliente la propria fattura personalizzata.

Anche quando la fatturazione mensile è completa, Danielle non può sfuggire a queste fatture. Alcuni clienti hanno cicli di fatturazione non mensili, quindi crea sempre una fattura per qualcuno. Di tanto in tanto, un cliente modifica la propria fattura e paga di più. Danielle passa quindi del tempo a risolvere questo problema di mancata corrispondenza delle fatture. A questo ritmo, deve assumere un assistente per stare al passo con tutto il lavoro!

Danielle ha bisogno di un modo per generare le fatture in modo rapido e accurato, sia in batch alla fine del mese che ad hoc in altre occasioni. Idealmente, se potesse proteggere queste fatture dalle modifiche, non dovrebbe preoccuparsi di risolvere problemi con importi non corrispondenti.

## Cosa puoi imparare

In questa esercitazione pratica, scopri come utilizzare l’API di generazione dei documenti di Adobe per generare automaticamente le fatture, proteggere con password i PDF e inviare una fattura a ciascun cliente. Tutto ciò che serve è una conoscenza minima di Node.js, JavaScript, Express.js, HTML e CSS.

Il codice completo per questo progetto è [disponibile su GitHub](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation). È necessario impostare la directory pubblica con il modello e le cartelle di dati raw. In produzione, devi recuperare i dati da un&#39;API esterna. Potete anche esplorare questa versione archiviata dell&#39;applicazione che contiene le risorse del modello.

## API e risorse pertinenti

* [API dei servizi PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Adobe API di generazione documenti](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [API Adobe Sign](https://www.adobe.io/apis/documentcloud/sign.html)

* [Codice progetto](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation)

## Preparazione dei dati

Questa esercitazione non analizza il modo in cui i dati vengono importati dai tuoi data warehouse. Gli ordini dei tuoi clienti potrebbero essere presenti in un database, un&#39;API esterna o un software personalizzato. L’API di generazione dei documenti di Adobe prevede un documento JSON contenente i dati di fatturazione, ad esempio informazioni provenienti dalla tua piattaforma CRM o eCommerce. Questa esercitazione presuppone che i dati siano già in formato JSON.

Per semplicità, utilizza la seguente struttura JSON per la fatturazione:

```
{ 
    "customerName": "John Doe", 
    "customerEmail": "john-doe@example.com", 
    "order": [ 
        { 
            "productId": 26, 
            "productTitle": "Bandages", 
            "price": 15.82 
        }, 
        { 
            "productId": 54, 
            "productTitle": "Masks", 
            "price": 25 
        }, 
        { 
            "productId": 76, 
            "productTitle": "Gloves", 
            "price": 7.59 
        } 
    ] 
} 
```

Il documento JSON contiene i dettagli del cliente e le informazioni sull&#39;ordine. Utilizza questo documento strutturato per compilare la fattura e visualizzare gli elementi in formato PDF.

## Creazione di un modello di fattura

L&#39;API di generazione dei documenti di Adobe prevede un modello basato su Microsoft Word e un documento JSON per creare un documento PDF dinamico o Word. Creare un modello Microsoft Word per l&#39;applicazione di fatturazione e utilizzare il metodo [componente aggiuntivo gratuito per tag Generazione documenti](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo) per generare i tag di modello. Installate il componente aggiuntivo e aprite la scheda in Microsoft Word.

![Screenshot del componente aggiuntivo Tag generazione documento](assets/invoices_1.png)

Dopo aver incollato il contenuto JSON nel componente aggiuntivo, come illustrato in precedenza, fai clic su Genera tag. Ora, questo plug-in mostra il formato dell’oggetto. Il modello di base può utilizzare il nome e l’indirizzo e-mail del cliente, ma non visualizza le informazioni sull’ordine. Le informazioni sull’ordine vengono discusse in seguito in questa esercitazione.

![Screenshot del modello Autore tag generazione documento](assets/invoices_2.png)

All’interno del documento Microsoft Word, inizia a scrivere il modello di fattura. Lasciate il cursore in cui dovete inserire i dati dinamici, quindi selezionate il tag dalla finestra del componente aggiuntivo di Adobe. Fai clic su **Inserisci testo** In questo modo, il componente aggiuntivo Adobe tag generazione può generare e inserire i tag. Per la personalizzazione, inseriamo il nome e l&#39;indirizzo e-mail del cliente.

Ora passa ai dati che cambiano con ogni nuova fattura. Selezionate la proprietà **Avanzate** scheda del componente aggiuntivo. Per visualizzare le opzioni disponibili per generare una tabella dinamica basata sui prodotti ordinati dal cliente, fate clic su **Tabelle ed elenchi** .

Seleziona **Ordine** dal primo menu a discesa. Nel secondo menu a discesa, selezionate le colonne per questa tabella. In questa esercitazione, selezionate tutte e tre le colonne per l&#39;oggetto in cui eseguire il rendering della tabella.

![Screenshot della scheda Avanzate del tag Generazione documento](assets/invoices_3.png)

L&#39;API di generazione dei documenti può anche eseguire operazioni complesse, ad esempio l&#39;aggregazione di elementi all&#39;interno di un array. Nel **Avanzate** , seleziona **Calcoli numerici** e nella **Aggregazione** , seleziona il campo in cui desideri applicare il calcolo.

![Schermata del tag Generazione documento Calcoli numerici](assets/invoices_4.png)

Fate clic sul **Inserisci calcolo** per inserire questo tag dove necessario all&#39;interno del documento. Il testo seguente viene visualizzato nel file Microsoft Word:

![Screenshot dei tag nel documento Microsoft Word](assets/invoices_5.png)

Questo esempio di fattura contiene informazioni sui clienti, i prodotti ordinati e l&#39;importo totale dovuto.

## Generazione di una fattura mediante l’API di generazione dei documenti di Adobe

Utilizzate il kit di sviluppo software (SDK) Adobe PDF Services Node.js per combinare i documenti Microsoft Word e JSON. Crea un&#39;applicazione Node.js per creare la fattura utilizzando l&#39;API di generazione del documento.

L’API dei servizi PDF include Document Generation Service, quindi puoi utilizzare le stesse credenziali per entrambi. Godetevi una [prova gratuita di sei mesi](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html)quindi pagare solo 0,05 dollari per transazione documentale.

Di seguito è riportato il codice per unire il PDF:

```
async function compileDocFile(json, inputFile, outputPdf) { 
    try { 
        // configurations 
        const credentials =  adobe.Credentials 
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

Questo codice prende informazioni dal documento JSON di input e dal file modello di input. Viene quindi creata un’operazione di unione dei documenti per combinare i file in un unico report PDF. Infine, esegue l&#39;operazione con le credenziali API. Se non li hai già, [creare le credenziali](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getting-credentials) (le API per la generazione di documenti e i servizi di PDF utilizzano le stesse credenziali).

Utilizzate questo codice all&#39;interno del router Express per gestire la richiesta di documenti:

```
// Create one report and send it back
try {
    console.log(\`[INFO] generating the report...\`);
    const fileContent = fs.readFileSync(\`./public/documents/raw/\${vendor}\`,
    'utf-8');
    const parsedObject = JSON.parse(fileContent);

    await pdf.compileDocFile(parsedObject,
    \`./public/documents/template/Adobe-Invoice-Sample.docx\`,
    \`./public/documents/processed/output.pdf\`);

    await pdf.applyPassword("p@55w0rd", './public/documents/processed/output.pdf',
    './public/documents/processed/output-secured.pdf');

    console.log(\`[INFO] sending the report...\`);
    res.status(200).render("preview", { page: 'invoice', filename: 'output.pdf' });
} catch(error) {
    console.log(\`[ERROR] \${JSON.stringify(error)}\`);
    res.status(500).render("crash", { error: error });
}
```

Una volta eseguito, il codice fornisce un documento di PDF contenente la fattura generata dinamicamente in base ai dati forniti. Con i dati JSON di esempio (forniti sopra), l&#39;output di questo codice è:

![Screenshot della fattura PDF generata dinamicamente](assets/invoices_6.png)

Questa fattura include i dati dinamici del documento JSON.

## Proteggere con password le fatture

Dal momento che Danielle, il revisore contabile, è preoccupata che i clienti possano modificare la fattura, applica una password per limitare le modifiche. [API dei servizi PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html) può applicare automaticamente una password ai documenti. In questo caso, si utilizza Adobe PDF Services SDK per proteggere i documenti con una password. Il codice è:

```
async function applyPassword(password, inputFile, outputFile) {
    try {
        // Initial setup, create credentials instance.
        const credentials = adobe.Credentials
        .serviceAccountCredentialsBuilder()
        .fromFile("./src/pdftools-api-credentials.json")
        .build();

        // Create an ExecutionContext using credentials
        const executionContext = adobe.ExecutionContext.create(credentials);
        // Create new permissions instance and add the required permissions
        const protectPDF = adobe.ProtectPDF,
        protectPDFOptions = protectPDF.options;
        // Build ProtectPDF options by setting an Owner/Permissions Password, Permissions,
        // Encryption Algorithm (used for encrypting the PDF file) and specifying the type of content to encrypt.
        const options = new protectPDFOptions.PasswordProtectOptions.Builder()
        .setOwnerPassword(password)
        .setEncryptionAlgorithm(protectPDFOptions.EncryptionAlgorithm.AES_256)
        .build();

        // Create a new operation instance.
        const protectPDFOperation = protectPDF.Operation.createNew(options);

        // Set operation input from a source file.
        const input = adobe.FileRef.createFromLocalFile(inputFile);
        protectPDFOperation.setInput(input);

        // Execute the operation and Save the result to the specified location.
        let result = await protectPDFOperation.execute(executionContext);

        result.saveAsFile(outputFile);
    } catch (err) {
        console.log('Exception encountered while executing operation', err);
    }
}
```

Quando utilizzi questo codice, protegge il documento con una password e carica una nuova fattura nel sistema. Per ulteriori informazioni sull&#39;uso di questo codice o per provarlo, vedere la [esempio di codice](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation).

Una volta completata la fattura, potrebbe essere opportuno inviarla automaticamente al client tramite e-mail. Esistono alcuni modi per inviare automaticamente e-mail ai clienti. Il modo più rapido consiste nell’utilizzare un’API e-mail di terze parti insieme a una libreria di supporto come [sendgrid-nodejs](https://github.com/sendgrid/sendgrid-nodejs). In alternativa, se disponete già dell’accesso a un server SMTP, potete utilizzare [nodatrice](https://www.npmjs.com/package/nodemailer) per inviare e-mail tramite SMTP.

## Fasi seguenti

In questo tutorial pratico hai creato una semplice app per aiutare Danielle a gestire la contabilità con [fatturazione](https://www.adobe.io/apis/documentcloud/dcsdk/invoices.html). Utilizzando l’API per i servizi PDF e l’SDK per la generazione di documenti, è stato compilato un modello Microsoft Word con le informazioni sull’ordine dei clienti provenienti da un documento JSON, creando una fattura PDF. Quindi, proteggere con password ogni documento utilizzando i servizi di protezione con password di [API dei servizi PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html).

Dal momento che Danielle può generare automaticamente le fatture e non deve preoccuparsi del fatto che i clienti ne modifichino le fatture, non dovrà assumere un assistente per svolgere tutto il lavoro manuale. Può usare il suo tempo extra per trovare risparmi sui costi nei file dei conti da pagare.

Ora che hai capito quanto sia semplice, puoi espandere questa semplice app utilizzando altri strumenti di Adobe per incorporare le fatture sul tuo sito web. Ad esempio, per consentire ai clienti di visualizzare le fatture o il saldo in qualsiasi momento. [API di incorporamento di Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) è gratuito. Puoi anche passare alle risorse umane o all&#39;ufficio vendite, aiutando ad automatizzare i loro accordi e a raccogliere firme elettroniche.

Per esplorare tutte le possibilità e iniziare a creare la tua pratica applicazione, crea un [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) account per iniziare subito. Prova gratuita di sei mesi [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html)
a soli $0,05 per transazione di documenti, a seconda del tuo business.
