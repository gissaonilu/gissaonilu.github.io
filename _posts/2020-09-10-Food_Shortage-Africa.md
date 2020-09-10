---
title: "Data Storytelling Project"
date: 2020-09-09
tags: [data storytelling, data science, data visualization, food, africa]
header:
  image: "/images/perceptron/percept.jpg"
excerpt: "Data Storytelling, Data Science, Data Visualization"
mathjax: "true"
---

# Solving the world food shortage problem through Data Storytelling
The project involves the use of many visualisations accompanied by narratives to tell a story making it easy to understand the problem (world food shortage) and how the problem can be solved.



### Information about the datasets:
- Africa food production dataset: the data contains information about the quantity (kt) of food produced in 45 African            countries between 2004 and 2013 inclusive. It consists of 23110 rows and 4 columns. The columns in the dataset are :            Country, Year, Item and Value.

- Africa food supply dataset: the data contains information about the quantity (kt) of food produced in 45 African            countries between 2004 and 2013 inclusive. It consists of 450 rows and 3 columns. The columns in the dataset are :            Country, Year and Value.

### Steps in undertaking the Project:
- Load datasets
- Exploratory Data Analysis
- Data Visualization and Storytelling
- Conclusion and Recommendation       -

---

## Load Datasets


```python
#Import necessary packages
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib as mpl
import seaborn as sns
import plotly.express as px
from plotly.offline import plot
```

### A) African food production dataset.


```python
#Read African food production dataset
#url = 'C:/Users/Ganiyat Omotola/Desktop/Hamoye DS Internship/Stage C/df_production.csv'
url = 'https://www.wolframcloud.com/obj/mar/Hamoye/Session%202/Data/Africa%20Food%20Production%20(2004%20-%202013).csv'
production_df = pd.read_csv(url, error_bad_lines=False)
#change the name of the Value column to Quantity [kt]
production_df.rename(columns={'Value':'Value [kt]'},inplace=True)
production_df.head()
```



```python
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Item</th>
      <th>Year</th>
      <th>Value [kt]</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Algeria</td>
      <td>Wheat and products</td>
      <td>2004</td>
      <td>2731</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Algeria</td>
      <td>Wheat and products</td>
      <td>2005</td>
      <td>2415</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>Wheat and products</td>
      <td>2006</td>
      <td>2688</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Algeria</td>
      <td>Wheat and products</td>
      <td>2007</td>
      <td>2319</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Algeria</td>
      <td>Wheat and products</td>
      <td>2008</td>
      <td>1111</td>
    </tr>
  </tbody>
</table>
</div>
```


### B) African food consumption (supply) dataset.


```python
#Read African food supply dataset
#url = 'C:/Users/Ganiyat Omotola/Desktop/Hamoye DS Internship/Stage C/df_supply.csv'
url = 'https://www.wolframcloud.com/obj/mar/Hamoye/Session%202/Data/Africa%20Food%20Supply%20(2004%20-%202013).csv'
supply_df = pd.read_csv(url, error_bad_lines=False)
#change the name of the Value column to Quantity [kcal/(person day)]
supply_df.rename(columns={'Value':'Value [kcal/(person day)]'}, inplace=True)
supply_df.head()
```



```python
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Year</th>
      <th>Value [kcal/(person day)]</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Algeria</td>
      <td>2004</td>
      <td>2987</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Algeria</td>
      <td>2005</td>
      <td>2958</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>2006</td>
      <td>3047</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Algeria</td>
      <td>2007</td>
      <td>3041</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Algeria</td>
      <td>2008</td>
      <td>3048</td>
    </tr>
  </tbody>
</table>
</div>
```


### C) External World population dataset.
 The external world population dataset was used for analysing the two datasets for food production and consumption in Africa.


```python
#Read External world population dataset
population_df = pd.read_csv('C:/Users/Ganiyat Omotola/Desktop/Hamoye DS Internship/Stage C/datasets/population_data.csv', skiprows=range(4))
population_df = population_df[['Country Name','Country Code','2004','2005','2006','2007','2008','2009','2010','2011','2012','2013']]
#converting the years into a column
population_df = pd.melt(population_df, id_vars= ['Country Name','Country Code'], var_name='Year')
population_df.columns = ['Country','Code','Year','Population']
population_df.to_csv('population_df.csv')
world_population = population_df.drop(['Year'], axis=1)
world_population.head()
```



```python
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Code</th>
      <th>Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aruba</td>
      <td>ABW</td>
      <td>98737.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Afghanistan</td>
      <td>AFG</td>
      <td>24726684.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Angola</td>
      <td>AGO</td>
      <td>18758145.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Albania</td>
      <td>ALB</td>
      <td>3026939.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Andorra</td>
      <td>AND</td>
      <td>76244.0</td>
    </tr>
  </tbody>
</table>
</div>
```


---

## Exploratory Data Analysis
The next step after loading the datasets is to perform EDA on the datasets using Statistical techniques.

### Data Preprocessing:
This is done before Data Analysis and Visualizations. It involves cleaning and transforming of Datasets.


```python
#Check information about production dataset
production_df['Year'] = production_df['Year'].astype(str)
#production_df.info()
```


```python
#check information about
supply_df['Year'] = supply_df['Year'].astype(str)
#supply_df.info()
```


```python
population_df['Year'] = population_df['Year'].astype(str)
#population_df.info()
```

### Grouping of African Food Production and Supply datasets for easy visualisations.

  #### a) Food Production Dataset.

Grouping by all variables based on similarity in observations


```python
#Total quantity of items for the african countries in each year
totalProd_CY = production_df.groupby(['Country','Year'], as_index=False).sum()
totalProd_CY
#Average quantity of items for the african countries in each year
meanProd_CY = production_df.groupby(['Country','Year'], as_index=False).mean()
meanProd_CY
#Total quantity of food items for each year
totalProd_IY = production_df.groupby(['Item','Year'],as_index=False).sum()
totalProd_IY
#Average quantity of food items for each year
meanProd_IY = production_df.groupby(['Item','Year'],as_index=False).mean()
meanProd_IY
#Total quantity of food items for each year
totalProd_CI = production_df.groupby(['Country','Item'], as_index=False).sum()
totalProd_CI
#Average quantity of food items for each year
meanProd_CI = production_df.groupby(['Country','Item'], as_index=False).mean()
meanProd_CI
#Total quantity of food items
totalProd_I = production_df.groupby('Item', as_index=False).sum()
totalProd_I
#Average quantity of food items
meanProd_I = production_df.groupby('Item', as_index=False).mean()
meanProd_I
#Total quantity of food items by country
totalProd_C = production_df.groupby('Country', as_index=False).sum()
totalProd_C
#Average quantity of food items by country
meanProd_C = production_df.groupby('Country', as_index=False).mean()
meanProd_C
#Total quantity of food items by year
totalProd_Y = production_df.groupby('Year', as_index=False).sum()
totalProd_Y
#Average quantity of food items by year
meanProd_Y = production_df.groupby('Year', as_index=False).mean()
meanProd_Y
```



```python
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Value [kt]</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2004</td>
      <td>286.767301</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2005</td>
      <td>298.986592</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2006</td>
      <td>310.814014</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2007</td>
      <td>305.215830</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2008</td>
      <td>318.686851</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2009</td>
      <td>323.040657</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2010</td>
      <td>340.166955</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2011</td>
      <td>351.303633</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2012</td>
      <td>364.831816</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2013</td>
      <td>378.227568</td>
    </tr>
  </tbody>
</table>
</div>
```


  #### a) Food Supply Dataset.

Grouping by Country and Year


```python
meanSupp_CY = supply_df.groupby(['Country','Year'],as_index=False).mean()
meanSupp_CY
#Total food items supplied by Country
totalSupp_C = supply_df.groupby('Country', as_index=False).sum()
totalSupp_C.head()
#Average food items supplied by Country
meanSupp_C = supply_df.groupby('Country', as_index=False).mean()
meanSupp_C.head()
#Total food items supplied by Year
totalSupp_Y = supply_df.groupby('Year', as_index=False).sum()
totalSupp_Y.head()
#Average food items supplied by Year
meanSupp_Y = supply_df.groupby('Year', as_index=False).mean()
#meanSupp_Y.head()
totalSupp_C.head()
```



```python
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Value [kcal/(person day)]</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Algeria</td>
      <td>31118</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Angola</td>
      <td>22556</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Benin</td>
      <td>25378</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Botswana</td>
      <td>22263</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Burkina Faso</td>
      <td>26072</td>
    </tr>
  </tbody>
</table>
</div>
```


### Ordering the production data (Top 20 and Bottom 20)


```python
#Sort values by country to get the top 20 countries with most food production
prodByC_orderedT = totalProd_C.sort_values('Value [kt]', ascending=False, axis = 0)
prodByC_top20 = prodByC_orderedT.head(20)
#Sort values by country to get the bottom 20 countries with most food production
prodByC_orderedB = totalProd_C.sort_values('Value [kt]', axis = 0).head(20)
prodByC_bottom20 = prodByC_orderedB.head(20)
#Sort values to get the top 20 items  mostly  produced
prodByI_orderedT = totalProd_I.sort_values('Value [kt]', ascending=False, axis = 0).head(20)
prodByI_top20 = prodByI_orderedT.head(20)
#Sort values to get the bottom 20 items  mostly  produced
prodByI_orderedB = totalProd_I.sort_values('Value [kt]', axis = 0).head(20)
prodByI_bottom20 = prodByI_orderedB.head(20)
#prodByI_bottom20
#prodByI_bottom20.Item.values
```

### Ordering the supply data (Top 20 and Bottom 20)


```python
#Sort values by country to get the top 20 countries with most food supply
suppByC_top20 = totalSupp_C.sort_values('Value [kcal/(person day)]', ascending=False, axis = 0).head(20)
suppByC_top20.head()
suppByC_bottom20 = totalSupp_C.sort_values('Value [kcal/(person day)]', ascending=True, axis = 0).head(20)
#suppByC_bottom20
#suppByC_bottom20.Country.values
```

### Convert the values for supply data to kT


