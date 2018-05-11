

```python
# Import Dependencies
import pandas as pd
import json
```


```python
# File Path
filename = "heroes_of_pymoli.json"
```


```python
# Read File
with open(filename, 'r') as datafile:
    data = json.load(datafile)
data[0:5]
```




    [{'Age': 38,
      'Gender': 'Male',
      'Item ID': 165,
      'Item Name': 'Bone Crushing Silver Skewer',
      'Price': 3.37,
      'SN': 'Aelalis34'},
     {'Age': 21,
      'Gender': 'Male',
      'Item ID': 119,
      'Item Name': 'Stormbringer, Dark Blade of Ending Misery',
      'Price': 2.32,
      'SN': 'Eolo46'},
     {'Age': 34,
      'Gender': 'Male',
      'Item ID': 174,
      'Item Name': 'Primitive Blade',
      'Price': 2.46,
      'SN': 'Assastnya25'},
     {'Age': 21,
      'Gender': 'Male',
      'Item ID': 92,
      'Item Name': 'Final Critic',
      'Price': 1.36,
      'SN': 'Pheusrical25'},
     {'Age': 23,
      'Gender': 'Male',
      'Item ID': 63,
      'Item Name': 'Stormfury Mace',
      'Price': 1.27,
      'SN': 'Aela59'}]




```python
# Convert A List Of Dictionaries To DataFrame
purchase_data = pd.DataFrame(data)
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
# PLAYER COUNT

# Calculate Number of Unique Players
player_demographics = purchase_data.loc[:, ["Gender", "SN", "Age"]]
player_demographics = player_demographics.drop_duplicates()
total_players = player_demographics.count()[0]

# Display in DataFrame
pd.DataFrame({"Total Players" : [total_players]})

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
# PURCHASING ANALYSIS (TOTAL)

# Number of Unique Items
unique_items = len(purchase_data["Item Name"].unique())
unique_items

# Average Purchase Price
average_price = purchase_data["Price"].mean()
average_price

# Total Number of Purchases
total_purchase_count = purchase_data["Price"].count()
total_purchase_count

# Total Revenue
total_revenue = purchase_data["Price"].sum()
total_revenue

# Display in DataFrame
summary_purchase_table = pd.DataFrame({"Number of Unique Items" : [unique_items], 
              "Average Price" : [average_price], 
              "Number of Purchases" : [total_purchase_count], 
              "Total Revenue" : [total_revenue]})

# Use Map To Format Columns
summary_purchase_table = summary_purchase_table.round(2)
summary_purchase_table["Average Price"] = summary_purchase_table["Average Price"].map("${:.2f}".format)
summary_purchase_table["Total Revenue"] = summary_purchase_table["Total Revenue"].map("${:.2f}".format)

summary_purchase_table

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
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Number of Unique Items</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.93</td>
      <td>780</td>
      <td>179</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
# GENDER DEMOGRAPHIC

# Total Gender Demographic
full_count = purchase_data["SN"].nunique()

# Percentage and Count of Male Players
male_count = purchase_data[purchase_data["Gender"] == "Male"]["SN"].nunique()
male_percent = ((male_count / full_count)* 100)

# Percentage and Count of Female Players
female_count = purchase_data[purchase_data["Gender"] == "Female"]["SN"].nunique()
female_percent = ((female_count / full_count)* 100)

# Percentage and Count of Other / Non-Disclosed
other_count = full_count - male_count - female_count
other_percent = ((other_count / full_count)* 100)

# Gender Demographic Summary DataFrame
summary_genderdemo_table = pd.DataFrame({"Gender": ["Male", "Female", "Other / Non-Disclosed"], "Percentage of Players": [male_percent, female_percent, other_percent],
                                        "Total Count": [male_count, female_count, other_count]}, columns = 
                                        ["Gender", "Percentage of Players", "Total Count"])

# Formating
gender_demo_final = summary_genderdemo_table.set_index("Gender")
gender_demo_final.style.format({"Percentage of Players": "{:.2f}%"})
```




