# AI Travel Analyst

Flight price analysis for the MIC AIML Department Recruitment Challenge — Data Science & Visualization track.

## Project Overview

This project analyzes a flight pricing dataset to understand what actually drives flight prices, using data cleaning, exploratory analysis, and visualization. It's built for the "AI Travel Analyst" challenge track, covering Part 1 (Exploration) requirements for a 1st-year submission.

## Problem Statement

Flight prices vary widely and can seem unpredictable to travelers. This project explores a real-world-style flight pricing dataset to identify which factors (airline, travel class, distance, timing, etc.) genuinely influence price, and translates those findings into practical recommendations for travelers.

## Installation Instructions
1. Clone this repository:

​```
git clone https://github.com/Bhavika-Asati/AI-Travel-Analyst.git
​``` 

2.  Install the required libraries:
```
pip install -r requirements.txt
```

3. Open `AI_Travel_Analyst_Analysis.ipynb` in Jupyter Notebook and run the cells in order.

## Dataset Used

`flight_pricing_dataset.csv` — the mandatory dataset provided for this challenge track, containing 100,000 flight records across 18 columns (Airline, Source, Destination, Departure/Arrival times, Duration, Total Stops, Distance, Travel Class, Days Before Departure, Season, Weekday, Aircraft Type, Booking Channel, Passenger Count, and Price).

## Methodology

1. **Data Cleaning**: Converted mixed-format numeric columns (Price, Distance, Duration, Total Stops) into proper numeric types, handled missing values per-column-type (dropped rows missing Price, median-filled other numeric columns, "Unknown" for categorical columns), converted date/time columns, and removed duplicate rows.
2. **Data Quality Fixes**: Standardized inconsistent airline name casing and consolidated city names that were represented in three different formats (full name, airport code, "City Airport").
3. **Exploratory Visualization**: Built 6 visualizations (5 required + 1 bonus) to examine price relationships with airline, travel class, total stops, days before departure, and distance, plus a correlation heatmap summarizing all numeric relationships.
4. **Insight Synthesis**: Interpreted each chart's pattern and combined findings into an overall picture of what drives flight price.

## Technologies Used

- Python
- pandas (data cleaning and manipulation)
- matplotlib & seaborn (visualization)
- Jupyter Notebook

## Results

**Major factors affecting flight price**, ranked by strength:
1. **Distance & Duration** (strongest, r = 0.65 each) — price rises steadily with route length
2. **Travel Class** — clear hierarchy: Economy < Premium Economy < Business < First
3. **Airline** — budget carriers cluster low; full-service airlines are pricier and more variable
4. **Total Stops** (weak, r = 0.11) — mild upward trend, not a strong driver
5. **Days Before Departure** (weak, r = -0.10) — no strong price effect, but last-minute bookings show wider price variability
6. **Passenger Count** (negligible, r ≈ 0.00) — no meaningful relationship

**Recommendations for travelers:**
- Airline and travel class choice matters more for saving money than booking timing
- Booking 50+ days ahead reduces price risk, even if it doesn't guarantee the lowest fare
- Non-stop flights aren't reliably cheaper — don't avoid connections to save money
- Expect long-haul fares to scale fairly predictably with distance

## Challenges Faced

1. **Mixed currency/number formatting in Price** (e.g. "Rs. 151,632.89" vs plain numbers) — fixed with string replacement + `pd.to_numeric`.
2. **Three different formats mixed in Duration** (decimal hours, "Xh Ym", "X min") — fixed with a custom regex-based parsing function.
3. **Missing values spread across ~5% of every column** — handled differently by column type (drop for target variable, median-fill for numeric, "Unknown" for categorical).
4. **1,864 duplicate rows** — removed with `drop_duplicates()`.
5. **Inconsistent capitalization in Airline fragmenting categories** (e.g. 'QATAR AIRWAYS' vs 'Qatar Airways') — only became visible once visualized, fixed with `.str.strip().str.title()`.
6. **Source/Destination columns had the same city represented three different ways** (full name, airport code, and "City Airport") — inflating unique value counts from ~18 real cities to 55 apparent categories.
   - **Challenge:** Source/Destination had the same city written 3 different ways ('Mumbai', 'BOM', 'Mumbai Airport') — inflating 18 real cities into 55 apparent categories, which would've fragmented any city-based analysis.
   - **Solution:** Built a dictionary mapping each airport code and "City Airport" variant to its standard city name (e.g. 'BOM': 'Mumbai'), then used `.replace(city_map)` to swap every inconsistent value in one pass — while values already in the correct form (like 'Mumbai') were left untouched since they weren't in the dictionary.

## Future Improvements

This project covers Part 1 (Exploration) requirements. With more time, this could be extended into Part 2 (Modeling) — engineering features from the cleaned data (e.g., encoding Airline and Travel_Class, extracting month/weekday from dates) and training a regression model to predict Price, using the strong correlations found here (Distance, Duration, Travel_Class) as key input features. Other possible extensions include an interactive dashboard and deeper investigation into the distinct price clustering observed around certain distance ranges.

## Screenshots

See the chart images in this repository:
- `chart1_price_by_airline.png`
- `chart2_price_by_class.png`
- `chart3_price_vs_days_before.png`
- `chart4_price_by_stops.png`
- `chart5_price_vs_distance.png`
- `chart6_correlation_heatmap.png`