```python
supply_df.head(2)
newsupply_df = supply_df
newsupply_df['Value [kcal/(person day)]'] = newsupply_df['Value [kcal/(person day)]'] * 365
new_population = population_df.groupby(['Country','Year'],as_index=False).sum()

supply_population = newsupply_df.merge(new_population, on=['Country','Year'], how='left')
supply_population.head()
supply_population['Value [kcal]'] = supply_population['Value [kcal/(person day)]'] * supply_population['Population']
supply_population.drop(['Value [kcal/(person day)]','Population'], axis=1, inplace=True)
supply_population['Value [kt]'] = supply_population['Value [kcal]'] * (1.0006692160612e-9)
supply_population.drop(['Value [kcal]'], axis=1, inplace=True)
supply_population.head()
supplyBy_C = supply_population.groupby('Country', as_index=False).sum()
supplyBy_Y = supply_population.groupby('Year', as_index=False).sum()
```

### Combine the food production and supply datasets grouped by Year and Country


```python
#Combine the food production and food supply datasets grouped by Year
totalPS_Y = totalProd_Y.merge(supplyBy_Y, on='Year')
totalPS_Y.columns =['Year','Quantity Produced','Quantity Supplied']
meanPS_Y = meanProd_Y.merge(meanSupp_Y, on='Year')
meanPS_Y.columns =['Year','Quantity Produced','Quantity Supplied']
#totalPC_Y
#totalProd_Y
```


```python
#Combine the food production and food supply datasets grouped by Year
totalPS_C = totalProd_C.merge(supplyBy_C, on='Country')
totalPS_C.columns =['Country','Quantity Produced','Quantity Supplied']
meanPS_C = meanProd_C.merge(meanSupp_C, on='Country')
meanPS_C.columns =['Country','Quantity Produced','Quantity Supplied']
#totalPC_Y
#totalProd_Y
```

----

## Data Visualization and Data Storytelling


```python
plt.style.use('ggplot')
sns.set(style='white') #to give the graph a particular design
plt.rc('figure', figsize=(16, 10))  # to make all the graph the same size
```

### Total Quantity produced and supplied


```python
prodQuantity = production_df['Value [kt]'].sum()
prodQuantity
suppQuantity = supply_df['Value [kcal/(person day)]'].sum()
suppQuantity
total_dict = {'Total Quantity Produced': 7575116, 'Total Quantity Supplied': 405660635}
totalQuantity = pd.DataFrame(total_dict, index=[0])
totalQuantity.plot(kind='bar', color=['green','red'])
#plt.label('Nigeria', rotation=0)
plt.ylabel('Quantity')
#plt.rcParams['figure.figsize'] = (10, 5)
plt.title('Total Quantity of food produced and supplied')
plt.legend()
plt.show()
```

```python
![png](Food_Shortage-Africa_files/Food_Shortage-Africa_36_0.png)
```

##### Narrative:
From the analysis, It was discovered that a total of 7,575,116 kiloTonnes of food items were produced in the African countries between the years 2004 to 2013 while a sum of 405,660,635 of food items were  consumed within these years.

### Top Countries based on quantity of food items produced


```python
productionBy_C = totalProd_C.reset_index().set_index('Country').sort_values('Value [kt]',ascending=False)
sns.barplot(x='Value [kt]', y=productionBy_C.index, data=productionBy_C, palette='dark')
plt.ylabel('Country')
plt.xlabel('Quantity of Food Items')
#plt.rcParams['figure.figsize'] = (20, 10)
plt.title('Top Countries based on quantity of food items produced (2004-2013)')
plt.legend()
plt.show()
```

    No handles with labels found to put in legend.


```python
![png](Food_Shortage-Africa_files/Food_Shortage-Africa_39_1.png)
```

#### Narratives:
Nigeria produced the highest quantity of food items between years 2004 to 2013, followed by Egypt and then South Africa.

### Top 40 Food Items based on quantity produced


```python
productionBy_I = totalProd_I.reset_index().set_index('Item').sort_values('Value [kt]',ascending=False).head(40)
sns.barplot(x='Value [kt]', y=productionBy_I.index, data=productionBy_I, palette='dark')
plt.ylabel('Item')
plt.xlabel('Quantity of Food Items')
#plt.rcParams['figure.figsize'] = (10, 5)
plt.title('Top 40 Food Items based on quantity produced (2004-2013)')
plt.legend()
plt.show()
```

    No handles with labels found to put in legend.


```python
![png](Food_Shortage-Africa_files/Food_Shortage-Africa_42_1.png)
```

#### Narratives:
"Cassava and products" was the most produced food item between the period 2004 to 2013, followed by "sugar cane" and then "maize and products".

### Bottom 40 Food Items based on quantity  produced


```python
productionBy_I = totalProd_I.reset_index().set_index('Item').sort_values('Value [kt]',ascending=False).tail(40)
sns.barplot(x='Value [kt]', y=productionBy_I.index, data=productionBy_I, palette='dark')
plt.ylabel('Item')
plt.xlabel('Quantity of Food Items')
#plt.rcParams['figure.figsize'] = (10, 5)
plt.title('Bottom 40 Food Items based on quantity produced (2004-2013)')
plt.legend()
plt.show()
```

    No handles with labels found to put in legend.


```python
![png](Food_Shortage-Africa_files/Food_Shortage-Africa_45_1.png)
```

##### Narrative:
"Aquatic Animals, Others" was the least produced food item between 2004 to 2013 inclusive followed by "Fish, Liver Oil" and then "Pepper".

### Top Countries based on quantity of food items supplied


```python
supplyBy_C = totalSupp_C.set_index('Country').sort_values('Value [kcal/(person day)]',ascending=False)
sns.barplot(x='Value [kcal/(person day)]', y=supplyBy_C.index, data=supplyBy_C, palette='dark')
plt.ylabel('Country')
plt.xlabel('Quantity of Food Items')
#plt.rcParams['figure.figsize'] = (10, 5)
plt.title('Countries in order of quantity of food items supplied (2004-2013)')
plt.legend()
plt.show()
```

    No handles with labels found to put in legend.


```python
![png](Food_Shortage-Africa_files/Food_Shortage-Africa_48_1.png)
```

#### Narrative:
Egypt produced the highest quantity of food items between the period 2004 to 2013 followed by Tunisia and then Morocco.

### Trend in Food Production over the years


```python
totalProd_Y.plot(kind='line',color='orange')
plt.title("Average Quantity of Food Items produced in 2004-2013")
plt.ylabel("Quantity")
plt.xlabel('Year')
plt.xticks(np.arange(10),['2004', '2005', '2006', '2007', '2008', '2009', '2010', '2011','2012', '2013'])
plt.show()
```

```python
![png](Food_Shortage-Africa_files/Food_Shortage-Africa_51_0.png)
```

##### Narrative:
The quantity of food items produced between 2004 to 2013 increased each year in respect to previous year except in 2007 that there was a fall.

### Trend in Food Consumption over the years


```python
supplyBy_Y.plot(kind='line',color='blue')
plt.title("Total Quantity of Food Items supplied in 2004-2013")
plt.ylabel("Quantity")
plt.xlabel('Year')
plt.xticks(np.arange(10),['2004', '2005', '2006', '2007', '2008', '2009', '2010', '2011','2012', '2013'])
plt.show()
```

```python
![png](Food_Shortage-Africa_files/Food_Shortage-Africa_54_0.png)
```

##### Narrative:
The quantity of food items supplied between 2004 to 2013 increased each year in respect to previous year.

### Comparison between food production and consumption over the years


```python
totalPS_Y.plot(kind='bar',color=['green','red'])
plt.title("Total Quantity of Food Items produced and supplied in 2004-2013")
plt.ylabel("Quantity")
plt.xlabel('Year')
#plt.yscale('log')
plt.xticks(np.arange(10),['2004', '2005', '2006', '2007', '2008', '2009', '2010', '2011','2012', '2013'],rotation=0)
plt.legend(loc=(1.02,0))
plt.show()
```

```python
![png](Food_Shortage-Africa_files/Food_Shortage-Africa_57_0.png)
```

##### Narrative:
There was significant difference in the quantity of food produced and quantity supplied across the year 2004 to 2013. Most food items produced were not supplied for consumption to the population in the African countries.

### Detecting extreme quantity of food items over the years


```python
#For Production dataset
meanProd_Y.head()
sns.boxplot(x='Year', y='Value [kt]', data=meanProd_CY, showmeans=True, meanprops={"marker":"o",
                       "markerfacecolor":"white",
                       "markeredgecolor":"black",
                      "markersize":"10"})
#totalProd_Y.head()
Q1 = meanProd_CY['Value [kt]'].quantile(0.25)
Q3 = meanProd_CY['Value [kt]'].quantile(0.75)
IQR = Q3 - Q1    #IQR is interquartile range.
lower_fence = Q1 - (1.5 * IQR)
upper_fence = Q3 + (1.5 * IQR)
filter = (meanProd_CY['Value [kt]'] < lower_fence) | (meanProd_CY['Value [kt]'] > upper_fence)
#meanProd_CY.loc[filter]  
```

```python
![png](Food_Shortage-Africa_files/Food_Shortage-Africa_60_0.png)
```

##### Narrative:
Some years had extreme values in food production.


```python
sns.boxplot(x='Year', y='Value [kcal/(person day)]', data=meanSupp_CY, showmeans=True, meanprops={"marker":"o",
                       "markerfacecolor":"white",
                       "markeredgecolor":"black",
                      "markersize":"10"})
Q1 = meanSupp_CY['Value [kcal/(person day)]'].quantile(0.25)
Q3 = meanSupp_CY['Value [kcal/(person day)]'].quantile(0.75)
IQR = Q3 - Q1    #IQR is interquartile range.
lower_fence2 = Q1 - 1.5 * (IQR)
upper_fence2 = Q3 + 1.5 * (IQR)
filter = (meanSupp_CY['Value [kcal/(person day)]'] < lower_fence2) | (meanSupp_CY['Value [kcal/(person day)]'] > upper_fence2)
#meanSupp_CY.loc[filter]  
```