<style  type="text/css" >
</style>  
<table id="T_a817ef06_54a7_11e8_824f_acde48001122" > 
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
        <th id="T_a817ef06_54a7_11e8_824f_acde48001122level0_row0" class="row_heading level0 row0" >Male</th> 
        <td id="T_a817ef06_54a7_11e8_824f_acde48001122row0_col0" class="data row0 col0" >81.15%</td> 
        <td id="T_a817ef06_54a7_11e8_824f_acde48001122row0_col1" class="data row0 col1" >465</td> 
    </tr>    <tr> 
        <th id="T_a817ef06_54a7_11e8_824f_acde48001122level0_row1" class="row_heading level0 row1" >Female</th> 
        <td id="T_a817ef06_54a7_11e8_824f_acde48001122row1_col0" class="data row1 col0" >17.45%</td> 
        <td id="T_a817ef06_54a7_11e8_824f_acde48001122row1_col1" class="data row1 col1" >100</td> 
    </tr>    <tr> 
        <th id="T_a817ef06_54a7_11e8_824f_acde48001122level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_a817ef06_54a7_11e8_824f_acde48001122row2_col0" class="data row2 col0" >1.40%</td> 
        <td id="T_a817ef06_54a7_11e8_824f_acde48001122row2_col1" class="data row2 col1" >8</td> 
    </tr></tbody> 
</table> 




```python
# PURCHASING ANALYSIS (GENDER)

# Purchase Count By Gender
total_purchase = purchase_data["Price"].count()
male_purchase = purchase_data[purchase_data["Gender"] == "Male"]["Price"].count()
female_purchase = purchase_data[purchase_data["Gender"] == "Female"]["Price"].count()
other_purchase = total_purchase - male_purchase - female_purchase

# Average Purchase Price By Gender
male_avg_purchase = purchase_data[purchase_data["Gender"] == "Male"]["Price"].mean()
female_avg_purchase = purchase_data[purchase_data["Gender"] == "Female"]["Price"].mean()
other_avg_purchase = purchase_data[purchase_data["Gender"] == "Other / Non-Disclosed"]["Price"].mean()

# Total Purchase Value By Gender
total_male_purchase = purchase_data[purchase_data["Gender"] == "Male"]["Price"].sum()
total_female_purchase = purchase_data[purchase_data["Gender"] == "Female"]["Price"].sum()
total_other_purchase = purchase_data[purchase_data["Gender"] == "Other / Non-Disclosed"]["Price"].sum()

# Normalized Totals By Gender
male_norm = total_male_purchase / male_count
female_norm = total_female_purchase / female_count
other_norm = total_other_purchase / other_count

# Summary Table / Formating
summary_genderpurchase_table = pd.DataFrame({"Gender": ["Male", "Female", "Other / Non-Disclosed"],
                                             "Purchase Count": [male_purchase, female_purchase, other_purchase],
                                             "Average Purchase Price": [male_avg_purchase, female_avg_purchase, other_avg_purchase], 
                                             "Total Purchase Value": [total_male_purchase, total_female_purchase, total_other_purchase], 
                                             "Normalized Totals": [male_norm, female_norm, other_norm]}, columns = ["Gender", "Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"])

summary_genderpurchase_table = summary_genderpurchase_table.set_index("Gender")
summary_genderpurchase_table["Average Purchase Price"] = summary_genderpurchase_table["Average Purchase Price"].map("${:.2f}".format)
summary_genderpurchase_table["Total Purchase Value"] = summary_genderpurchase_table["Total Purchase Value"].map("${:.2f}".format)
summary_genderpurchase_table["Normalized Totals"] = summary_genderpurchase_table["Normalized Totals"].map("${:.2f}".format)

summary_genderpurchase_table

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
      <th>Normalized Totals</th>
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
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1867.68</td>
      <td>$4.02</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$3.83</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$4.47</td>
    </tr>
  </tbody>
</table>
</div>




```python
# AGE DEMOGRAPHICS

