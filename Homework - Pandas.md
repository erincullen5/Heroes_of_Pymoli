
# Heroes of Pymoli


```python
#import dependencies 
import os 
import pandas as pd 
import numpy as np 
```


```python
#create a path for the JSON and check if it exists
json_path = os.path.join("purchase_data.json")
os.path.isfile(json_path)
```




    True




```python
purchase_data_master = pd.read_json(json_path)
purchase_data_master.head(10)
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
    <tr>
      <th>5</th>
      <td>20</td>
      <td>Male</td>
      <td>10</td>
      <td>Sleepwalker</td>
      <td>1.73</td>
      <td>Tanimnya91</td>
    </tr>
    <tr>
      <th>6</th>
      <td>20</td>
      <td>Male</td>
      <td>153</td>
      <td>Mercenary Sabre</td>
      <td>4.57</td>
      <td>Undjaskla97</td>
    </tr>
    <tr>
      <th>7</th>
      <td>29</td>
      <td>Female</td>
      <td>169</td>
      <td>Interrogator, Blood Blade of the Queen</td>
      <td>3.32</td>
      <td>Iathenudil29</td>
    </tr>
    <tr>
      <th>8</th>
      <td>25</td>
      <td>Male</td>
      <td>118</td>
      <td>Ghost Reaver, Longsword of Magic</td>
      <td>2.77</td>
      <td>Sondenasta63</td>
    </tr>
    <tr>
      <th>9</th>
      <td>31</td>
      <td>Male</td>
      <td>99</td>
      <td>Expiration, Warscythe Of Lost Worlds</td>
      <td>4.53</td>
      <td>Hilaerin92</td>
    </tr>
  </tbody>
</table>
</div>



# Player Count


```python
#Calculate the total number of players (unique)

player_count = purchase_data_master["SN"].nunique()
print(f'The number of unique players: {player_count}')
```

    The number of unique players: 573


# Purchasing Analysis (Total)


```python
#Calculate the total number of unique items for purchase
item_count = purchase_data_master["Item ID"].nunique()
#Calcualte the average price of the items
avg_price = purchase_data_master["Price"].mean()
avg_price = round(avg_price, 2)
#Calculate the total number of purchases
total_num_purchases = purchase_data_master["Item Name"].count()
#Calculate the Total Revenue 
total_rev = purchase_data_master["Price"].sum()


#Print Outputs
purch_analysis = (
    f'Purchasing Analysis (Total)\n'
    f'-------------------------------------------------\n'
    f'The number of unique items: {item_count}\n'
    f'The average price is: ${avg_price}\n'
    f'The total number of purcases: {total_num_purchases}\n'
    f'The Total revenue is : ${total_rev}'
        )
print(purch_analysis)
```

    Purchasing Analysis (Total)
    -------------------------------------------------
    The number of unique items: 183
    The average price is: $2.93
    The total number of purcases: 780
    The Total revenue is : $2286.33


# Gender Demographics


```python
#create a data frame just for female purchases 
female_data = purchase_data_master.loc[purchase_data_master["Gender"] == "Female",:]
# create a data frame just for male purchases
male_data = purchase_data_master.loc[purchase_data_master["Gender"] == "Male",:]
#create a data frame just for unknown purchases 
unknown_data = purchase_data_master.loc[purchase_data_master["Gender"]=="Other / Non-Disclosed",:]

#Percentage and Count of Female Players
fem_count = female_data["SN"].nunique()
fem_perc = (fem_count/player_count)*100
fem_perc =int(round(fem_perc))

#Percentage and Count of Male Players
male_count = male_data["SN"].nunique()
male_perc = (male_count/player_count)*100
male_perc = int(round(male_perc))

#Percentage and count of Unknown Players
unknown_count = unknown_data["SN"].nunique()
unknown_perc = (unknown_count/player_count)*100
unknown_perc = int(round(unknown_perc))

#Create Outputs
gender_analysis = (
        f'Gender Demographics\n'
        f'-----------------------------------------------------\n'
        f'Other/Non-Disclosed Players:{unknown_count} ({unknown_perc}%)\n'
        f'Female Players: {fem_count} ({fem_perc}%)\n'
        f'Male Players: {male_count} ({male_perc}%)'
)
print(gender_analysis)    
```

    Gender Demographics
    -----------------------------------------------------
    Other/Non-Disclosed Players:8 (1%)
    Female Players: 100 (17%)
    Male Players: 465 (81%)


# Purchasing Analysis (Gender)


```python
#Calculate number of purchases by demographic
gender_breakdown = purchase_data_master["Gender"].value_counts()
female_purchaes = gender_breakdown["Female"]
male_purchases = gender_breakdown["Male"]
unknown_purchases = gender_breakdown["Other / Non-Disclosed"]

