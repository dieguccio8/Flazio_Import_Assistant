# 🚀 Flazio Import Assistant — V11

> **Senior Python Developer & Automation Architect Edition**  
> Crawler Playwright-based per l'importazione professionale di siti web su Flazio.

---

## 📋 Funzionalità

| Modulo | Descrizione |
|--------|-------------|
| 🔤 **Font** | Estrazione @font-face, conversione woff/woff2→TTF, organizzazione per famiglia, deduplicazione |
| 🖼️ **Immagini** | Download da `src`, `data-src`, `background-image`, organizzate per pagina |
| 📄 **Documenti** | Download automatico di `.pdf`, `.doc`, `.docx`, `.xls`, `.ppt` |
| 📝 **Testi** | Estrazione pulita via BeautifulSoup (`get_text`), un `.txt` per pagina |
| 🛍️ **Prodotti** | Rilevamento JSON-LD + OG meta + euristica DOM → CSV Flazio |
| 🗺️ **Sitemap** | Caricamento automatico `sitemap.xml` per scansione completa |

---

## 📁 Struttura Output

```
Imported_Sites/
└── [nome-dominio]/
    ├── Fonts/
    │   └── [FamigliaFont]/
    │       ├── Open Sans Regular.ttf
    │       └── Open Sans Bold.ttf
    ├── Images/
    │   ├── home/
    │   └── [nome-pagina]/
    ├── Documents/
    │   └── catalogo.pdf
    ├── Text/
    │   ├── home.txt
    │   └── [nome-pagina].txt
    └── products_[sito].csv
```

---

## ⚙️ Installazione

### 1. Prerequisiti
- Python 3.8+
- pip3

### 2. Installa le dipendenze
```bash
cd /percorso/a/Flazio_Import_Assistant
pip3 install -r requirements.txt
```

### 3. Installa il browser Chromium per Playwright
```bash
python3 -m playwright install chromium
```

---

## ▶️ Utilizzo

```bash
python3 import_assistant.py
```

Il programma chiederà:
```
🔗 Inserisci l'URL del sito da analizzare: https://www.esempio.com
```

---

## 📊 Schema CSV Flazio (obbligatorio)

Il file `products_[sito].csv` viene generato con **esattamente** queste 17 colonne, separatore `;`:

```
MODEL | ID | REF | CODE | TYPE | NAME | DESCRIPTION | STATUS | VAT | OPTIONS | PRICE | QUANTITY | WEIGHT | CATEGORIES | TAGS | IMAGE | BRAND
```

> ⚠️ Le celle senza dato rimangono **vuote**. Nessuna colonna extra viene aggiunta.

---

## 🔧 Configurazione avanzata

Per modificare il numero massimo di pagine da scansionare (default: **60**):

```python
# In import_assistant.py, costante MAX_PAGES
MAX_PAGES = 100  # Aumenta per siti molto grandi
```

---

## 🛡️ Gestione errori

- Ogni errore su singola pagina viene loggato senza bloccare il crawler
- Il browser Playwright viene chiuso correttamente anche in caso di eccezione
- Font non apribili vengono ignorati con warning
- Download falliti vengono segnalati e saltati

---

## 📦 Dipendenze

| Pacchetto | Versione minima | Scopo |
|-----------|----------------|-------|
| `playwright` | ≥ 1.40 | Navigazione browser headless |
| `beautifulsoup4` | ≥ 4.12 | Parsing HTML e estrazione testi |
| `requests` | ≥ 2.31 | Download file (immagini, doc, font) |
| `fonttools[woff]` | ≥ 4.47 | Conversione font woff/woff2→TTF |
| `brotli` | ≥ 1.1 | Decompressione woff2 |
| `lxml` | ≥ 5.0 | Parser HTML alternativo per BS4 |
