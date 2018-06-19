
# Heroes of Pymoli Data Analysis

Trend 1: Males are the largest player demographic, constituting 81.15% of all Heroes of Pymoli players.
Trend 2: The game is most popular amongst players in the 20-24 age group, consituting 45.20% of all players.
Trend 3: The most popular items of the game are "Betrayal, Whisper of Grieving Widows" and "Arcade Gem."



```python
# Import dependencies
import pandas as pd
import numpy as np

# Store file path in variable and read file with Pandas
dataFile = "../HeroesOfPymoli/purchase_data.json"
PymoliDF = pd.read_json(dataFile)

```


```python
## Player Count
# Calculate total number of players

uniquePlayers = PymoliDF["SN"].nunique()
playerCountDF = pd.DataFrame({"Total Players": [uniquePlayers]})
playerCountDF
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
## Purchasing Analysis (Total) 
# Calculate Number of Unique Items, Average Purchase Price, Total Number of Purchases, Total Revenue and assign variables

uniqueItems = PymoliDF["Item Name"].nunique()
avgPrice = PymoliDF['Price'].mean()
totalNumPurchases = PymoliDF["Price"].count()
totalRevenue = PymoliDF["Price"].sum()

# Print data frame for purchase analysis with specified columns
purchaseAnalysisDF = pd.DataFrame({"Number of Unique Items": [uniqueItems], 
                                   "Average Price": [avgPrice], 
                                   "Number of Purchases": [totalNumPurchases], 
                                   "Total Revenue": [totalRevenue]})\
                                   .style.format({"Average Price": "${:,.2f}", 
                                                  "Total Revenue": "${:,.2f}"})
purchaseAnalysisDF

```




<style  type="text/css" >
</style>  
<table id="T_b10b7d52_73f2_11e8_8c28_88e9fe57aac0" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Price</th> 
        <th class="col_heading level0 col1" >Number of Purchases</th> 
        <th class="col_heading level0 col2" >Number of Unique Items</th> 
        <th class="col_heading level0 col3" >Total Revenue</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_b10b7d52_73f2_11e8_8c28_88e9fe57aac0level0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_b10b7d52_73f2_11e8_8c28_88e9fe57aac0row0_col0" class="data row0 col0" >$2.93</td> 
        <td id="T_b10b7d52_73f2_11e8_8c28_88e9fe57aac0row0_col1" class="data row0 col1" >780</td> 
        <td id="T_b10b7d52_73f2_11e8_8c28_88e9fe57aac0row0_col2" class="data row0 col2" >179</td> 
        <td id="T_b10b7d52_73f2_11e8_8c28_88e9fe57aac0row0_col3" class="data row0 col3" >$2,286.33</td> 
    </tr></tbody> 
</table> 




```python
## Gender Demographics
# Calculate percentage and count of Male, Female and Other/Non-Disclosed players

gCount = PymoliDF.groupby("Gender")["SN"].nunique()
gPercent = ((gCount / uniquePlayers)*100)

genderDemoDF = pd.DataFrame({"Percentage of Players": gPercent, "Total Count": gCount})\
                             .style.format({"Percentage of Players":"{0:.2f}%"})
genderDemoDF

```




<style  type="text/css" >
</style>  
<table id="T_3eacc834_73f3_11e8_a05f_88e9fe57aac0" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Percentage of Players</th> 
        <th class="col_heading level0 col1" >Total Count</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_3eacc834_73f3_11e8_a05f_88e9fe57aac0level0_row0" class="row_heading level0 row0" >Female</th> 
        <td id="T_3eacc834_73f3_11e8_a05f_88e9fe57aac0row0_col0" class="data row0 col0" >17.45%</td> 
        <td id="T_3eacc834_73f3_11e8_a05f_88e9fe57aac0row0_col1" class="data row0 col1" >100</td> 
    </tr>    <tr> 
        <th id="T_3eacc834_73f3_11e8_a05f_88e9fe57aac0level0_row1" class="row_heading level0 row1" >Male</th> 
        <td id="T_3eacc834_73f3_11e8_a05f_88e9fe57aac0row1_col0" class="data row1 col0" >81.15%</td> 
        <td id="T_3eacc834_73f3_11e8_a05f_88e9fe57aac0row1_col1" class="data row1 col1" >465</td> 
    </tr>    <tr> 
        <th id="T_3eacc834_73f3_11e8_a05f_88e9fe57aac0level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_3eacc834_73f3_11e8_a05f_88e9fe57aac0row2_col0" class="data row2 col0" >1.40%</td> 
        <td id="T_3eacc834_73f3_11e8_a05f_88e9fe57aac0row2_col1" class="data row2 col1" >8</td> 
    </tr></tbody> 