# Binning
bins = [0,10,15,20,25,30,35,40,200]
binLab = ['Under 10', '10 - 14', '15 - 19', '20 - 24', '25 - 29', '30 - 34', '35 - 39', 'Over 40']

# Add bins to new dataframe and groupby
bin_df = purchase_data.copy()
bin_df["Age Groups"] = pd.cut(bin_df["Age"], bins, labels=binLab)
group_bin = bin_df.groupby(["Age Groups"])

# Data Manipulation
bin_count = group_bin["SN"].count()
total_count = purchase_data["SN"].count()
percentage = (bin_count / total_count) * 100
percentage

# Create New DataFrame
Age_Percent = pd.DataFrame({"Total Count": bin_count,
                         "Percentage of Players": percentage})

# DataFrame Formatting
Age_Percent["Percentage of Players"] = Age_Percent["Percentage of Players"].map("{:.2f}%".format)

Age_Percent

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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Age Groups</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Under 10</th>
      <td>4.10%</td>
      <td>32</td>
    </tr>
    <tr>
      <th>10 - 14</th>
      <td>10.00%</td>
      <td>78</td>
    </tr>
    <tr>
      <th>15 - 19</th>
      <td>23.59%</td>
      <td>184</td>
    </tr>
    <tr>
      <th>20 - 24</th>
      <td>39.10%</td>
      <td>305</td>
    </tr>
    <tr>
      <th>25 - 29</th>
      <td>9.74%</td>
      <td>76</td>
    </tr>
    <tr>
      <th>30 - 34</th>
      <td>7.44%</td>
      <td>58</td>
    </tr>
    <tr>
      <th>35 - 39</th>
      <td>5.64%</td>
      <td>44</td>
    </tr>
    <tr>
      <th>Over 40</th>
      <td>0.38%</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
# AGE DEMOGRAPHICS CONTINUED

# The below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.) 
# Binning
bins = [0,10,15,20,25,30,35,40,200]
binLab = ['Under 10', '10 - 14', '15 - 19', '20 - 24', '25 - 29', '30 - 34', '35 - 39', 'Over 40']

# Add Bins To New DataFrame And Groupby
binning_df = purchase_data.copy()
binning_df["Age Groups"] = pd.cut(binning_df["Age"], bins, labels=binLab)
bin_column = pd.cut(binning_df["Age"], bins, labels=binLab)
grouped_bin = binning_df.groupby(["Age Groups"])

# Data Manipulation: Purchase Count, Average Purchase Price, and Total Purchase Value
bin_purchase_count = grouped_bin["Age"].count()
bin_purchase_average = grouped_bin["Price"].mean()
bin_purchase_total = grouped_bin["Price"].sum()

# Normalized Totals
binningduplicate = purchase_data.drop_duplicates(subset='SN', keep="first")
binningduplicate["Age Groups"] = pd.cut(binningduplicate["Age"], bins, labels=binLab)
binningduplicate = binningduplicate.groupby(["Age Groups"])

binning_norm = (grouped_bin["Price"].sum() / binningduplicate["SN"].count())
binning_norm

# New DataFrame / Format
age_demo = pd.DataFrame({"Purchase Count": bin_purchase_count,
                         "Average Purchase Price": bin_purchase_average,
                         "Total Purchase Value": bin_purchase_total,
                         "Normalized Totals": binning_norm})