#Calculate Average Purchase price by demographic
avg_fem_price = female_data["Price"].mean()
avg_fem_price = round(avg_fem_price,2)
avg_male_price = male_data["Price"].mean()
avg_male_price = round(avg_male_price,2)
avg_unknown_price = unknown_data["Price"].mean()
avg_unknown_price = round(avg_unknown_price,2)

#Total Purchase Value
fem_tot = female_data["Price"].sum()
fem_tot = round(fem_tot,2)
male_tot = male_data["Price"].sum()
male_tot = round(male_tot,2)
unknown_tot = unknown_data["Price"].sum()
unknown_tot = round(unknown_tot,2)
```


```python
#Create Outputs
demo_perc_data = (
    f'Purchasing Analysis (Gender)\n'
    f'--------------------------------\n'
    f'Female purchaes:\n'
        f'-Number of Purchases: {female_purchaes} \n'
        f'-Average Purchase Price: ${avg_fem_price} \n'
        f'-Total Purchase Value: ${fem_tot}\n'
     '\n'
    f'Male Purchases:\n'
        f'-Number of Purchases:{male_purchases}\n'
        f'-Average Purchase Price: ${avg_male_price}\n'
        f'-Total Purchase Value: ${male_tot}\n'
    '\n'
    f'Other / Non-Disclosed:\n'
        f'-Number of Purchases:{unknown_purchases}\n'
        f'-Average Purchase Price: ${avg_unknown_price}\n'
        f'-Total Purchase Value: ${unknown_tot}'
        )
print(demo_perc_data)
```

    Purchasing Analysis (Gender)
    --------------------------------
    Female purchaes:
    -Number of Purchases: 136 
    -Average Purchase Price: $2.82 
    -Total Purchase Value: $382.91
    
    Male Purchases:
    -Number of Purchases:633
    -Average Purchase Price: $2.95
    -Total Purchase Value: $1867.68
    
    Other / Non-Disclosed:
    -Number of Purchases:11
    -Average Purchase Price: $3.25
    -Total Purchase Value: $35.74


# Age Demographics


```python
#Create Bins and group names
bins = [0,10,14,18,22,26,30,34,38,42,46]
group_names = ["Under 10", "10 - 14","15 - 18","19 - 22","23 - 26", "27 - 30", "31 - 34","35 - 38","39 - 42", "43 - 46"]

#Cut the master Data Frame
age_df = pd.cut(purchase_data_master["Age"],bins,labels=group_names)
purchase_data_master["Age Group"] = pd.cut(purchase_data_master["Age"],bins,labels=group_names)

purchase_data_master.head()
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
      <th>Age Group</th>
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
      <td>35 - 38</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
      <td>19 - 22</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
      <td>31 - 34</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
      <td>19 - 22</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
      <td>23 - 26</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Making sure every person is in a bin 
min_age = purchase_data_master["Age"].max()
min_age = purchase_data_master["Age"].min()
```


```python
grouped_age_data = purchase_data_master.groupby("Age Group")

#Average Price per age group
avg_price_group = grouped_age_data["Price"].mean()
avg_price_group = round(avg_price_group,2)

#Purchase Count
num_purch_group = grouped_age_data["Price"].count()

#Total Purchase Value per age group
total_val_group = grouped_age_data["Price"].sum()
total_val_group = round(total_val_group,2)

# #Normalized Totals 
# norm_child = total_rev/total_val_group["Child"]
# norm_teen = total_rev/total_val_group["Teens"]
# norm_twenties = total_rev/total_val_group["Twenties"]
# norm_thirties = total_rev/total_val_group["Thirties"]
# norm_fourties = total_rev/total_val_group["Fourties"]

group_names = ["Under 10", "10 - 14","15 - 18","19 - 22","23 - 26", "27 - 30", "31 - 34","35 - 38","39 - 42", "43 - 46"]

