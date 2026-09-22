# Bird Biodiversity in Greece

Exploratory analysis of bird occurrence records in Greece using **Python, Pandas, and GBIF data**. The project explores species recording frequency, temporal and seasonal patterns, and geographic differences through statistical charts and interactive maps.

Developed by **Stella Moumtzi** for the **Applied Data Science** course at **Athens University of Economics and Business**, Spring 2026. The notebook follows the supplied coursework template; analysis and explanatory comments are primarily in Greek.

## Analysis overview

- **Data exploration:** inspect the dataset, identify missing values, summarize species, and examine recording-frequency distributions.
- **Temporal analysis:** compare observation counts and recorded species richness across years.
- **Seasonality:** examine monthly observations and compare the patterns of a migratory species and a resident species.
- **Spatial analysis:** visualize occurrence locations with Folium heatmaps and seasonal maps.
- **Regional comparisons:** compare observation counts, unique species, common species, and seasonal patterns around Athens, Thessaloniki, Patras, Heraklion, and Lesvos.
- **Interpretation:** discuss sampling limitations and the distinction between recording activity and bird abundance.

## Technologies

| Purpose | Tools |
| --- | --- |
| Data processing | Pandas, NumPy |
| Statistical visualization | Matplotlib, Seaborn |
| Interactive maps | Folium |
| API requests | Requests |
| Image handling | Pillow |
| Progress reporting | tqdm |
| Execution environment | Jupyter Notebook / JupyterLab |

## Files

| File | Purpose |
| --- | --- |
| `coding_exercise_ads_2026_student_notebook_3220127.ipynb` | Main notebook with code, recorded outputs, and interpretation |
| `coding_exercise_ads_2026_student_notebook_3220127.html` | HTML export of the notebook |
| `coding_exercise_ads_2026_student_notebook_3220127.pdf` | Static PDF export |
| `coding_exercise_ads_2026_template_prompts_3220127.md` | Coursework log documenting AI assistance |

The input CSV is **not included** in the supplied archive. The notebook can load a local CSV or request records from the GBIF API.

If you rename the files for publication, update the filenames in this README accordingly.

## Data source and sampling

The data-loading cell requests bird occurrence records from the GBIF occurrence search API, using:

- Taxonomic group: **Aves**, `taxonKey=212`.
- Country: **Greece**, `country=GR`.
- Records with coordinates: `hasCoordinate=true`.
- Years: **2001–2025**.
- A request limit of **100 records per month/year combination**.

The local cache filename used by the actual code is:

```text
birds_greece_100per_month.csv
```

The code retains species, observation date, year, month, latitude, longitude, dataset name, the GBIF record key stored as `occurrenceID`, license, and common name when available.

The saved notebook commentary reports 26,886 records before removing entries without a species name and 26,515 afterward. These are historical reported counts, not a guarantee of the output of a new API run.

### Sampling limitations

The downloader requests the first returned records rather than a random sample and does not paginate through all available observations. The data therefore represent a limited set of recorded occurrences, not a census of birds or an unbiased estimate of abundance.

Regional comparisons use rectangular latitude/longitude bounds extending **0.3 degrees** from each selected center. These are neither administrative boundaries nor equal-distance circular areas.

Differences in observation counts or recorded species richness may reflect observer activity, coverage, data availability, and sampling choices. They should not be interpreted directly as differences in bird populations or as proof of ecological causes.

## Run locally

### 1. Install dependencies

Use a Python 3 environment and install the packages imported by the notebook, together with JupyterLab:

```shell
python -m pip install jupyterlab ipykernel pandas numpy matplotlib seaborn folium requests pillow tqdm
```

The project does not provide pinned dependency versions. A fresh environment has not been tested as part of this README update.

### 2. Open the notebook

From the directory containing the `.ipynb` file, run:

```shell
python -m jupyterlab
```

Open `coding_exercise_ads_2026_student_notebook_3220127.ipynb` and select a Python kernel with the dependencies installed.

Alternatively, open the notebook in VS Code with Jupyter support and select the same Python environment.

### 3. Supply or download the data

If you have the original `birds_greece_100per_month.csv`, place it beside the notebook and run with that directory as the working directory. The loading cell uses it automatically.

If the CSV is absent, the loading cell requests data from GBIF and saves the result under that name. Internet access is needed for API requests, image retrieval, and online map tiles.

**Before downloading a new sample, review the loop behavior described below.**

### 4. Execute the analysis

Restart the kernel and run the cells in order. Review API error messages and the returned record counts before interpreting plots. Live API results can change, so a new run may differ from the saved outputs.

To inspect the existing work without rerunning the analysis, open the PDF or download and open the HTML export in a browser. Interactive maps are best viewed in Jupyter or HTML; a PDF is static, and online map resources may still require a connection.

## Known reproducibility issues

### Monthly download loop

The current downloader contains two `break` statements inside the month loop:

```python
if not results:
    break
```

and:

```python
if data.get('endOfRecords', True):
    break
```

Either can stop processing the remaining months of that year. An empty month or the end of one month's result set should not end the entire year's sampling.

To request up to 100 records for every month, change the empty-result branch to `continue` and remove the `endOfRecords` break. This is a suggested correction; the supplied notebook has not been modified by this README update. If a local CSV already exists, the API branch is skipped, so archive or rename the old CSV before intentionally generating a new sample.

### Template text and actual configuration

Some introductory notebook text still refers to `birds_greece.csv` and `MAX_RECORDS`. The implemented loader instead uses `birds_greece_100per_month.csv` and `RECORDS_PER_MONTH = 100`. Follow the code values when configuring the run.

## AI assistance

The accompanying prompt log documents the declared use of Gemini for selected plotting tasks and a discussion of regional recording counts versus species richness. It also identifies sections completed without AI assistance. The log is retained as part of the coursework documentation; some template fields remain unfilled.

## Data attribution

The source is **GBIF — Global Biodiversity Information Facility**. Records retain their dataset and license fields, and the notebook discusses multiple license types. Check the individual source datasets and their license terms before redistributing observations or images; no single license is asserted here for all source material.

## Verification scope

This README was prepared from the supplied notebook source and accompanying prompt log. The analysis was not rerun, live data were not downloaded, and the recorded counts and conclusions were not independently reproduced for this documentation update.