age_demo["Average Purchase Price"] = age_demo["Average Purchase Price"].map("${:.2f}".format)
age_demo["Total Purchase Value"] = age_demo["Total Purchase Value"].map("${:.2f}".format)
age_demo["Normalized Totals"] = age_demo["Normalized Totals"].map("${:.2f}".format)
age_demo = age_demo[["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]

age_demo
```

    /Users/catesoane/anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:21: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy





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
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age Groups</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Under 10</th>
      <td>32</td>
      <td>$3.02</td>
      <td>$96.62</td>
      <td>$4.39</td>
    </tr>
    <tr>
      <th>10 - 14</th>
      <td>78</td>
      <td>$2.87</td>
      <td>$224.15</td>
      <td>$4.15</td>
    </tr>
    <tr>
      <th>15 - 19</th>
      <td>184</td>
      <td>$2.87</td>
      <td>$528.74</td>
      <td>$3.80</td>
    </tr>
    <tr>
      <th>20 - 24</th>
      <td>305</td>
      <td>$2.96</td>
      <td>$902.61</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>25 - 29</th>
      <td>76</td>
      <td>$2.89</td>
      <td>$219.82</td>
      <td>$4.23</td>
    </tr>
    <tr>
      <th>30 - 34</th>
      <td>58</td>
      <td>$3.07</td>
      <td>$178.26</td>
      <td>$4.05</td>
    </tr>
    <tr>
      <th>35 - 39</th>
      <td>44</td>
      <td>$2.90</td>
      <td>$127.49</td>
      <td>$5.10</td>
    </tr>
    <tr>
      <th>Over 40</th>
      <td>3</td>
      <td>$2.88</td>
      <td>$8.64</td>
      <td>$2.88</td>
    </tr>
  </tbody>
</table>
</div>




```python
# TOP SPENDERS

# Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
top_spender = purchase_data[["SN","Price","Item Name"]]
total_spent = top_spender.groupby("SN").sum()
total_spent.sort_values(by = "Price", ascending = False, inplace = True)

# SN
names = list(total_spent.index.values)
top_names = [names[0],names[1],names[2],names[3],names[4]]

# Total Purchase Value
total_purchase_values_1 = total_spent.iloc[0,0]
total_purchase_values_2 = total_spent.iloc[1,0]
total_purchase_values_3 = total_spent.iloc[2,0]
total_purchase_values_4 = total_spent.iloc[3,0]
total_purchase_values_5 = total_spent.iloc[4,0]
top_purchase_values = [total_spent.iloc[0,0], 
                       total_spent.iloc[1,0], 
                       total_spent.iloc[2,0], 
                       total_spent.iloc[3,0],
                       total_spent.iloc[4,0]]

# Purchase Count 
top_purchase_counts_1 = top_spender[top_spender["SN"] == names[0]].count()[0]
top_purchase_counts_2 = top_spender[top_spender["SN"] == names[1]].count()[0]
top_purchase_counts_3 = top_spender[top_spender["SN"] == names[2]].count()[0]
top_purchase_counts_4 = top_spender[top_spender["SN"] == names[3]].count()[0]
top_purchase_counts_5 = top_spender[top_spender["SN"] == names[4]].count()[0]
top_purchase_counts = [top_purchase_counts_1, 
                       top_purchase_counts_2, 
                       top_purchase_counts_3, 
                       top_purchase_counts_4,
                       top_purchase_counts_5]

# Average Purchase Price
average_price_1 = total_purchase_values_1 / top_purchase_counts_1
average_price_2 = total_purchase_values_2 / top_purchase_counts_2
average_price_3 = total_purchase_values_3 / top_purchase_counts_3
average_price_4 = total_purchase_values_4 / top_purchase_counts_4
average_price_5 = total_purchase_values_5 / top_purchase_counts_5
average_prices = [average_price_1, average_price_2, average_price_3, average_price_4, average_price_5]

# New DataFrame / Format
top_spenders = pd.DataFrame ({"Purchase Count": top_purchase_counts,
                             "Average Purchase Price": average_prices,
                             "Total Purchase Value": top_purchase_values,
                             "SN": top_names})

top_spenders["Average Purchase Price"] = top_spenders["Average Purchase Price"].map("${:.2f}".format)
top_spenders["Total Purchase Value"] = top_spenders["Total Purchase Value"].map("${:.2f}".format)

top_spenders = top_spenders[["Purchase Count", "Average Purchase Price", "Total Purchase Value", "SN"]]

top_spenders
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
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
      <td>Undirrala66</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
      <td>Saedue76</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
      <td>Mindimnya67</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>$4.24</td>
      <td>$12.73</td>
      <td>Haellysu29</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
      <td>$3.86</td>
      <td>$11.58</td>
      <td>Eoda93</td>
    </tr>
  </tbody>
</table>
</div>




```python
# MOST POPULAR ITEMS

# Identify the 5 most popular items by purchase count, then list (in a table):
popular_item = purchase_data[["Item ID", "Item Name", "Price"]]
pop_items = popular_item.groupby("Item ID").count()
pop_items.sort_values(by = "Item Name", ascending = False, inplace = True)
popular_item= popular_item.drop_duplicates(["Item ID", "Item Name"])

# Item ID
item_ids = [pop_items.index[0], 
            pop_items.index[1], 
            pop_items.index[2], 
            pop_items.index[3], 
            pop_items.index[4]]

# Item Name
name_1 = popular_item.loc[popular_item["Item ID"] == item_ids[0], "Item Name"].item()
name_2 = popular_item.loc[popular_item["Item ID"] == item_ids[1], "Item Name"].item()
name_3 = popular_item.loc[popular_item["Item ID"] == item_ids[2], "Item Name"].item()
name_4 = popular_item.loc[popular_item["Item ID"] == item_ids[3], "Item Name"].item()
name_5 = popular_item.loc[popular_item["Item ID"] == item_ids[4], "Item Name"].item()
popular_item_names = [name_1, name_2, name_3, name_4, name_5]

# Purchase Count
item_counts = [popular_item.iloc[0,0], 
               popular_item.iloc[1,0], 
               popular_item.iloc[2,0], 
               popular_item.iloc[3,0], 
               popular_item.iloc[4,0]]

# Item Price
price_1 = popular_item.loc[popular_item["Item Name"] == popular_item_names[0], "Price"].item()
price_2 = popular_item.loc[popular_item["Item Name"] == popular_item_names[1], "Price"].item()
price_3 = popular_item.loc[popular_item["Item Name"] == popular_item_names[2], "Price"].item()
price_4 = popular_item.loc[popular_item["Item Name"] == popular_item_names[3], "Price"].item()
price_5 = popular_item.loc[popular_item["Item Name"] == popular_item_names[4], "Price"].item()
item_prices = [price_1,price_2,price_3,price_4,price_5]

# Total Purchase Value
total_values = [pop_items.iloc[0,0]*price_1, 
                pop_items.iloc[1,0]*price_2, 
                pop_items.iloc[2,0]*price_3, 
                pop_items.iloc[3,0]*price_4, 
                pop_items.iloc[4,0]*price_5]

# New DataFrame / Format
most_popular_items = pd.DataFrame ({"Item ID": item_ids,
                                   "Item Name": popular_item_names,
                                   "Purchase Count": item_counts,
                                   "Item Price": item_prices,
                                   "Total Purchase Value": total_values})

most_popular_items["Item Price"] = most_popular_items["Item Price"].map("${:.2f}".format)
most_popular_items["Total Purchase Value"] = most_popular_items["Total Purchase Value"].map("${:.2f}".format)

most_popular_items[["Item ID", "Item Name", "Purchase Count", "Item Price", "Total Purchase Value"]]

most_popular_items
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
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>$2.35</td>
      <td>165</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>1</th>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>$2.23</td>
      <td>119</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>2</th>
      <td>31</td>
      <td>Trickster</td>
      <td>$2.07</td>
      <td>174</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>3</th>
      <td>175</td>
      <td>Woeful Adamantite Claymore</td>
      <td>$1.24</td>
      <td>92</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>4</th>
      <td>13</td>
      <td>Serenity</td>
      <td>$1.49</td>
      <td>63</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
# MOST PROFITABLE ITEMS

# Identify the 5 most profitable items by total purchase value, then list (in a table):
profitable_items = purchase_data[["Item ID", "Item Name", "Price"]]
profit_items = profitable_items.groupby("Item ID").sum()
profit_items.sort_values(by = "Price", ascending = False, inplace = True)
profitable_items = profitable_items.drop_duplicates(["Item ID", "Price"])

# Item ID
item_ids = [profit_items.index[0], 
            profit_items.index[1], 
            profit_items.index[2], 
            profit_items.index[3], 
            profit_items.index[4]]

# Item Name
name_1 = profitable_items.loc[profitable_items["Item ID"] == item_ids[0], "Item Name"].item()
name_2 = profitable_items.loc[profitable_items["Item ID"] == item_ids[1], "Item Name"].item()
name_3 = profitable_items.loc[profitable_items["Item ID"] == item_ids[2], "Item Name"].item()
name_4 = profitable_items.loc[profitable_items["Item ID"] == item_ids[3], "Item Name"].item()
name_5 = profitable_items.loc[profitable_items["Item ID"] == item_ids[4], "Item Name"].item()
profit_names = [name_1, name_2, name_3, name_4, name_5]

# Total Purchase Value
values = [profit_items.iloc[0,0],
          profit_items.iloc[1,0],
          profit_items.iloc[2,0],
          profit_items.iloc[3,0],
          profit_items.iloc[4,0]]

# Item Price
price_1 = profitable_items.loc[profitable_items["Item ID"] == item_ids[0], "Price"].item()
price_2 = profitable_items.loc[profitable_items["Item ID"] == item_ids[1], "Price"].item()
price_3 = profitable_items.loc[profitable_items["Item ID"] == item_ids[2], "Price"].item()
price_4 = profitable_items.loc[profitable_items["Item ID"] == item_ids[3], "Price"].item()
price_5 = profitable_items.loc[profitable_items["Item ID"] == item_ids[4], "Price"].item()
profit_prices = [price_1,price_2,price_3,price_4,price_5]

# Purchase Count
purchase_count = purchase_data[["Item ID", "Item Name", "Price"]].groupby("Item Name").count()
count_1 = purchase_count.loc[purchase_count.index == profit_names[0], "Item ID"].item()
count_2 = purchase_count.loc[purchase_count.index == profit_names[1], "Item ID"].item()
count_3 = purchase_count.loc[purchase_count.index == profit_names[2], "Item ID"].item()
count_4 = purchase_count.loc[purchase_count.index == profit_names[3], "Item ID"].item()
count_5 = purchase_count.loc[purchase_count.index == profit_names[4], "Item ID"].item()
counts = [count_1, count_2, count_3, count_4, count_5]

# New DataFrame / Format
most_profitable_items = pd.DataFrame ({"Item ID": item_ids,
                                      "Item Name": profit_names,
                                      "Purchase Count": counts,
                                      "Item Price": profit_prices,
                                      "Total Purchase Value": values})

most_profitable_items["Item Price"] = most_profitable_items["Item Price"].map("${:.2f}".format)
most_profitable_items["Total Purchase Value"] = most_profitable_items["Total Purchase Value"].map("${:.2f}".format)

most_profitable_items[["Item ID", "Item Name", "Purchase Count", "Item Price", "Total Purchase Value"]]

most_profitable_items

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
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>$4.14</td>
      <td>9</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>1</th>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>$4.25</td>
      <td>7</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>2</th>
      <td>32</td>
      <td>Orenmir</td>
      <td>$4.95</td>
      <td>6</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>3</th>
      <td>103</td>
      <td>Singed Scalpel</td>
      <td>$4.87</td>
      <td>6</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>4</th>
      <td>107</td>
      <td>Splitter, Foe Of Subtlety</td>
      <td>$3.61</td>
      <td>8</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


