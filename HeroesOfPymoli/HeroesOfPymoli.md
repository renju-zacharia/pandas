
### Heroes Of Pymoli Data Analysis
* Of the 1163 active players, the vast majority are male (84%). There also exists, a smaller, but notable proportion of female players (14%).

* Our peak age demographic falls between 20-24 (44.8%) with secondary groups falling between 15-19 (18.60%) and 25-29 (13.4%).  
-----

1. 84% of purchasers are Males, but Females have a higher average price.

2. Majority of players are between 20 & 24. 

3. Most popular Item and Most profitable item are the same - "Oathbreaker, Last Hope of the Breaking Storm"

4. The average of the mean prices for the age groups would be $3.096.
   with a Standard Deviation of $0.25 and Standard Error of $0.09.
   The mean and median are very close - $3.1 and $3.0 respectively.
   95% of the values lie within a range of +-$0.21. 

### Note
* Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.


```python
# Dependencies and Setup
import pandas as pd
import numpy as np
import os

# File to Load (Remember to Change These)

file_to_load = os.path.join("Resources", "purchase_data.csv")

# Read Purchasing File and store into Pandas data frame
purchase_data = pd.read_csv(file_to_load, encoding = "UTF-8")
purchase_data.head()
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
num_players = len(purchase_data["SN"].unique())
num_players_df = pd.DataFrame( 
                                 { "Total Players" : [ num_players ] }
                             )
num_players_df
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
      <td>576</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Total)

* Run basic calculations to obtain number of unique items, average price, etc.


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame



```python
num_items = len(purchase_data["Item ID"].unique())
num_pur = purchase_data["Item ID"].count()
tot_rev = purchase_data["Price"].sum()
avg_price = tot_rev / num_pur

pur_df = pd.DataFrame( { 
                         "Number of Unique Items" : [num_items], 
                         "Average Price"          : [avg_price], 
                         "Number of Purchases"    : [num_pur], 
                         "Total Revenue"          : [tot_rev]   
                       }
                     ) 

pur_df["Average Price"] = pur_df["Average Price"].astype(float).map("${:,.2f}".format)
pur_df["Total Revenue"] = pur_df["Total Revenue"].astype(float).map("${:,.2f}".format)
pur_df
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
      <th>Number of Unique Items</th>
      <th>Average Price</th>
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
      <td>$2,379.77</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics

* Percentage and Count of Male Players


* Percentage and Count of Female Players


* Percentage and Count of Other / Non-Disclosed