```python
![png](Food_Shortage-Africa_files/Food_Shortage-Africa_62_0.png)
```

##### Narrative:


### Trend in Food Production for each country over the years


```python
country_ordered = totalProd_CY.sort_values(['Year','Value [kt]'], ascending=False, axis=0)
fig = plt.gcf()
fig.set_size_inches(12, 8)
line_plot = sns.lineplot(x='Year', y='Value [kt]', hue='Country',data=country_ordered )
line_plot.set_yscale('log')
plt.xlabel('Years')
plt.ylabel('Quantity of Food Produced')
line_plot.legend(loc='upper right', bbox_to_anchor=(1.3,1))
plt.title('Trend in Food Production for each country over the years', fontsize=15)
```



```python
    Text(0.5, 1.0, 'Trend in Food Production for each country over the years')




![png](Food_Shortage-Africa_files/Food_Shortage-Africa_65_1.png)
```

##### Narrative:
Nigeria, Egypt and South Africa maintained first, second and third position
respectively in the quantity of food produced in Africa while Djibouti, Sao Tome and Principe,
and Cabo Verde maintained the least, second to the least and third to the least respectively in
the quantity of food produced in Africa.

### Trend in Food Supply for each country over the years


```python
supply_ordered = supply_df.sort_values(['Year','Value [kcal/(person day)]'], ascending=False, axis=0)
#supply_ordered
fig = plt.gcf()
fig.set_size_inches(12, 8)
line_plot = sns.lineplot(x='Year', y='Value [kcal/(person day)]', hue='Country',data=supply_ordered)
#line_plot.set_yscale('log')
plt.xlabel('Years')
plt.ylabel('Quantity of Food Supplied')
line_plot.legend(loc='upper right', bbox_to_anchor=(1.3,1))
plt.title('Trend in Food Supply for each country over the years', fontsize=15)

```



```python
    Text(0.5, 1.0, 'Trend in Food Supply for each country over the years')




![png](Food_Shortage-Africa_files/Food_Shortage-Africa_68_1.png)
```

##### Narrative:
Egypt maintained the top country that supplied the highest quantity of food items across the years 2004 to 2013 while Central African Republic supplied the least in all the years except in 2013 that Zambia was the lowest food supplied country.

### Relationship between the Mean production and Mean population


```python
population_df.head()
meanPop_Y = population_df.groupby(['Year'],as_index=False).mean()
meanPop_C = population_df.groupby(['Country'], as_index=False).mean()
meanPPo_Y = meanProd_Y.merge(meanPop_Y, on='Year')
meanPPo_C = meanProd_C.merge(meanPop_C, on='Country')
meanPPo_C
meanPPo_Y
meanSPo_Y = meanSupp_Y.merge(meanPop_Y, on='Year')
meanSPo_C = meanSupp_C.merge(meanPop_C, on='Country')
meanPSPo_C = meanPPo_C.merge(meanSPo_C, on=['Country','Population'])
meanPSPo_Y = meanPPo_Y.merge(meanSPo_Y, on=['Year','Population'])
px.scatter(meanPPo_Y, x='Value [kt]', y='Population', text='Year')
#meanPSPo_Y
```

```python
<div>


            <div id="cc540f62-5fd3-4d6d-b936-5b9674b54a86" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};

                if (document.getElementById("cc540f62-5fd3-4d6d-b936-5b9674b54a86")) {
                    Plotly.newPlot(
                        'cc540f62-5fd3-4d6d-b936-5b9674b54a86',
                        [{"hovertemplate": "Value [kt]=%{x}<br>Population=%{y}<br>Year=%{text}<extra></extra>", "legendgroup": "", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers+text", "name": "", "orientation": "v", "showlegend": false, "text": ["2004", "2005", "2006", "2007", "2008", "2009", "2010", "2011", "2012", "2013"], "type": "scatter", "x": [286.76730103806227, 298.9865916955017, 310.81401384083046, 305.21583044982697, 318.6868512110727, 323.04065743944636, 340.16695501730106, 351.3036332179931, 364.8318162115301, 378.22756827048113], "xaxis": "x", "y": [256940573.58174905, 260334020.52471483, 263751672.97718632, 267189536.74904943, 270675379.8022814, 274182282.24334604, 277695205.20912546, 281194230.8136882, 285839770.4198473, 289474781.5496183], "yaxis": "y"}],
                        {"legend": {"tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "xaxis": {"anchor": "y", "domain": [0.0, 1.0], "title": {"text": "Value [kt]"}}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "Population"}}},
                        {"responsive": true}
                    ).then(function(){

var gd = document.getElementById('cc540f62-5fd3-4d6d-b936-5b9674b54a86');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>

```
##### Narrative:
An increase in the population approximately brought an increase in the food produced across the years 2004 to 2013.


```python
px.scatter(meanPPo_C, x='Value [kt]', y='Population', text='Country')
```

```python
<div>


            <div id="da7fedc8-c0af-4ec0-a530-af36664d6003" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};

                if (document.getElementById("da7fedc8-c0af-4ec0-a530-af36664d6003")) {
                    Plotly.newPlot(
                        'da7fedc8-c0af-4ec0-a530-af36664d6003',
                        [{"hovertemplate": "Value [kt]=%{x}<br>Population=%{y}<br>Country=%{text}<extra></extra>", "legendgroup": "", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers+text", "name": "", "orientation": "v", "showlegend": false, "text": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Cote d'Ivoire", "Djibouti", "Ethiopia", "Gabon", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Togo", "Tunisia", "Uganda", "Zambia", "Zimbabwe"], "type": "scatter", "x": [325.2586206896552, 362.0351851851852, 149.27457627118645, 14.141176470588235, 232.85813953488372, 5.024242424242424, 304.10769230769233, 55.38, 115.58461538461539, 285.134375, 3.0272727272727273, 584.4883333333333, 33.915, 508.6275862068965, 133.696, 16.581818181818182, 342.496, 16.456521739130434, 34.897619047619045, 192.85652173913044, 320.24375, 192.275, 26.41842105263158, 112.57608695652173, 353.01428571428573, 254.16774193548386, 33.01627906976744, 201.87906976744185, 2668.901639344262, 206.9, 3.972413793103448, 95.02692307692308, 95.474, 772.7893333333334, 558.4888888888889, 57.419642857142854, 151.98852459016393, 497.18035714285713, 172.29375, 114.55625], "xaxis": "x", "y": [35187726.8, 22215754.4, 8843947.3, 1924970.3, 14969566.8, 483988.5, 19579946.6, 4261508.8, 11416858.2, 19911217.5, 824435.3, 84379216.5, 1561006.7, 23893615.1, 9872165.8, 1471722.7, 40438113.6, 1999905.2, 3679967.4, 20322261.1, 13983633.8, 14368774.1, 3358440.2, 1243177.2, 31808785.9, 22647164.3, 2065220.3, 15655202.6, 152796108.3, 9683902.7, 173157.2, 12220570.0, 6186025.7, 50278228.8, 33519180.5, 6184305.0, 10475219.2, 31056602.7, 13116783.2, 12547206.7], "yaxis": "y"}],
                        {"legend": {"tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "xaxis": {"anchor": "y", "domain": [0.0, 1.0], "title": {"text": "Value [kt]"}}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "Population"}}},
                        {"responsive": true}
                    ).then(function(){

var gd = document.getElementById('da7fedc8-c0af-4ec0-a530-af36664d6003');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>

```
##### Narrative:
Nigeria is the country with the most population and produced most quantities of food items in Africa.

### Relationship between the Mean consumption and Mean population


```python
px.scatter(meanSPo_Y, x='Value [kcal/(person day)]', y='Population', text='Year')
```

```python
<div>


            <div id="c6324e8e-ce37-4c70-9e3f-e7472a6c3787" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};

                if (document.getElementById("c6324e8e-ce37-4c70-9e3f-e7472a6c3787")) {
                    Plotly.newPlot(
                        'c6324e8e-ce37-4c70-9e3f-e7472a6c3787',
                        [{"hovertemplate": "Value [kcal/(person day)]=%{x}<br>Population=%{y}<br>Year=%{text}<extra></extra>", "legendgroup": "", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers+text", "name": "", "orientation": "v", "showlegend": false, "text": ["2004", "2005", "2006", "2007", "2008", "2009", "2010", "2011", "2012", "2013"], "type": "scatter", "x": [2394.222222222222, 2409.288888888889, 2430.8, 2447.7555555555555, 2460.7555555555555, 2482.222222222222, 2497.4, 2515.4222222222224, 2527.6444444444446, 2532.2444444444445], "xaxis": "x", "y": [256940573.58174905, 260334020.52471483, 263751672.97718632, 267189536.74904943, 270675379.8022814, 274182282.24334604, 277695205.20912546, 281194230.8136882, 285839770.4198473, 289474781.5496183], "yaxis": "y"}],
                        {"legend": {"tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "xaxis": {"anchor": "y", "domain": [0.0, 1.0], "title": {"text": "Value [kcal/(person day)]"}}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "Population"}}},
                        {"responsive": true}
                    ).then(function(){

var gd = document.getElementById('c6324e8e-ce37-4c70-9e3f-e7472a6c3787');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>
```

##### Narrative:
An increase in the population approximately brought an increase in the food supplied across the years 2004 to 2013.


```python
px.scatter(meanSPo_C, x='Value [kcal/(person day)]', y='Population', text='Country')

```

