# agid-accessibilita-autovalutazione

Skill per Claude (Cowork) che valuta l'accessibilità di un sito web, dei suoi documenti scaricabili o di un'app mobile secondo il **Modello di autovalutazione AgID** (Allegato 2 alle Linee guida sull'accessibilità degli strumenti informatici, versione V.01) e compila il report criterio per criterio, con evidenze.

## A cosa serve

La skill produce il modello di autovalutazione richiesto dalla normativa italiana sull'accessibilità (Legge 4/2004 e Linee guida AgID), applicando:

- **WCAG 2.0 e 2.1, livello A e AA** (50 criteri, mappati sulla clausola 9 della UNI EN 301 549:2018) per le pagine web;
- **clausola 10 della EN 301 549** ("Documenti non web") per i PDF e gli altri documenti scaricabili;
- **clausole 5, 6, 7, 10, 11 e 12 della EN 301 549** per le applicazioni mobili.

Il risultato è un report che riproduce la struttura dell'Allegato 2: intestazione (data, descrizione, metodologia, standard, responsabile), tabella dei requisiti tecnici, tabella dei criteri con conformità e note, tabella dei documenti, riepilogo e limiti della valutazione.

## Quando si attiva

La skill risponde a richieste come:

- "valuta l'accessibilità del sito https://…"
- "compila il modello di autovalutazione AgID per …"
- "verifica se queste pagine rispettano le WCAG 2.1 AA"
- "controlla l'accessibilità dei PDF pubblicati su …"
- "valuta l'app … secondo la EN 301 549"

## Come lavora

1. **Informazioni iniziali**: oggetto della valutazione, accessi disponibili, strumenti presenti nella sessione, responsabile e data.
2. **Campione delle pagine**: inventario da menu, footer e `sitemap.xml`; campione minimo composto da home, tutte le voci di primo livello del menu, pagine istituzionali (privacy, cookie, dichiarazione di accessibilità, contatti) e una pagina per ogni template rilevato (articolo, archivio, modulo, multimedia, documenti, ricerca, 404). Fino a 15 pagine si valuta tutto il sito; oltre, 8–15 pagine con copertura di tutti i template. Il campione viene proposto e confermato dall'utente.
3. **Verifiche per pagina**: struttura del DOM, alternative testuali, moduli, tastiera (con pressioni di tasto reali), contrasto, reflow e zoom, multimedia e movimento, coerenza tra pagine, puntatore, parsing e ARIA, hover/focus, inventario dei documenti linkati.
4. **Documenti scaricabili**: inventario, campionamento (tutti fino a 10; altrimenti almeno 10 o il 10 %, con priorità a documenti recenti, moduli, formati e generatori diversi), richiesta esplicita di permesso al download, analisi di tagging, titolo, lingua, testo estraibile, alternative testuali, segnalibri, link, campi modulo, restrizioni.
5. **Report**: giudizi con i soli valori del modello (Soddisfatto / Non soddisfatto / Non applicabile), più "Non verificato" per ciò che non è stato testato; un criterio è Soddisfatto a livello di sito solo se lo è su tutte le pagine del campione; le raccomandazioni sono separate dai giudizi.

## Regole di rigore

- Nessun giudizio senza evidenza osservata (codice, attributo, misura, prova da tastiera, proprietà del file).
- Il campione non è il sito: il report dichiara sempre cosa è stato esaminato e con quale criterio.
- Verifiche automatiche, verifiche manuali e verifiche che richiedono tecnologie assistive reali sono distinte esplicitamente.
- Il download di file avviene solo dopo un consenso esplicito dell'utente, sull'elenco presentato.
- Se il modello o le norme citate sono di versione diversa (WCAG 2.2, EN 301 549 v3.x), la skill chiede il documento aggiornato invece di presumerne il contenuto.

## Requisiti tecnici

- **Un browser che raggiunga il sito**: browser integrato dell'app Claude oppure Claude in Chrome. Gli script di verifica (axe-core, pdf.js) vengono iniettati nella pagina da cdnjs tramite lo strumento JavaScript del browser, quindi non serve installare nulla.
- **Facoltativi**: una shell (cloud o sul computer dell'utente) con poppler (`pdfinfo`, `pdftotext`, `pdffonts`, `pdftoppm`), `qpdf`, `pikepdf` o PyMuPDF, veraPDF per la validazione PDF/UA; `pandoc` per esportare il report in HTML.
- Se lo spazio di lavoro cloud non raggiunge il dominio (proxy), le pagine e i PDF possono essere analizzati interamente nel browser: i PDF vengono letti con pdf.js senza scaricarli su disco.

## Output

- Report Markdown con le tabelle del modello AgID; su richiesta esportato in **HTML** (file autonomo, accessibile: `lang`, un solo `h1`, tabelle con intestazioni, focus visibile), DOCX o PDF.
- Sintesi in chat: campione esaminato, conteggi Soddisfatto / Non soddisfatto / Non applicabile / Non verificato per Web e Documenti, esito Sì/No per ciascuno standard, criticità principali e quelle ricorrenti dovute al template.

## Limiti

- Non sostituisce i test con screen reader, ingranditore e utenti reali: i criteri che li richiedono restano "Non verificato".
- Il contrasto del testo sovrapposto a immagini e quello dei componenti non testuali di norma non è misurabile automaticamente.
- Per i PDF lunghi l'analisi strutturale/Users/giovanni/SVILUPPO/AccessibilitySkill/SKILL.md copre di default le prime pagine; ordine di lettura completo, contrasto e testo dei link vanno verificati manualmente.
- I criteri "tra pagine" (2.4.5, 3.2.3, 3.2.4) sono valutabili solo con un campione di più pagine.
- L'esito "Non soddisfatto" per il criterio 2.2.2 in presenza di caroselli che si fermano solo su hover/focus è una scelta interpretativa dichiarata nel report; la decisione finale spetta al responsabile.

## File della skill

- `SKILL.md`: istruzioni complete (procedura, checklist Web, checklist Documenti non web, checklist App mobili, formato del report).
- Fonte normativa di riferimento: Allegato 2 AgID "Modello di autovalutazione [V. 01]" (39 pagine), da cui derivano terminologia, campi dell'intestazione, tabella degli standard ed elenchi dei criteri.

## Licenza

SKILL.md e README.md sono distribuiti con licenza [Creative Commons Attribuzione 4.0 Internazionale (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/): puoi usare, modificare e ridistribuire la skill, anche a fini commerciali, citando il titolare (CNR-ITD), il nome della skill e la licenza, e indicando le modifiche apportate. Il testo completo è nel file `LICENSE`. Il Modello di autovalutazione AgID e le norme citate non sono inclusi nel pacchetto.