age_demo = (
    f'Age Demographics\n'
    f'----------------------------------------------------\n'
    f'Group: Under 10 \n'
    f'- Purchase Count: {num_purch_group["Under 10"]}\n'
    f'- Average Purchase Price: ${avg_price_group["Under 10"]}\n'
    f'- Total Purchase Value: ${total_val_group["Under 10"]}\n'
    
    '\n'
    f'Group: 10 - 14 \n'
    f'- Purchase Count: {num_purch_group["10 - 14"]}\n'
    f'- Average Purchase Price: ${avg_price_group["10 - 14"]}\n'
    f'- Total Purchase Value: ${total_val_group["10 - 14"]}\n'

    
    '\n'
    f'Group: 15 - 18 \n'
    f'- Purchase Count: {num_purch_group["15 - 18"]}\n'
    f'- Average Purchase Price: ${avg_price_group["15 - 18"]}\n'
    f'- Total Purchase Value: ${total_val_group["15 - 18"]}\n'
    
    
    '\n'
    f'Group: 19 - 22 \n'
    f'- Purchase Count: {num_purch_group["19 - 22"]}\n'
    f'- Average Purchase Price: ${avg_price_group["19 - 22"]}\n'
    f'- Total Purchase Value: ${total_val_group["19 - 22"]}\n'
    
    '\n'
    f'Group: 23 - 26 \n'
    f'- Purchase Count: {num_purch_group["23 - 26"]}\n'
    f'- Average Purchase Price: ${avg_price_group["23 - 26"]}\n'
    f'- Total Purchase Value: ${total_val_group["23 - 26"]}\n'
    
    '\n'
    f'Group: 27 - 30 \n'
    f'- Purchase Count: {num_purch_group["27 - 30"]}\n'
    f'- Average Purchase Price: ${avg_price_group["27 - 30"]}\n'
    f'- Total Purchase Value: ${total_val_group["27 - 30"]}\n'
    
    '\n'
    f'Group: 31 - 34 \n'
    f'- Purchase Count: {num_purch_group["31 - 34"]}\n'
    f'- Average Purchase Price: ${avg_price_group["31 - 34"]}\n'
    f'- Total Purchase Value: ${total_val_group["31 - 34"]}\n'
    
    '\n'
    f'Group: 35 - 38 \n'
    f'- Purchase Count: {num_purch_group["35 - 38"]}\n'
    f'- Average Purchase Price: ${avg_price_group["35 - 38"]}\n'
    f'- Total Purchase Value: ${total_val_group["35 - 38"]}\n'
    
    '\n'
    f'Group: 39 - 42 \n'
    f'- Purchase Count: {num_purch_group["39 - 42"]}\n'
    f'- Average Purchase Price: ${avg_price_group["39 - 42"]}\n'
    f'- Total Purchase Value: ${total_val_group["39 - 42"]}\n'
    
    '\n'
    f'Group: 43 - 46 \n'
    f'- Purchase Count: {num_purch_group["43 - 46"]}\n'
    f'- Average Purchase Price: ${avg_price_group["43 - 46"]}\n'
    f'- Total Purchase Value: ${total_val_group["43 - 46"]}\n'
)
print(age_demo)
```

    Age Demographics
    ----------------------------------------------------
    Group: Under 10 
    - Purchase Count: 32
    - Average Purchase Price: $3.02
    - Total Purchase Value: $96.62
    
    Group: 10 - 14 
    - Purchase Count: 31
    - Average Purchase Price: $2.7
    - Total Purchase Value: $83.79
    
    Group: 15 - 18 
    - Purchase Count: 111
    - Average Purchase Price: $2.88
    - Total Purchase Value: $319.32
    
    Group: 19 - 22 
    - Purchase Count: 231
    - Average Purchase Price: $2.93
    - Total Purchase Value: $676.2
    
    Group: 23 - 26 
    - Purchase Count: 207
    - Average Purchase Price: $2.94
    - Total Purchase Value: $608.02
    
    Group: 27 - 30 
    - Purchase Count: 63
    - Average Purchase Price: $2.98
    - Total Purchase Value: $187.99
    
    Group: 31 - 34 
    - Purchase Count: 46
    - Average Purchase Price: $3.07
    - Total Purchase Value: $141.24
    
    Group: 35 - 38 
    - Purchase Count: 37
    - Average Purchase Price: $2.81
    - Total Purchase Value: $104.06
    
    Group: 39 - 42 
    - Purchase Count: 20
    - Average Purchase Price: $3.13
    - Total Purchase Value: $62.56
    
    Group: 43 - 46 
    - Purchase Count: 2
    - Average Purchase Price: $3.26
    - Total Purchase Value: $6.53
    


# Top Spenders


```python
#Top Spenders 
top_spend = purchase_data_master.groupby("SN")
top_spend = top_spend.sum().sort_values(by=["Price"],ascending = False)
top_spend = top_spend.reset_index()
top_five = top_spend[0:5]
del(top_five["Age"])
top_five = top_five.rename(columns={"Price":"Total Purchase Value"})

