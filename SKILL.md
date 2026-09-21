---
name: "agid-accessibilita-autovalutazione"
description: "Valuta l'accessibilità di un sito web (campione delle pagine principali e dei documenti scaricabili, PDF inclusi) o di un'app mobile secondo il Modello di autovalutazione AgID (Allegato 2, WCAG 2.0/2.1 AA, UNI EN 301 549:2018) e compila il report con evidenze."
---

# Autovalutazione di accessibilità secondo il modello AgID (Allegato 2)

Usa questa skill quando l'utente chiede di valutare, verificare o certificare l'accessibilità di un sito web, di una o più pagine web, di documenti pubblicati sul web o di un'applicazione mobile secondo la checklist AgID, le WCAG 2.1 livello AA o la norma EN 301 549, oppure quando chiede di compilare il "Modello di autovalutazione" (Allegato 2 alle Linee guida sull'accessibilità degli strumenti informatici, versione V.01).

Una valutazione di un sito comprende **sempre** due oggetti: (1) un campione delle pagine principali del sito e (2) un campione dei documenti scaricabili linkati da quelle pagine (in particolare PDF). Valutare la sola home page è ammesso solo se l'utente lo chiede esplicitamente, e il report deve dichiararlo come limite.

## Fonte e vincoli di rigore

Il riferimento è l'Allegato 2 AgID "Modello di autovalutazione [V. 01]", redatto in conformità alla Legge "Disposizioni per favorire e semplificare l'accesso degli utenti e, in particolare, delle persone con disabilità agli strumenti informatici". Il modello elenca i criteri, non descrive procedure di test: le modalità di verifica indicate più avanti derivano dal testo delle WCAG 2.1 e della EN 301 549 e vanno applicate come tali.

Regole non negoziabili durante la valutazione:

1. Non inventare e non dedurre. Ogni giudizio di conformità deve poggiare su un'evidenza osservata (codice HTML letto, attributo verificato, misurazione di contrasto, prova da tastiera eseguita, proprietà del PDF lette, ecc.). Se un criterio non è stato verificato, non assegnare "Soddisfatto": scrivi "Non verificato" e spiega cosa servirebbe per verificarlo (test con screen reader, accesso al codice, prova su dispositivo, download del documento).
2. Un campione non è il sito. Dichiara sempre quali URL, schermate e documenti sono stati esaminati e con quale criterio di campionamento; il giudizio vale per quelli. Non estendere il risultato a pagine o documenti non visti.
3. Distingui esplicitamente tra verifica automatica (strumenti, script, lettura del DOM o delle proprietà del file), verifica manuale eseguita da te e verifica che richiede un tester umano o tecnologia assistiva reale (screen reader, switch, ingranditore). Non attribuire a controlli automatici un valore che non hanno.
4. Usa solo i tre valori di conformità definiti dal modello: **Soddisfatto** (tutte le funzionalità dell'ICT soddisfano il criterio), **Non soddisfatto** (la maggior parte delle funzionalità dell'ICT non soddisfano il criterio), **Non applicabile** (il criterio non è applicabile alle funzionalità dell'ICT). "Non applicabile" va motivato. Un criterio è "Soddisfatto" a livello di sito solo se risulta soddisfatto su **tutte** le pagine (o documenti) del campione in cui è applicabile; se fallisce anche su una sola, riporta "Non soddisfatto" ed elenca le pagine coinvolte, così che il responsabile possa valutarne l'estensione.
5. Se il modello fornito dall'utente è una versione diversa dalla V.01 o l'utente cita un aggiornamento normativo (es. WCAG 2.2, EN 301 549 v3.x), non presumere il contenuto: chiedi il documento aggiornato o verifica sul sito AgID prima di procedere.
6. Il download di file richiede il consenso dell'utente: prima di scaricare i documenti del campione, presenta l'elenco (nome, URL di origine, dimensione se nota) e attendi un sì esplicito. Non scaricare documenti non inclusi nel campione approvato.

## Procedura

### 1. Raccogliere le informazioni iniziali

Prima di iniziare, ottieni (chiedi se mancano):

- Oggetto della valutazione: sito web intero (campione), singole pagine, documenti, app mobile (iOS/Android), o combinazione.
- Accesso disponibile: solo URL pubblico, codice sorgente, build dell'app, credenziali di test.
- Strumenti disponibili nella sessione: browser integrato o Chrome (lettura DOM, testo, console, tastiera), possibilità di eseguire script (axe-core, pa11y, lighthouse), shell cloud o shell sul computer dell'utente (per scaricare e analizzare i documenti), strumenti PDF (poppler `pdfinfo`/`pdftotext`/`pdffonts`/`pdftoppm`, `pikepdf`, `PyMuPDF`, `qpdf`, `veraPDF` se installabile).
- Chi è il "Responsabile alla compilazione" e la data del report.

### 2. Costruire il campione delle pagine principali

Non valutare una sola pagina se l'oggetto è "il sito". Procedi così:

1. **Inventario**: apri la home e raccogli tutti i link del menu principale (compresi i sottomenu), del footer e, se esiste, della mappa del sito o della `sitemap.xml`. Annota per ogni URL il tipo di pagina (template).
2. **Selezione**: il campione minimo comprende la home; **tutte** le voci di primo livello del menu principale; le pagine obbligatorie o istituzionali presenti (privacy, cookie policy, dichiarazione di accessibilità, contatti); almeno una pagina per ciascun template diverso rilevato: pagina di contenuto/articolo, pagina elenco/archivio (news, eventi, blog), pagina con modulo (contatti, iscrizione, ricerca), pagina con tabella o contenuti multimediali (video, audio, mappe), pagina che linka documenti scaricabili, pagina di risultati di ricerca, pagina di errore 404. Per siti piccoli (fino a circa 15 pagine) valuta tutte le pagine. Per siti grandi, un campione di 8–15 pagine è di norma sufficiente se copre tutti i template; dichiara il totale stimato delle pagine e la copertura.
3. **Conferma**: proponi l'elenco all'utente e fallo confermare o integrare prima di iniziare le verifiche. Se l'utente non è disponibile, procedi con il campione minimo e dichiaralo nel report.
4. **Registro**: mantieni una tabella "Campione esaminato" con URL, titolo, template, data e ora della verifica, strumenti usati. Va inserita nel report.

### 3. Eseguire le verifiche su ogni pagina del campione

Per ogni pagina, in quest'ordine:

1. **Lettura strutturale del DOM**: `lang` sull'elemento `html` coerente con la lingua del contenuto, `title` univoco e descrittivo, gerarchia dei titoli (h1–h6), landmark/regioni, skip link funzionante (l'ancora esiste), ordine del DOM rispetto all'ordine visivo, tabelle con `th`/`scope`/`caption`, liste marcate come liste, id duplicati.
2. **Contenuti non testuali**: ogni `img`, `svg`, `canvas`, `input type=image`, `[role=img]`, icona: presenza e pertinenza dell'alternativa testuale (`alt`, `aria-label`, `aria-labelledby`, testo visibile associato). Alt derivati dal nome del file (es. "IMG_1234", "WhatsApp Image …") non sono pertinenti. Immagini decorative con `alt=""`; link che contengono solo un'immagine devono avere un nome.
3. **Form**: ogni campo ha un'etichetta programmaticamente associata (`label for`, `aria-label`, `aria-labelledby`); istruzioni e campi obbligatori indicati nel testo; `autocomplete` sui campi relativi all'utente (1.3.5); messaggi di errore testuali con identificazione del campo e suggerimenti; conferma/annullamento per operazioni legali, finanziarie o su dati. Testa l'invio a vuoto per osservare la gestione degli errori; non inserire dati personali reali.
4. **Tastiera**: usa pressioni di tasto reali (Tab, Shift+Tab, Invio, Spazio, frecce, Esc), non `focus()` da script: solo così `:focus-visible` e i gestori di evento si comportano come per l'utente. Verifica che tutti i controlli siano raggiungibili e azionabili, che i sottomenu si aprano da tastiera, che non ci siano trappole, che l'ordine di focus sia coerente e non passi su contenuti nascosti o clonati (caroselli con slide duplicate), che l'indicatore di focus sia visibile (screenshot), che le scorciatoie a singolo carattere siano disattivabili.
5. **Colore e contrasto**: misura il rapporto di contrasto testo/sfondo (4,5:1 testo normale, 3:1 testo grande ≥ 18 pt o 14 pt grassetto) e 3:1 per componenti dell'interfaccia e grafica informativa (1.4.11); per il testo sovrapposto a immagini gli strumenti automatici restituiscono "incomplete": misura manualmente o dichiara "Non verificato". Verifica che nessuna informazione sia veicolata dal solo colore.
6. **Ridimensionamento e reflow**: zoom al 200% senza perdita di contenuto o funzionalità; larghezza 320 CSS px senza scorrimento bidimensionale; spaziatura del testo (1.4.12) senza perdita di contenuto. Se usi il browser integrato, imposta esplicitamente il viewport prima di misurare (con il pannello nascosto le dimensioni possono risultare 0) e ripristinalo alla fine.
7. **Multimedia e movimento**: audio/video preregistrati con trascrizione, sottotitoli, audiodescrizione; audio in tempo reale con sottotitoli; audio automatico > 3 s controllabile; caroselli e contenuti in movimento con un controllo esplicito di pausa/stop (la pausa al passaggio del mouse o al focus va documentata ma non equivale a un controllo); nessun lampeggio > 3 volte al secondo; limiti di tempo regolabili.
8. **Navigazione e coerenza (criteri tra pagine)**: con il campione a disposizione verifica 2.4.5 (più modalità per raggiungere le pagine: menu, ricerca, mappa del sito, link correlati), 3.2.3 (menu e blocchi ripetuti nello stesso ordine su tutte le pagine) e 3.2.4 (componenti con la stessa funzione identificati allo stesso modo su tutte le pagine). Scopo dei link chiaro nel contesto; nessun cambio di contesto al focus o all'input senza avviso.
9. **Puntatore e movimento**: alternative a gesti multipunto o basati su percorso; azione su up-event o annullabile; etichetta visibile contenuta nel nome accessibile (2.5.3: attenzione agli `aria-label` in lingua diversa dal testo visibile); funzioni attivate da movimento con alternativa.
10. **Parsing e semantica**: HTML valido quanto a apertura/chiusura tag, id univoci, nidificazione; per ogni componente personalizzato nome, ruolo, stato e valore esposti; stati ARIA (`aria-expanded`, `aria-current`) che cambiano davvero; messaggi di stato con `role="status"`/`aria-live` senza spostare il focus.
11. **Orientamento e hover/focus**: contenuto non bloccato su un solo orientamento; contenuti aggiuntivi al passaggio del mouse/focus chiudibili con Esc, sorvolabili e persistenti.
12. **Inventario dei documenti**: raccogli tutti i link a file scaricabili presenti nella pagina (estensioni `.pdf`, `.doc`, `.docx`, `.xls`, `.xlsx`, `.ppt`, `.pptx`, `.odt`, `.ods`, `.odp`, `.rtf`, `.zip`, link con attributo `download` o con tipo MIME di documento) e aggiungili all'inventario per la fase 4.

