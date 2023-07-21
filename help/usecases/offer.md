---
title: Lettere sull'offerta per la gestione dei dipendenti
description: Scopri come generare una lettera di offerta che può essere consegnata a un nuovo dipendente per la firma
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8096.jpg
jira: KT-8096
exl-id: 92f955f0-add5-4570-aa3a-ea63055dadb2
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1794'
ht-degree: 2%

---

# Gestione delle lettere di offerta dei dipendenti

![Usa banner eroe caso](assets/UseCaseOfferHero.jpg)

Le lettere di offerta per i dipendenti sono una delle prime esperienze che i dipendenti hanno con la tua organizzazione. Di conseguenza, vuoi assicurarti che le lettere di offerta siano in linea con il marchio, ma non vuoi dover creare una lettera nel tuo elaboratore di testi da zero ogni volta. [!DNL Adobe Acrobat Services] Le API offrono un modo rapido, semplice ed efficace per gestire le parti principali di [generare e inviare lettere di offerta ai nuovi dipendenti](https://www.adobe.io/apis/documentcloud/dcsdk/employee-offer-letters.html).

## Cosa puoi imparare

Questa esercitazione pratica illustra la configurazione di un progetto Node Express che visualizza un modulo Web per consentire a un utente di compilare i dettagli dei dipendenti. Questi dettagli utilizzano [!DNL Acrobat Services] sul web per generare una lettera di offerta come PDF che può essere consegnata a un cliente per la firma tramite l&#39;API di Adobe Sign.

## API e risorse pertinenti

* [API dei servizi PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Adobe API di generazione documenti](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [API Adobe Sign](https://www.adobe.io/apis/documentcloud/sign.html)

* [Componente aggiuntivo Word per tag Generazione di documenti](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=docgen-addin)

* [Esempio di progetto](https://www.adobe.io/apis/documentcloud/dcsdk/employee-offer-letters.html)

## Introduzione

[Node.js](https://nodejs.org/) è la piattaforma di programmazione. È dotato di un enorme insieme di librerie, come il server web Express. [Scarica Node.js](https://nodejs.org/en/download/) e segui i passaggi per installare questo ambiente di sviluppo open source di grande impatto.

Per utilizzare l’API di generazione dei documenti di Adobe in Node.js, accedi alla [API per la generazione di documenti](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html) per accedere al tuo account o registrarti per ottenerne uno nuovo. Il tuo account è [gratis per sei mesi, poi paghi come sei](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) a soli 0,05 dollari per transazione documentale, così puoi provarla senza rischi e pagare solo per la crescita della tua azienda.

Dopo aver effettuato l&#39;accesso al [Console Adobe Developer](https://console.adobe.io/), fate clic **[!UICONTROL Crea nuovo progetto]**. Per impostazione predefinita, il progetto è denominato &quot;Progetto 1&quot;. Fate clic sul **[!UICONTROL Modifica progetto]** e cambiare il nome in &quot;Offer Letter Generator&quot;. Al centro dello schermo si trova un **[!UICONTROL Introduzione Al Nuovo Progetto]** sezione. Per abilitare la protezione nel progetto, effettua le seguenti operazioni:

Fai clic su **Aggiungi API**. Sono disponibili diverse API tra cui scegliere. Nel **[!UICONTROL Filtro per prodotto]** seleziona **[!UICONTROL Document Cloud]**, quindi fai clic su **[!UICONTROL Avanti]**.

Ora, genera le credenziali per accedere all&#39;API. Le credenziali sono sotto forma di token Web JSON ([JWT](https://jwt.io/)): uno standard aperto per una comunicazione sicura. Se hai familiarità con JWT e hai già generato le chiavi, puoi caricare la chiave pubblica qui. In alternativa, procedi selezionando **Opzione 1** per fare in modo che l&#39;Adobe generi le chiavi per te.

![Schermata di generazione delle credenziali](assets/offer_1.png)

Fate clic sul **[!UICONTROL Genera tastiera]** pulsante. Viene scaricato un file config.zip. Decomprimete il file di archivio. Contiene due file: certificate_pub.crt e private.key. Assicurati che quest&#39;ultimo sia protetto, in quanto contiene le tue credenziali private e può essere utilizzato per generare documenti spurie, se non sei in tuo controllo.

Fai clic su **[!UICONTROL Avanti]**. No, abilita l’accesso all’API di generazione PDF. Sulla **[!UICONTROL Selezionare i profili di prodotto]** schermo, verifica **[!UICONTROL Sviluppatore di servizi per PDF aziendali]** e fare clic sul **[!UICONTROL Salva API configurata]** pulsante. A questo punto è possibile iniziare a utilizzare l&#39;API.

## Configurazione del progetto

Configurate un progetto Node per eseguire il codice. Questo esempio utilizza [Visual Studio Code](https://code.visualstudio.com/) (VS Code) come editor. Create una cartella denominata &quot;letter-generator&quot; e apritela nel Codice VS. Dal **[!UICONTROL File]** menu, seleziona **[!UICONTROL Terminale]** \> **[!UICONTROL Nuovo Terminale]** per aprire una shell in questa cartella. Verificate che Nodo sia installato e che si trovi sul percorso immettendo quanto segue:

```
node -v
```

Dovresti visualizzare la versione di Node installata.

Ora che hai installato il tuo ambiente di sviluppo, puoi creare il tuo progetto.

Innanzitutto, inizializzate il progetto utilizzando Node Package Manager (npm). Digitate quanto segue:

```
npm init
```

Vengono poste alcune domande sul progetto Node. Puoi ignorare la maggior parte di queste domande, ma assicurati che il nome del progetto sia &quot;letter-generator&quot; e che il punto di ingresso sia **index.js**. Seleziona **Sì** per completare l&#39;inizializzazione del progetto.

Ora disponi di un file package.json. Node utilizza questo file per organizzare il progetto. Prima di creare index.js, è necessario aggiungere librerie di Adobi con il comando seguente:

```
npm install --save @adobe/documentservices-pdftools-node-sdk
```

Al progetto deve essere aggiunta una nuova cartella denominata node_modules. Questa cartella è la posizione in cui vengono scaricate tutte le librerie (dette dipendenze in Node). Anche il file package.json viene aggiornato con riferimento ai servizi Adobe PDF.

Ora vuoi installare Express come framework web leggero. Immetti il comando seguente:

```
npm install express –save
```

Come prima, la sezione delle dipendenze di package.json viene aggiornata di conseguenza.

## Creazione di un modello di lettera di offerta

Ora, nella cartella principale del progetto, crea un file denominato &quot;app.js&quot;. Inseriamo il seguente codice di avvio:

```
const express = require('express');
const bodyParser = require('body-parser');
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk')
const path = require('path');
const app = express();
const port = 8000;
app.use(bodyParser.urlencoded({ extended: true }));
app.get('/', (req, res) => {
res.sendFile(path.join(__dirname + '/index.html'));
});
app.post('/', (req, res) => {
console.log('Got body:', req.body);
res.sendStatus(200);
});
app.listen(port, () => {
console.log(`Candidate offer letter app listening on port ${port}!`)
});
```

Si noti che il percorso get restituisce un **index.html** file. Creiamo un file HTML con quel nome e il seguente semplice modulo. Potete aggiungere stili CSS e altri elementi di progettazione in un secondo momento, secondo le vostre esigenze. Questo modulo contiene i dati di base del candidato per la redazione di una lettera di benvenuto:

```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Offer Letter Generator</title>
</head>
<body>
<h1>Enter Candidate Details</h1>
<form action="" method="post">
<div>
<label for="firstname">First name: </label>
<input type="text" name="firstname" id="firstname" required>
</div>
<div>
<label for="lastname">Last name: </label>
<input type="text" name="lastname" id="lastname" required>
</div>
<div>
<label for="salary">Salary ($): </label>
<input type="number" name="salary" id="salary" required>
</div>
<div>
<label for="startdate">Start Date: </label>
<input type="date" name="startdate" id="startdate" required>
</div>
<div>
<input type="submit" value="Generate Letter">
</div>
</form>
</body>
</html>
```

Esegui il server Web con il comando seguente:

```
node app.js
```

Viene visualizzato il messaggio &quot;Candidate offer letter app listening on port 8000&quot;. Se si apre il browser su <http://localhost:8000/>, il modulo deve avere l&#39;aspetto seguente:

![Screenshot del modulo Web](assets/offer_2.png)

Il modulo viene inviato a se stesso. Se si compilano dati e si fa clic su **Genera lettera,** sulla console devono essere visualizzate le seguenti informazioni:

```
Got body: { firstname: 'John',
lastname: 'Doe',
salary: '887888',
startdate: '2021-04-01' }
```

Sostituisci la registrazione della console con una chiamata al servizio Web per [!DNL Acrobat Services]. Innanzitutto, è necessario creare un modello delle informazioni basato su JSON. Il formato di questo modello è il seguente:

```
{
    "offer_letter": {
    "firstname": "John",
    "lastname": "Doe",
    "salary": "887888",
    "startdate": "2021-04-01"
    }
}
```

Puoi rendere questo modello più elaborato se lo desideri, ma per questo tutorial, attieniti a questo semplice esempio. Questo modulo non viene convalidato perché non rientra nell’ambito di questo articolo. Per convertire il corpo del modulo nel modello di dati sopra descritto, modificare il metodo del gestore app.post in modo che disponga del codice seguente:

```
app.post('/', (req, res) => {
const docModel = {'offer_letter': req.body};
generateLetter(docModel);
res.sendStatus(200);
});
```

La prima riga inserisce i dati JSON nel formato desiderato. Questi dati vengono ora passati alla funzione generateLetter. Arresta il server e incolla il codice seguente alla fine di app.js. Questo codice utilizza un documento Word come modello e riempie i segnaposto con le informazioni di un documento JSON.

```
// Letter generation function
function generateLetter(jsonDataForMerge) {
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Create an ExecutionContext using credentials
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials);
// Create a new DocumentMerge options instance
const documentMerge = PDFToolsSdk.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge,
documentMergeOptions.OutputFormat.PDF);
// Create a new operation instance using the options instance
const documentMergeOperation = documentMerge.Operation.createNew(options)
// Set operation input document template from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile(
'resources/OfferLetter-Template.docx');
documentMergeOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
.then(result => result.saveAsFile('output/OfferLetter.pdf'))
.catch(err => {
if(err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log(
'Exception encountered while executing operation', err);
} else {
console.log(
'Exception encountered while executing operation', err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
```

C&#39;è molto codice da scoprire. Prendiamo innanzitutto la parte principale: il `documentMergeOperation`. In questa sezione puoi prendere i dati JSON e unirli con un modello di documento Word. È possibile utilizzare il metodo [Adobe sul sito](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html#sample-blade) come riferimento, ma facciamo un semplice esempio. Apri Word e crea un nuovo documento vuoto. Puoi personalizzarlo quanto vuoi, ma almeno hai qualcosa di simile:

Gentile Sig. X,

Siamo lieti di offrirti una posizione di lavoro per $X all&#39;anno. La data di inizio sarà X.

Benvenuti

Salvare il documento come &quot;OfferLetter-Template.docx&quot; in una cartella denominata &quot;resources&quot; nella cartella principale del progetto. Notate le tre X nel documento. Questi Xs sono segnaposto temporanei per le informazioni JSON. Sebbene sia possibile utilizzare una sintassi speciale per sostituire questi segnaposto, nell&#39;Adobe è disponibile un componente aggiuntivo Word che semplifica l&#39;operazione. Per installare il componente aggiuntivo, vai all’Adobe [Componente aggiuntivo Word per tag Generazione di documenti](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=docgen-addin) sito.

Nel modello OfferLetter, fai clic sul nuovo **Generazione documento** pulsante. Si apre un pannello laterale. Fai clic su **Guida introduttiva**. Viene fornita un&#39;area di testo da incollare nei dati JSON di esempio. Copia lo snippet &quot;offer-data&quot; di JSON dall&#39;alto nell&#39;area di testo. Dovrebbe avere l&#39;aspetto seguente:

![Screenshot di lettere e codice](assets/offer_3.png)

Fate clic sul **Genera tag** pulsante. È disponibile un menu a discesa di tag da inserire nei punti appropriati del documento. Evidenzia la prima X nel documento e seleziona **[!UICONTROL nome]**. Fai clic su **[!UICONTROL Inserisci testo]** e &quot;Dear X,&quot; viene modificato in &quot;Dear ```{{`offer_letter`.firstname}}```,&quot;. Questo tag è il formato corretto per `documentMergeOperation`. Continua e aggiungi i tre tag rimanenti all&#39;Xs appropriato. Non dimenticare di salvare OfferLetter-template.docx. Dovrebbe avere questo aspetto:

Gentile ```{{`offer_letter`.firstname}} {{`offer_letter`.lastname}}```,

Siamo lieti di offrirti una posizione di prezzo ```{{`offer_letter`.salary}}``` un anno. La data di inizio sarà ```{{`offer_letter`.startdate}}```.

Benvenuti

Ora il modello Word ha una marcatura che corrisponde al formato JSON. Ad esempio, ```{{`offer_letter`.`firstname`}}``` all&#39;inizio del documento Word viene sostituito dal valore nella sezione &quot;firstname&quot; dei dati JSON.

Torna al tuo `generateLetter` funzione. Per proteggere la chiamata REST, crea un nuovo file denominato pdftools-api-credentials.json nella cartella principale del progetto. Incolla i seguenti dati JSON e modificali con i dettagli della sezione Account di servizio (JWT) del [Console per sviluppatori](https://console.adobe.io/).

```
{
"client_credentials": {
"client_id": "<YOUR_CLIENT_ID>",
"client_secret": "<YOUR_CLIENT_SECRET>"
},
"service_account_credentials": {
"organization_id": "<YOUR_ORGANIZATION_ID>",
"account_id": "<YOUR_TECHNICAL_ACCOUNT_ID>",
"private_key_file": "<PRIVATE_KEY_FILE_PATH>"
}
}
```

* L&#39;ID client, il segreto client e l&#39;ID dell&#39;organizzazione possono essere copiati direttamente dal **[!UICONTROL Dettagli credenziali]** della console.

* L&#39;ID account è **ID account tecnico**.

* Copiate il file private.key generato in precedenza nel progetto e immettetene il nome nella sezione private_key_file del file pdftools-api-credentials.json. Se lo desideri, puoi inserire un percorso al file della chiave privata qui. Ricordati di proteggerlo perché potrebbe essere usato male una volta fuori controllo.

Per generare un PDF con i dati JSON compilati, torna al **[!UICONTROL Immetti dettagli candidato]** modulo web e pubblica alcuni dati. Il download del documento da un Adobe richiede un po’ di tempo, ma è consigliabile inserire un file denominato OfferLetter.pdf in una nuova cartella denominata output.

## Fasi seguenti

È tutto! Questo è solo l&#39;inizio. Se si studia la sezione Avanzate della scheda Generazione documento del componente aggiuntivo Word, si nota che non tutti i marcatori segnaposto provengono dai dati JSON associati. È anche possibile aggiungere tag per firma. Questi tag consentono di prendere il documento risultante e caricarlo in [Adobe Sign](https://acrobat.adobe.com/ca/en/sign.html) per la consegna e la firma al nuovo dipendente. Leggi la Guida introduttiva all’API di Adobe Sign per scoprire come farlo. Questo processo è simile perché si utilizzano chiamate REST protette con un token JWT.

L&#39;esempio di documento singolo fornito sopra può essere utilizzato come base per un&#39;applicazione quando un&#39;organizzazione deve [aumentare le assunzioni stagionali](https://www.adobe.io/apis/documentcloud/dcsdk/employee-offer-letters.html) di dipendenti in più sedi. Come dimostrato, il flusso principale è quello di prendere dati dai candidati attraverso un&#39;applicazione online. I dati vengono utilizzati per compilare i campi di una lettera di offerta e inviarla per la firma elettronica.

[!DNL Adobe Acrobat Services] è gratuito per sei mesi, [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) a soli $0,05 per transazione di documenti, così potrai provarlo e adattare il flusso di lavoro delle lettere di offerta man mano che la tua azienda cresce. A [inizia](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
creazione di modelli personalizzati, [registrazione dell&#39;account sviluppatore](https://www.adobe.io/).