</table> 




```python
## Purchasing Analysis (Gender)
# Calculate purchase count, avg puchase price, total purchase value and normalized totals by gender

pCount = PymoliDF.groupby("Gender")["Price"].count()
avgPurchPrice = round(PymoliDF.groupby("Gender")["Price"].mean(), 2)
tPurchValue = round(PymoliDF.groupby("Gender")["Price"].sum(), 2)
normTotal = round(tPurchValue / gCount, 2)

gPurchAnalysisDF = pd.DataFrame({"Purchase Count": pCount, 
                                 "Average Purchase Price": avgPurchPrice, 
                                 "Total Purchase Value": tPurchValue,
                                 "Normalized Totals": normTotal}).style.format({"Average Purchase Price":"${:,.2f}", 
                                                                                "Total Purchase Value":"${:,.2f}"})
gPurchAnalysisDF

```




<style  type="text/css" >
</style>  
<table id="T_de020b34_73f2_11e8_ae4c_88e9fe57aac0" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Purchase Price</th> 
        <th class="col_heading level0 col1" >Normalized Totals</th> 
        <th class="col_heading level0 col2" >Purchase Count</th> 
        <th class="col_heading level0 col3" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_de020b34_73f2_11e8_ae4c_88e9fe57aac0level0_row0" class="row_heading level0 row0" >Female</th> 
        <td id="T_de020b34_73f2_11e8_ae4c_88e9fe57aac0row0_col0" class="data row0 col0" >$2.82</td> 
        <td id="T_de020b34_73f2_11e8_ae4c_88e9fe57aac0row0_col1" class="data row0 col1" >3.83</td> 
        <td id="T_de020b34_73f2_11e8_ae4c_88e9fe57aac0row0_col2" class="data row0 col2" >136</td> 
        <td id="T_de020b34_73f2_11e8_ae4c_88e9fe57aac0row0_col3" class="data row0 col3" >$382.91</td> 
    </tr>    <tr> 
        <th id="T_de020b34_73f2_11e8_ae4c_88e9fe57aac0level0_row1" class="row_heading level0 row1" >Male</th> 
        <td id="T_de020b34_73f2_11e8_ae4c_88e9fe57aac0row1_col0" class="data row1 col0" >$2.95</td> 
        <td id="T_de020b34_73f2_11e8_ae4c_88e9fe57aac0row1_col1" class="data row1 col1" >4.02</td> 
        <td id="T_de020b34_73f2_11e8_ae4c_88e9fe57aac0row1_col2" class="data row1 col2" >633</td> 
        <td id="T_de020b34_73f2_11e8_ae4c_88e9fe57aac0row1_col3" class="data row1 col3" >$1,867.68</td> 
    </tr>    <tr> 
        <th id="T_de020b34_73f2_11e8_ae4c_88e9fe57aac0level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_de020b34_73f2_11e8_ae4c_88e9fe57aac0row2_col0" class="data row2 col0" >$3.25</td> 
        <td id="T_de020b34_73f2_11e8_ae4c_88e9fe57aac0row2_col1" class="data row2 col1" >4.47</td> 
        <td id="T_de020b34_73f2_11e8_ae4c_88e9fe57aac0row2_col2" class="data row2 col2" >11</td> 
        <td id="T_de020b34_73f2_11e8_ae4c_88e9fe57aac0row2_col3" class="data row2 col3" >$35.74</td> 
    </tr></tbody> 
