# Gurgaon Real Estate Analytics

An end-to-end data science project on residential property listings in Gurgaon: data cleaning, exploratory analysis, feature engineering, price modelling and a content-based apartment recommender. The results are served through a multi-page [Streamlit](https://streamlit.io/) app.

## Features

The Streamlit app (`app/`) has three pages:

| Page | What it does |
|------|--------------|
| **Price Prediction** | Enter property details (type, sector, bedrooms, bathrooms, balconies, age, built-up area, servant/store room, furnishing, luxury and floor category) and get an estimated price range in crores. |
| **Analysis App** | Interactive charts: sector-wise price-per-sqft geomap, amenities word cloud, area vs. price scatter, BHK distribution pie chart, BHK price box plot and flat vs. house price distribution. |
| **Recommend Apartments** | Find societies within a given radius of a landmark, and get the top 5 similar apartments for a selected society. |

## Project structure

```
dataScience-/
├── app/                          # Streamlit application
│   ├── home.py                   # Entry point
│   ├── pages/
│   │   ├── 1_Price_prediction.py
│   │   ├── 2_Analysis_App.py
│   │   └── 3_Recommend_Appartment.py
│   ├── dataset/                  # Data used by the app
│   │   ├── data_viz1.csv
│   │   ├── location_distance.pkl
│   │   └── cosine_sim{1,2,3}.pkl
│   ├── df1.pkl                   # Cleaned training data (used for dropdown options)
│   ├── pipeline1.pkl             # Trained price-prediction pipeline
│   └── feature_text.pkl          # Amenities text for the word cloud
└── Real_Estate_Project/          # Jupyter notebooks for the full workflow
```

## Workflow (notebooks)

The notebooks in `Real_Estate_Project/` roughly follow this order:

1. **Preprocessing** – `flats_data_preprocessing`, `independent_house_preprocessing`, `merge_flate_and_house_data`, `gurgaon_properties_preprocessing`
2. **Feature engineering** – `feature-engineering`
3. **Exploratory data analysis** – `EDA_univariate_analysis`, `EDA_multivariate_analysis`, `EDA_pandas_profiling` (report in `output_report.html`)
4. **Data cleaning** – `outlier_detection_&_remove`, `missing_value_imputation`
5. **Feature selection** – `feature_selection`
6. **Modelling** – `baseline_model`, `model_selection`, `model_selection2`
7. **Visualisation & insights** – `data_visualization`, `insights_module`
8. **Recommender system** – `recommender_system`

### How it works

- **Price prediction:** a scikit-learn pipeline trained on the log of the price; the app inverts it with `np.expm1` and shows a range of ±0.03 Cr around the prediction.
- **Recommender:** three cosine-similarity matrices (built from different property attributes) are combined as a weighted sum (`0.5·sim1 + 0.8·sim2 + 1·sim3`) to rank similar societies.
- **Location search:** a precomputed distance matrix between societies and landmarks, filtered by the chosen radius.

## Getting started

### Requirements

Python 3.8+ and the following packages:

```bash
pip install streamlit pandas numpy scikit-learn plotly matplotlib seaborn wordcloud
```

> The pickled pipeline should be loaded with the same scikit-learn version it was trained with. If loading fails, re-run the modelling notebooks to regenerate `pipeline1.pkl`.

### Configure file paths

The app pages currently load data from hard-coded absolute paths such as:

```python
r'C:\Users\aijul\Desktop\dataScience-\app\df1.pkl'
```

Update these paths in the three files under `app/pages/` to match where you cloned the repository (or change them to relative paths like `app/df1.pkl` and run from the repository root).

### Run the app

```bash
git clone https://github.com/aijulhussain/dataScience-.git
cd dataScience-
streamlit run app/home.py
```

Then open the URL Streamlit prints (usually http://localhost:8501) and choose a page from the sidebar.

## Tech stack

Python · pandas · NumPy · scikit-learn · Plotly · Matplotlib · Seaborn · WordCloud · Streamlit · Jupyter
