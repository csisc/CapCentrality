# CapCentrality: The Effect of Capital City Centrality on Political Stability
[![Ask DeepWiki](https://devin.ai/assets/askdeepwiki.png)](https://deepwiki.com/csisc/CapCentrality)

This project studies the effect of the centrality of capital cities on the political stability of countries. This work is an adaptation of projects by Houcemeddine Turki for the Google Science Fair (2012 and 2013), the National Qualifiers of the Intel International Science and Engineering Fair (2012), the International Environmental Project Olympiad (2012), and a Capstone Project for the Certificate in Research Methodology from Alexis Foundation (2013).

## Methodology

The analysis measures the "centrality" of a country's capital and correlates it with political stability indicators from the Fragile States Index (FSI).

### 1. Data Collection

*   **Geographic & Population Data:** Country, governorate (state/province), population, and area data are fetched from **Wikidata** using a SPARQL query. This includes the coordinates of capital cities and governorate centers.
*   **Political Stability Data:** Stability metrics are taken from the **Fragile States Index (FSI) 2023**.

The final analysis is performed on a set of 55 countries for which complete governorate-level data was available.

### 2. Centrality Calculation

The core of the project is to calculate a country's "center of mass," or **barycenter**. The distance between this barycenter and the actual capital city serves as the primary metric for centrality. To explore different definitions of a country's center, the barycenter is calculated using several weighting schemes for the governorates:

*   **Population-weighted:** `population`
*   **Area-weighted:** `area`
*   **Geographic (unweighted):** Constant weight of `1`
*   **Composite-weighted:** `population * area`

Additionally, an iterative method is used to find the point that minimizes the weighted distance to all governorates. This is calculated for weights divided by distance (`w/d`):

*   `population / distance`
*   `area / distance`
*   `(population * area) / distance`
*   `1 / distance`

The final centrality metric is the **Haversine distance** from the calculated barycenter to the capital, normalized by the square root of the country's total area to enable cross-country comparisons.

### 3. Correlation Analysis

A Pearson correlation is computed between the FSI political stability indicators (e.g., "State Legitimacy", "Demographic Pressures", "Total" score) and each of the derived capital centrality metrics to identify potential relationships.

## Repository Structure

*   `Data Collection and Analysis.ipynb`: Jupyter notebook to fetch data from Wikidata, calculate the various barycenter and centrality metrics, and merge the results with FSI data.
*   `Correlation Study.ipynb`: Jupyter notebook that loads the final dataset and performs the correlation analysis.
*   `Country_Data_Final.xlsx`: The final compiled dataset containing FSI data and all calculated capital centrality metrics.
*   `Governorate_Data.xlsx`: Raw data for governorates (state, country, coordinates, population, area) extracted from Wikidata.
*   `Country_Data.xlsx`: Intermediate file combining FSI 2023 data with capital coordinates and country area.

## How to Run

1.  **Prerequisites:** Install the required Python libraries.
    ```bash
    pip install pandas numpy sparqlwrapper matplotlib openpyxl
    ```
2.  **Execute Notebooks:**
    *   First, run the `Data Collection and Analysis.ipynb` notebook. This will fetch the latest data from Wikidata and generate the final dataset (`Country_Data_Final.xlsx`).
    *   Then, run the `Correlation Study.ipynb` notebook to see the results of the correlation analysis.

## Results

The analysis produces a correlation matrix that quantifies the relationship between capital city centrality and the 12 indicators of the Fragile States Index. Significant correlations (where the absolute value > 0.1) are highlighted.

For example, the study reveals a weak negative correlation of **-0.24** between the `area/distance` centrality metric and the "S1: Demographic Pressures" indicator, suggesting that countries with a less central capital (by this measure) tend to experience slightly lower demographic pressures. The notebooks provide a full breakdown of all calculated correlations.