```python
<div>


            <div id="b98a0f97-909a-4ff6-838f-7c43e7cc860f" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};

                if (document.getElementById("b98a0f97-909a-4ff6-838f-7c43e7cc860f")) {
                    Plotly.newPlot(
                        'b98a0f97-909a-4ff6-838f-7c43e7cc860f',
                        [{"hovertemplate": "Value [kcal/(person day)]=%{x}<br>Population=%{y}<br>Country=%{text}<extra></extra>", "legendgroup": "", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers+text", "name": "", "orientation": "v", "showlegend": false, "text": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Cote d'Ivoire", "Djibouti", "Ethiopia", "Gabon", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Togo", "Tunisia", "Uganda", "Zambia", "Zimbabwe"], "type": "scatter", "x": [3111.8, 2255.6, 2537.8, 2226.3, 2607.2, 2551.4, 2460.3, 2071.9, 2051.1, 2766.6, 2416.5, 2029.2, 2729.9, 2918.0, 2518.0, 2296.3, 2145.3, 2558.8, 2182.7, 2060.8, 2292.5, 2750.2, 2744.3, 3054.3, 3296.7, 2170.2, 2160.2, 2502.4, 2698.8, 2130.9, 2446.2, 2378.4, 2229.1, 2962.9, 2323.8, 2333.9, 3305.5, 2220.5, 1870.1, 2120.9], "xaxis": "x", "y": [35187726.8, 22215754.4, 8843947.3, 1924970.3, 14969566.8, 483988.5, 19579946.6, 4261508.8, 11416858.2, 19911217.5, 824435.3, 84379216.5, 1561006.7, 23893615.1, 9872165.8, 1471722.7, 40438113.6, 1999905.2, 3679967.4, 20322261.1, 13983633.8, 14368774.1, 3358440.2, 1243177.2, 31808785.9, 22647164.3, 2065220.3, 15655202.6, 152796108.3, 9683902.7, 173157.2, 12220570.0, 6186025.7, 50278228.8, 33519180.5, 6184305.0, 10475219.2, 31056602.7, 13116783.2, 12547206.7], "yaxis": "y"}],
                        {"legend": {"tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "xaxis": {"anchor": "y", "domain": [0.0, 1.0], "title": {"text": "Value [kcal/(person day)]"}}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "Population"}}},
                        {"responsive": true}
                    ).then(function(){

var gd = document.getElementById('b98a0f97-909a-4ff6-838f-7c43e7cc860f');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>

```
##### Narrative:

### Average Food Production by Country


```python
from plotly.offline import plot
fig = px.scatter(meanProd_C, x='Country', y='Value [kt]', title ='AVERAGE FOOD PRODUCTION BY COUNTRY',text='Country')
#add two horizontal lines
fig.update_layout(width=1000, height=900, autosize=False, shapes=[
    #lower fence
    dict(type='line', xref='paper', x0=0, x1=1, yref='y', y0=lower_fence, y1=lower_fence, line=dict(
    color='Red', width=1)),
    #mean
    dict(type='line', xref='paper', x0=0, x1=1, yref='y', y0=meanProd_C['Value [kt]'].mean(), y1=meanProd_C['Value [kt]'].mean(), line=dict(
    color='Orange', width=1)),
    #median
    dict(type='line', xref='paper', x0=0, x1=1, yref='y', y0=meanProd_C['Value [kt]'].median(), y1=meanProd_C['Value [kt]'].median(), line=dict(
    color='Blue', width=1)),
    #upper fence
    dict(type='line', xref='paper', x0=0, x1=1, yref='y', y0=upper_fence, y1=upper_fence, line=dict(
    color='Green', width=1))], showlegend=False)

fig.update_traces(textposition='top center')
fig.update_xaxes(showticklabels= False)

fig
```

```python
<div>


            <div id="ef18d304-abf8-4d79-aa64-87f4d439fa87" class="plotly-graph-div" style="height:900px; width:1000px;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};

                if (document.getElementById("ef18d304-abf8-4d79-aa64-87f4d439fa87")) {
                    Plotly.newPlot(
                        'ef18d304-abf8-4d79-aa64-87f4d439fa87',
                        [{"hovertemplate": "Country=%{text}<br>Value [kt]=%{y}<extra></extra>", "legendgroup": "", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers+text", "name": "", "orientation": "v", "showlegend": false, "text": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "textposition": "top center", "type": "scatter", "x": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "xaxis": "x", "y": [325.2586206896552, 362.0351851851852, 149.27457627118645, 14.141176470588235, 232.85813953488372, 5.024242424242424, 304.10769230769233, 55.38, 115.58461538461539, 51.582, 285.134375, 3.0272727272727273, 1253.5685714285714, 584.4883333333333, 33.915, 13.052777777777777, 508.6275862068965, 133.696, 16.581818181818182, 342.496, 16.456521739130434, 34.897619047619045, 192.85652173913044, 320.24375, 192.275, 26.41842105263158, 112.57608695652173, 353.01428571428573, 254.16774193548386, 33.01627906976744, 201.87906976744185, 2668.901639344262, 206.9, 3.972413793103448, 95.02692307692308, 95.474, 772.7893333333334, 558.4888888888889, 149.20238095238096, 57.419642857142854, 151.98852459016393, 497.18035714285713, 421.26835443037976, 172.29375, 114.55625], "yaxis": "y"}],
                        {"autosize": false, "height": 900, "legend": {"tracegroupgap": 0}, "shapes": [{"line": {"color": "Red", "width": 1}, "type": "line", "x0": 0, "x1": 1, "xref": "paper", "y0": -381.85068548387096, "y1": -381.85068548387096, "yref": "y"}, {"line": {"color": "Orange", "width": 1}, "type": "line", "x0": 0, "x1": 1, "xref": "paper", "y0": 277.62439418567044, "y1": 277.62439418567044, "yref": "y"}, {"line": {"color": "Blue", "width": 1}, "type": "line", "x0": 0, "x1": 1, "xref": "paper", "y0": 151.98852459016393, "y1": 151.98852459016393, "yref": "y"}, {"line": {"color": "Green", "width": 1}, "type": "line", "x0": 0, "x1": 1, "xref": "paper", "y0": 763.517809139785, "y1": 763.517809139785, "yref": "y"}], "showlegend": false, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "title": {"text": "AVERAGE FOOD PRODUCTION BY COUNTRY"}, "width": 1000, "xaxis": {"anchor": "y", "domain": [0.0, 1.0], "showticklabels": false, "title": {"text": "Country"}}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "Value [kt]"}}},
                        {"responsive": true}
                    ).then(function(){

var gd = document.getElementById('ef18d304-abf8-4d79-aa64-87f4d439fa87');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>

```
##### Narratives:
Nigeria, Egypt and South Africa produced extremely high quantities of food items above other countries in Africa.

### Average Food Supply by Country


```python
from plotly.offline import plot
fig = px.scatter(meanSupp_C, x='Country', y='Value [kcal/(person day)]', title ='AVERAGE FOOD SUPPLY BY COUNTRY',text='Country')
#add two horizontal lines
fig.update_layout(width=1000, height=900, autosize=False, shapes=[
    #lower fence
    dict(type='line', xref='paper', x0=0, x1=1, yref='y', y0=lower_fence2, y1=lower_fence2, line=dict(
    color='Red', width=1)),
    #mean
    dict(type='line', xref='paper', x0=0, x1=1, yref='y', y0=meanSupp_C['Value [kcal/(person day)]'].mean(), y1=meanSupp_C['Value [kcal/(person day)]'].mean(), line=dict(
    color='Orange', width=1)),
    #median
    dict(type='line', xref='paper', x0=0, x1=1, yref='y', y0=meanSupp_C['Value [kcal/(person day)]'].median(), y1=meanSupp_C['Value [kcal/(person day)]'].median(), line=dict(
    color='Blue', width=1)),
    #upper fence
    dict(type='line', xref='paper', x0=0, x1=1, yref='y', y0=upper_fence2, y1=upper_fence2, line=dict(
    color='Green', width=1))], showlegend=False)

fig.update_traces(textposition='top center')
fig.update_xaxes(showticklabels= False)

fig
```

```python
<div>


            <div id="7fa65ae1-d442-4090-af19-cbe1c947971b" class="plotly-graph-div" style="height:900px; width:1000px;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};

                if (document.getElementById("7fa65ae1-d442-4090-af19-cbe1c947971b")) {
                    Plotly.newPlot(
                        '7fa65ae1-d442-4090-af19-cbe1c947971b',
                        [{"hovertemplate": "Country=%{text}<br>Value [kcal/(person day)]=%{y}<extra></extra>", "legendgroup": "", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers+text", "name": "", "orientation": "v", "showlegend": false, "text": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "textposition": "top center", "type": "scatter", "x": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "xaxis": "x", "y": [3111.8, 2255.6, 2537.8, 2226.3, 2607.2, 2551.4, 2460.3, 2071.9, 2051.1, 2153.0, 2766.6, 2416.5, 3458.0, 2029.2, 2729.9, 2569.5, 2918.0, 2518.0, 2296.3, 2145.3, 2558.8, 2182.7, 2060.8, 2292.5, 2750.2, 2744.3, 3054.3, 3296.7, 2170.2, 2160.2, 2502.4, 2698.8, 2130.9, 2446.2, 2378.4, 2229.1, 2962.9, 2323.8, 2317.1, 2333.9, 3305.5, 2220.5, 2155.0, 1870.1, 2120.9], "yaxis": "y"}],
                        {"autosize": false, "height": 900, "legend": {"tracegroupgap": 0}, "shapes": [{"line": {"color": "Red", "width": 1}, "type": "line", "x0": 0, "x1": 1, "xref": "paper", "y0": 1412.375, "y1": 1412.375, "yref": "y"}, {"line": {"color": "Orange", "width": 1}, "type": "line", "x0": 0, "x1": 1, "xref": "paper", "y0": 2469.775555555555, "y1": 2469.775555555555, "yref": "y"}, {"line": {"color": "Blue", "width": 1}, "type": "line", "x0": 0, "x1": 1, "xref": "paper", "y0": 2378.4, "y1": 2378.4, "yref": "y"}, {"line": {"color": "Green", "width": 1}, "type": "line", "x0": 0, "x1": 1, "xref": "paper", "y0": 3443.375, "y1": 3443.375, "yref": "y"}], "showlegend": false, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "title": {"text": "AVERAGE FOOD SUPPLY BY COUNTRY"}, "width": 1000, "xaxis": {"anchor": "y", "domain": [0.0, 1.0], "showticklabels": false, "title": {"text": "Country"}}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "Value [kcal/(person day)]"}}},
                        {"responsive": true}
                    ).then(function(){

var gd = document.getElementById('7fa65ae1-d442-4090-af19-cbe1c947971b');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>

```
##### Narratives:
Only Egypt supplied extremely high quantities of food between 2004 to 2013 above other countries in Africa.