Annota per ogni riscontro: URL, elemento (selettore o descrizione), evidenza (frammento di codice, misura, screenshot), strumento o metodo usato.

Se la shell cloud non raggiunge il dominio, usa il browser integrato o Chrome: gli script di verifica (es. axe-core) possono essere iniettati nella pagina tramite lo strumento JavaScript del browser caricandoli da cdnjs.

### 4. Valutare i documenti scaricabili (in particolare i PDF)

Il modello AgID rimanda, per i documenti inseriti nelle pagine web (inclusi documenti e moduli scaricabili), ai punti di controllo della tabella "Documenti non web" (clausola 10 EN 301 549). Questa fase è obbligatoria quando il sito pubblica documenti.

1. **Inventario completo**: consolida i link raccolti nella fase 3 in una tabella: URL, nome file, formato, pagina di provenienza, dimensione e data (se ottenibili dalle intestazioni HTTP o dal link), funzione (modulo da compilare, delibera, bando, informativa, allegato, verbale, materiale didattico...).
2. **Campionamento**: se i documenti sono al massimo 10, valutali tutti. Altrimenti seleziona un campione di almeno 10 documenti o del 10% dell'inventario (il maggiore dei due), che includa: i documenti più recenti; i moduli da compilare e i documenti destinati al pubblico (bandi, informative, avvisi); almeno un documento per ciascun formato presente; documenti prodotti con generatori diversi (leggi il campo Producer/Creator); almeno un documento lungo (oltre 20 pagine) se esiste; almeno un documento sospetto di essere una scansione. Dichiara il criterio usato e chiedi conferma all'utente insieme al permesso di download.
3. **Acquisizione**: scarica i documenti del campione con la shell disponibile (cloud se il dominio è raggiungibile; altrimenti shell sul computer dell'utente in una cartella connessa, o richiesta all'utente di allegarli). Se nessun canale è disponibile, i criteri dei documenti vanno segnati "Non verificato".
4. **Verifiche per ogni PDF** (con `pdfinfo`, `pdffonts`, `pdftotext`, `pikepdf`/`PyMuPDF`; `veraPDF` per PDF/UA se installabile). Registra i valori letti, non impressioni:
   - **Testo reale o scansione**: `pdftotext` restituisce testo? `pdffonts` elenca font? Un PDF senza testo estraibile è un'immagine: non soddisfa 10.1.1.1 e rende non soddisfatti o non applicabili quasi tutti gli altri criteri; segnala la necessità di OCR e di rifacimento accessibile.
   - **Struttura (tagging)**: `Tagged: yes` in `pdfinfo` e presenza di `/StructTreeRoot` e `/MarkInfo << /Marked true >>`. Senza tag: 10.1.3.1 e 10.1.3.2 non soddisfatti. Con tag: verifica che l'albero contenga titoli (`/H1`…`/H6`), paragrafi, liste (`/L`, `/LI`), tabelle con `/TH` e, per le figure, `/Figure` con `/Alt`.
   - **Titolo**: campo `Title` dei metadati (`pdfinfo`, XMP `dc:title`) presente e descrittivo, e `ViewerPreferences /DisplayDocTitle true`; un titolo vuoto o uguale al nome file non soddisfa 10.2.4.2.
   - **Lingua**: `/Lang` nel catalogo coerente con la lingua del testo (10.3.1.1); parti in altra lingua marcate nei tag (10.3.1.2).
   - **Alternative testuali**: ogni `/Figure` ha `/Alt` pertinente o è marcata come artefatto (10.1.1.1).
   - **Ordine di lettura**: confronta l'ordine del testo estratto con il layout visivo (colonne, riquadri) (10.1.3.2).
   - **Segnalibri e navigazione**: documenti lunghi con outline/segnalibri; titoli taggati (10.2.4.6).
   - **Link**: annotazioni link con testo significativo e taggate `/Link` (10.2.4.4).
   - **Moduli**: campi `AcroForm` con `/TU` (etichetta) e ordine di tabulazione (`/Tabs /S`); moduli solo da stampare e compilare a mano vanno segnalati (10.3.3.2, 10.4.1.2).
   - **Contrasto e colore**: campionatura visiva delle pagine (rendering con `pdftoppm`) per testo con contrasto insufficiente o informazioni veicolate dal solo colore (10.1.4.1, 10.1.4.3).
   - **Restrizioni**: permessi di estrazione del contenuto per l'accessibilità (`pikepdf` `allow.accessibility`, `pdfinfo` "Encrypted"); se negati, le tecnologie assistive non possono leggere il documento.
   - **Sottotitoli/audiodescrizione** (10.1.2.x, 10.5, 10.6): applicabili solo a documenti con contenuti multimediali incorporati.
