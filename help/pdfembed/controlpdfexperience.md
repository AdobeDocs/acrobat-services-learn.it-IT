---
title: Controlla la tua esperienza online di PDF e raccogli analisi
description: In questo tutorial pratico imparerai a utilizzare l’API di incorporamento di Adobe PDF per controllare l’aspetto, abilitare la collaborazione e raccogliere analisi su come l’utente interagisce con i PDF, incluso il tempo dedicato a una pagina e le ricerche
feature: PDF Embed API
role: Developer
level: Intermediate
type: Tutorial
jira: KT-7487
thumbnail: KT-7487.jpg
exl-id: 64ffdacb-d6cb-43e7-ad10-bbd8afc0dbf4
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '1489'
ht-degree: 0%

---

# Controlla la tua esperienza online di PDF e raccogli analisi

La tua organizzazione pubblica PDF sul tuo sito Web? Scopri come utilizzare l’API Adobe PDF Embed per controllare l’aspetto, abilitare la collaborazione e raccogliere informazioni analitiche sulle modalità di interazione degli utenti con i PDF, incluso il tempo dedicato a una pagina e alle ricerche. Per iniziare questo tutorial pratico in 4 parti, seleziona *Guida introduttiva all&#39;API PDF Embed*.

<table style="table-layout:fixed">
<tr>
  <td>
    <a href="controlpdfexperience.md#part1">
        <img alt="Parte 1: Guida introduttiva all’API PDF Embed" src="assets/ControlPDFPart1_thumb.png" />
    </a>
    <div>
    <a href="controlpdfexperience.md#part1"><strong>Parte 1: introduzione all'API PDF Embed</strong></a>
    </div>
  </td>
  <td>
    <a href="controlpdfexperience.md#part2">
        <img alt="Parte 2: aggiunta dell’API PDF Embed a una pagina Web" src="assets/ControlPDFPart2_thumb.png" />
    </a>
    <div>
    <a href="controlpdfexperience.md#part2"><strong>Parte 2: aggiunta dell'API PDF Embed a una pagina Web</strong></a>
    </div>
  </td>
  <td>
   <a href="controlpdfexperience.md#part3">
      <img alt="Parte 3: Accesso alle API di Analytics" src="assets/ControlPDFPart3_thumb.png" />
   </a>
    <div>
    <a href="controlpdfexperience.md#part3"><strong>Parte 3: accesso alle API di analisi</strong></a>
    </div>
  </td>
  <td>
   <a href="controlpdfexperience.md#part4">
      <img alt="Parte 4: Aggiungere interattività in base agli eventi" src="assets/ControlPDFPart4_thumb.png" />
   </a>
    <div>
    <a href="controlpdfexperience.md#part4"><strong>Parte 4: aggiunta di interattività in base agli eventi</strong></a>
    </div>
  </td>
</tr>
</table>

## Parte 1: Guida introduttiva all’API PDF Embed {#part1}

Nella parte 1, scopri come iniziare con tutto ciò di cui hai bisogno per le parti 1-3. Inizierai con il recupero delle credenziali API.

**Requisiti**