### Map showing the quantity of food items produced in African countries


```python
country_code = population_df.drop(['Population','Year'], axis=1)
country_code.head()
country_code = country_code.drop_duplicates(subset='Country',keep='first')
country_code['Country'] = country_code['Country'].replace(['Congo, Dem. Rep.','Egypt, Arab Rep.','Eswatini','Gambia, The','Tanzania'],['Congo','Egypt','Swaziland','Gambia','United Republic of Tanzania'])
country_code
production_map = totalProd_CY
production_map['Year'] = production_map['Year'].astype(str)
totalProd_CCo = production_map.merge(country_code, on='Country', how='right')
totalProd_CCo.head()
px.choropleth(totalProd_CCo, locations='Code',hover_name='Country', title = 'Food Production in Africa (2004-2013)',
                   color='Value [kt]',labels={'Value [kt]':'Quantity'}, scope='africa',
                    color_continuous_scale=px.colors.sequential.Plasma, animation_frame='Year')
```

```python
<div>


            <div id="34973262-ff66-4663-b144-46517b28af14" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};

                if (document.getElementById("34973262-ff66-4663-b144-46517b28af14")) {
                    Plotly.newPlot(
                        '34973262-ff66-4663-b144-46517b28af14',
                        [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2004<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "type": "choropleth", "z": [15536.0, 13028.0, 7963.0, 461.0, 8323.0, 148.0, 13739.0, 2242.0, 3660.0, 2241.0, 16675.0, 55.0, 75989.0, 26246.0, 1211.0, 473.0, 24040.0, 5891.0, 637.0, 20664.0, 371.0, 1382.0, 10360.0, 10917.0, 6275.0, 906.0, 6081.0, 27244.0, 12867.0, 1421.0, 5852.0, 149857.0, 6863.0, 113.0, 4368.0, 3592.0, 54949.0, 28646.0, 5950.0, 2765.0, 8503.0, 31819.0, 27389.0, 6310.0, 8984.0]}],
                        {"coloraxis": {"colorbar": {"title": {"text": "Quantity"}}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "geo": {"center": {}, "domain": {"x": [0.0, 1.0], "y": [0.0, 1.0]}, "scope": "africa"}, "legend": {"tracegroupgap": 0}, "sliders": [{"active": 0, "currentvalue": {"prefix": "Year="}, "len": 0.9, "pad": {"b": 10, "t": 60}, "steps": [{"args": [["2004"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2004", "method": "animate"}, {"args": [["2005"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2005", "method": "animate"}, {"args": [["2006"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2006", "method": "animate"}, {"args": [["2007"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2007", "method": "animate"}, {"args": [["2008"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2008", "method": "animate"}, {"args": [["2009"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2009", "method": "animate"}, {"args": [["2010"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2010", "method": "animate"}, {"args": [["2011"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2011", "method": "animate"}, {"args": [["2012"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2012", "method": "animate"}, {"args": [["2013"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2013", "method": "animate"}], "x": 0.1, "xanchor": "left", "y": 0, "yanchor": "top"}], "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "title": {"text": "Food Production in Africa (2004-2013)"}, "updatemenus": [{"buttons": [{"args": [null, {"frame": {"duration": 500, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 500, "easing": "linear"}}], "label": "&#9654;", "method": "animate"}, {"args": [[null], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "&#9724;", "method": "animate"}], "direction": "left", "pad": {"r": 10, "t": 70}, "showactive": false, "type": "buttons", "x": 0.1, "xanchor": "right", "y": 0, "yanchor": "top"}]},
                        {"responsive": true}
                    ).then(function(){
                            Plotly.addFrames('34973262-ff66-4663-b144-46517b28af14', [{"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2004<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [15536.0, 13028.0, 7963.0, 461.0, 8323.0, 148.0, 13739.0, 2242.0, 3660.0, 2241.0, 16675.0, 55.0, 75989.0, 26246.0, 1211.0, 473.0, 24040.0, 5891.0, 637.0, 20664.0, 371.0, 1382.0, 10360.0, 10917.0, 6275.0, 906.0, 6081.0, 27244.0, 12867.0, 1421.0, 5852.0, 149857.0, 6863.0, 113.0, 4368.0, 3592.0, 54949.0, 28646.0, 5950.0, 2765.0, 8503.0, 31819.0, 27389.0, 6310.0, 8984.0], "type": "choropleth"}], "name": "2004"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2005<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [15667.0, 13811.0, 7764.0, 460.0, 9392.0, 156.0, 16033.0, 2293.0, 4360.0, 2318.0, 17114.0, 62.0, 80422.0, 29527.0, 1246.0, 440.0, 24448.0, 6099.0, 703.0, 23745.0, 383.0, 1443.0, 12228.0, 9786.0, 7233.0, 1016.0, 5720.0, 22794.0, 11391.0, 1434.0, 7170.0, 158149.0, 7370.0, 117.0, 4757.0, 3040.0, 59577.0, 31369.0, 6407.0, 2730.0, 8795.0, 31645.0, 27329.0, 6075.0, 7239.0], "type": "choropleth"}], "name": "2005"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2006<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [16417.0, 14264.0, 7110.0, 454.0, 9393.0, 158.0, 16962.0, 2329.0, 4423.0, 2365.0, 17660.0, 56.0, 83191.0, 30117.0, 1263.0, 458.0, 25054.0, 6166.0, 705.0, 24656.0, 383.0, 1358.0, 12608.0, 13078.0, 7301.0, 875.0, 5471.0, 28594.0, 12665.0, 1448.0, 7855.0, 168987.0, 7405.0, 117.0, 4030.0, 3657.0, 54024.0, 30834.0, 6124.0, 2908.0, 8688.0, 31415.0, 30472.0, 6879.0, 8225.0], "type": "choropleth"}], "name": "2006"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2007<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [14763.0, 16025.0, 7679.0, 439.0, 8467.0, 157.0, 17657.0, 2426.0, 4243.0, 2438.0, 17570.0, 63.0, 86828.0, 29891.0, 1289.0, 360.0, 25286.0, 6393.0, 654.0, 24433.0, 344.0, 1446.0, 12720.0, 14908.0, 8249.0, 913.0, 4891.0, 20869.0, 12640.0, 1331.0, 8152.0, 157273.0, 7295.0, 113.0, 4024.0, 3731.0, 53795.0, 31689.0, 6096.0, 2938.0, 9052.0, 31891.0, 30673.0, 6743.0, 6822.0], "type": "choropleth"}], "name": "2007"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2008<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [13841.0, 17288.0, 8723.0, 470.0, 10373.0, 172.0, 18459.0, 2494.0, 4332.0, 2530.0, 18496.0, 62.0, 89489.0, 31880.0, 1308.0, 472.0, 28290.0, 6512.0, 725.0, 24928.0, 347.0, 1491.0, 12774.0, 14910.0, 8855.0, 909.0, 5200.0, 24679.0, 12646.0, 1335.0, 9820.0, 167935.0, 8371.0, 114.0, 5987.0, 3991.0, 61162.0, 30731.0, 6149.0, 3085.0, 8842.0, 23491.0, 30876.0, 6383.0, 5877.0], "type": "choropleth"}], "name": "2008"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2009<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [19820.0, 22244.0, 9235.0, 484.0, 9607.0, 158.0, 20729.0, 2569.0, 4516.0, 2693.0, 17351.0, 68.0, 90375.0, 34508.0, 1387.0, 549.0, 30647.0, 6606.0, 700.0, 26049.0, 339.0, 1457.0, 13928.0, 16677.0, 10853.0, 953.0, 5369.0, 30354.0, 14107.0, 1359.0, 7715.0, 141270.0, 9838.0, 116.0, 5689.0, 5084.0, 59590.0, 31778.0, 6128.0, 3389.0, 9731.0, 24574.0, 31828.0, 8695.0, 5754.0], "type": "choropleth"}], "name": "2009"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2010<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [20263.0, 23805.0, 9200.0, 503.0, 11122.0, 149.0, 22310.0, 2602.0, 4633.0, 2707.0, 18363.0, 75.0, 88450.0, 38707.0, 1424.0, 614.0, 32140.0, 6904.0, 743.0, 27991.0, 451.0, 1490.0, 14215.0, 16867.0, 10671.0, 1110.0, 5060.0, 28577.0, 19611.0, 1422.0, 10576.0, 158709.0, 10780.0, 114.0, 5737.0, 5783.0, 56863.0, 28568.0, 6157.0, 3424.0, 8854.0, 25850.0, 36006.0, 10089.0, 6777.0], "type": "choropleth"}], "name": "2010"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2011<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [21778.0, 25672.0, 9735.0, 539.0, 9954.0, 181.0, 22964.0, 2702.0, 4070.0, 2745.0, 18918.0, 72.0, 90767.0, 39573.0, 1443.0, 411.0, 33650.0, 7084.0, 807.0, 26605.0, 397.0, 1535.0, 14735.0, 17569.0, 10773.0, 1024.0, 4911.0, 30978.0, 20976.0, 1457.0, 8923.0, 167403.0, 11777.0, 107.0, 4613.0, 6083.0, 56788.0, 30869.0, 6221.0, 3605.0, 9790.0, 25704.0, 38424.0, 10331.0, 7551.0], "type": "choropleth"}], "name": "2011"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2012<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [24206.0, 20505.0, 9797.0, 496.0, 11634.0, 187.0, 24044.0, 2761.0, 5688.0, 2786.0, 19919.0, 77.0, 96139.0, 44142.0, 1474.0, 465.0, 34860.0, 7458.0, 787.0, 28851.0, 352.0, 1522.0, 15343.0, 19074.0, 11486.0, 1182.0, 4610.0, 26236.0, 19939.0, 1522.0, 10719.0, 178816.0, 12523.0, 111.0, 5237.0, 6228.0, 59581.0, 26373.0, 6670.0, 3848.0, 10509.0, 25711.0, 38956.0, 10670.0, 8173.0], "type": "choropleth"}], "name": "2012"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2013<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [26359.0, 28857.0, 10866.0, 502.0, 11864.0, 192.0, 24773.0, 2503.0, 5153.0, 2968.0, 20420.0, 76.0, 95848.0, 46102.0, 1521.0, 457.0, 36589.0, 7735.0, 835.0, 28950.0, 418.0, 1533.0, 14160.0, 19931.0, 10596.0, 1151.0, 4472.0, 31496.0, 20742.0, 1468.0, 10026.0, 179631.0, 12952.0, 130.0, 4972.0, 6548.0, 63263.0, 30727.0, 6763.0, 3463.0, 9949.0, 26321.0, 40849.0, 10526.0, 7914.0], "type": "choropleth"}], "name": "2013"}]);
                        }).then(function(){

var gd = document.getElementById('34973262-ff66-4663-b144-46517b28af14');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>
```