</table> 




```python
## Age Demographics
# Calculate percentage of players and total count

bins = [0, 9.9, 14.9, 19.9, 24.9, 29.9, 34.9, 39.9, 99] 
ageGroups = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40>"]

PymoliDF["Age Group"] = pd.cut(PymoliDF["Age"], bins, labels=ageGroups)

ageGroupCount = PymoliDF.groupby("Age Group")['SN'].nunique()
ageGroupPercent = round((ageGroupCount / uniquePlayers)*100, 2)

ageGroupDemoDF = pd.DataFrame({"Percentage of Players": ageGroupPercent, 
                               "Total Count": ageGroupCount})\
                               .style.format({"Percentage of Players":"{0:.2f}%"})

ageGroupDemoDF

```




<style  type="text/css" >
</style>  
<table id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Percentage of Players</th> 
        <th class="col_heading level0 col1" >Total Count</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Age Group</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0level0_row0" class="row_heading level0 row0" ><10</th> 
        <td id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0row0_col0" class="data row0 col0" >3.32%</td> 
        <td id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0row0_col1" class="data row0 col1" >19</td> 
    </tr>    <tr> 
        <th id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0level0_row1" class="row_heading level0 row1" >10-14</th> 
        <td id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0row1_col0" class="data row1 col0" >4.01%</td> 
        <td id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0row1_col1" class="data row1 col1" >23</td> 
    </tr>    <tr> 
        <th id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0level0_row2" class="row_heading level0 row2" >15-19</th> 
        <td id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0row2_col0" class="data row2 col0" >17.45%</td> 
        <td id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0row2_col1" class="data row2 col1" >100</td> 
    </tr>    <tr> 
        <th id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0level0_row3" class="row_heading level0 row3" >20-24</th> 
        <td id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0row3_col0" class="data row3 col0" >45.20%</td> 
        <td id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0row3_col1" class="data row3 col1" >259</td> 
    </tr>    <tr> 
        <th id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0level0_row4" class="row_heading level0 row4" >25-29</th> 
        <td id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0row4_col0" class="data row4 col0" >15.18%</td> 
        <td id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0row4_col1" class="data row4 col1" >87</td> 
    </tr>    <tr> 
        <th id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0level0_row5" class="row_heading level0 row5" >30-34</th> 
        <td id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0row5_col0" class="data row5 col0" >8.20%</td> 
        <td id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0row5_col1" class="data row5 col1" >47</td> 
    </tr>    <tr> 
        <th id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0level0_row6" class="row_heading level0 row6" >35-39</th> 
        <td id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0row6_col0" class="data row6 col0" >4.71%</td> 
        <td id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0row6_col1" class="data row6 col1" >27</td> 
    </tr>    <tr> 
        <th id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0level0_row7" class="row_heading level0 row7" >40></th> 
        <td id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0row7_col0" class="data row7 col0" >1.92%</td> 
        <td id="T_08afabdc_73f3_11e8_b7ff_88e9fe57aac0row7_col1" class="data row7 col1" >11</td> 
    </tr></tbody> 
</table> 




```python
## Age Demographics
# Calculate purchase count, average purchase price, total purchase value and normalized total 
# in bins of 4 years (i.e. &lt;10, 10-14, 15-19, etc.) 

purchaseCount = PymoliDF.groupby("Age Group")["Price"].count()
avgPurchPrice = PymoliDF.groupby("Age Group")["Price"].mean()
tPurchValue = PymoliDF.groupby("Age Group")["Price"].sum()
normTotal = round((tPurchValue / ageGroupCount), 2)

agePurchAnalysisDF = pd.DataFrame({"Purchase Count": purchaseCount,
                                   "Average Purchase Price": avgPurchPrice,
                                   "Total Purchase Value": tPurchValue, 
                                   "Normalized Total": normTotal}).style.format({"Average Purchase Price":"${:,.2f}", 
                                                                                 "Total Purchase Value":"${:,.2f}" })
agePurchAnalysisDF

```




