---
title: Gestione delle lettere di offerta dei dipendenti
description: Scopri come generare una lettera di offerta che può essere consegnata a un nuovo dipendente per la sua firma
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8096
thumbnail: KT-8096.jpg
exl-id: 92f955f0-add5-4570-aa3a-ea63055dadb2
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '1794'
ht-degree: 2%

---

# Gestione delle lettere di offerta dei dipendenti

![Banner Hero per casi di utilizzo](assets/UseCaseOfferHero.jpg)

Le lettere di offerta dei dipendenti sono una delle prime esperienze che i dipendenti hanno con la tua organizzazione. Di conseguenza, si desidera assicurarsi che le lettere di offerta siano di marca, ma non si desidera dover costruire una lettera nel vostro elaboratore testi da zero ogni volta. [!DNL Adobe Acrobat Services] Le API offrono un modo rapido, semplice ed efficace per gestire le parti principali [generazione e consegna di lettere di offerta ai nuovi dipendenti](https://www.adobe.io/apis/documentcloud/dcsdk/employee-offer-letters.html).

## Cosa puoi imparare

Questa esercitazione pratica descrive la configurazione di un progetto Node Express che visualizza un modulo Web che un utente può compilare con i dettagli del dipendente. Questi dettagli utilizzano [!DNL Acrobat Services] sul web, per generare una lettera di offerta come PDF che può essere inviata a un cliente per la firma utilizzando l’API di Adobe Sign.

## API e risorse pertinenti

* [API di PDF Services](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Adobe dell’API di Document Generation](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [API Adobe Sign](https://www.adobe.io/apis/documentcloud/sign.html)

* [Componente aggiuntivo di Word per Tagger Document Generation](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=docgen-addin)

* [Esempio di progetto](https://www.adobe.io/apis/documentcloud/dcsdk/employee-offer-letters.html)

## Introduzione

[Node.js](https://nodejs.org/) è la piattaforma di programmazione. Viene fornito con un enorme set di librerie, come il server Web Express. [Scarica Node.js](https://nodejs.org/en/download/) e segui i passaggi per installare questo fantastico ambiente di sviluppo open-source.

Per utilizzare l’API di Document Generation di Adobe in Node.js, accedi al [API di Document Generation](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html) per accedere al tuo account o per registrarne uno nuovo. Il tuo account è [gratis per sei mesi e poi paghi in base al consumo](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) per soli $ 0,05 per transazione documento, in modo da poter provare senza rischi e pagare solo in base alla crescita dell&#39;azienda.

Dopo l&#39;accesso a [Console Adobe Developer](https://console.adobe.io/), fare clic su **[!UICONTROL Crea nuovo progetto]**. Per impostazione predefinita, il progetto è denominato &quot;Progetto 1&quot;. Fare clic sul pulsante **[!UICONTROL Modifica progetto]** e modificarne il nome in &quot;Generatore di lettere di offerta&quot;. Al centro dello schermo si trova una **[!UICONTROL Introduzione Al Nuovo Progetto]** sezione. Per abilitare la protezione per il progetto, procedi come segue:

Fai clic **Aggiungi API**. Sono disponibili diverse API tra cui scegliere. Nella **[!UICONTROL Filtra per prodotto]** , seleziona **[!UICONTROL Document Cloud]**, quindi fare clic su **[!UICONTROL Avanti]**.

Ora genera le credenziali per accedere all’API. Le credenziali sono sotto forma di token Web JSON ([JWT](https://jwt.io/)): uno standard aperto per comunicazioni sicure. Se conosci JWT e hai già generato delle chiavi, puoi caricare qui la tua chiave pubblica. In alternativa, procedere selezionando **Opzione 1** per fare in modo che Adobe generi automaticamente le chiavi.

![Schermata di generazione delle credenziali](assets/offer_1.png)

Fare clic sul pulsante **[!UICONTROL Genera coppia di chiavi]** pulsante. Viene scaricato un file config.zip. Decomprimere il file di archivio. Contiene due file: certificate_pub.crt e private.key. Assicurati che quest&#39;ultimo sia protetto, in quanto contiene le tue credenziali private e potrebbe essere utilizzato per generare documenti spuri se fuori dal tuo controllo.

Fai clic su **[!UICONTROL Avanti]**. No, abilita l’accesso all’API di generazione PDF. Nella **[!UICONTROL Selezionare i profili di prodotto]** schermo, controllo **[!UICONTROL Sviluppatore di Enterprise PDF Services]** e fare clic sul pulsante **[!UICONTROL Salva API configurata]** pulsante. Ora sei pronto per iniziare a utilizzare l’API.

## Configurazione del progetto

Imposta un progetto Nodo per eseguire il codice. In questo esempio viene utilizzato [Codice di Visual Studio](https://code.visualstudio.com/) (Codice VS) come editor. Creare una cartella denominata &quot;letter-generator&quot; e aprirla in VS Code. Dal menu **[!UICONTROL File]** menu, seleziona **[!UICONTROL Terminale]** \> **[!UICONTROL Nuovo terminale]** per aprire una shell in questa cartella. Verifica che Nodo sia installato e sul percorso immettendo quanto segue:

```
node -v
```

Dovresti visualizzare la versione del nodo che hai installato.

Ora che hai installato il tuo ambiente di sviluppo, puoi procedere e creare il tuo progetto.

Inizializzare innanzitutto il progetto utilizzando il Gestore pacchetti nodi (npm). Digitare quanto segue:

```
npm init
```

Vengono poste alcune domande sul progetto Nodo. Potete ignorare la maggior parte di queste domande, ma assicuratevi che il nome del progetto sia &quot;letter-generator&quot; e che il punto di ingresso sia **index.js**. Seleziona **Sì** per completare l&#39;inizializzazione del progetto.

Ora hai un file package.json. Nodo utilizza questo file per organizzare il progetto. Prima di creare index.js, è necessario aggiungere librerie di Adobi con il comando seguente:

```
npm install --save @adobe/documentservices-pdftools-node-sdk
```

Dovrebbe essere presente una nuova cartella denominata node_modules aggiunta al progetto. In questa cartella vengono scaricate tutte le librerie (chiamate dipendenze nel nodo). Il file package.json viene inoltre aggiornato con un riferimento ai Servizi Adobe PDF.

Ora vuoi installare Express come framework Web leggero. Immetti il comando seguente:

```
npm install express –save
```

Come prima, la sezione delle dipendenze di package.json viene aggiornata di conseguenza.

## Creazione di un modello di lettera di offerta

Nella directory principale del progetto, crea un file denominato &quot;app.js&quot;. Inseriamo il seguente codice di avvio:

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

Notare che la route GET restituisce un **index.html** file. Creiamo un file HTML con quel nome e il seguente modulo semplice. Potete aggiungere stili CSS e altri elementi di progettazione in un secondo momento. Questo modulo prende i dettagli fondamentali del candidato per generare una lettera di benvenuto:

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

Dovresti visualizzare il messaggio &quot;Candidate offer letter app in ascolto sulla porta 8000&quot;. Se apri il browser per <http://localhost:8000/>, il modulo deve essere simile al seguente:

![Schermata del modulo Web](assets/offer_2.png)

Si noti che il modulo viene pubblicato su se stesso. Se si compilano i dati e si fa clic su **Genera lettera,** dovresti visualizzare le seguenti informazioni sulla console:

```
Got body: { firstname: 'John',
lastname: 'Doe',
salary: '887888',
startdate: '2021-04-01' }
```

Sostituisci la registrazione della console con una chiamata al servizio Web a [!DNL Acrobat Services]. In primo luogo, devi creare un modello delle informazioni basato su JSON. Il formato di questo modello è simile al seguente:

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

Se lo desideri, puoi rendere questo modello più elaborato, ma per questo tutorial, attieniti a questo semplice esempio. Nessuna convalida nel modulo perché non rientra nell&#39;ambito di questo articolo. Per convertire il corpo del modulo nel modello di dati descritto in precedenza, modificare il metodo del gestore app.post in modo che disponga del codice seguente:

```
app.post('/', (req, res) => {
const docModel = {'offer_letter': req.body};
generateLetter(docModel);
res.sendStatus(200);
});
```

La prima riga inserisce i dati JSON nel formato desiderato. Ora si passano questi dati a una funzione generateLetter. Arresta il server e incolla il seguente codice alla fine di app.js. Questo codice prende un documento Word come modello e inserisce nei segnaposto le informazioni di un documento JSON.

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

C&#39;è molto codice da disimballare lì. Per prima cosa, prendiamo la parte principale: `documentMergeOperation`. In questa sezione puoi prendere i tuoi dati JSON e unirli a un modello di documento Word. È possibile utilizzare [esempio sul sito di Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html#sample-blade) come riferimento, ma facciamo il vostro semplice esempio. Aprire Word e creare un nuovo documento vuoto. Puoi personalizzarlo a tuo piacimento, ma hai almeno qualcosa del genere:

Gentile X,

Siamo lieti di offrirti una posizione per $X all&#39;anno. La data di inizio sarà X.

Benvenuti

Salvate il documento come &quot;OfferLetter-Template.docx&quot; in una cartella denominata &quot;resources&quot; nella cartella principale del progetto. Osservate le tre X nel documento. Quelle X sono segnaposto temporanei per le tue informazioni JSON. Sebbene sia possibile utilizzare una sintassi speciale per sostituire questi segnaposto, l&#39;Adobe fornisce un componente aggiuntivo di Word che semplifica questa attività. Per installare il componente aggiuntivo, consulta l’Adobe [Componente aggiuntivo di Word per Tagger Document Generation](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=docgen-addin) sito.

Nel modello OfferLetter-Template, fare clic sul nuovo **Document Generation** pulsante. Viene aperto un pannello laterale. Fai clic su **Guida introduttiva**. Viene fornita un’area di testo da incollare nei dati JSON di esempio. Copia il frammento &quot;offer-data&quot; di JSON dall’alto nell’area di testo. Dovrebbe essere simile alla seguente:

![Schermata della lettera e del codice](assets/offer_3.png)

Fare clic sul pulsante **Genera tag** pulsante. Viene visualizzato un menu a discesa con i tag da inserire nei punti appropriati del documento. Evidenzia la prima X nel documento e seleziona **[!UICONTROL firstname]**. Fai clic **[!UICONTROL Inserisci testo]** e &quot;Cara X,&quot; diventa &quot;Cara ```{{`offer_letter`.firstname}}```,&quot;. Il formato di questo tag è corretto per `documentMergeOperation`. Procedi e aggiungi i tre tag rimanenti alla X appropriata. Non dimenticare di salvare OfferLetter-template.docx. Il formato dovrebbe essere il seguente:

Gentile ```{{`offer_letter`.firstname}} {{`offer_letter`.lastname}}```,

Siamo lieti di offrirti una posizione per $ ```{{`offer_letter`.salary}}``` un anno. La data di inizio sarà ```{{`offer_letter`.startdate}}```.

Benvenuti

Ora il modello Word include markup che corrispondono al formato JSON. Ad esempio, ```{{`offer_letter`.`firstname`}}``` all’inizio del documento Word è sostituito dal valore nella sezione &quot;firstname&quot; dei dati JSON.

Torna al tuo `generateLetter` funzione. Per proteggere la tua chiamata REST, crea un nuovo file denominato pdftools-api-credentials.json nella directory principale del progetto. Incolla i seguenti dati JSON e regolali con i dettagli dalla sezione Account di servizio (JWT) del [Developer Console](https://console.adobe.io/).

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

* L&#39;ID client, il segreto client e l&#39;ID organizzazione possono essere copiati direttamente dal **[!UICONTROL Dettagli credenziali]** della console.

* L’ID account è il **ID account tecnico**.

* Copia il file private.key generato in precedenza nel progetto e immettine il nome nella sezione private_key_file del file pdftools-api-credentials.json. Se lo desideri, puoi inserire qui un percorso per il file della chiave privata. Ricordati di tenerlo al sicuro, poiché può essere utilizzato in modo improprio una volta fuori dal tuo controllo.

Per generare un PDF con i dati JSON compilati, torna al **[!UICONTROL Immetti dettagli candidato]** modulo web e pubblicare alcuni dati. Il download del documento dall&#39;Adobe richiede un po&#39; di tempo, ma dovresti avere un file denominato OfferLetter.pdf in una nuova cartella denominata output.

## Fasi seguenti

È tutto! Questo è solo l&#39;inizio. Se si studia la sezione Avanzate della scheda Document Generation del componente aggiuntivo di Word, si nota che non tutti i marcatori segnaposto provengono dai dati JSON associati. È inoltre possibile aggiungere tag di firma. Questi tag consentono di prendere il documento risultante e caricarlo in [Adobe Sign](https://acrobat.adobe.com/ca/en/sign.html) per la consegna e la firma al nuovo dipendente. Per informazioni su come eseguire questa operazione, consulta la Guida introduttiva all’API di Adobe Sign. Questo processo è simile perché si utilizzano chiamate REST protette con un token JWT.

L&#39;esempio di documento unico sopra riportato può essere utilizzato come base per un&#39;applicazione quando un&#39;organizzazione deve [aumentare le assunzioni stagionali](https://www.adobe.io/apis/documentcloud/dcsdk/employee-offer-letters.html) di dipendenti in più sedi. Come dimostrato, il flusso principale consiste nel raccogliere i dati dai candidati tramite un&#39;applicazione online. I dati vengono utilizzati per compilare i campi di una lettera di offerta e inviarla per la firma elettronica.

[!DNL Adobe Acrobat Services] è libero di utilizzare per sei mesi, quindi [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) a soli $ 0,05 per transazione documento, in modo da poter provare e ridimensionare il flusso di lavoro delle lettere di offerta in base alla crescita dell&#39;azienda. A [introduzione](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
creazione di modelli personalizzati, [iscriviti al tuo account sviluppatore](https://www.adobe.io/).
