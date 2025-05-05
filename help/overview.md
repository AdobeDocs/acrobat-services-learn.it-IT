---
title: '[!DNL Adobe Acrobat Services] Tutorials API'
description: Pagina Panoramica per  [!DNL Adobe Acrobat Services]
feature: Acrobat Sign API, PDF Services API, PDF Embed API, Document Generation API, PDF Electronic Seal API, PDF Extract API, PDF Accessibility Auto-Tag API
role: Developer
level: Beginner, Intermediate, Experienced
jira: KT-7463
type: Tutorial
thumbnail: KT-7463.jpg
exl-id: c73feb77-4057-42fd-831c-a5004c7637c1
source-git-commit: 2e23ffef7e4438a9287c909d2fdb13968195c31f
workflow-type: tm+mt
source-wordcount: '353'
ht-degree: 2%

---

# Esercitazioni per l&#39;API [!DNL Adobe Acrobat Services]

[!DNL Adobe Acrobat Services] dispone di sei API principali:

* [!DNL Adobe PDF Services API]
* [!DNL Adobe PDF Embed API]
* [!DNL Adobe Document Generation API]
* [!DNL Adobe PDF Electronic Seal API]
* [!DNL Adobe PDF Extract API]
* [!DNL Adobe PDF Accessibility Auto-Tag API]

Le ultime due API e i relativi SDK sono inclusi in [!DNL Adobe PDF Services API] come parte di un&#39;offerta a pagamento. [!DNL PDF Embed API] è un&#39;offerta gratuita. Queste API automatizzano la generazione, la manipolazione e la trasformazione dei contenuti dei documenti tramite un set di moderni servizi Web basati su cloud. Consentono di offrire esperienze più semplici, veloci e con marchio, in modo da controllare l’interazione degli utenti con i documenti, semplificare i flussi di lavoro di PDF e promuovere l’utilizzo e la conservazione. Questi tutorial ti aiutano a creare esperienze con marchio più semplici e veloci con le API [!DNL Adobe Acrobat Services].

<!-- Comment -->
<!-- CARDS