<style  type="text/css" >
</style>  
<table id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Purchase Price</th> 
        <th class="col_heading level0 col1" >Normalized Total</th> 
        <th class="col_heading level0 col2" >Purchase Count</th> 
        <th class="col_heading level0 col3" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Age Group</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0level0_row0" class="row_heading level0 row0" ><10</th> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row0_col0" class="data row0 col0" >$2.98</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row0_col1" class="data row0 col1" >4.39</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row0_col2" class="data row0 col2" >28</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row0_col3" class="data row0 col3" >$83.46</td> 
    </tr>    <tr> 
        <th id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0level0_row1" class="row_heading level0 row1" >10-14</th> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row1_col0" class="data row1 col0" >$2.77</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row1_col1" class="data row1 col1" >4.22</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row1_col2" class="data row1 col2" >35</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row1_col3" class="data row1 col3" >$96.95</td> 
    </tr>    <tr> 
        <th id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0level0_row2" class="row_heading level0 row2" >15-19</th> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row2_col0" class="data row2 col0" >$2.91</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row2_col1" class="data row2 col1" >3.86</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row2_col2" class="data row2 col2" >133</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row2_col3" class="data row2 col3" >$386.42</td> 
    </tr>    <tr> 
        <th id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0level0_row3" class="row_heading level0 row3" >20-24</th> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row3_col0" class="data row3 col0" >$2.91</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row3_col1" class="data row3 col1" >3.78</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row3_col2" class="data row3 col2" >336</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row3_col3" class="data row3 col3" >$978.77</td> 
    </tr>    <tr> 
        <th id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0level0_row4" class="row_heading level0 row4" >25-29</th> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row4_col0" class="data row4 col0" >$2.96</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row4_col1" class="data row4 col1" >4.26</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row4_col2" class="data row4 col2" >125</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row4_col3" class="data row4 col3" >$370.33</td> 
    </tr>    <tr> 
        <th id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0level0_row5" class="row_heading level0 row5" >30-34</th> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row5_col0" class="data row5 col0" >$3.08</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row5_col1" class="data row5 col1" >4.2</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row5_col2" class="data row5 col2" >64</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row5_col3" class="data row5 col3" >$197.25</td> 
    </tr>    <tr> 
        <th id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0level0_row6" class="row_heading level0 row6" >35-39</th> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row6_col0" class="data row6 col0" >$2.84</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row6_col1" class="data row6 col1" >4.42</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row6_col2" class="data row6 col2" >42</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row6_col3" class="data row6 col3" >$119.40</td> 
    </tr>    <tr> 
        <th id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0level0_row7" class="row_heading level0 row7" >40></th> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row7_col0" class="data row7 col0" >$3.16</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row7_col1" class="data row7 col1" >4.89</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row7_col2" class="data row7 col2" >17</td> 
        <td id="T_75b0e3d8_73f3_11e8_85b2_88e9fe57aac0row7_col3" class="data row7 col3" >$53.75</td> 
    </tr></tbody> 
</table> 




```python
## Top Spenders
# Identify top 5 spenders by total purchase value: include SN, Purchase Count, Avg Purchase Price, Total Purchase Price
 
tPurchValue = PymoliDF.groupby("SN")["Price"].sum()
purchCount = PymoliDF.groupby("SN")["Price"].count()
avgPurchPrice = PymoliDF.groupby("SN")["Price"].mean()

topSpendersDF = pd.DataFrame({"Total Purchase Price": tPurchValue, 
                             "Purchase Count": purchCount, 
                             "Average Purchase Price": avgPurchPrice})

topSpendersDF.nlargest(5, "Total Purchase Price").style.format({"Total Purchase Price":"${:,.2f}", 
                                                               "Average Purchase Price":"${:,.2f}"})


```