#create a second data frame that groups by SN and counts the totals in the Item ID cols
top_count = purchase_data_master.groupby("SN")
top_count = top_count.count().sort_values(by=["Item ID"],ascending = False)
top_count = top_count.reset_index()
top_count.head()

#Merge the two data frames on SN and rename the Age col to purchase count 
top_five = top_count.merge(top_five, on = "SN")
top_five = top_five.rename(columns = {"Age":"Purchase Count"})
cols = ["SN","Purchase Count","Total Purchase Value"]
top_df = top_five[cols]

#Create the Average Purchase Price  
top_df["Average Purchase Price"] = top_df["Total Purchase Value"]/top_df["Purchase Count"]
top_df
```

    /anaconda3/lib/python3.6/site-packages/ipykernel/__main__.py:22: SettingWithCopyWarning: 
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
      <th>SN</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
      <th>Average Purchase Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Undirrala66</td>
      <td>5</td>
      <td>17.06</td>
      <td>3.412000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Mindimnya67</td>
      <td>4</td>
      <td>12.74</td>
      <td>3.185000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Saedue76</td>
      <td>4</td>
      <td>13.56</td>
      <td>3.390000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Haellysu29</td>
      <td>3</td>
      <td>12.73</td>
      <td>4.243333</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Eoda93</td>
      <td>3</td>
      <td>11.58</td>
      <td>3.860000</td>
    </tr>
  </tbody>
</table>
</div>



# Most Popular Items# 


```python
#create a data Frame to calculate the most popular items
pop_df = purchase_data_master[["Item ID","Item Name", "Price"]]
pop_items = pop_df.groupby("Item ID").count()
pop_df = pop_df.drop_duplicates(["Item ID", "Item Name"])
pop_items = pop_items.reset_index()
pop_items = pop_items.rename(columns = {"Item Name":"Num of Purchases"})
pop_items = pop_items[["Item ID", "Num of Purchases"]]

#Merge the two df 
merge_df = pop_df.merge(pop_items, on ="Item ID")
merge_df["Total Purchase Value"] = merge_df["Price"]*merge_df["Num of Purchases"]
merge_df_pop = merge_df.sort_values(by = "Num of Purchases", ascending = False)
merge_df_pop.head()
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
      <th>Price</th>
      <th>Num of Purchases</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>53</th>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>11</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>88</th>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>2.23</td>
      <td>11</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>68</th>
      <td>175</td>
      <td>Woeful Adamantite Claymore</td>
      <td>1.24</td>
      <td>9</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>33</th>
      <td>13</td>
      <td>Serenity</td>
      <td>1.49</td>
      <td>9</td>
      <td>13.41</td>
    </tr>
    <tr>
      <th>49</th>
      <td>31</td>
      <td>Trickster</td>
      <td>2.07</td>
      <td>9</td>
      <td>18.63</td>
    </tr>
  </tbody>
</table>
</div>



# Most Profitable Items


```python
top_profit = merge_df.sort_values(by = "Total Purchase Value", ascending=False)
top_profit.head()
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
      <th>Price</th>
      <th>Num of Purchases</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>50</th>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>9</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>84</th>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>4.25</td>
      <td>7</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>45</th>
      <td>32</td>
      <td>Orenmir</td>
      <td>4.95</td>
      <td>6</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>79</th>
      <td>103</td>
      <td>Singed Scalpel</td>
      <td>4.87</td>
      <td>6</td>
      <td>29.22</td>
    </tr>
    <tr>
      <th>112</th>
      <td>107</td>
      <td>Splitter, Foe Of Subtlety</td>
      <td>3.61</td>
      <td>8</td>
      <td>28.88</td>
    </tr>
  </tbody>
</table>
</div>



# Three Observable Trends

- Users aged 19 -21 spent the most on items 
- Over three quarters of the users are males
- The most profitable items are not the most popular items
