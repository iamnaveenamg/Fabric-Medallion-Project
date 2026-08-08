# Fabric-Medallion-Project

## What this is
A small data engineering reference implementing a medallion (Bronze → Silver → Gold) architecture for retail/e-commerce sample data. It contains example Jupyter notebooks that demonstrate ingesting raw files, applying silver-layer transformations (orders, returns, inventory), and producing gold-layer outputs, alongside pipeline definitions (JSON/ZIP) suitable for deployment to an Azure data pipeline environment.

### Stack
- **Language(s):** Jupyter Notebook (Python notebooks)
- **Framework / runtime:** Notebook-driven data transformations; pipeline definitions target Azure Data Factory / ARM-style templates
- **Notable artifacts:** Jupyter notebooks for each medallion layer, sample raw data files (CSV/JSON/XLSX), and pipeline definition JSON/ZIP files for automated deployment

## How it's organized
```
NoteBooks/                Jupyter notebooks for each stage (Source, Silver layers, Gold layer)
  SourceChanges.ipynb     Ingest / initial parsing of raw sources
  SilverLayer-OrderChanges.ipynb
  SilverLayer-ReturnsChanges.ipynb
  SilverLayer-InventoryChanges.ipynb
  GoldLayer.ipynb         Final aggregation / reporting transformations

PipelinesCode/
  text/
    Pipeline_medallion.json    ARM/ADF-style template for medallion pipeline
    end-end-pipeline.json      End-to-end pipeline template
    *.zip                      Zipped pipeline exports (importable to ADF or similar)

rawdata/
  orders_data.csv
  inventory_data.json
  returns_data.xlsx
  (sample/raw data used by the notebooks)

README.md                 Project README (this file)
```

How it fits together:
- The notebooks represent the transformation logic for each medallion layer. Typical flow: raw files in rawdata/ -> run SourceChanges.ipynb to normalize/parquet (bronze) -> SilverLayer notebooks to clean and standardize domain tables (silver) -> GoldLayer.ipynb to generate aggregated / analysis-ready tables (gold).
- Pipeline JSON and ZIP artifacts in PipelinesCode/text are intended to orchestrate or reproduce the same workflow in an Azure environment (deploy/import into Azure Data Factory or Synapse pipelines).

## How to run it (shortest path)
1. Clone:
   ```
   git clone https://github.com/iamnaveenamg/Fabric-Medallion-Project.git
   cd Fabric-Medallion-Project
   ```

2. Prepare Python environment and install typical data packages (adjust versions as needed):
   ```
   python -m venv venv
   source venv/bin/activate   # or venv\Scripts\activate on Windows
   pip install --upgrade pip
   pip install jupyterlab pandas numpy openpyxl pyarrow
   ```

3. Start Jupyter and open the notebooks:
   ```
   jupyter lab
   ```
   - Run notebooks in logical order: SourceChanges.ipynb → SilverLayer-*.ipynb (orders, returns, inventory) → GoldLayer.ipynb.
   - Confirm notebooks' first code cells for any environment-specific variables or external storage paths and update them for your machine.

4. (Optional) Deploy pipeline templates to Azure:
   - Sign in and set up a resource group:
     ```
     az login
     az group create --name <rg-name> --location <location>
     ```
   - Deploy an ARM template (replace parameter file/values as needed):
     ```
     az deployment group create --resource-group <rg-name> --template-file PipelinesCode/text/end-end-pipeline.json --parameters @params.json
     ```
   - Alternatively import the ZIP files into Azure Data Factory via the UI (authoring/import pipeline) or use ADF REST APIs.

Notes:
- The notebooks assume local file access to files in rawdata/ unless you edit paths to point to remote storage (e.g., Azure Blob / ADLS). Search for path variables in each notebook.
- The pipeline JSON/ZIP files appear targeted for Azure — check their parameter names before deployment and provide required parameters (resource names, storage account, linked service definitions).
- No explicit requirements.txt or environment specification is present; adding one will improve reproducibility.
- The repo contains sample data files (CSV/JSON/XLSX) for testing transformations — validate that notebook parsing matches file formats (e.g., sheet names in the Excel file).

## Notes & suggested README additions (todo / improvements)
- Add a requirements.txt or environment.yml capturing exact package versions used by the notebooks.
- Add a "Notebook execution order" section listing the recommended order and purpose of each notebook.
- Provide a data dictionary / schema definitions for raw and silver/gold tables.
- Add an examples/ or outputs/ directory with sample result snapshots (Parquet/CSV) to validate successful runs.
- Document required parameters for the pipeline templates and include a sample params.json.
- Add instructions for deploying pipeline ZIPs to Azure Data Factory and troubleshooting tips.
- Consider adding a LICENSE file and short contributing guide if you expect collaborators.

## Try asking
- Which notebook cell(s) set external storage paths or credentials (search for "storage", "path", or "account" in NoteBooks/)?
- Which parameters does PipelinesCode/text/end-end-pipeline.json expect — can you provide a sample params.json for local testing?
- Would you like me to generate a requirements.txt from the notebooks (extract imports) and a short CONTRIBUTING.md?

---
If you want, I can:
- produce the concrete README.md text as a patch you can copy into the repository,
- extract imports from the notebooks to generate a suggested requirements.txt,
- or create a sample params.json for deploying the pipeline templates. Which would you like next?