<style  type="text/css" >
</style>  
<table id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Purchase Price</th> 
        <th class="col_heading level0 col1" >Purchase Count</th> 
        <th class="col_heading level0 col2" >Total Purchase Price</th> 
    </tr>    <tr> 
        <th class="index_name level0" >SN</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0level0_row0" class="row_heading level0 row0" >Undirrala66</th> 
        <td id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0row0_col0" class="data row0 col0" >$3.41</td> 
        <td id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0row0_col1" class="data row0 col1" >5</td> 
        <td id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0row0_col2" class="data row0 col2" >$17.06</td> 
    </tr>    <tr> 
        <th id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0level0_row1" class="row_heading level0 row1" >Saedue76</th> 
        <td id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0row1_col0" class="data row1 col0" >$3.39</td> 
        <td id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0row1_col1" class="data row1 col1" >4</td> 
        <td id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0row1_col2" class="data row1 col2" >$13.56</td> 
    </tr>    <tr> 
        <th id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0level0_row2" class="row_heading level0 row2" >Mindimnya67</th> 
        <td id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0row2_col0" class="data row2 col0" >$3.18</td> 
        <td id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0row2_col1" class="data row2 col1" >4</td> 
        <td id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0row2_col2" class="data row2 col2" >$12.74</td> 
    </tr>    <tr> 
        <th id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0level0_row3" class="row_heading level0 row3" >Haellysu29</th> 
        <td id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0row3_col0" class="data row3 col0" >$4.24</td> 
        <td id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0row3_col1" class="data row3 col1" >3</td> 
        <td id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0row3_col2" class="data row3 col2" >$12.73</td> 
    </tr>    <tr> 
        <th id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0level0_row4" class="row_heading level0 row4" >Eoda93</th> 
        <td id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0row4_col0" class="data row4 col0" >$3.86</td> 
        <td id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0row4_col1" class="data row4 col1" >3</td> 
        <td id="T_7fe1f264_73f3_11e8_816f_88e9fe57aac0row4_col2" class="data row4 col2" >$11.58</td> 
    </tr></tbody> 
</table> 




```python
## Most Popular Items
# Identify 5 top items by purchase count: include Item ID, Item Name, Purchase Count, Item Price, Total Purchase Value
 
purchCount = PymoliDF.groupby(["Item ID", "Item Name"])["Price"].count()
tPurchValue = PymoliDF.groupby(["Item ID", "Item Name"])["Price"].sum()
itemPrice = PymoliDF.groupby(["Item ID", "Item Name"])["Price"].mean()


mostPopItemsDF = pd.DataFrame({"Purchase Count": purchCount,
                             "Total Purchase Value": tPurchValue, 
                             "Item Price": itemPrice}).nlargest(5, "Purchase Count")\
                            .style.format({"Total Purchase Value":"${:,.2f}", "Item Price":"${:,.2f}"})
mostPopItemsDF

```




<style  type="text/css" >
</style>  
<table id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Item Price</th> 
        <th class="col_heading level0 col1" >Purchase Count</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="index_name level1" >Item Name</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0level0_row0" class="row_heading level0 row0" >39</th> 
        <th id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0level1_row0" class="row_heading level1 row0" >Betrayal, Whisper of Grieving Widows</th> 
        <td id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0row0_col0" class="data row0 col0" >$2.35</td> 
        <td id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0row0_col1" class="data row0 col1" >11</td> 
        <td id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0row0_col2" class="data row0 col2" >$25.85</td> 
    </tr>    <tr> 
        <th id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0level0_row1" class="row_heading level0 row1" >84</th> 
        <th id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0level1_row1" class="row_heading level1 row1" >Arcane Gem</th> 
        <td id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0row1_col0" class="data row1 col0" >$2.23</td> 
        <td id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0row1_col1" class="data row1 col1" >11</td> 
        <td id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0row1_col2" class="data row1 col2" >$24.53</td> 
    </tr>    <tr> 
        <th id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0level0_row2" class="row_heading level0 row2" >13</th> 
        <th id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0level1_row2" class="row_heading level1 row2" >Serenity</th> 
        <td id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0row2_col0" class="data row2 col0" >$1.49</td> 
        <td id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0row2_col1" class="data row2 col1" >9</td> 
        <td id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0row2_col2" class="data row2 col2" >$13.41</td> 
    </tr>    <tr> 
        <th id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0level0_row3" class="row_heading level0 row3" >31</th> 
        <th id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0level1_row3" class="row_heading level1 row3" >Trickster</th> 
        <td id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0row3_col0" class="data row3 col0" >$2.07</td> 
        <td id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0row3_col1" class="data row3 col1" >9</td> 
        <td id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0row3_col2" class="data row3 col2" >$18.63</td> 
    </tr>    <tr> 
        <th id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0level0_row4" class="row_heading level0 row4" >34</th> 
        <th id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0level1_row4" class="row_heading level1 row4" >Retribution Axe</th> 
        <td id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0row4_col0" class="data row4 col0" >$4.14</td> 
        <td id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0row4_col1" class="data row4 col1" >9</td> 
        <td id="T_92e6ae7a_73f3_11e8_badb_88e9fe57aac0row4_col2" class="data row4 col2" >$37.26</td> 
    </tr></tbody> 
