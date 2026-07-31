# DataVisuals

An interactive command-line tool for exploring tabular data. Point it at a folder, pick the files
and columns you care about, and it profiles the data and generates a set of charts, optionally
saving them to PDF. Built on pandas, matplotlib and seaborn.

## What it does

Run it and it walks you through four steps:

1. **Import** — scans a directory for supported files and lists what it found
2. **Select** — choose which files and which columns to work with, with a preview of each table
   showing inferred dtypes and the percentage of missing values per column
3. **Classify** — sorts columns into numerical, categorical and datetime, and asks you to confirm
4. **Analyse or visualise** — either print summary statistics or render charts, then optionally
   save them to a timestamped PDF

### Single table

- Histograms with a kernel density estimate for numerical columns
- Bar charts of value counts for categorical columns
- Line charts over time where a datetime column exists, with a rolling average overlay
- Scatter plots for the most strongly correlated numerical pairs
- Summary statistics, missing-value counts and correlation matrices

### Multiple tables

- Detects columns common to the selected tables and compares them
- Comparative box plots for shared numerical columns
- Grouped bar charts for shared categorical columns
- Cross-table summary statistics

## Supported formats

`.csv`, `.xlsx`, `.xls`, `.json` and `.ods`. All five are read through pandas; `.xlsx`/`.xls` need
`openpyxl` and `.ods` needs `odfpy`, both of which are in `requirements.txt`.

## Getting started

Requires Python 3.10+.

```bash
git clone https://github.com/ryjord/DataVisuals.git
cd DataVisuals

python3 -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt

python visualhandler.py
```

It will prompt for a directory. The repository ships two sample files in `tables/`, so entering
`tables` is enough to try it:

```
Enter the path to your data files >>> tables
```

`tables/SampleData.ods` is a small 43-row sales sheet, and `tables/products-10000.csv` is a
10,000-row product catalogue for testing with something larger.

Saved charts are written to `visualizations/` as timestamped PDFs. That directory is gitignored.

## Layout

| File | Contains |
|---|---|
| `visualhandler.py` | Entry point: the interactive flow, column classification and PDF export |
| `handling.py` | Finding, loading and previewing files; column selection |
| `singletablehandler.py` | Analysis and charts for one table |
| `multitablehandler.py` | Comparisons and charts across several tables |
| `handling.ipynb` | A notebook walking through the data-handling functions |

## Known limitations

- **It is interactive only.** There is no importable API and no command-line arguments; every run
  goes through the prompts. The functions are callable directly if you import the modules, but
  they are not designed as a library.
- Pie charts are not implemented, despite being a natural fit alongside the bar charts.
- Large files are loaded fully into memory; there is no chunking.
- There are no tests.

## License

MIT — see [LICENSE](LICENSE).
