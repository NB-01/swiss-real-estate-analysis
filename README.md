# Swiss Real Estate Analysis

## Project Overview
This project aims to analyze the Swiss real estate market by scraping data from various real estate websites, focusing on rents and buying prices. The primary objective is to calculate buy-to-rent ratios in major Swiss cities and visualize these ratios geographically using choropleth maps. This analysis will provide insights into the Swiss real estate market, particularly beneficial for investors and researchers interested in understanding market trends and investment potentials in different regions.

## Objectives
- To scrape real estate data (rental and buying prices) from various Swiss real estate websites.
- To process and clean the collected data for accurate analysis.
- To calculate the buy-to-rent ratios in major Swiss cities.
- To visualize the data using choropleth maps, highlighting regional differences and trends.

## Data Sources
Data will be collected from various Swiss real estate websites, including but not limited to:
- Flatfox
- Homegate
- Immoscout24

## Tools and Technologies
- Data Scraping: Python (BeautifulSoup, Selenium)
- Data Analysis and Processing: Python (Pandas, NumPy)
- Visualization: Python (Matplotlib, Plotly) for choropleth maps
- Version Control: Git, GitHub

## Project Structure
- `data/`: Raw and processed data
- `notebooks/`: Jupyter notebooks for data analysis and visualization
- `scripts/`: Scripts for data scraping and processing
- `output/`: Output data, figures, and choropleth maps
- `src/`: Source code for custom Python modules
- `docs/`: Additional project documentation


## Example code:

```
import seaborn as sns
import matplotlib.pyplot as plt
import plotly.express as px
import pandas as pd

# Load your dataset
data = pd.read_csv('flatfox_data.csv')

# Seaborn Visualizations
# Violin plot for rent_net across different cities
top_cities_for_violin = data['city'].value_counts().head(10).index
data_top_cities = data[data['city'].isin(top_cities_for_violin)]

plt.figure(figsize=(12, 6))
sns.violinplot(x='city', y='rent_net', data=data_top_cities)
plt.title('Distribution of Net Rent Across Top 10 Cities')
plt.xticks(rotation=45)
plt.show()

# Pair plot for rent_net, rent_gross, and price_display
sns.pairplot(data[['rent_net', 'rent_gross', 'price_display']].dropna(), diag_kind='kde')
plt.show()

# Plotly Visualizations
# Scatter plot comparing rent_net and price_display
scatter_plot = px.scatter(data.dropna(subset=['rent_net', 'price_display']), 
                          x='rent_net', 
                          y='price_display', 
                          hover_data=['city', 'id'], 
                          title='Rent Net vs. Price Display Scatter Plot')

# Bar chart showing the average price_display by city
avg_price_by_city = data.groupby('city')['price_display'].mean().reset_index()
bar_chart = px.bar(avg_price_by_city.sort_values('price_display', ascending=False).head(10),
                   x='city', 
                   y='price_display', 
                   title='Top 10 Cities by Average Displayed Price') 
