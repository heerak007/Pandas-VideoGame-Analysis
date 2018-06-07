
### Heroes Of Pymoli Data Analysis
* Of the 1163 active players, the vast majority are male (84%). There also exists, a smaller, but notable proportion of female players (14%).

* Our peak age demographic falls between 20-24 (44.8%) with secondary groups falling between 15-19 (18.60%) and 25-29 (13.4%).  
-----

### Note
* Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.


```python
# Dependencies and Setup
import pandas as pd
import numpy as np

# Raw data file
file_to_load = "Resources/purchase_data.csv"

# Read purchasing file and store into pandas data frame
GameDF = pd.read_csv(file_to_load)

print(f'{GameDF.count()}\n')
print(GameDF.dtypes)

GameDF.head()
```

    Purchase ID    780
    SN             780
    Age            780
    Gender         780
    Item ID        780
    Item Name      780
    Price          780
    dtype: int64
    
    Purchase ID      int64
    SN              object
    Age              int64
    Gender          object
    Item ID          int64
    Item Name       object
    Price          float64
    dtype: object





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
      <th>Purchase ID</th>
      <th>SN</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Lisim78</td>
      <td>20</td>
      <td>Male</td>
      <td>108</td>
      <td>Extraction, Quickblade Of Trembling Hands</td>
      <td>3.53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Lisovynya38</td>
      <td>40</td>
      <td>Male</td>
      <td>143</td>
      <td>Frenzied Scimitar</td>
      <td>1.56</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Ithergue48</td>
      <td>24</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>4.88</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Chamassasya86</td>
      <td>24</td>
      <td>Male</td>
      <td>100</td>
      <td>Blindscythe</td>
      <td>3.27</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Iskosia90</td>
      <td>23</td>
      <td>Male</td>
      <td>131</td>
      <td>Fury</td>
      <td>1.44</td>
    </tr>
  </tbody>
</table>
</div>



## Player Count

* Display the total number of players



```python
TotalPlayers = GameDF["SN"].unique()
len(TotalPlayers)
```




    576



## Purchasing Analysis (Total)

* Run basic calculations to obtain number of unique items, average price, etc.


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame



```python
Unique = GameDF["Item ID"].unique()
Average = GameDF["Price"].mean()
NumofPur = len(GameDF["Purchase ID"])
TotalRev = GameDF["Price"].sum()

SumTotal = pd.DataFrame([{
            'Unique Items':len(Unique), 
            'Avg Price':Average, 
            "Number of Purchases":NumofPur, 
            "Total Revenue":TotalRev
            }])

# SumTotal.style.format({
#             'Avg price': "${:,.2f}",  
#             'Total Revenue': '${:,.2f}'
#             })

SumTotal["Avg Price"] = SumTotal["Avg Price"].map("${:.2f}".format)
SumTotal["Total Revenue"] = SumTotal["Total Revenue"].map("${:.2f}".format)

SumTotal = SumTotal[["Unique Items","Avg Price","Number of Purchases","Total Revenue"]]

SumTotal
```




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
      <th>Unique Items</th>
      <th>Avg Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$3.05</td>
      <td>780</td>
      <td>$2379.77</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics

* Run basic calculations to obtain number of unique items, average price, etc.


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame



```python
GenderVCount = GameDF["Gender"].value_counts()
GenderPercent = GenderVCount/GenderVCount.sum()

GenderDemo = pd.DataFrame({"Count": GenderVCount,
                        "Percentage": GenderPercent}) 
GenderDemo["Percentage"] = GenderDemo["Percentage"].map('{:.1%}'.format)

GenderDemo
```




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
      <th>Count</th>
      <th>Percentage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>652</td>
      <td>83.6%</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>113</td>
      <td>14.5%</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>15</td>
      <td>1.9%</td>
    </tr>
  </tbody>
</table>
</div>




## Purchasing Analysis (Gender)

* Run basic calculations to obtain purchase count, avg. purchase price, etc. by gender


* For normalized purchasing, divide total purchase value by purchase count, by gender


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
GenderGroup = GameDF.groupby(["Gender"])
GenTotalPurchase = GenderGroup["Price"].sum()
GenAvgPurchase = GenderGroup["Price"].mean()
GenNormalize = GenTotalPurchase/GenderVCount

GenderAnalysis = pd.DataFrame({ "Purchase Count": GenderVCount,
                                "Average Purchase": GenAvgPurchase,
                                "Total Purchase": GenTotalPurchase,
                                "Normalized Value": GenNormalize})