* Risorse esercitazione [download](https://github.com/benvanderberg/adobe-pdf-embed-api-tutorial)
* [scaricane uno qui](https://accounts.adobe.com/it) da Adobe ID
* Server Web (Nodo JS, PHP, ecc.)
* Conoscenza operativa di HTML / JavaScript / CSS

**Elementi in uso**

* Un server Web di base (nodo)
* Codice di Visual Studio
* GitHub

### Recupero delle credenziali

1. Passa al [sito Web Adobe.io](https://www.adobe.io/).
1. Fai clic su **[!UICONTROL Ulteriori informazioni]** in Genera esperienze documento coinvolgenti.

   ![Schermata del pulsante Ulteriori informazioni](assets/ControlPDF_1.png)

   Viene visualizzata la home page di [!DNL Adobe Acrobat Services].

1. Fai clic su **[!UICONTROL Introduzione]** nella barra di navigazione.

   Verrà visualizzata un&#39;opzione in **Introduzione alle API [!DNL Acrobat Services]** per **Creare nuove credenziali** o **Gestire le credenziali esistenti**.

1. Fai clic sul pulsante **[!UICONTROL Inizia]** in **[!UICONTROL Crea nuove credenziali]**.

   ![Schermata del pulsante Introduzione](assets/ControlPDF_2.png)

1. Scegliere il pulsante di scelta **[!UICONTROL Incorpora API]** di PDF e aggiungere nella finestra successiva un nome credenziale desiderato e un dominio applicazione.

   >[!NOTE]
   >
   >Queste credenziali possono essere utilizzate solo nel dominio applicazione elencato qui. Puoi utilizzare qualsiasi dominio scelto.

   ![Schermata delle credenziali](assets/ControlPDF_3.png)

1. Fai clic su **[!UICONTROL Crea credenziali]**.

   Nella pagina finale della procedura guidata vengono forniti i dettagli delle credenziali client. Lascia aperta questa finestra in modo da potervi tornare e copiare l’ID client (chiave API) per un utilizzo successivo.

1. Fai clic su **[!UICONTROL Visualizza documentazione]** per accedere alla documentazione con informazioni dettagliate su come utilizzare questa API.

   ![Schermata del pulsante Crea credenziali](assets/ControlPDF_4.png)

## Parte 2: aggiunta dell’API PDF Embed a una pagina Web {#part2}

Nella parte 2, scoprirai come incorporare facilmente l’API PDF Embed in una pagina Web. A tale scopo, utilizza la demo online di Adobe PDF Embed API per creare il nostro codice.

### Ottieni il codice dell&#39;esercizio

Abbiamo creato il codice da utilizzare. Sebbene sia possibile utilizzare il codice personalizzato, le dimostrazioni verranno eseguite nel contesto delle risorse per l&#39;esercitazione. Scarica il codice di esempio [qui](https://github.com/benvanderberg/adobe-pdf-embed-api-tutorial).

1. Vai al [[!DNL Adobe Acrobat Services] sito Web](https://www.adobe.io/apis/documentcloud/dcsdk/).

   ![Schermata del sito Web [!DNL Adobe Acrobat Services]](assets/ControlPDF_6.png)

1. Fai clic su **[!UICONTROL API]** nella barra di navigazione, quindi vai alla pagina **[!UICONTROL PDF Embed API]** nel collegamento a discesa.

   ![Schermata del menu a discesa dell&#39;API PDF Embed](assets/ControlPDF_7.png)

1. Fai clic su **[!UICONTROL Prova la demo]**.

   Viene visualizzata una nuova finestra con la sandbox dello sviluppatore per l’API PDF Embed.

   ![Schermata della demo](assets/ControlPDF_8.png)

   Qui puoi vedere le opzioni per le diverse modalità di visualizzazione.

1. Fate clic sulle diverse modalità di visualizzazione per Finestra intera, Contenitore dimensioni, In linea e Lightbox.

   ![Schermata delle modalità di visualizzazione](assets/ControlPDF_9.png)

1. Fai clic sulla modalità di visualizzazione **[!UICONTROL Finestra intera]**, quindi fai clic sul pulsante **[!UICONTROL Personalizza]** per attivare e disattivare le opzioni.

   ![Schermata del pulsante Personalizza](assets/ControlPDF_10.png)

1. Disabilita l&#39;opzione **[!UICONTROL Scarica]** PDF.
1. Fare clic sul pulsante **[!UICONTROL Genera codice]** per visualizzare l&#39;anteprima del codice.
1. Copiare **[!UICONTROL ID client]** dalla finestra Credenziali client dalla parte 1.

   ![Schermata dell&#39;ID client](assets/ControlPDF_11.png)

1. Apri il file **[!UICONTROL Web]** -> **[!UICONTROL resources]** -> **[!UICONTROL js]** -> **[!UICONTROL dc-config.js]** nell&#39;editor di codice.

   Verrà visualizzata la variabile clientID.

1. Incolla le credenziali client tra virgolette per impostare l’ID client sulle credenziali.

1. Torna all’anteprima del codice sandbox per sviluppatori.

1. Copiare la seconda riga con lo script di Adobe:

   ```
   <script src=https://documentccloud.adobe.com/view-sdk/main.js></script>
   ```

   ![Schermata dello script](assets/ControlPDF_12.png)

1. Passa all’editor di codice e apri il file **[!UICONTROL Web]** -> **[!UICONTROL esercizio]** -> **[!UICONTROL index.html]**.

1. Incollare il codice script in `<head>` del file alla riga 18 sotto il commento seguente: **TODO: ESERCIZIO 1: INSERT EMBED API SCRIPT TAG**.

   ![Schermata in cui incollare il codice script](assets/ControlPDF_13.png)

1. Torna all’anteprima del codice sandbox per sviluppatori e copia la prima riga di codice che presenta:

   ```
   <div id="adobe-dc-view"></div>
   ```

   ![Schermata in cui copiare il codice](assets/ControlPDF_14.png)

1. Passa all’editor di codice e apri nuovamente il file **[!UICONTROL Web]** -> **[!UICONTROL esercizio]** -> **[!UICONTROL index.html]**.

1. Incollare il codice `<div>` in `<body>` del file alla riga 67 sotto il commento **TODO: ESERCIZIO 1: INSERT PDF EMBED API CODE**.

   ![Schermata in cui incollare il codice](assets/ControlPDF_15.png)

1. Torna all&#39;anteprima del codice sandbox per sviluppatori e copia le righe di codice per `<script>` di seguito:

   ```
   <script type="text/javascript">
       document.addEventListener("adobe_dc_view_sdk.ready",             function(){ 
           var adobeDCView = new AdobeDC.View({clientId:                     "<YOUR_CLIENT_ID>", divId: "adobe-dc-view"});
           adobeDCView.previewFile({
               content:{location: {url: "https://documentcloud.                adobe.com/view-sdk-demo/PDFs/Bodea Brochure.                    pdf"}},
               metaData:{fileName: "Bodea Brochure.pdf"}
           }, {showDownloadPDF: false});
       });
   </script>
   ```

1. Passa all’editor di codice e apri nuovamente il file **[!UICONTROL Web]** -> **[!UICONTROL esercizio]** -> **[!UICONTROL index.html]**.

1. Incolla il codice `<script>` in `<body>` del file alla riga 68 sotto il tag `<div>`.

1. Modificare la riga 70 dello stesso file **index.html** per includere la variabile clientID creata in precedenza.

   ![Schermata della riga 70](assets/ControlPDF_16.png)

1. Modificare la riga 72 dello stesso file **index.html** per aggiornare il percorso del file PDF in modo che utilizzi un file locale.

   Ne è disponibile uno nei file dell&#39;esercitazione in **/resources/pdfs/whitepaper.pdf**.

1. Salva i file modificati e visualizza in anteprima il tuo sito Web accedendo a **`<your domain>`/summit21/web/esercizio/**.

   Dovresti visualizzare il rendering del white paper tecnico in modalità a finestra intera all’interno del browser.

## Parte 3: Accesso alle API di Analytics {#part3}

Dopo aver creato correttamente una pagina Web con API PDF Embed che esegue il rendering di un PDF, nella parte 3 è ora possibile scoprire come utilizzare gli eventi JavaScript per misurare le analisi e comprendere come gli utenti utilizzano i PDF.

### Ricerca della documentazione

Nell’API PDF Embed sono disponibili numerosi eventi JavaScript diversi. Puoi accedervi dalla documentazione di [!DNL Adobe Acrobat Services].

1. Accedi al sito [documentazione](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html).
1. Esamina i diversi tipi di evento disponibili come parte dell’API. Queste sono utili come riferimento e saranno utili anche per i tuoi progetti futuri.

   ![Schermata della guida di riferimento](assets/ControlPDF_17.png)

1. Copiare il codice di esempio elencato nel sito Web.

   Utilizzalo come base per il nostro codice e modificalo.

   ![Schermata in cui copiare il codice di esempio](assets/ControlPDF_18.png)

   ```
   const eventOptions = {
     //Pass the PDF analytics events to receive.
      //If no event is passed in listenOn, then all PDF         analytics events will be received.
   listenOn: [ AdobeDC.View.Enum.PDFAnalyticsEvents.    PAGE_VIEW, AdobeDC.View.Enum.PDFAnalyticsEvents.DOCUMENT_DOWNLOAD],
     enablePDFAnalytics: true
   }
   
   
   adobeDCView.registerCallback(
     AdobeDC.View.Enum.CallbackType.EVENT_LISTENER,
     function(event) {
       console.log("Type " + event.type);
       console.log("Data " + event.data);
     }, eventOptions
   );
   ```

1. Trova la sezione di codice che hai aggiunto in precedenza che ha l&#39;aspetto seguente e aggiungi il codice sopra riportato dopo questo codice in **index.html**:

   ![Schermata in cui incollare il codice](assets/ControlPDF_19.png)

1. Caricate la pagina nel browser Web e aprite la console per visualizzare gli output della console dai diversi eventi mentre interagite con il visualizzatore PDF.

   ![Schermata di caricamento della pagina](assets/ControlPDF_20.png)

   ![Schermata del codice per il caricamento della pagina](assets/ControlPDF_21.png)

### Aggiungi switch per l’acquisizione di eventi

Ora che hai gli eventi in output su console.log, cambiamo il comportamento in base agli eventi. A tale scopo, verrà utilizzato un esempio di switch.

1. Passa a **snippets/eventsSwitch.js** e copia il contenuto del file nel codice del tutorial.

   ![Schermata in cui copiare il codice](assets/ControlPDF_22.png)

1. Incollare il codice nella funzione listener di eventi.

   ![Schermata in cui incollare il codice](assets/ControlPDF_23.png)

1. Verificate che l’output della console sia corretto quando la pagina viene caricata e che interagiate con il visualizzatore di PDF.

### Adobe Analytics

Se desideri aggiungere il supporto di Adobe Analytics al tuo visualizzatore, puoi seguire le istruzioni documentate sul sito Web.

>[!IMPORTANT]
>
>È necessario che Adobe Analytics sia già caricato nella pagina Web nell’intestazione.

Passa alla [documentazione Adobe Analytics](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtodata.html#adobe-analytics) e verifica se Adobe Analytics è già abilitato nella tua pagina Web. Seguire le istruzioni per configurare una ReportSuite.

### Google Analytics

![Schermata di integrazione con la Google Analytics](assets/ControlPDF_24.png)

L’API Adobe PDF Embed fornisce un’integrazione pronta per l’uso con Adobe Analytics. Tuttavia, poiché tutti gli eventi sono disponibili come eventi JavaScript, è possibile integrarli con la Google Analytics acquisendo gli eventi PDF e utilizzando la funzione ga() per aggiungere l’evento ad Adobe Analytics.

1. Passa a **snippets/eventsSwitchGA.js** per vedere come puoi integrarti con la Google Analytics.
1. Rivedi e utilizza questo codice come esempio se la tua pagina Web viene monitorata utilizzando Adobe Analytics e è già incorporata nella pagina Web.

   ![Schermata del codice Adobe Analytics](assets/ControlPDF_25.png)

## Parte 4: Aggiungere interattività in base agli eventi {#part4}

Nella parte 4, scoprirai come sovrapporre al tuo visualizzatore di PDF una busta paga che appare dopo aver scorrere oltre la seconda pagina.

### Esempio di paywall

Passare a questo [esempio di PDF dietro un paywall](https://www3.technologyevaluation.com/research/white-paper/the-forrester-wave-digital-decisioning-platforms-q4-2020.html). In questo esempio imparerai ad aggiungere interattività sopra un&#39;esperienza di visualizzazione PDF.

### Aggiungi codice paywall

1. Visita il sito snippets/paywallCode.html e copia il contenuto.
1. Cerca `<!-- TODO: EXERCISE 3: INSERT PAYWALL CODE -->` in exercise/index.html.

   ![Schermata in cui copiare il codice](assets/ControlPDF_26.png)

1. Incolla il codice copiato dopo il commento.
1. Passa a **snippets/paywallCode.js** e copia il contenuto.

   ![Schermata in cui incollare il codice](assets/ControlPDF_27.png)

1. Incollare il codice in tale posizione.

### Prova demo con Paywall

Ora puoi visualizzare la demo.

1. Ricarica **index.html** sul sito Web.
1. Scorri verso il basso fino a una pagina > 2.
1. Mostra la finestra di dialogo visualizzata per sfidare l&#39;utente dopo la seconda pagina.

   ![Schermata di visualizzazione della demo](assets/ControlPDF_28.png)

## Risorse aggiuntive

Ulteriori risorse sono disponibili [qui](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html).