* https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/pdfservices/overview-pdfservices
  {target = _self}
  {title = PDF Services API}
  {description = PDF APIs with SDKs for node.js, .Net, and Java to create, convert, OCR PDFs, and more}
  {image = https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/media_1a7b3ae4fc2b8c33c920f81a3eee05dc358108a74.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/docgen/overview-docgen
  {target = _self}
  {title = Document Generation API}
  {description = Generate PDF and Word documents from Word templates and JSON data}
  {image = https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/media_1e4df708e549cf00ce0fb7fa1782957786ad4886b.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/pdfaccessibility/overview-accessibility
  {target = _self}
  {title = Adobe PDF Accessibility Auto-Tag API tutorials}
  {description = This AI-powered API automatically tags documents making it easy to scale PDF accessibiity}
  {image = https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/media_1441e8f0ef7b4a044f3e6355d66fa8f88146f9394.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/pdfextract/overview-extract
  {target = _self}
  {title = PDF Extract API}
  {description = Unlock the structure and content elements of any PDF with a web service powered by Adobe Sensi's machine learning}
  {image = https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/media_1441e8f0ef7b4a044f3e6355d66fa8f88146f9394.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/eseal/overview-electronic-seal
  {target = _self}
  {title = PDF Electronic Seal API}
  {description = Learn how to apply a tamper-evident electronic seal to PDFs at scale}
  {image = https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/media_1144424efd29c8faaf22aceff3f73d80fb7fb3ac1.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/pdfembed/overview-embed
  {target = _self}
  {title = PDF Embed API}
  {description = Free Javascript API to embed high-fidelity PDFs, enable collaboration, and see analytics}
  {image = https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/media_100c64db6899f092f8a30e0d153091398242f8abc.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/acrobatsign/overview-sign
  {target = _self}
  {title = Acrobat Sign API}
  {description = Integrate e-signatures into your platform or application}
  {image = https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/media_1f579a346e7d3a7647236d7b81daf49d45dafde35.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}
* https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/usecases/overview-usecases
  {target = _self}
  {title = Adobe Acrobat Services API use cases}
  {description = A wide variety of Acrobat Services API use cases}
  {image = https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/media_13219295abc6d94806a7cccf552effa9b54f25ecf.png?width=400&format=webply&optimize=medium}
  {cta = Browse tutorials}

-->
<!-- End Comment -->

<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="PDF Services API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/pdfservices/overview-pdfservices" title="API di PDF Services" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/media_1a7b3ae4fc2b8c33c920f81a3eee05dc358108a74.png?width=400&format=webply&optimize=medium" alt="API di PDF Services"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/pdfservices/overview-pdfservices" target="_self" rel="referrer" title="API di PDF Services">API dei servizi PDF</a>
                    </p>
                    <p class="is-size-6">API PDF con SDK per node.js, .Net e Java per creare, convertire, PDF OCR e altro ancora</p>
                </div>
                <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/pdfservices/overview-pdfservices" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Sfoglia esercitazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Document Generation API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/docgen/overview-docgen" title="API di Document Generation" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/media_1e4df708e549cf00ce0fb7fa1782957786ad4886b.png?width=400&format=webply&optimize=medium" alt="API di Document Generation"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/docgen/overview-docgen" target="_self" rel="referrer" title="API di Document Generation">API di Document Generation</a>
                    </p>
                    <p class="is-size-6">Generare documenti PDF e Word da modelli Word e dati JSON</p>
                </div>
                <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/docgen/overview-docgen" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Sfoglia esercitazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adobe PDF Accessibility Auto-Tag API tutorials">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/pdfaccessibility/overview-accessibility" title="Esercitazioni per l’API di assegnazione automatica dei tag di accessibilità di Adobe PDF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/media_1441e8f0ef7b4a044f3e6355d66fa8f88146f9394.png?width=400&format=webply&optimize=medium" alt="Esercitazioni per l’API di assegnazione automatica dei tag di accessibilità di Adobe PDF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/pdfaccessibility/overview-accessibility" target="_self" rel="referrer" title="Esercitazioni per l’API di assegnazione automatica dei tag di accessibilità di Adobe PDF">Esercitazioni per l’assegnazione automatica dei tag di Adobe PDF Accessibility</a>
                    </p>
                    <p class="is-size-6">Questa API basata sull'intelligenza artificiale applica automaticamente i tag ai documenti per semplificare la scalabilità dell'accessibilità PDF</p>
                </div>
                <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/pdfaccessibility/overview-accessibility" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Sfoglia esercitazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="PDF Extract API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/pdfextract/overview-extract" title="API PDF Extract" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/media_1441e8f0ef7b4a044f3e6355d66fa8f88146f9394.png?width=400&format=webply&optimize=medium" alt="API PDF Extract"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/pdfextract/overview-extract" target="_self" rel="referrer" title="API PDF Extract">API PDF Extract</a>
                    </p>
                    <p class="is-size-6">Sblocca la struttura e gli elementi di contenuto di qualsiasi PDF con un servizio Web basato sull'apprendimento automatico di Adobe Sensi</p>
                </div>
                <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/pdfextract/overview-extract" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Sfoglia esercitazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="PDF Electronic Seal API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/eseal/overview-electronic-seal" title="API sigillo elettronico PDF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/media_1144424efd29c8faaf22aceff3f73d80fb7fb3ac1.png?width=400&format=webply&optimize=medium" alt="API sigillo elettronico PDF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/eseal/overview-electronic-seal" target="_self" rel="referrer" title="API sigillo elettronico PDF">API PDF sigillo elettronico</a>
                    </p>
                    <p class="is-size-6">Scopri come applicare un sigillo elettronico a prova di manomissione ai PDF su larga scala</p>
                </div>
                <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/eseal/overview-electronic-seal" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Sfoglia esercitazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="PDF Embed API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/pdfembed/overview-embed" title="API PDF Embed" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/media_100c64db6899f092f8a30e0d153091398242f8abc.png?width=400&format=webply&optimize=medium" alt="API PDF Embed"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/pdfembed/overview-embed" target="_self" rel="referrer" title="API PDF Embed">API di incorporamento PDF</a>
                    </p>
                    <p class="is-size-6">API Javascript gratuita per incorporare PDF ad alta fedeltà, abilitare la collaborazione e consultare analisi</p>
                </div>
                <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/pdfembed/overview-embed" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Sfoglia esercitazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Acrobat Sign API">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/acrobatsign/overview-sign" title="API di Acrobat Sign" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/media_1f579a346e7d3a7647236d7b81daf49d45dafde35.png?width=400&format=webply&optimize=medium" alt="API di Acrobat Sign"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/acrobatsign/overview-sign" target="_self" rel="referrer" title="API di Acrobat Sign">API Acrobat Sign</a>
                    </p>
                    <p class="is-size-6">Integrare le firme elettroniche nella piattaforma o nell’applicazione</p>
                </div>
                <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/acrobatsign/overview-sign" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Sfoglia esercitazioni</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adobe Acrobat Services API use cases">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/usecases/overview-usecases" title="Casi di utilizzo dell’API di Adobe Acrobat Services" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/media_13219295abc6d94806a7cccf552effa9b54f25ecf.png?width=400&format=webply&optimize=medium" alt="Casi di utilizzo dell’API di Adobe Acrobat Services"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/usecases/overview-usecases" target="_self" rel="referrer" title="Casi di utilizzo dell’API di Adobe Acrobat Services">Esempi di utilizzo dell'API di Adobe Acrobat Services</a>
                    </p>
                    <p class="is-size-6">Un’ampia gamma di casi d’uso per l’API di Acrobat Services</p>
                </div>
                <a href="https://experienceleague.adobe.com/it/docs/acrobat-services-learn/tutorials/usecases/overview-usecases" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Sfoglia esercitazioni</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