##### Narrative:
From the map, it is obvious that Nigeria produced the most food items followed by
Egypt and then South Africa. Other countries produced food in low quantities compared to
Nigeria.

### Map showing the quantity of food items supplied in African countries


```python
supply_map = supply_df
supply_map['Year'] = supply_map['Year'].astype(str)
totalSupp_CCo = supply_map.merge(country_code, on='Country', how='right')
totalSupp_CCo.head()
px.choropleth(totalSupp_CCo, locations='Code',hover_name='Country', title = 'Food Supply in Africa (2004-2013)',
                   color='Value [kcal/(person day)]',labels={'Value [kcal/(person day)]':'Quantity'}, scope='africa',
                    color_continuous_scale=px.colors.sequential.Plasma, animation_frame='Year')
```

```python
<div>


            <div id="1c9b6fb4-369e-41d9-85c6-6ccdcdda2625" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};

                if (document.getElementById("1c9b6fb4-369e-41d9-85c6-6ccdcdda2625")) {
                    Plotly.newPlot(
                        '1c9b6fb4-369e-41d9-85c6-6ccdcdda2625',
                        [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2004<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "type": "choropleth", "z": [1090255.0, 740950.0, 898265.0, 799715.0, 914325.0, 920895.0, 819790.0, 725985.0, 739490.0, 825995.0, 999370.0, 779640.0, 1207785.0, 686930.0, 976740.0, 899725.0, 978930.0, 885490.0, 823805.0, 738030.0, 927465.0, 763580.0, 722700.0, 790590.0, 927100.0, 948635.0, 1103395.0, 1190995.0, 751535.0, 827820.0, 895345.0, 969075.0, 718685.0, 917245.0, 820520.0, 738760.0, 1073100.0, 828915.0, 901185.0, 819790.0, 1185520.0, 858115.0, 765770.0, 681090.0, 746060.0]}],
                        {"coloraxis": {"colorbar": {"title": {"text": "Quantity"}}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "geo": {"center": {}, "domain": {"x": [0.0, 1.0], "y": [0.0, 1.0]}, "scope": "africa"}, "legend": {"tracegroupgap": 0}, "sliders": [{"active": 0, "currentvalue": {"prefix": "Year="}, "len": 0.9, "pad": {"b": 10, "t": 60}, "steps": [{"args": [["2004"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2004", "method": "animate"}, {"args": [["2005"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2005", "method": "animate"}, {"args": [["2006"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2006", "method": "animate"}, {"args": [["2007"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2007", "method": "animate"}, {"args": [["2008"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2008", "method": "animate"}, {"args": [["2009"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2009", "method": "animate"}, {"args": [["2010"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2010", "method": "animate"}, {"args": [["2011"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2011", "method": "animate"}, {"args": [["2012"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2012", "method": "animate"}, {"args": [["2013"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2013", "method": "animate"}], "x": 0.1, "xanchor": "left", "y": 0, "yanchor": "top"}], "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "title": {"text": "Food Supply in Africa (2004-2013)"}, "updatemenus": [{"buttons": [{"args": [null, {"frame": {"duration": 500, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 500, "easing": "linear"}}], "label": "&#9654;", "method": "animate"}, {"args": [[null], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "&#9724;", "method": "animate"}], "direction": "left", "pad": {"r": 10, "t": 70}, "showactive": false, "type": "buttons", "x": 0.1, "xanchor": "right", "y": 0, "yanchor": "top"}]},
                        {"responsive": true}
                    ).then(function(){
                            Plotly.addFrames('1c9b6fb4-369e-41d9-85c6-6ccdcdda2625', [{"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2004<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [1090255.0, 740950.0, 898265.0, 799715.0, 914325.0, 920895.0, 819790.0, 725985.0, 739490.0, 825995.0, 999370.0, 779640.0, 1207785.0, 686930.0, 976740.0, 899725.0, 978930.0, 885490.0, 823805.0, 738030.0, 927465.0, 763580.0, 722700.0, 790590.0, 927100.0, 948635.0, 1103395.0, 1190995.0, 751535.0, 827820.0, 895345.0, 969075.0, 718685.0, 917245.0, 820520.0, 738760.0, 1073100.0, 828915.0, 901185.0, 819790.0, 1185520.0, 858115.0, 765770.0, 681090.0, 746060.0], "type": "choropleth"}], "name": "2004"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2005<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [1079670.0, 758105.0, 888775.0, 802270.0, 898995.0, 918705.0, 843515.0, 739490.0, 742410.0, 745695.0, 1017255.0, 826725.0, 1228955.0, 721970.0, 990975.0, 891330.0, 1004115.0, 891695.0, 829645.0, 788765.0, 936955.0, 788400.0, 751900.0, 799715.0, 942795.0, 959585.0, 1098285.0, 1170190.0, 743870.0, 818695.0, 892060.0, 987325.0, 743505.0, 929290.0, 816505.0, 759930.0, 1077480.0, 838040.0, 865780.0, 816870.0, 1175300.0, 849355.0, 778910.0, 683645.0, 739125.0], "type": "choropleth"}], "name": "2005"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2006<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [1112155.0, 773435.0, 894250.0, 784750.0, 934035.0, 917245.0, 859210.0, 740585.0, 738030.0, 782195.0, 1015795.0, 844610.0, 1236985.0, 717955.0, 991340.0, 874905.0, 1030760.0, 913595.0, 822345.0, 788765.0, 947175.0, 772340.0, 760295.0, 828185.0, 968710.0, 950095.0, 1103030.0, 1193915.0, 770515.0, 813950.0, 891330.0, 994625.0, 746060.0, 932210.0, 843150.0, 786575.0, 1068355.0, 850815.0, 848625.0, 833295.0, 1194645.0, 830740.0, 781465.0, 670870.0, 771975.0], "type": "choropleth"}], "name": "2006"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2007<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [1109965.0, 793145.0, 935860.0, 790590.0, 929290.0, 939510.0, 867970.0, 762850.0, 740220.0, 779275.0, 1019080.0, 881110.0, 1257425.0, 714305.0, 989515.0, 925275.0, 1023095.0, 914325.0, 836945.0, 770150.0, 934400.0, 782925.0, 765040.0, 836945.0, 993530.0, 994990.0, 1122740.0, 1186615.0, 760295.0, 809570.0, 898265.0, 992800.0, 749345.0, 916515.0, 854830.0, 800810.0, 1065800.0, 855560.0, 843150.0, 837675.0, 1186980.0, 807380.0, 808475.0, 650065.0, 769785.0], "type": "choropleth"}], "name": "2007"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2008<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [1112520.0, 819425.0, 920165.0, 797160.0, 944620.0, 927100.0, 892790.0, 778545.0, 739855.0, 770150.0, 999735.0, 893155.0, 1273850.0, 736570.0, 968345.0, 944985.0, 1076750.0, 924910.0, 840960.0, 768690.0, 933670.0, 812855.0, 752265.0, 846070.0, 1013605.0, 1011780.0, 1095365.0, 1193915.0, 789860.0, 773800.0, 919800.0, 993895.0, 766865.0, 900090.0, 890235.0, 801905.0, 1067260.0, 855560.0, 823805.0, 845340.0, 1205230.0, 794605.0, 778180.0, 657365.0, 764310.0], "type": "choropleth"}], "name": "2008"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2009<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [1135150.0, 840595.0, 936225.0, 802635.0, 963235.0, 933670.0, 909580.0, 780735.0, 737665.0, 781830.0, 995355.0, 900090.0, 1255965.0, 748980.0, 985135.0, 956665.0, 1106680.0, 929290.0, 831470.0, 790225.0, 937685.0, 814315.0, 753725.0, 843515.0, 1033315.0, 1008130.0, 1140260.0, 1201945.0, 790225.0, 752630.0, 920895.0, 979295.0, 801540.0, 888775.0, 892790.0, 821615.0, 1071640.0, 856290.0, 820885.0, 854100.0, 1217275.0, 800810.0, 778545.0, 685470.0, 783655.0], "type": "choropleth"}], "name": "2009"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2010<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [1146830.0, 855925.0, 932575.0, 815410.0, 978565.0, 921260.0, 916150.0, 789860.0, 750805.0, 782195.0, 999005.0, 900455.0, 1280055.0, 759200.0, 997180.0, 976010.0, 1123105.0, 930750.0, 856290.0, 788400.0, 938780.0, 802270.0, 754820.0, 850085.0, 1026745.0, 1020905.0, 1150115.0, 1202310.0, 818330.0, 750075.0, 924545.0, 987690.0, 802270.0, 864320.0, 888775.0, 830375.0, 1092080.0, 847895.0, 833660.0, 865050.0, 1213990.0, 803000.0, 765405.0, 694960.0, 791320.0], "type": "choropleth"}], "name": "2010"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2011<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [1174205.0, 878555.0, 948270.0, 829645.0, 972360.0, 936590.0, 939145.0, 786210.0, 752630.0, 797525.0, 1016160.0, 913960.0, 1295385.0, 767595.0, 1011780.0, 982580.0, 1095730.0, 929290.0, 852640.0, 792050.0, 930750.0, 822710.0, 761025.0, 857020.0, 1033680.0, 1024190.0, 1116170.0, 1222385.0, 824535.0, 762850.0, 924910.0, 987690.0, 807745.0, 850815.0, 886220.0, 852275.0, 1095730.0, 856290.0, 833295.0, 869795.0, 1226400.0, 794970.0, 803000.0, 696055.0, 803000.0], "type": "choropleth"}], "name": "2011"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2012<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [1194280.0, 870160.0, 952650.0, 854830.0, 988055.0, 945350.0, 957030.0, 772340.0, 775260.0, 787670.0, 1014700.0, 928925.0, 1299765.0, 775260.0, 1020175.0, 967980.0, 1110695.0, 934765.0, 850815.0, 800080.0, 929655.0, 803000.0, 751170.0, 851545.0, 1043900.0, 1048645.0, 1100110.0, 1228590.0, 838770.0, 782925.0, 936955.0, 972725.0, 828550.0, 853370.0, 891695.0, 866510.0, 1100110.0, 839865.0, 836945.0, 881110.0, 1237350.0, 788400.0, 800080.0, 701895.0, 801905.0], "type": "choropleth"}], "name": "2012"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2013<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Algeria", "Angola", "Benin", "Botswana", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Djibouti", "Egypt", "Ethiopia", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Lesotho", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritania", "Mauritius", "Morocco", "Mozambique", "Namibia", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "South Africa", "Sudan", "Swaziland", "Togo", "Tunisia", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["DZA", "AGO", "BEN", "BWA", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "DJI", "EGY", "ETH", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LSO", "LBR", "MDG", "MWI", "MLI", "MRT", "MUS", "MAR", "MOZ", "NAM", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "ZAF", "SDN", "SWZ", "TGO", "TUN", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [1203040.0, 902645.0, 955935.0, 848990.0, 992800.0, 952285.0, 974915.0, 685835.0, 770150.0, 805920.0, 1021635.0, 951555.0, 1285530.0, 777815.0, 1032950.0, 959220.0, 1100840.0, 936590.0, 836580.0, 805190.0, 923085.0, 804460.0, 748980.0, 863955.0, 1054850.0, 1049740.0, 1118725.0, 1242095.0, 833295.0, 792415.0, 929655.0, 985500.0, 813220.0, 876000.0, 896440.0, 877460.0, 1103030.0, 852640.0, 850085.0, 895710.0, 1222385.0, 777450.0, 805920.0, 704450.0, 770150.0], "type": "choropleth"}], "name": "2013"}]);
                        }).then(function(){

var gd = document.getElementById('1c9b6fb4-369e-41d9-85c6-6ccdcdda2625');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>
```