5. **Verifiche per documenti Office (docx, xlsx, pptx)**: titolo nelle proprietà, lingua, uso di stili di titolo (non solo grassetto), testo alternativo sulle immagini, tabelle con riga di intestazione, ordine di lettura nelle diapositive, contrasto. Leggibili con `python-docx`, `openpyxl`, `python-pptx` o ispezionando l'XML.
6. **Giudizio**: compila la tabella "Documenti non web" (10.0–10.6) con il giudizio aggregato sul campione e una tabella di dettaglio per documento (formato, taggato sì/no, testo sì/no, titolo, lingua, alt sulle figure, esito e problemi principali). Un documento scansionato o non taggato non può essere dichiarato conforme.

### 5. Compilare il report

Riprodurre la struttura del modello AgID, nello stesso ordine:

**Intestazione "Modello di autovalutazione [V. 01]"** con i campi: Data; Breve descrizione del prodotto; Metodologia di valutazione (campione di pagine e documenti con criterio di selezione, strumenti, verifiche manuali, verifiche non eseguite); Standard applicabili e linee guida; Responsabile alla compilazione.

**Tabella "Campione esaminato"**: pagine (URL, titolo, template, data) e documenti (nome, URL, formato, pagina di provenienza), con l'indicazione del totale stimato e della copertura.

**Tabella "Requisiti tecnici di accessibilità"** (Standard utilizzato / Livello di conformità):

| Standard utilizzato | Livello di conformità |
|---|---|
| Linee guida per l'accessibilità dei contenuti Web (WCAG) 2.0 — https://www.w3.org/Translations/WCAG20-it/ | Livello AA (Sì / No) |
| Linee guida per l'accessibilità dei contenuti Web (WCAG) 2.1 — https://www.w3.org/Translations/WCAG21-it/ | Livello AA (Sì / No) |
| UNI EN 301549:2018 — requisiti di accessibilità funzionali applicabili ai prodotti e servizi | (Sì / No) |

Rispondere "Sì" solo se tutti i criteri applicabili risultano Soddisfatti su tutte le pagine e i documenti del campione; in caso contrario "No" e rimandare al dettaglio.

**Tabella dei criteri Web** con colonne Criterio / Conformità / Note: nelle Note riportare evidenza, metodo e le pagine in cui il criterio fallisce; per "Non soddisfatto" indicare gli elementi coinvolti e, separatamente, una proposta di correzione contrassegnata come raccomandazione.

**Tabella "Documenti non web"** (clausola 10) con il giudizio aggregato sul campione di documenti, seguita dalla tabella di dettaglio per documento.