```python
num_players = len(purchase_data["SN"].unique())

males_df = pd.DataFrame( purchase_data.loc [ ( purchase_data["Gender"] == "Male" ), : ] )
num_males = len(males_df["SN"].unique())

females_df = pd.DataFrame( purchase_data.loc [ ( purchase_data["Gender"] == "Female" ), : ] )
num_females = len(females_df["SN"].unique())

males_pct = num_males * 100 / num_players
females_pct = num_females * 100 / num_players

num_others = num_players - (  num_males + num_females )
others_pct = num_others * 100 / num_players

list_gender = ["Male" , "Female" , "Other / Non-Disclosed" ]

mydf = pd.DataFrame ( { 
                        "Gender" : list_gender , 
                        "Total Count" : [num_males, num_females, num_others ]  ,
                        "Percentage of Players" : [ males_pct, females_pct, others_pct ]
                      }
                     )
mydf["Percentage of Players"] = mydf["Percentage of Players"].astype(float).map( "%{:,.2f}".format )

mydf = mydf.set_index( "Gender" )

mydf
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
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>484</td>
      <td>%84.03</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>81</td>
      <td>%14.06</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>%1.91</td>
    </tr>
  </tbody>
</table>
</div>




## Purchasing Analysis (Gender)

* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. by gender




* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
m_pur_count = males_df["SN"].count()
f_pur_count = females_df["SN"].count()
o_pur_count = num_pur - ( m_pur_count + f_pur_count )

m_pur_total = males_df["Price"].sum()
m_pur_avg = m_pur_total / m_pur_count

f_pur_total = females_df["Price"].sum()
f_pur_avg = f_pur_total / f_pur_count

o_pur_total = tot_rev - ( m_pur_total + f_pur_total )
o_pur_avg = o_pur_total / o_pur_count

m_avg_pp = m_pur_total / num_males
f_avg_pp = f_pur_total / num_females
o_avg_pp = o_pur_total / num_others

m_pur_count, f_pur_count, o_pur_count, 
m_pur_avg , f_pur_avg, o_pur_avg, 
m_pur_total, f_pur_total, o_pur_total, 
m_avg_pp, f_avg_pp, o_avg_pp

pa_df = pd.DataFrame  ( { 
                          "Gender" : list_gender,
                          "Purchase Count" : [ m_pur_count, f_pur_count, o_pur_count ],
                          "Average Purchase Price" : [ m_pur_avg , f_pur_avg, o_pur_avg ],
                          "Total Purchase Value" : [ m_pur_total, f_pur_total, o_pur_total ],
                          "Avg Total Purchase per Person" : [ m_avg_pp, f_avg_pp, o_avg_pp ]
                        }
                       )
pa_df["Average Purchase Price"] = pa_df["Average Purchase Price"].astype(float).map( "${:,.2f}".format )
pa_df["Total Purchase Value"] = pa_df["Total Purchase Value"].astype(float).map( "${:,.2f}".format )
pa_df["Avg Total Purchase per Person"] = pa_df["Avg Total Purchase per Person"].astype(float).map( "${:,.2f}".format )

pa_df = pa_df.set_index("Gender").sort_values("Gender")
pa_df
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
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Avg Total Purchase per Person</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>113</td>
      <td>$3.20</td>
      <td>$361.94</td>
      <td>$4.47</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>652</td>
      <td>$3.02</td>
      <td>$1,967.64</td>
      <td>$4.07</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>15</td>
      <td>$3.35</td>
      <td>$50.19</td>
      <td>$4.56</td>
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
bins = [0, 9, 14, 19, 24, 29, 34, 39, 50]

grps = ['<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', '40+']

purchase_data["Age_Grp"] = pd.cut(purchase_data["Age"], bins, labels=grps)

players_df = purchase_data.loc[: , ["SN", "Age_Grp"]]

players_uniq_df = players_df.drop_duplicates()

pur_grp = players_uniq_df.groupby("Age_Grp")

pur_age_df  = pd.DataFrame( { 
                                "Total Count" : pur_grp["Age_Grp"].count(),
                                "Percentage of Players" : pur_grp["Age_Grp"].count() * 100 / num_players
                            }
                          )

pur_age_df["Percentage of Players"] = pur_df_137["Percentage of Players"].astype(float).map("{:,.2f}".format)

pur_age_df
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
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
    <tr>
      <th>Age_Grp</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>17</td>
      <td>2.95</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>22</td>
      <td>3.82</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>107</td>
      <td>18.58</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>258</td>
      <td>44.79</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>77</td>
      <td>13.37</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>52</td>
      <td>9.03</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>31</td>
      <td>5.38</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>12</td>
      <td>2.08</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)

* Bin the purchase_data data frame by age


* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. in the table below


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
price_df = purchase_data.loc[: , ["Price", "Age_Grp"]]

price_grp_df  = price_df.groupby('Age_Grp')

price_age_df  = pd.DataFrame( { 
                                "Purchase Count" : price_grp_df["Price"].count(),
                                "Average Purchase Price" : price_grp_df["Price"].sum() / price_grp_df["Price"].count(),
                                "Total Purchase Value" : price_grp_df["Price"].sum()
                              }
                            )

price_final_df = price_age_df.merge( pur_age_df, on = "Age_Grp", how='inner')

price_final_df = price_final_df[["Purchase Count" , "Average Purchase Price", "Total Purchase Value", "Total Count"]]

price_final_df["Avg Total Purchase per Person"] = price_final_df["Total Purchase Value"] / price_final_df["Total Count"]

price_final_df = price_final_df[["Purchase Count" , "Average Purchase Price", "Total Purchase Value", "Avg Total Purchase per Person"]]

price_final_df["Average Purchase Price"] = price_final_df["Average Purchase Price"].astype(float).map("${:,.2f}".format)
price_final_df["Total Purchase Value"] = price_final_df["Total Purchase Value"].astype(float).map("${:,.2f}".format)
price_final_df["Avg Total Purchase per Person"] = price_final_df["Avg Total Purchase per Person"].astype(float).map("${:,.2f}".format)

price_final_df.sort_values('Age_Grp', ascending=True)

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
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Avg Total Purchase per Person</th>
    </tr>
    <tr>
      <th>Age_Grp</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>23</td>
      <td>$3.35</td>
      <td>$77.13</td>
      <td>$4.54</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>28</td>
      <td>$2.96</td>
      <td>$82.78</td>
      <td>$3.76</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>136</td>
      <td>$3.04</td>
      <td>$412.89</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>365</td>
      <td>$3.05</td>
      <td>$1,114.06</td>
      <td>$4.32</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>101</td>
      <td>$2.90</td>
      <td>$293.00</td>
      <td>$3.81</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>73</td>
      <td>$2.93</td>
      <td>$214.00</td>
      <td>$4.12</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>41</td>
      <td>$3.60</td>
      <td>$147.67</td>
      <td>$4.76</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>13</td>
      <td>$2.94</td>
      <td>$38.24</td>
      <td>$3.19</td>
    </tr>
  </tbody>
</table>
</div>



* Run basic calculations to obtain the results in the table below


* Create a summary data frame to hold the results


* Sort the total purchase value column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
pur_name_df = purchase_data.loc[: , ["Purchase ID" , "SN" , "Price"] ]

pur_nm_df = pur_name_df.groupby('SN')

nm_an1_df = pd.DataFrame ( { 
                        
                           "Purchase Count" : pur_nm_df["SN"].count(),
                           "Average Purchase Price" : pur_nm_df["Price"].sum() / pur_nm_df["SN"].count(),
                           "Total Purchase Value" : pur_nm_df["Price"].sum()
                          }
                        )

nm_an1_df.sort_values('Total Purchase Value', ascending=False).head()
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
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
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
      <td>3.792000</td>
      <td>18.96</td>
    </tr>
    <tr>
      <th>Idastidru52</th>
      <td>4</td>
      <td>3.862500</td>
      <td>15.45</td>
    </tr>
    <tr>
      <th>Chamjask73</th>
      <td>3</td>
      <td>4.610000</td>
      <td>13.83</td>
    </tr>
    <tr>
      <th>Iral74</th>
      <td>4</td>
      <td>3.405000</td>
      <td>13.62</td>
    </tr>
    <tr>
      <th>Iskadarya95</th>
      <td>3</td>
      <td>4.366667</td>
      <td>13.10</td>
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

item_name_df = purchase_data.loc[: , ["Purchase ID" , "Item ID" , "Item Name", "Price"] ]
item_name_df.head()

item_grp = item_name_df.groupby(['Item ID', 'Item Name'])
item_grp.count()

item_an1_df = pd.DataFrame( { 
                                "Purchase Count" : item_grp["Purchase ID"].count(),
                                "Item Price" : item_grp["Price"].max(),
                                "Total Purchase Value" : item_grp["Price"].sum()
                            }
                          )

item_an1_df['Item Price'] = item_an1_df['Item Price'].astype(float).map("${:,.2f}".format)
item_an1_df['Total Purchase Value'] = item_an1_df['Total Purchase Value'].astype(float).map("${:,.2f}".format)

item_an1_df.sort_values('Purchase Count', ascending=False).head()

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
      <th>Item Price</th>
      <th>Total Purchase Value</th>
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
item_name_df = purchase_data.loc[: , ["Purchase ID" , "Item ID" , "Item Name", "Price"] ]
item_name_df.head()

item_grp = item_name_df.groupby(['Item ID', 'Item Name'])
item_grp.count()

item_an1_df = pd.DataFrame( { 
                                "Purchase Count" : item_grp["Purchase ID"].count(),
                                "Item Price" : item_grp["Price"].max(),
                                "Total Purchase Value" : item_grp["Price"].sum()
                            }
                          )

#item_an1_df["Item Price"] = item_an1_df["Item Price"].astype(float).map("${:,.2f}".format)
#item_an1_df["Total Purchase Value"] = item_an1_df["Total Purchase Value"].astype(float).map("${:,.2f}".format)
pd.options.display.float_format = '${:,.2f}'.format

item_an1_df.sort_values('Total Purchase Value', ascending=False).head()

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
      <th>Item Price</th>
      <th>Total Purchase Value</th>
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


