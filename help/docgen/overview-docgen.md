---
title: Tutorials API di Document Generation
description: Panoramica delle esercitazioni API di Document Generation
feature: Document Generation API
role: Developer
level: Beginner, Intermediate, Experienced
type: Tutorial
jira: KT-7480
thumbnail: KT-7480.jpg
exl-id: 519a41a2-33af-4022-8919-2cb69995c46c
source-git-commit: 0f62890207722245f33be41ad4c77ba68bf7a0fc
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---


# Esercitazioni per l’API di Document Generation

L’API Document Generation crea documenti PDF e Word da modelli Word e dati JSON.

>[!NOTE]
>
>L’API di Document Generation è inclusa nell’API di PDF Services.

## Generazione dei documenti

<!-- Comment -->
<!-- CARDS

* https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen
  {target = _self}
  {title = Automate document generation}
  {description = Learn how to automatically generate documents at scale}
  {image = https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_120e532325e3fdc7f559ad9b446d9ec08c1e239a1.png?width=400&format=webply&optimize=medium}
  {cta = Watch}

-->
<!-- End Comment -->

<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Automate document generation">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen" title="Generazione automatica dei documenti" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_120e532325e3fdc7f559ad9b446d9ec08c1e239a1.png?width=400&format=webply&optimize=medium" alt="Generazione automatica dei documenti"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen" target="_self" rel="referrer" title="Generazione automatica dei documenti">Generazione automatica dei documenti</a>
                    </p>
                    <p class="is-size-6">Scoprite come generare automaticamente i documenti su larga scala</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Guarda</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Creazione di modelli

L’API di Document Generation accetta un modello di documento (con tag di modello) insieme ai dati di input per generare il documento finale. Il documento finale viene generato sostituendo tutti i tag di modello nel modello di documento con il contenuto dinamico basato sui valori effettivi corrispondenti all’input dei dati.

<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Overview of the Adobe Document Generation Tagger">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeroverview" title="Panoramica di Adobe Document Generation Tagger" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_17b19864efffdb1f8c54017812c7de662e17bf163.png?width=400&format=webply&optimize=medium" alt="Panoramica di Adobe Document Generation Tagger"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeroverview" target="_self" rel="referrer" title="Panoramica di Adobe Document Generation Tagger">Panoramica di Adobe Document Generation Tagger</a>
                    </p>
                    <p class="is-size-6">Panoramica di Adobe Document Generation Tagger progettato per l’utilizzo con l’API di Adobe Document Generation</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeroverview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Guarda</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adding text tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddtexttags" title="Aggiunta di tag di testo" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_113bb0b6c3dfa1329810d3afbba3498af82c6c875.png?width=400&format=webply&optimize=medium" alt="Aggiunta di tag di testo"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddtexttags" target="_self" rel="referrer" title="Aggiunta di tag di testo">Aggiunta di tag di testo</a>
                    </p>
                    <p class="is-size-6">Scopri come aggiungere tag di testo ai modelli Microsoft Word utilizzando Adobe Document Generation Tagger</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddtexttags" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Guarda</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adding image tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddimagetags" title="Aggiunta di tag immagine" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_1095ed3adad9c9360bb3184dccc41a72a3da94edc.png?width=400&format=webply&optimize=medium" alt="Aggiunta di tag immagine"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddimagetags" target="_self" rel="referrer" title="Aggiunta di tag immagine">Aggiunta di tag immagine</a>
                    </p>
                    <p class="is-size-6">Scopri come aggiungere tag di immagine ai modelli Microsoft Word utilizzando Adobe Document Generation Tagger per inviare dinamicamente le immagini nei documenti</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddimagetags" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Guarda</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adding tables and list tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggertables" title="Aggiunta di tabelle e tag di elenco" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_1c2cc8e4bf3a85a977104a3d94073c37b93dcfdf9.png?width=400&format=webply&optimize=medium" alt="Aggiunta di tabelle e tag di elenco"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggertables" target="_self" rel="referrer" title="Aggiunta di tabelle e tag di elenco">Aggiunta di tabelle e tag di elenco</a>
                    </p>
                    <p class="is-size-6">Scopri come aggiungere tabelle ed elenchi di tag ai modelli di Microsoft Word utilizzando Adobe Document Generation Tagger per aggiungere dinamicamente righe di tabelle o elenchi in base ai dati</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggertables" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Guarda</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Setting numerical calculation tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggercalculations" title="Impostazione dei tag di calcolo numerico" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_1a64d90689430bd8a1f7a272a66f006f0808ab6cf.png?width=400&format=webply&optimize=medium" alt="Impostazione dei tag di calcolo numerico"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggercalculations" target="_self" rel="referrer" title="Impostazione dei tag di calcolo numerico">Impostazione dei tag di calcolo numerici</a>
                    </p>
                    <p class="is-size-6">Scopri come impostare i tag di calcolo numerico nei modelli di Microsoft Word utilizzando Adobe Document Generation Tagger per calcolare le aggregazioni o l’aritmetica dei valori dei dati</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggercalculations" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Guarda</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Setting conditional content">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggerconditional" title="Impostazione del contenuto condizionale" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_145cebc42cffee358ed1beffcd5015ecb595fc82a.png?width=400&format=webply&optimize=medium" alt="Impostazione del contenuto condizionale"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggerconditional" target="_self" rel="referrer" title="Impostazione del contenuto condizionale">Impostazione del contenuto condizionale</a>
                    </p>
                    <p class="is-size-6">Scopri come impostare le sezioni nei modelli di Microsoft Word utilizzando Adobe Document Generation Tagger per includere o escludere dinamicamente sezioni di un documento basate sui dati</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggerconditional" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Guarda</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