**Riepilogo**: numero di criteri Soddisfatti / Non soddisfatti / Non applicabili / Non verificati per la sezione Web e per la sezione Documenti; elenco dei criteri di livello A non soddisfatti; le criticità ricorrenti su più pagine (tipicamente dovute al template) vanno evidenziate perché una sola correzione le risolve ovunque.

Chiudere con una sezione "Limiti della valutazione" che elenchi pagine e documenti non esaminati, i criteri non verificati e le verifiche demandate a test con utenti o tecnologie assistive.

Se l'utente vuole il report come file, produrlo nel formato richiesto (html, docx, pdf, xlsx) seguendo la skill di formato corrispondente; un report HTML deve essere esso stesso accessibile (lingua, un solo h1, tabelle con intestazioni, focus visibile, contrasto). In assenza di indicazioni, proporre un documento con le tabelle sopra.

## Checklist Web — Requisiti minimi per contenuti livello A e AA (obbligatori)

Nota del modello: per i documenti inseriti nelle pagine web (inclusi i documenti e moduli scaricabili) si fa riferimento ai punti di controllo della tabella "Documenti non web". Ogni criterio è mappato sulla EN 301 549 con prefisso 9 (es. 1.1.1 → 9.1.1.1). "Solo 2.1" indica criteri introdotti con WCAG 2.1.

### Principio 1 — Percepibile

| Criterio | Livello | Cosa verificare |
|---|---|---|
| 1.1.1 Contenuti non testuali | A | Alternativa testuale equivalente per immagini, icone, CAPTCHA, controlli grafici; `alt=""` per elementi decorativi; nessun alt uguale al nome del file |
| 1.2.1 Solo audio e solo video (preregistrati) | A | Trascrizione per solo-audio; trascrizione o traccia audio per solo-video |
| 1.2.2 Sottotitoli (preregistrati) | A | Sottotitoli sincronizzati per video con audio |
| 1.2.3 Audiodescrizione o tipo di media alternativo (preregistrato) | A | Audiodescrizione o trascrizione descrittiva per video |
| 1.2.4 Sottotitoli (in tempo reale) | AA | Sottotitoli per audio dal vivo |
| 1.2.5 Audiodescrizione (preregistrata) | AA | Audiodescrizione per video preregistrati |
| 1.3.1 Informazioni e correlazioni | A | Struttura (titoli, liste, tabelle, etichette) esposta programmaticamente, non solo visivamente |
| 1.3.2 Sequenza significativa | A | Ordine del DOM coerente con l'ordine di lettura |
| 1.3.3 Caratteristiche sensoriali | A | Istruzioni non basate solo su forma, dimensione, posizione, suono |
| 1.3.4 Orientamento | AA (solo 2.1) | Contenuto fruibile in verticale e orizzontale |
| 1.3.5 Identificare lo scopo degli input | AA (solo 2.1) | `autocomplete` appropriato sui campi relativi all'utente |
| 1.4.1 Uso del colore | A | Nessuna informazione trasmessa dal solo colore |
| 1.4.2 Controllo del sonoro | A | Audio automatico > 3 s con pausa/stop/volume |
| 1.4.3 Contrasto minimo | AA | 4,5:1 testo normale, 3:1 testo grande |
| 1.4.4 Ridimensionamento del testo | AA | Testo ingrandibile al 200% senza perdita |
| 1.4.5 Immagini di testo | AA | Testo reale invece di immagini di testo, salvo loghi |
| 1.4.10 Ricalcolo del flusso | AA (solo 2.1) | Nessuno scorrimento bidimensionale a 320 CSS px |
| 1.4.11 Contrasto in contenuti non testuali | AA (solo 2.1) | 3:1 per componenti UI e grafica informativa |
| 1.4.12 Spaziatura del testo | AA (solo 2.1) | Nessuna perdita con interlinea 1,5, spaziatura paragrafi 2, lettere 0,12, parole 0,16 |
| 1.4.13 Contenuto con Hover o Focus | AA (solo 2.1) | Contenuti aggiuntivi chiudibili, sorvolabili, persistenti |

### Principio 2 — Utilizzabile