##### Narrative:
Egypt, Morocco and Tunisia supplied the highest quantity of food items in Africa.

### Map showing the cassava produced in the African countries between 2004 to 2013


```python
cassava_prod = production_df.loc[production_df['Item'] == 'Cassava and products', ['Country','Year','Value [kt]']]
cassava_prod.head()
cassava_map = cassava_prod
cassava_map['Year'] = cassava_map['Year'].astype(str)
cassava_map = cassava_map.merge(country_code, on='Country', how='right')
cassava_map.head()
px.choropleth(cassava_map, locations='Code',hover_name='Country', title = 'Cassava Food Production in Africa (2004-2013)',
                   color='Value [kt]',labels={'Value [kt]':'Quantity'}, scope='africa',
                    color_continuous_scale=px.colors.sequential.Plasma, animation_frame='Year')
```

```python
<div>


            <div id="bf07a09d-fc37-4afa-b8c3-fcf75c6cb4b9" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};

                if (document.getElementById("bf07a09d-fc37-4afa-b8c3-fcf75c6cb4b9")) {
                    Plotly.newPlot(
                        'bf07a09d-fc37-4afa-b8c3-fcf75c6cb4b9',
                        [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2004<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Angola", "Benin", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritius", "Mozambique", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "Sudan", "Togo", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["AGO", "BEN", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LBR", "MDG", "MWI", "MLI", "MUS", "MOZ", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "SDN", "TGO", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "type": "choropleth", "z": [8587.0, 2955.0, 3.0, 4.0, 2093.0, 566.0, 328.0, 932.0, 2074.0, 228.0, 9.0, 9739.0, 969.0, 39.0, 643.0, 509.0, 1949.0, 2532.0, 13.0, 0.0, 6413.0, 106.0, 38845.0, 766.0, 6.0, 401.0, 1812.0, 11.0, 675.0, 5500.0, 4441.0, 957.0, 190.0]}],
                        {"coloraxis": {"colorbar": {"title": {"text": "Quantity"}}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "geo": {"center": {}, "domain": {"x": [0.0, 1.0], "y": [0.0, 1.0]}, "scope": "africa"}, "legend": {"tracegroupgap": 0}, "sliders": [{"active": 0, "currentvalue": {"prefix": "Year="}, "len": 0.9, "pad": {"b": 10, "t": 60}, "steps": [{"args": [["2004"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2004", "method": "animate"}, {"args": [["2005"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2005", "method": "animate"}, {"args": [["2006"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2006", "method": "animate"}, {"args": [["2007"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2007", "method": "animate"}, {"args": [["2008"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2008", "method": "animate"}, {"args": [["2009"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2009", "method": "animate"}, {"args": [["2010"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2010", "method": "animate"}, {"args": [["2011"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2011", "method": "animate"}, {"args": [["2012"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2012", "method": "animate"}, {"args": [["2013"], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "2013", "method": "animate"}], "x": 0.1, "xanchor": "left", "y": 0, "yanchor": "top"}], "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "title": {"text": "Cassava Food Production in Africa (2004-2013)"}, "updatemenus": [{"buttons": [{"args": [null, {"frame": {"duration": 500, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 500, "easing": "linear"}}], "label": "&#9654;", "method": "animate"}, {"args": [[null], {"frame": {"duration": 0, "redraw": true}, "fromcurrent": true, "mode": "immediate", "transition": {"duration": 0, "easing": "linear"}}], "label": "&#9724;", "method": "animate"}], "direction": "left", "pad": {"r": 10, "t": 70}, "showactive": false, "type": "buttons", "x": 0.1, "xanchor": "right", "y": 0, "yanchor": "top"}]},
                        {"responsive": true}
                    ).then(function(){
                            Plotly.addFrames('bf07a09d-fc37-4afa-b8c3-fcf75c6cb4b9', [{"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2004<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Angola", "Benin", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritius", "Mozambique", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "Sudan", "Togo", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["AGO", "BEN", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LBR", "MDG", "MWI", "MLI", "MUS", "MOZ", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "SDN", "TGO", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [8587.0, 2955.0, 3.0, 4.0, 2093.0, 566.0, 328.0, 932.0, 2074.0, 228.0, 9.0, 9739.0, 969.0, 39.0, 643.0, 509.0, 1949.0, 2532.0, 13.0, 0.0, 6413.0, 106.0, 38845.0, 766.0, 6.0, 401.0, 1812.0, 11.0, 675.0, 5500.0, 4441.0, 957.0, 190.0], "type": "choropleth"}], "name": "2004"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2005<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Angola", "Benin", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritius", "Mozambique", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "Sudan", "Togo", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["AGO", "BEN", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LBR", "MDG", "MWI", "MLI", "MUS", "MOZ", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "SDN", "TGO", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [8806.0, 2861.0, 3.0, 4.0, 2394.0, 578.0, 300.0, 1007.0, 2198.0, 229.0, 9.0, 9567.0, 1017.0, 41.0, 348.0, 533.0, 2964.0, 2198.0, 56.0, 0.0, 4782.0, 120.0, 41565.0, 782.0, 6.0, 281.0, 1121.0, 11.0, 678.0, 5576.0, 5539.0, 1056.0, 209.0], "type": "choropleth"}], "name": "2005"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2006<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Angola", "Benin", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritius", "Mozambique", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "Sudan", "Togo", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["AGO", "BEN", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LBR", "MDG", "MWI", "MLI", "MUS", "MOZ", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "SDN", "TGO", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [9037.0, 2524.0, 4.0, 4.0, 2652.0, 572.0, 280.0, 1072.0, 2267.0, 235.0, 10.0, 9638.0, 1069.0, 40.0, 657.0, 500.0, 2982.0, 2832.0, 96.0, 0.0, 5481.0, 138.0, 45721.0, 765.0, 5.0, 121.0, 1457.0, 10.0, 767.0, 4926.0, 6158.0, 1060.0, 212.0], "type": "choropleth"}], "name": "2006"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2007<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Angola", "Benin", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritius", "Mozambique", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "Sudan", "Togo", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["AGO", "BEN", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LBR", "MDG", "MWI", "MLI", "MUS", "MOZ", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "SDN", "TGO", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [9730.0, 3102.0, 5.0, 4.0, 2767.0, 598.0, 250.0, 1140.0, 2342.0, 240.0, 8.0, 10218.0, 1122.0, 43.0, 398.0, 530.0, 2994.0, 3239.0, 96.0, 0.0, 4959.0, 146.0, 43410.0, 779.0, 5.0, 308.0, 1894.0, 10.0, 773.0, 4973.0, 5199.0, 1060.0, 192.0], "type": "choropleth"}], "name": "2007"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2008<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Angola", "Benin", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritius", "Mozambique", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "Sudan", "Togo", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["AGO", "BEN", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LBR", "MDG", "MWI", "MLI", "MUS", "MOZ", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "SDN", "TGO", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [10057.0, 3145.0, 5.0, 4.0, 2883.0, 622.0, 161.0, 1196.0, 2531.0, 243.0, 8.0, 11351.0, 1052.0, 48.0, 751.0, 510.0, 3021.0, 3491.0, 72.0, 0.0, 4055.0, 110.0, 44582.0, 1682.0, 3.0, 921.0, 1989.0, 10.0, 795.0, 2876.0, 5392.0, 1186.0, 206.0], "type": "choropleth"}], "name": "2008"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2009<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Angola", "Benin", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritius", "Mozambique", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "Sudan", "Togo", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["AGO", "BEN", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LBR", "MDG", "MWI", "MLI", "MUS", "MOZ", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "SDN", "TGO", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [12828.0, 3788.0, 4.0, 4.0, 3341.0, 643.0, 230.0, 1231.0, 2262.0, 245.0, 7.0, 12231.0, 1051.0, 28.0, 820.0, 495.0, 3020.0, 3823.0, 80.0, 0.0, 5670.0, 108.0, 36822.0, 2020.0, 3.0, 266.0, 2815.0, 13.0, 896.0, 2952.0, 5916.0, 1161.0, 216.0], "type": "choropleth"}], "name": "2009"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2010<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Angola", "Benin", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritius", "Mozambique", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "Sudan", "Togo", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["AGO", "BEN", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LBR", "MDG", "MWI", "MLI", "MUS", "MOZ", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "SDN", "TGO", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [13859.0, 3445.0, 4.0, 4.0, 3808.0, 679.0, 231.0, 1149.0, 2307.0, 250.0, 8.0, 13504.0, 1062.0, 68.0, 323.0, 493.0, 3009.0, 4001.0, 38.0, 1.0, 9738.0, 113.0, 42533.0, 2377.0, 2.0, 181.0, 3250.0, 14.0, 909.0, 3017.0, 4548.0, 1152.0, 204.0], "type": "choropleth"}], "name": "2010"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2011<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Angola", "Benin", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritius", "Mozambique", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "Sudan", "Togo", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["AGO", "BEN", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LBR", "MDG", "MWI", "MLI", "MUS", "MOZ", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "SDN", "TGO", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [14334.0, 3646.0, 4.0, 7.0, 4083.0, 706.0, 230.0, 1150.0, 2359.0, 255.0, 10.0, 14241.0, 1113.0, 60.0, 679.0, 515.0, 3490.0, 4259.0, 46.0, 0.0, 10094.0, 98.0, 52403.0, 2579.0, 1.0, 155.0, 3460.0, 16.0, 999.0, 2712.0, 4647.0, 1087.0, 225.0], "type": "choropleth"}], "name": "2011"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2012<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Angola", "Benin", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritius", "Mozambique", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "Sudan", "Togo", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["AGO", "BEN", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LBR", "MDG", "MWI", "MLI", "MUS", "MOZ", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "SDN", "TGO", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [10636.0, 3296.0, 5.0, 7.0, 4287.0, 684.0, 240.0, 1200.0, 2412.0, 260.0, 11.0, 14547.0, 1165.0, 68.0, 893.0, 500.0, 3621.0, 4692.0, 40.0, 1.0, 10051.0, 107.0, 54000.0, 2716.0, 1.0, 189.0, 3585.0, 15.0, 960.0, 2806.0, 5462.0, 1062.0, 228.0], "type": "choropleth"}], "name": "2012"}, {"data": [{"coloraxis": "coloraxis", "geo": "geo", "hovertemplate": "<b>%{hovertext}</b><br><br>Year=2013<br>Code=%{location}<br>Quantity=%{z}<extra></extra>", "hovertext": ["Angola", "Benin", "Burkina Faso", "Cabo Verde", "Cameroon", "Central African Republic", "Chad", "Congo", "Cote d'Ivoire", "Gabon", "Gambia", "Ghana", "Guinea", "Guinea-Bissau", "Kenya", "Liberia", "Madagascar", "Malawi", "Mali", "Mauritius", "Mozambique", "Niger", "Nigeria", "Rwanda", "Sao Tome and Principe", "Senegal", "Sierra Leone", "Sudan", "Togo", "Uganda", "United Republic of Tanzania", "Zambia", "Zimbabwe"], "locations": ["AGO", "BEN", "BFA", "CPV", "CMR", "CAF", "TCD", "COD", "CIV", "GAB", "GMB", "GHA", "GIN", "GNB", "KEN", "LBR", "MDG", "MWI", "MLI", "MUS", "MOZ", "NER", "NGA", "RWA", "STP", "SEN", "SLE", "SDN", "TGO", "UGA", "TZA", "ZMB", "ZWE"], "name": "", "z": [16412.0, 3696.0, 4.0, 4.0, 4596.0, 580.0, 250.0, 1250.0, 2436.0, 265.0, 12.0, 15990.0, 1219.0, 23.0, 1112.0, 520.0, 3115.0, 4814.0, 38.0, 1.0, 10000.0, 156.0, 53000.0, 2948.0, 1.0, 146.0, 3810.0, 15.0, 903.0, 2979.0, 4755.0, 1070.0, 230.0], "type": "choropleth"}], "name": "2013"}]);
                        }).then(function(){

var gd = document.getElementById('bf07a09d-fc37-4afa-b8c3-fcf75c6cb4b9');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>
```

