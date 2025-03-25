---
title: Recupero delle credenziali per Microsoft Power Automate
description: Scopri come ottenere le credenziali per iniziare a utilizzare o provare i servizi Adobe PDF
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-10382
thumbnail: KT-10382.jpg
exl-id: 68ec654f-74aa-41b7-9103-44df13402032
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '872'
ht-degree: 1%

---

# Recupero delle credenziali per Microsoft Power Automate

[Microsoft Power Automate](https://powerautomate.microsoft.com/it-it/) offre ai cittadini e agli sviluppatori un modo efficace per creare potenti processi automatizzati per migliorare le proprie attività senza scrivere codice. Il connettore [Adobe PDF Services](https://us.flow.microsoft.com/en-us/connectors/shared_adobepdftools/adobe-pdf-services/), come parte di [[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services), consente agli utenti di eseguire qualsiasi azione disponibile nell&#39;API dei servizi Adobe PDF all&#39;interno di Microsoft Power Automate.

In questo tutorial, scoprite come ottenere le credenziali per iniziare a utilizzare o provare i servizi Adobe PDF. A seconda che tu sia un utente di prova o un cliente esistente, questa esercitazione descrive i passaggi corretti per ottenere le credenziali.

## In che modo gli utenti di Microsoft Power Automate possono iniziare a utilizzare il connettore Adobe PDF Services?

Gli attuali utenti di Microsoft Power Automate possono [ottenere credenziali di prova](https://www.adobe.com/go/powerautomate_getstarted_it) per i servizi Adobe PDF. Il collegamento riportato qui sopra è un collegamento speciale per la registrazione che consente di semplificare questo processo in modo specifico per gli utenti di Microsoft Power Automate.

![Accesso utente Adobe Developer](assets/credentials_1.png)


>[!IMPORTANT]
> Se accedi per una versione di prova, devi utilizzare un Adobe ID e non un&#39;Enterprise ID. Se non sei un abbonato corrente all’API di Adobe PDF Services e provi ad accedere con la tua Enterprise ID, potresti ricevere un errore di autorizzazioni perché l’azienda non ti ha autorizzato a utilizzare l’API di Adobe PDF Services. Per questo motivo, ti consigliamo di utilizzare un Adobe ID personale gratuito.
>

1. Dopo l’accesso, ti verrà richiesto di selezionare un nome per le nuove credenziali. Immetti il *nome credenziali*.
1. Seleziona la casella di controllo per accettare le condizioni per lo sviluppatore.
1. Selezionare **[!UICONTROL Crea credenziali]**.

   ![Assegnazione di un nome alle credenziali](assets/credentials_2.png)

Queste credenziali coprono cinque valori diversi:

* ID client (chiave API)
* Segreto client
* ID organizzazione
* ID account tecnico
* Base64 (chiave privata codificata)

![Nuove credenziali](assets/credentials_3.png)

Viene scaricato automaticamente nel sistema anche un file JSON contenente tutti questi valori. Questo file è denominato `pdfservices-api-pa-credentials.json` e ha un aspetto simile al seguente:

```json
{
 "client_id": "client id value",
 "client_secret": "client secret value",
 "organization_id": "organized id value",
 "account_id": "account id value",
 "base64_encoded_private_key": "base64 version of the private key"
}
```

Archivia il file in un percorso sicuro perché non è possibile ottenere una copia della chiave privata.

### Aggiungere una connessione in Microsoft Power Automate

Ora che hai le tue credenziali, puoi iniziare a utilizzarle nei flussi di Microsoft Power Automate.

1. Nel menu della barra laterale, apri il menu **[!UICONTROL Dati]** e seleziona **Connessioni**:

   ![Menu Connessioni nel sito Microsoft Power Automate](assets/credentials_4.png)

1. Selezionare **+ [!UICONTROL Nuova connessione]**.

1. Nella schermata successiva viene visualizzato un elenco dei possibili tipi di connessione. Nell&#39;angolo superiore destro, inserisci &quot;adobe&quot; per filtrare le opzioni:

   ![Elenco delle connessioni di Adobe](assets/credentials_5.png)

1. Seleziona **[!UICONTROL Servizi Adobe PDF (anteprima)]**.
1. Nella finestra modale, immettete tutti e cinque i valori generati in precedenza. Al termine, seleziona **[!UICONTROL Crea]**.

   ![Campi modulo per immettere le informazioni sulle credenziali](assets/credentials_6.png)

Ora puoi utilizzare i servizi Adobe PDF in Microsoft Power Automate.

### Accesso alle credenziali dopo la creazione

Se hai già creato delle credenziali e le hai spostate in modo errato, puoi recuperarle nuovamente in [Adobe Developer Console](https://developer.adobe.com/console).

1. Dopo aver effettuato l&#39;accesso ad [Adobe Developer Console](https://developer.adobe.com/console), trova prima il tuo progetto e selezionalo.
1. Nel menu a sinistra sotto *Credenziali*, seleziona **Account di servizio (JWT)**:

   ![Credenziali esistenti](assets/credentials_7.png)

1. Prendi nota dei cinque valori presentati qui: *ID client*, *Segreto client*, *ID account tecnico*, *E-mail account tecnico* e *ID organizzazione*.

Non puoi scaricare la chiave privata precedente, ma puoi utilizzare il pulsante &quot;Genera una tastiera pubblica/privata&quot; per crearne una nuova.

## Utilizzo delle credenziali esistenti di Adobe PDF Services

Se disponi di credenziali API di Adobe PDF Services esistenti generate dal sito Web [!DNL Adobe Acrobat Services], puoi utilizzarle con Microsoft Power Automate. Se hai scaricato un SDK durante la registrazione, le credenziali esistenti sono state fornite sotto forma di un file JSON denominato `pdfservices-api-credentials.json`. Il file JSON contiene le cinque chiavi necessarie per creare le credenziali di connessione. Copia ogni valore dal file JSON nel campo di connessione corrispondente.

Il valore della chiave privata proviene da un secondo file denominato `private.key`.

Puoi anche ottenere i valori da Adobe Developer Console come descritto in precedenza.

## Come possono [!DNL Adobe Acrobat Services] utenti iniziare a utilizzare Microsoft Power Automate?

Per iniziare a lavorare con Power Automate, passa a <https://powerautomate.microsoft.com> e utilizza il pulsante &quot;Inizia gratis&quot;. Se non disponi di un account Microsoft, devi crearne uno. Dopo l’accesso, ti verrà presentata la dashboard di Power Automate.

![Dashboard PA, vista iniziale](assets/credentials_8.png)

Come descritto all’inizio di questa esercitazione, crea un nuovo flusso, aggiungi un passaggio e trova i servizi Adobe PDF. Seleziona un&#39;azione e potresti ricevere un avviso che ti informa che è richiesto un account premium.

![Avviso account Premium](assets/credentials_9.png)

Come mostrato nella schermata precedente, puoi passare a un account aziendale o configurare un nuovo account dell&#39;organizzazione. Una volta completata l’operazione, potrai aggiungere l’azione Servizi Adobe PDF.

Per ulteriori informazioni sulla creazione del primo flusso di lavoro di Microsoft Power Automate con [!DNL Adobe Acrobat Services], consulta [Creazione del primo flusso di lavoro in Microsoft Power Automate](https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfservices/create-workflow-power-automate).

## Risorse aggiuntive

Per ulteriori informazioni, consulta un elenco di risorse aggiuntive:

* I primi sono i documenti di Adobe PDF Services Power Automate: <https://docs.microsoft.com/en-us/connectors/adobepdftools/>. Queste risorse completano quanto appreso qui.
* Servono esempi? Sono disponibili numerosi [modelli Power Automate](https://powerautomate.microsoft.com/en-us/connectors/details/shared_adobepdftools/adobe-pdf-services/) che illustrano i servizi PDF.
* Il nostro contenuto video live, [Paper Clips](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF), contiene anche video che illustrano l’utilizzo di Power Automate.
* Nel [blog Adobe Tech](https://medium.com/adobetech/tagged/microsoft-power-automate) sono disponibili numerosi articoli su come utilizzare Power Automate.
* Infine, assicurati di consultare anche la documentazione di base di [PDF Services](https://developer.adobe.com/document-services/docs/overview/).