| Criterio | Livello | Cosa verificare |
|---|---|---|
| 2.1.1 Tastiera | A | Tutte le funzionalità operabili da tastiera (sottomenu inclusi) |
| 2.1.2 Nessun impedimento all'uso della tastiera | A | Nessuna trappola di focus |
| 2.1.4 Tasti di scelta rapida | A (solo 2.1) | Scorciatoie a singolo carattere disattivabili/rimappabili/attive solo al focus |
| 2.2.1 Regolazione tempi di esecuzione | A | Limiti di tempo disattivabili, regolabili o estendibili |
| 2.2.2 Pausa, Stop, Nascondi | A | Contenuti in movimento o auto-aggiornanti con controllo esplicito |
| 2.3.1 Tre lampeggiamenti o inferiore alla soglia | A | Nessun lampeggio > 3/s |
| 2.4.1 Salto di blocchi | A | Skip link funzionante o landmark per saltare blocchi ripetuti |
| 2.4.2 Titolazione della pagina | A | `title` descrittivo e univoco per ogni pagina del campione |
| 2.4.3 Ordine del focus | A | Ordine di focus significativo, senza contenuti nascosti o clonati |
| 2.4.4 Scopo del collegamento (nel contesto) | A | Testo del link comprensibile nel contesto |
| 2.4.5 Differenti modalità | AA | Più modi per trovare le pagine (verifica sull'insieme del campione) |
| 2.4.6 Intestazioni ed etichette | AA | Titoli ed etichette descrittivi |
| 2.4.7 Focus visibile | AA | Indicatore di focus visibile |
| 2.5.1 Movimenti del puntatore | A (solo 2.1) | Alternativa a gesti multipunto o basati su percorso |
| 2.5.2 Cancellazione delle azioni del puntatore | A (solo 2.1) | Azione su up-event o annullabile |
| 2.5.3 Etichetta nel nome | A (solo 2.1) | Nome accessibile contiene l'etichetta visibile |
| 2.5.4 Azionamento da movimento | A (solo 2.1) | Alternativa e disattivazione per funzioni da movimento |

### Principio 3 — Comprensibile

| Criterio | Livello | Cosa verificare |
|---|---|---|
| 3.1.1 Lingua della pagina | A | `lang` su `html` coerente con la lingua del contenuto |
| 3.1.2 Parti in lingua | AA | `lang` sulle porzioni in altra lingua |
| 3.2.1 Al focus | A | Nessun cambio di contesto al focus |
| 3.2.2 All'input | A | Nessun cambio di contesto all'input senza avviso |
| 3.2.3 Navigazione coerente | AA | Navigazione ripetuta nello stesso ordine su tutte le pagine del campione |
| 3.2.4 Identificazione coerente | AA | Componenti con stessa funzione identificati allo stesso modo su tutte le pagine |
| 3.3.1 Identificazione di errori | A | Errori identificati e descritti in testo |
| 3.3.2 Etichette o istruzioni | A | Etichette/istruzioni per gli input, campi obbligatori indicati |
| 3.3.3 Suggerimenti per gli errori | AA | Suggerimenti di correzione quando noti |
| 3.3.4 Prevenzione degli errori (legali, finanziari, dati) | AA | Invio reversibile, verificato o confermabile |

### Principio 4 — Robusto

| Criterio | Livello | Cosa verificare |
|---|---|---|
| 4.1.1 Analisi sintattica (parsing) | A | Tag chiusi, id univoci, nidificazione corretta |
| 4.1.2 Nome, ruolo, valore | A | Componenti con nome, ruolo, stato esposti e aggiornati |
| 4.1.3 Messaggi di stato | AA (solo 2.1) | Messaggi di stato annunciati senza spostare il focus |

## Checklist Documenti non web (clausola 10 EN 301 549)

Da compilare per i documenti scaricabili del campione (il modello precisa che questi criteri si applicano anche ai documenti pubblicati nel web). Nella tabella del modello compaiono, nell'ordine: 10.0 Generale (informativa); 10.1.1.1 Contenuto non testuale; 10.1.2.1 Solo audio e solo video (preregistrato); 10.1.2.2 Didascalie (preregistrate); 10.1.2.3 Audiodescrizione o tipo di media alternativo (preregistrato); 10.1.2.4 Sottotitoli (in tempo reale); 10.1.2.5 Audiodescrizione (preregistrata); 10.1.3.1 Informazioni e correlazioni; 10.1.3.2 Sequenza significativa; 10.1.3.3 Caratteristiche sensoriali; 10.1.3.4 Orientamento; 10.1.3.5 Identificare lo scopo degli input; 10.1.4.1 Uso del colore; 10.1.4.2 Controllo del sonoro; 10.1.4.3 Contrasto (minimo); 10.1.4.4 Ridimensionamento del testo; 10.1.4.5 Immagini di testo; 10.1.4.10 Ricalcolo del flusso; 10.1.4.11 Contrasto in contenuti non testuali; 10.1.4.12 Spaziatura del testo; 10.1.4.13 Contenuto con Hover o Focus; 10.2.1.1 Tastiera; 10.2.1.2 Nessun impedimento all'uso della tastiera; 10.2.1.4 Tasti di scelta rapida; 10.2.2.1 Regolazione tempi di esecuzione; 10.2.2.2 Pausa, stop, nascondi; 10.2.3.1 Tre lampeggiamenti o inferiore alla soglia; 10.2.4.2 Titolazione del documento; 10.2.4.3 Ordine del focus; 10.2.4.4 Scopo del collegamento (nel contesto); 10.2.4.6 Intestazioni ed etichette; 10.2.4.7 Focus visibile; 10.2.5.1 Movimenti del puntatore; 10.2.5.2 Cancellazione delle azioni del puntatore; 10.2.5.3 Etichette nel nome; 10.2.5.4 Azionamento da movimento; 10.3.1.1 Lingua del documento; 10.3.1.2 Parti in lingua; 10.3.2.1 Al focus; 10.3.2.2 All'input; 10.3.3.1 Identificazione di errori; 10.3.3.2 Etichette o istruzioni; 10.3.3.3 Suggerimenti per gli errori; 10.3.3.4 Prevenzione degli errori (legali, finanziari, dati); 10.4.1.1 Analisi sintattica (parsing); 10.4.1.2 Nome, ruolo, valore; 10.4.1.3 Messaggi di stato; 10.5 Posizionamento sottotitoli; 10.6 Temporizzazione della descrizione audio.

Corrispondenza pratica per i PDF: 10.1.1.1 → `/Alt` sulle figure e testo estraibile; 10.1.3.1 e 10.1.3.2 → PDF taggato con struttura corretta e ordine di lettura; 10.1.4.3 → contrasto del testo; 10.2.4.2 → metadato Title e DisplayDocTitle; 10.2.4.4 → link taggati con testo significativo; 10.2.4.6 → tag di titolo H1–H6; 10.3.1.1 → `/Lang`; 10.3.3.2 e 10.4.1.2 → campi modulo con etichetta `/TU` e tipo corretto; 10.4.1.1 → file conforme alla specifica (nessun errore strutturale, controllo con `qpdf --check` o `veraPDF`). I criteri relativi a tastiera, focus, tempo, movimento e messaggi di stato sono di norma "Non applicabile" per documenti statici senza script o moduli, con motivazione; per i moduli PDF interattivi vanno verificati.

## Checklist Applicazioni mobili (EN 301 549)

Il modello elenca per le app mobili i criteri seguenti; compilare Conformità e Note per ciascuno, usando "Non applicabile" con motivazione quando la condizione della clausola non ricorre (es. l'app non offre comunicazione vocale bidirezionale).

**Clausola 5 — Requisiti generici**: 5.2 Attivazione delle caratteristiche di accessibilità; 5.3 Biometrica; 5.4 Conservazione delle informazioni sull'accessibilità durante la conversione; 5.5.1 Modalità d'uso; 5.5.2 Discernibilità delle parti utilizzabili; 5.6.1 Stato tattile o uditivo; 5.6.2 Stato visivo; 5.7 Ripetizione tasti; 5.8 Accettazione del doppio tasto; 5.9 Azioni simultanee dell'utente.

**Clausola 6 — ICT con comunicazione bidirezionale**: 6.1 Larghezza di banda audio per il parlato; 6.2.1.1 Comunicazione di testo in tempo reale (RTT); 6.2.1.2 Voce e testo concomitanti; 6.2.2.1 Visualizzazione visivamente distinguibile; 6.2.2.2 Direzione di invio e ricezione determinabile programmaticamente; 6.2.3 Interoperabilità; 6.2.4 Riadattabilità del testo in tempo reale; 6.3 Identificazione delle chiamate; 6.4 Alternative ai servizi basati sulla voce; 6.5.2 Risoluzione; 6.5.3 Frequenza dei fotogrammi; 6.5.4 Sincronizzazione tra audio e video.

**Clausola 7 — ICT con funzionalità video**: 7.1.1 Riproduzione di sottotitoli; 7.1.2 Sincronizzazione dei sottotitoli; 7.1.3 Conservazione dei sottotitoli; 7.2.1 Riproduzione della descrizione audio; 7.2.2 Sincronizzazione della descrizione audio; 7.2.3 Conservazione della descrizione audio; 7.3 Controlli utente per sottotitoli e descrizione audio.

**Clausola 10 — Documenti non web** (per i documenti gestiti dall'app): stessi criteri della checklist Documenti non web sopra.

**Clausola 11 — Software**: 11.1.1.1 Contenuti non testuali (funzionalità aperte); 11.1.1.2 Contenuti non testuali (funzionalità chiusa); 11.1.2.1.1 e 11.1.2.1.2 Solo audio e solo video (preregistrati, funzionalità aperta/chiusa); 11.1.2.2 Sottotitoli preregistrati; 11.1.2.3.1 e 11.1.2.3.2 Audiodescrizione o media alternativo (funzionalità aperta/chiusa); 11.1.2.4 Sottotitoli (in tempo reale); 11.1.2.5 Audiodescrizione (preregistrata); 11.1.3.1.1 Informazioni e correlazioni; 11.1.3.2.1 Sequenza significativa; 11.1.3.3 Caratteristiche sensoriali; 11.1.3.4 Orientamento; 11.1.3.5.1 e 11.1.3.5.2 Identificare lo scopo degli input (aperta/chiusa); 11.1.4.1 Uso del colore; 11.1.4.2 Controllo del sonoro; 11.1.4.3 Contrasto (minimo); 11.1.4.4.1 e 11.1.4.4.2 Ridimensionamento del testo (aperta/chiuse); 11.1.4.5.1 Immagini di testo (aperta); 11.1.4.10 Ricalcolo del flusso; 11.1.4.11 Contrasto in contenuti non testuali; 11.1.4.12 Spaziatura del testo; 11.1.4.13 Contenuto con Hover o Focus; 11.2.1.1.1 e 11.2.1.1.2 Tastiera (aperta/chiuse); 11.2.1.2 Nessun impedimento all'uso della tastiera; 11.2.1.4.1 e 11.2.1.4.2 Tasti di scelta rapida (aperta/chiuse); 11.2.2.1 Regolazione tempi di esecuzione; 11.2.2.2 Pausa, stop, nascondi; 11.2.3.1 Tre lampeggiamenti o inferiore alla soglia; 11.2.4.3 Ordine del focus; 11.2.4.4 Scopo del collegamento (nel contesto); 11.2.4.6 Intestazioni ed etichette; 11.2.4.7 Focus visibile; 11.2.5.1 Movimenti del puntatore.

La sezione "Software" completa del modello aggiunge inoltre: 11.2.5.2, 11.2.5.3, 11.2.5.4, 11.3.1.1.1 e 11.3.1.1.2 Lingua del software (aperta/chiusa), 11.3.2.1, 11.3.2.2, 11.3.3.1.1 e 11.3.3.1.2 Identificazione degli errori (aperta/chiusa), 11.3.3.2, 11.3.3.3, 11.3.3.4, 11.4.1.1.1 e 11.4.1.1.2 Parsing (aperta/chiusa), 11.4.1.2.1 e 11.4.1.2.2 Nome, ruolo, valore (aperta/chiusa), 11.5.1 Funzionalità chiusa, 11.5.2.1–11.5.2.17 (interoperabilità con le tecnologie assistive: supporto del servizio di accessibilità della piattaforma, uso dei servizi di accessibilità, tecnologia assistiva, informazioni sull'oggetto, riga/colonna/intestazioni, valori, relazioni etichetta, relazioni genitore-figlio, testo, elenco ed esecuzione delle azioni disponibili, tracciamento e modifica degli attributi di focus e selezione, notifica delle modifiche, modifica di stati e proprietà, modifica di valori e testo), 11.6.1 Controllo dell'utente delle funzionalità di accessibilità, 11.6.2 Nessuna interruzione delle funzionalità di accessibilità, 11.7 Preferenze utente, 11.8.1–11.8.5 Strumenti di authoring (tecnologia del contenuto, creazione di contenuto accessibile, conservazione nelle trasformazioni, suggerimenti di riparazione, modelli). Usare questa sezione quando l'oggetto valutato è software non web (app native, desktop).

**Clausola 12 — Documentazione e servizi a supporto**: 12.1.1 Caratteristiche di accessibilità e compatibilità; 12.1.2 Documentazione accessibile; 12.2.2 Informazioni sulle caratteristiche di accessibilità e compatibilità; 12.2.3 Comunicazione effettiva; 12.2.4 Documentazione accessibile.

Per le app mobili le verifiche pratiche richiedono di norma il dispositivo o l'emulatore con lo screen reader di piattaforma (VoiceOver/TalkBack) attivo, l'ispezione dell'albero di accessibilità (Accessibility Inspector, Android Accessibility Scanner) e prove con tastiera esterna e ingrandimento di sistema. Se questi strumenti non sono disponibili nella sessione, segnalare i criteri corrispondenti come "Non verificato" invece di stimarli.

## Formato della risposta finale

In chat: sintesi in poche righe (campione di pagine e documenti esaminato, numero di criteri Soddisfatti / Non soddisfatti / Non applicabili / Non verificati per Web e per Documenti, esito "Sì/No" per la tabella degli standard, criticità principali e quelle ricorrenti su più pagine) seguita dal report completo o dal file. Le raccomandazioni di correzione vanno tenute separate dai giudizi di conformità.