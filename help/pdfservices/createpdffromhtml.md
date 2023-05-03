---
title: Crea un PDF da HTML o MS Office in pochi minuti con l'API PDF Services e Node.js
description: Nell'API dei servizi PDF, sono disponibili diversi servizi per creare e manipolare PDF o esportare da PDF a MS Office e altri formati
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-6673.jpg
kt: 6673
exl-id: 1bd01bb8-ca5e-4a4a-8646-3d97113e2c51
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '711'
ht-degree: 0%

---

# Crea un PDF da HTML o MS Office in pochi minuti con l&#39;API PDF Services e Node.js

![Crea immagine PDF Hero](assets/createpdffromhtml_hero.jpg)

Con la nuova API di Adobe PDF Services, che offre agli sviluppatori un’ampia scelta di servizi avanzati per la manipolazione delle PDF, la digitalizzazione dei flussi di lavoro basati su documenti non è mai stata così semplice da soddisfare le esigenze di flussi di lavoro complessi. Architetture complicate, strategie di implementazione e accelerazione tecnologica possono essere ottimizzate grazie a questi servizi web basati su cloud.

All&#39;interno dell&#39;API dei servizi PDF, sono disponibili diversi servizi per creare e manipolare PDF o esportare da PDF a MS Office e altri formati.

* Creare un file PDF da statico o dynamic HTML, MS Word, PowerPoint, Excel e altro ancora
* Export PDF a MS Word, PowerPoint, Excel e altro ancora
* OCR per riconoscere il testo nei file PDF e attivare la ricerca nei documenti
* Protect PDF con password all’apertura dei documenti
* Combinare pagine di PDF o documenti di PDF in un unico PDF
* Comprimi i PDF per ridurre le dimensioni della condivisione tramite e-mail o online
* Linearizzare per ottimizzare un PDF per una visualizzazione rapida sul Web
* Organizzare le pagine PDF con i servizi di inserimento, sostituzione, riordine, eliminazione e rotazione

Gli sviluppatori possono iniziare in pochi minuti con i file di esempio pronti per l&#39;esecuzione forniti per accedere a tutti i servizi Web disponibili. Ecco come iniziare.

## Acquisizione delle credenziali e download dei file di esempio

Il primo passo consiste nell&#39;ottenere una credenziale (chiave API) per sbloccare l&#39;uso. [Registrati per la versione di prova gratuita](https://www.adobe.com/go/dcsdks_credentials) e fai clic su &quot;Inizia&quot; per creare le tue nuove credenziali.

![Chiave API](assets/apikey.png)

È importante scegliere un &#39;Account personale&#39; per registrarsi alla versione di prova gratuita:

![Account personale](assets/personalaccount.png)

Nel passaggio successivo, scegli il servizio API dei servizi di PDF, quindi aggiungi un nome e una descrizione per le tue credenziali.

È disponibile una casella di controllo per &quot;Creare un esempio di codice personalizzato&quot;. Scegli questa opzione per aggiungere automaticamente le tue nuove credenziali ai file di esempio, saltando il passaggio manuale.

Quindi scegli Node.js come lingua per ricevere gli esempi specifici di Node.js e fai clic sul pulsante &quot;Crea credenziali&quot;.

![Crea credenziali](assets/createcredentials.png)

Riceverai un file .zip da scaricare denominato PDFToolsSDK-Node.jsSamples.zip che può essere salvato nel file system locale.

## Aggiunta delle credenziali agli esempi di codice

Se si sceglie l&#39;opzione &quot;Crea campione di codice personalizzato&quot;, non è necessario aggiungere manualmente l&#39;ID client ai file di esempio di codice e può saltare il passaggio successivo e andare direttamente alla sezione Esempi di codice in esecuzione di seguito.

Se non hai scelto l&#39;opzione &quot;Crea un esempio di codice personalizzato&quot;, dovrai copiare l&#39;ID client (chiave API) dalla console Adobe.io:

![Esempio di codice](assets/codesample.png)

Decomprimi il contenuto di PDFToolsSDK-Node.jsSamples.zip.

Accedi alla directory principale nella cartella adobe-dc-pdf-tools-sdk-node-samples.

Apri pdftools-api-credentials.json con qualsiasi editor di testo o IDE.

Incolla le credenziali nel campo per l&#39;ID client nel codice:

```javascript
{
 "client_credentials": {
  "client_id": "abcdefghijklmnopqrstuvwxyz",
```

Salvate il file e proseguite con il passaggio successivo per eseguire gli esempi di codice.

## Esecuzione del primo esempio di codice

Dal prompt dei comandi, accedi alla directory principale nella cartella adobe-dc-pdf-tools-sdk-node-samples.

Digita installazione npm:

C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples>npm install

Ora è possibile eseguire i file di esempio.

Per il primo esempio, create un PDF:

Mentre è ancora nel prompt dei comandi, esegui l&#39;esempio create PDF con il comando seguente:

C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples>nodo src/createpdf/create-pdf-from-docx.js

Output di esempio:

![Output di esempio](assets/exampleoutput.png)

Il PDF verrà creato nella posizione specificata nell&#39;output, che per impostazione predefinita è la directory pdfServicesSdkResult.

## Risorse e passaggi successivi

* Per ulteriore assistenza e supporto, visita l&#39;Adobe [[!DNL Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) forum della community

API dei servizi PDF [Documentazione](https://www.adobe.com/go/pdftoolsapi_doc)

* [Domande frequenti](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) per domande sull’API di PDF Services

* [Contattaci](https://www.adobe.com/go/pdftoolsapi_requestform) per domande su licenze e prezzi

* Articoli correlati:
   [La nuova API dei servizi PDF offre ulteriori funzionalità per i flussi di lavoro basati su documenti](https://community.adobe.com/t5/document-services-apis/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

   [Versione di luglio [!DNL Adobe Acrobat Services]: Servizi di incorporazione e PDF per PDF](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