</table> 




```python
## Most Profitable Items
# Identify 5 most profitable items by total purchase value: 
# include Item ID, Item Name, Purchase Count, Item Price, Total Purchase Value
 
mostProfitItemsDF = pd.DataFrame({"Purchase Count": purchCount, 
                               "Total Purchase Value": tPurchValue, 
                               "Item Price": itemPrice}).nlargest(5, "Total Purchase Value")\
                               .style.format({"Total Purchase Value":"${:,.2f}", "Item Price":"${:,.2f}"})
mostProfitItemsDF

    
```




<style  type="text/css" >
</style>  
<table id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Item Price</th> 
        <th class="col_heading level0 col1" >Purchase Count</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="index_name level1" >Item Name</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0level0_row0" class="row_heading level0 row0" >34</th> 
        <th id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0level1_row0" class="row_heading level1 row0" >Retribution Axe</th> 
        <td id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0row0_col0" class="data row0 col0" >$4.14</td> 
        <td id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0row0_col1" class="data row0 col1" >9</td> 
        <td id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0row0_col2" class="data row0 col2" >$37.26</td> 
    </tr>    <tr> 
        <th id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0level0_row1" class="row_heading level0 row1" >115</th> 
        <th id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0level1_row1" class="row_heading level1 row1" >Spectral Diamond Doomblade</th> 
        <td id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0row1_col0" class="data row1 col0" >$4.25</td> 
        <td id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0row1_col1" class="data row1 col1" >7</td> 
        <td id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0row1_col2" class="data row1 col2" >$29.75</td> 
    </tr>    <tr> 
        <th id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0level0_row2" class="row_heading level0 row2" >32</th> 
        <th id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0level1_row2" class="row_heading level1 row2" >Orenmir</th> 
        <td id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0row2_col0" class="data row2 col0" >$4.95</td> 
        <td id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0row2_col1" class="data row2 col1" >6</td> 
        <td id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0row2_col2" class="data row2 col2" >$29.70</td> 
    </tr>    <tr> 
        <th id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0level0_row3" class="row_heading level0 row3" >103</th> 
        <th id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0level1_row3" class="row_heading level1 row3" >Singed Scalpel</th> 
        <td id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0row3_col0" class="data row3 col0" >$4.87</td> 
        <td id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0row3_col1" class="data row3 col1" >6</td> 
        <td id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0row3_col2" class="data row3 col2" >$29.22</td> 
    </tr>    <tr> 
        <th id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0level0_row4" class="row_heading level0 row4" >107</th> 
        <th id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0level1_row4" class="row_heading level1 row4" >Splitter, Foe Of Subtlety</th> 
        <td id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0row4_col0" class="data row4 col0" >$3.61</td> 
        <td id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0row4_col1" class="data row4 col1" >8</td> 
        <td id="T_9e1a8570_73f3_11e8_ba80_88e9fe57aac0row4_col2" class="data row4 col2" >$28.88</td> 
    </tr></tbody> 
</table> 


