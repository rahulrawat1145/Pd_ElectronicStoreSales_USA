# Python-Pandas Electronic Store Sales Analysis

## About the data set: 

The data set "sales_data" is a collection of yearly sales data of an electronic store in USA. The dataset has 12 seperate files, which may be combined to form one dataset for our analysis. The data set "sales_data" is available to download in the repository.  

## Objective: 
To analyze sales data of an electronic store by using python pandas and matplotlib to answer the following three question:
- Q1. What was the best month for sales? How much was earned in that month?
- Q2 What US city has highest number of sales?
- Q3 At what time should the advertisements be displayed to maximize the likelyhood of customers buying the product ?

Initially, we focus on refining our data. This involves tasks such as eliminating NaN values from the DataFrame, filtering out rows based on specific conditions, and adjusting column types using methods like to_numeric, to_datetime, and astype.
Once our data is cleaned, we dive into the exploration phase. To address these inquiries, we employ various Pandas and Matplotlib techniques, including concatenating multiple CSV files to form a consolidated DataFrame using pd.concat, adding new columns, parsing string cells to generate new data, employing the .apply() method, conducting aggregate analysis using groupby, and visualizing outcomes through bar charts and line graphs. Additionally, we ensure clarity by labeling our graphs appropriately.

## Results and discussion

Cleaning the data:
```
all_data['Quantity Ordered'] = all_data['Quantity Ordered'].astype('int')
all_data['Price Each'] = all_data['Price Each'].astype('float')

all_data['Sales'] = all_data['Quantity Ordered'] * all_data['Price Each']
all_data.head()
```
Adding a city column using apply() method:
```
# Using .apply method
# all_data['City'] = all_data['Purchase Address'].apply(lambda x: x.split(',')[1])

def get_city(address):
    return address.split(',')[1]
    
def get_state(address):
    return address.split(',')[2]. split(' ')[1]
    
# all_data['City'] = all_data['Purchase Address'].apply(lambda x: get_city(x) + ' (' + get_state(x) + ')')

all_data['City'] = all_data['Purchase Address'].apply(lambda x: f"{get_city(x)} ({get_state(x)})")

all_data.head()
```
The three business questions and thier answers in code and as graphs are are under:

- 1. Identifying the most profitable month and determining the revenue generated during that period.

```
import matplotlib.pyplot as plt

results = all_data.groupby('Month').sum()
months = range(1,13)

plt.bar(months, results['Sales'])
plt.xticks(months)
plt.ylabel('Sales in USD (Millions)')
plt.xlabel('Month number')

plt.show()

```
![ans1](https://github.com/rrawat1145/Pd_ElectronicStoreSales_USA/blob/main/Screenshot/1.png?raw=true)

- 2. Identifying the city with the highest sales volume.
```
cities = [city for city, df in all_data.groupby('City')]

plt.bar(cities, results)
plt.xticks(cities, rotation = 'vertical', fontsize =8)
plt.ylabel('Sales in USD (Millions)') 
plt.xlabel('City Names')

plt.show()

```
![ans2](https://github.com/rrawat1145/Pd_ElectronicStoreSales_USA/blob/main/Screenshot/2.png?raw=true)

- 3. Determining the optimal time to display advertisements to enhance the probability of customer purchases.

```
hours = [hour for hour, df in all_data.groupby('Hour')]

plt.plot(hours, all_data.groupby(['Hour']).count())
plt.xticks(hours)
plt.xlabel('Hours')
plt.ylabel('Number of Orders')
plt.grid()

plt.show()

```
![ans3](https://github.com/rrawat1145/Pd_ElectronicStoreSales_USA/blob/main/Screenshot/3.png?raw=true)
