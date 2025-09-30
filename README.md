# E-Commerce Sales Data 360 – Analysis
Analiza end-to-end: pobranie danych (Kaggle), staging w PostgreSQL, warstwy SQL, notebook ML (churn) i artefakty do BI.

## 🏁 Quickstart
```bash
git clone https://github.com/DWoyda/E-Commerce-sales-data-360-analysis.git
cd E-Commerce-sales-data-360-analysis
```
```bash
python -m venv .venv
# Windows:
.venv\Scripts\Activate.ps1
pip install -U pip
pip install -r requirements.txt
```
```bash
# .env (patrz sekcja niżej)
copy .env.example .env    # lub utwórz ręcznie
```
```bash
# Dane z Kaggle (wymaga pliku %USERPROFILE%\.kaggle\kaggle.json)
pip install kaggle
kaggle datasets download -d engrmazeem/e-commerce-sales-data -p data/raw --unzip
```
```bash
# ETL → staging w Postgres (schema: stage, tabela: sales_raw)
python src/load_stage.py
```
```bash
# Notebook demo ML
code notebooks/01_ml_churn.ipynb
```

## 📦 Struktura
```
├── bi/                         # artefakty do dashboardów (np. .pbix, .twbx, exporty)
├── data/
│   ├── processed/              # pliki pośrednie (gitignore)
│   └── raw/                    # surowe dane (gitignore)
│       ├── README.md
│       └── ecommerce_sales_data.csv
├── notebooks/
│   └── 01_ml_churn.ipynb       # EDA / modele ML
├── sql/                        # widoki / transformacje
│   ├── 01_build_core.sql       # warstwa core (tabele pośrednie)
│   ├── 02_qc.sql               # kontrola jakości danych
│   ├── 03_rfm.sql              # metryki RFM
│   ├── 04_cohorts.sql          # kohorty
│   └── 05_ml_dataset.sql       # dataset do ML
├── src/                        # kod źródłowy (importowalny)
│   ├── check_db.py             # sprawdzenie poprawności/połączenia
│   ├── db.py                   # get_engine() / utils DB
│   └── load_stage.py           # CSV → Postgres (schema: stage)
├── .env                        # konfiguracja (zmienne środowiskowe)
├── .gitignore
├── README.md
├── requirements.txt
└── venv/                       # wirtualne środowisko (zwykle w .gitignore)
```

## 🗂️ Dane
- **Źródło**: Kaggle – *E-Commerce Sales Data* (`engrmazeem/e-commerce-sales-data`)  https://www.kaggle.com/datasets/engrmazeem/e-commerce-sales-data/data
- **Licencja danych**: CC0 / Public Domain (zgodnie z kartą datasetu).  
- **Zasada**: surowe pliki trzymaj w `data/raw`; wyniki i pliki pośrednie w `data/processed` (oba katalogi są ignorowane przez Git).

Pobranie:
```bash
kaggle datasets download -d engrmazeem/e-commerce-sales-data -p data/raw --unzip
```

## ⚙️ Konfiguracja (.env)
Utwórz plik `.env` w katalogu głównym:
```
DB_HOST=localhost
DB_PORT=5432
DB_NAME=ecom_portfolio
DB_USER=ecom_user
DB_PASSWORD=***TWOJE_HASLO***
# (opcjonalnie) dla importów w notebookach:
PYTHONPATH=${workspaceFolder}
```
> `.env` jest w `.gitignore` – nie commituj sekretów.

## 🏗️ Jak uruchomić (ETL → SQL → Notebook/BI)
1. **ETL**: `python src/load_stage.py` → zapisuje `stage.sales_raw` w Postgres.  
2. **SQL**: uruchamiaj skrypty z `/sql` (np. budowa widoków core/feature).  
3. **Notebook/BI**: przeprowadź eksperymenty w `notebooks/` i przygotuj artefakty do `bi/`.

## 🛠️ Troubleshooting (skrót)
- **`ModuleNotFoundError: No module named 'src'`**  
  Dodaj `src/__init__.py` i w pierwszej komórce notebooka:
  ```python
  import sys, pathlib; sys.path.append(str(pathlib.Path().resolve().parent))
  ```
  lub ustaw `PYTHONPATH=${workspaceFolder}` w `.env` i zrestartuj kernel.
- **`kaggle : not recognized`**  
  `pip install kaggle`, zamknij/otwórz terminal; sprawdź `kaggle --version`.
- **`psycopg2.OperationalError` / brak połączenia**  
  Sprawdź działanie usługi Postgres i wartości w `.env` (`DB_HOST`, `DB_PORT`, …).

## 🔒 Licencje 
- **Dane**: licencja źródła (CC0).