GenderAnalysis["Average Purchase"] = GenderAnalysis["Average Purchase"].map('${:,.2f}'.format)
GenderAnalysis["Total Purchase"] = GenderAnalysis["Total Purchase"].map('${:,.2f}'.format)
GenderAnalysis["Normalized Value"] = GenderAnalysis["Normalized Value"].map('${:,.2f}'.format)

GenderAnalysis = GenderAnalysis[["Purchase Count","Average Purchase","Total Purchase","Normalized Value"]]


GenderAnalysis

```




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
      <th>Purchase Count</th>
      <th>Average Purchase</th>
      <th>Total Purchase</th>
      <th>Normalized Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>113</td>
      <td>$3.20</td>
      <td>$361.94</td>
      <td>$3.20</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>652</td>
      <td>$3.02</td>
      <td>$1,967.64</td>
      <td>$3.02</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>15</td>
      <td>$3.35</td>
      <td>$50.19</td>
      <td>$3.35</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographics

* Establish bins for ages


* Categorize the existing players using the age bins. Hint: use pd.cut()


* Calculate the numbers and percentages by age group


* Create a summary data frame to hold the results


* Optional: round the percentage column to two decimal points


* Display Age Demographics Table



```python
# Establish bins for ages
age_bins = [0, 9.90, 14.90, 19.90, 24.90, 29.90, 34.90, 39.90, 99999]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

GameDF["Age Range"] = pd.cut(GameDF["Age"], age_bins, labels=group_names)

AgeVCount = GameDF["Age Range"].value_counts()
AgePercent = AgeVCount/AgeVCount.sum()

AgeDemo = pd.DataFrame({"Count": AgeVCount,
                        "Percentage": AgePercent}) 

AgeDemo["Percentage"] = AgeDemo["Percentage"].map('{:.1%}'.format)

AgeDemo
```




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
      <th>Count</th>
      <th>Percentage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>20-24</th>
      <td>365</td>
      <td>46.8%</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>136</td>
      <td>17.4%</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>101</td>
      <td>12.9%</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>73</td>
      <td>9.4%</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>41</td>
      <td>5.3%</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>28</td>
      <td>3.6%</td>
    </tr>
    <tr>
      <th>&lt;10</th>
      <td>23</td>
      <td>2.9%</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>13</td>
      <td>1.7%</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)

* Bin the purchase_data data frame by age


* Run basic calculations to obtain purchase count, avg. purchase price, etc. in the table below


* Calculate Normalized Purchasing


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
AgeGroup = GameDF.groupby(["Age Range"])
AgeTotalPurchase = AgeGroup["Price"].sum()
AgeAvgPurchase = AgeGroup["Price"].mean()
AgeNormalize = AgeTotalPurchase/AgeVCount

AgeAnalysis = pd.DataFrame({ "Purchase Count": AgeVCount,
                                "Average Purchase": AgeAvgPurchase,
                                "Total Purchase": AgeTotalPurchase,
                                "Normalized Value": AgeNormalize})

AgeAnalysis["Average Purchase"] = AgeAnalysis["Average Purchase"].map('${:,.2f}'.format)
AgeAnalysis["Total Purchase"] = AgeAnalysis["Total Purchase"].map('${:,.2f}'.format)
AgeAnalysis["Normalized Value"] = AgeAnalysis["Normalized Value"].map('${:,.2f}'.format)

AgeAnalysis = AgeAnalysis[["Purchase Count","Average Purchase","Total Purchase","Normalized Value"]]
AgeAnalysis
```




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
      <th>Purchase Count</th>
      <th>Average Purchase</th>
      <th>Total Purchase</th>
      <th>Normalized Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10-14</th>
      <td>28</td>
      <td>$2.96</td>
      <td>$82.78</td>
      <td>$2.96</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>136</td>
      <td>$3.04</td>
      <td>$412.89</td>
      <td>$3.04</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>365</td>
      <td>$3.05</td>
      <td>$1,114.06</td>
      <td>$3.05</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>101</td>
      <td>$2.90</td>
      <td>$293.00</td>
      <td>$2.90</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>73</td>
      <td>$2.93</td>
      <td>$214.00</td>
      <td>$2.93</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>41</td>
      <td>$3.60</td>
      <td>$147.67</td>
      <td>$3.60</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>13</td>
      <td>$2.94</td>
      <td>$38.24</td>
      <td>$2.94</td>
    </tr>
    <tr>
      <th>&lt;10</th>
      <td>23</td>
      <td>$3.35</td>
      <td>$77.13</td>
      <td>$3.35</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders

* Run basic calculations to obtain the results in the table below


* Create a summary data frame to hold the results