##### Narrative:
 Not all African countries produce Cassava although it is the food item with the
highest quantity of food production. Nigeria produces the highest quantity of cassava.

### Nigeria data


```python
nigeria_data = production_df.loc[production_df['Country'] == 'Nigeria', ['Country','Item','Year','Value [kt]']]
```


```python
nigerian_food = nigeria_data.groupby('Item').sum()
nigerian_food = nigerian_food.sort_values('Value [kt]', ascending=False).head(30)
sns.barplot(x=nigerian_food['Value [kt]'], y=nigerian_food.index, data=nigerian_food, palette='dark')
plt.ylabel('Items')
plt.xlabel('Quantity of food')
#plt.rcParams['figure.figsize'] = (20, 10)
plt.title('Top 30 Food Items in order of quantity produced in Nigeria (2004-2013)')
plt.legend()
plt.show()
```

    No handles with labels found to put in legend.


```python
![png](Food_Shortage-Africa_files/Food_Shortage-Africa_97_1.png)
```

###### Narrative:
Cassava, Yams and Beverages were the items produced most in Nigeria between 2004 to 2013.


```python
nigerian_food = nigeria_data.groupby('Item').sum()
nigerian_food = nigerian_food.sort_values('Value [kt]', ascending=False).tail(30)
sns.barplot(x=nigerian_food['Value [kt]'], y=nigerian_food.index, data=nigerian_food, palette='dark')
plt.ylabel('Items')
plt.xlabel('Quantity of food')
#plt.rcParams['figure.figsize'] = (20, 10)
plt.title('Bottom 30 Food Items in order of quantity produced in Nigeria (2004-2013)')
plt.legend()
plt.show()
```

    No handles with labels found to put in legend.


```python
![png](Food_Shortage-Africa_files/Food_Shortage-Africa_99_1.png)
```

##### Narrative:
Cephalopods, Molluscs and Soyabean oil were the least produced food items in Nigeria between 2004 to 2013.


```python
nigeria = production_df.loc[production_df['Country'] == 'Nigeria',['Country','Item','Year','Value [kt]']]
nigeria.head()
totalQ_nigeria = nigeria.groupby('Country',as_index=False).sum()
totalQ_nigeria
nigeriaS_df = supply_df.loc[supply_df['Country'] == 'Nigeria',['Country','Year','Value [kcal/(person day)]']]
nigeriaS_df =nigeriaS_df.groupby('Country', as_index=False).sum()
nigeria_data = totalQ_nigeria.merge(nigeriaS_df, on='Country')
nigeria_data
```



```python
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Value [kt]</th>
      <th>Value [kcal/(person day)]</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Nigeria</td>
      <td>1628030</td>
      <td>9850620</td>
    </tr>
  </tbody>
</table>
</div>
```



```python
#sns.barplot(x=nigeria_df.index, y=['Quantity produced','Quantity supplied'], data=nigeria_df, palette='dark')
nigeria_df.reset_index()
nigeria_df.plot(kind='bar', color=['green','red'])
#plt.label('Nigeria', rotation=0)
plt.ylabel('Quantity')
plt.rcParams['figure.figsize'] = (10, 5)
plt.title('Quantity of food produced and supplied in Nigeria')
plt.legend()
plt.show()
```

```python
![png](Food_Shortage-Africa_files/Food_Shortage-Africa_102_0.png)
```

##### Narrative:
The quantity supplied in Nigeria is much higher than the quantity produced between 2004 to 2013. This indicates that more food was imported to the country than what was produced between these years.


```python
percent_supplied =(nigeria_df['Quantity supplied'] - nigeria_df['Quantity produced'])/ nigeria_df['Quantity supplied']
```


```python
percent_supplied * 100
```



```python
    31    83.472817
    dtype: float64
```


### Egypt data


```python
egypt_df = production_df.loc[production_df['Country'] == 'Egypt',['Country','Item','Year','Value [kt]']]
egypt_df.head()
totalQ_egypt = egypt_df.groupby('Country',as_index=False).sum()
totalQ_egypt
egyptS_df = newsupply_df.loc[newsupply_df['Country'] == 'Egypt',['Country','Year','Value [kcal/(person day)]']]
egyptS_df =egyptS_df.groupby('Country', as_index=False).sum()
eypyt_Quantity = totalQ_egypt.merge(egyptS_df, on='Country')
eypyt_Quantity
```



```python
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Value [kt]</th>
      <th>Value [kcal/(person day)]</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Egypt</td>
      <td>877498</td>
      <td>12621700</td>
    </tr>
  </tbody>
</table>
</div>
```



```python
#sns.barplot(x=nigeria_df.index, y=['Quantity produced','Quantity supplied'], data=nigeria_df, palette='dark')
eypyt_Quantity.reset_index()
eypyt_Quantity.plot(kind='bar', color=['green','red'])
#plt.label('Nigeria', rotation=0)
plt.ylabel('Quantity')
#plt.rcParams['figure.figsize'] = (10, 5)
plt.title('Quantity of food produced and supplied in Egypt')
plt.legend()
plt.show()
```

```python
![png](Food_Shortage-Africa_files/Food_Shortage-Africa_108_0.png)
```

##### Narrative:
The quantity of food items supplied in Egypt between the years 2004 to 2013  is extremely higher than the quantity produced. This indicates that a lot of food that is consumed in Egypt were imported from other countries.

---

## Conclusion and Recommendation:

Majority of these African countries rely on imported food items from other countries in the world. There can be shortage od food because the food produced is extremely low compared to the food consumed by people in the African countries between the years 2004 to 2013.

###### Suggestion:
More food items need to be produced in African countries to sustain the population




```python

```