* Sort the total purchase value column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
TopSpenders = GameDF.groupby(["SN"])
TopCount = TopSpenders["SN"].count()
TopTotalPurchase = TopSpenders["Price"].sum()
TopAvgPurchase = TopSpenders["Price"].mean()

TopTable = pd.DataFrame({   "Purchase Count": TopCount,
                            "Average Purchase": TopAvgPurchase,
                            "Total Purchase": TopTotalPurchase
                            })

TopTable = TopTable.sort_values("Total Purchase", ascending=False)

TopTable["Average Purchase"] = TopTable["Average Purchase"].map('${:,.2f}'.format)
TopTable["Total Purchase"] = TopTable["Total Purchase"].map('${:,.2f}'.format)

TopTable = TopTable[["Purchase Count","Average Purchase","Total Purchase"]]

TopTable.head(10)
```




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
      <th>Purchase Count</th>
      <th>Average Purchase</th>
      <th>Total Purchase</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Lisosia93</th>
      <td>5</td>
      <td>$3.79</td>
      <td>$18.96</td>
    </tr>
    <tr>
      <th>Idastidru52</th>
      <td>4</td>
      <td>$3.86</td>
      <td>$15.45</td>
    </tr>
    <tr>
      <th>Chamjask73</th>
      <td>3</td>
      <td>$4.61</td>
      <td>$13.83</td>
    </tr>
    <tr>
      <th>Iral74</th>
      <td>4</td>
      <td>$3.40</td>
      <td>$13.62</td>
    </tr>
    <tr>
      <th>Iskadarya95</th>
      <td>3</td>
      <td>$4.37</td>
      <td>$13.10</td>
    </tr>
    <tr>
      <th>Ilarin91</th>
      <td>3</td>
      <td>$4.23</td>
      <td>$12.70</td>
    </tr>
    <tr>
      <th>Ialallo29</th>
      <td>3</td>
      <td>$3.95</td>
      <td>$11.84</td>
    </tr>
    <tr>
      <th>Tyidaim51</th>
      <td>3</td>
      <td>$3.94</td>
      <td>$11.83</td>
    </tr>
    <tr>
      <th>Lassilsala30</th>
      <td>3</td>
      <td>$3.84</td>
      <td>$11.51</td>
    </tr>
    <tr>
      <th>Chadolyla44</th>
      <td>3</td>
      <td>$3.82</td>
      <td>$11.46</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items

* Retrieve the Item ID, Item Name, and Item Price columns


* Group by Item ID and Item Name. Perform calculations to obtain purchase count, item price, and total purchase value


* Create a summary data frame to hold the results


* Sort the purchase count column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
TopItems = GameDF.groupby(["Item ID","Item Name"])
TopItemCount = TopItems["Item ID"].count()
ItemTotalPurchase = TopItems["Price"].sum()
ItemAvgPurchase = TopItems["Price"].mean()

TopItemTable = pd.DataFrame({   "Purchase Count": TopItemCount,
                            "Average Purchase": ItemAvgPurchase,
                            "Total Purchase": ItemTotalPurchase
                            })

TopItemDF = TopItemTable.sort_values("Purchase Count", ascending=False)

TopItemDF["Average Purchase"] = TopItemDF["Average Purchase"].map('${:,.2f}'.format)
TopItemDF["Total Purchase"] = TopItemDF["Total Purchase"].map('${:,.2f}'.format)

TopItemDF = TopItemDF[["Purchase Count","Average Purchase","Total Purchase"]]

TopItemDF.head()
```




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
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase</th>
      <th>Total Purchase</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <td>9</td>
      <td>$3.53</td>
      <td>$31.77</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>19</th>
      <th>Pursuit, Cudgel of Necromancy</th>
      <td>8</td>
      <td>$1.02</td>
      <td>$8.16</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items

* Sort the above table by total purchase value in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the data frame




```python
TopProfitTable = TopItemTable.sort_values("Total Purchase", ascending=False)

TopProfitTable["Average Purchase"] = TopProfitTable["Average Purchase"].map('${:,.2f}'.format)
TopProfitTable["Total Purchase"] = TopProfitTable["Total Purchase"].map('${:,.2f}'.format)

TopProfitTable = TopProfitTable[["Purchase Count","Average Purchase","Total Purchase"]]

TopProfitTable.head()
```




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
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase</th>
      <th>Total Purchase</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>92</th>
      <th>Final Critic</th>
      <td>8</td>
      <td>$4.88</td>
      <td>$39.04</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>8</td>
      <td>$4.35</td>
      <td>$34.80</td>
    </tr>
  </tbody>
</table>
</div>


