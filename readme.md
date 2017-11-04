# Heroes of Pymoli

* Heroes of Pymoli are predominantly male and spend more per item than female counterparts.
* Player age appears to have a normal distribution with the central value falling into the 20-24 age range.
* The top 5 most popular items account for just 5.9% of item purchases and 5.0% of revenue.  Additionally, the top 5 most profitable items account for just 5.0% of item purchases and 6.3% of revenue.  This indicates that item popularity and profitability are not dependent on a small few items being successful, but instead distrubuted across a wider range of items.

```python
import os
import pandas as pd
import numpy as np

purchase_file_1 = os.path.join('purchase_data.json')
purchase_file_2 = os.path.join('purchase_data2.json')

record1 = pd.read_json(purchase_file_1, orient='records')
record2 = pd.read_json(purchase_file_2, orient='records')
```


```python
#Concatenate purchase records

purchase_record = pd.concat([record1, record2])
```





## Player Count
```python
#Total number of players

unique_players = purchase_record.drop_duplicates('SN')
total_players = len(unique_players)

total_players_table = pd.DataFrame({"Total Players":[total_players]})
total_players_table
```




<div>

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
      <td>612</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Total)
```python
#Purchasing Analysis (Total)

unique_items = len(purchase_record['Item Name'].unique())

avg_price = round(purchase_record['Price'].mean(), 2)

total_purchases = len(purchase_record.index)

total_revenue = round(purchase_record['Price'].sum(), 2)

total_analysis_table = pd.DataFrame({"Number of Unique Items":[unique_items],
                                     "Average Price":[avg_price],
                                     "Number of Purchases":[total_purchases],
                                     "Total Revenue":[total_revenue]})
total_analysis_table = total_analysis_table.style.format({'Average Price':'${}','Total Revenue':'${}'})
total_analysis_table
```




<style  type="text/css" >
</style>  
<table id="T_9f0986da_c129_11e7_80b0_e4a4711df334" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Price</th> 
        <th class="col_heading level0 col1" >Number of Purchases</th> 
        <th class="col_heading level0 col2" >Number of Unique Items</th> 
        <th class="col_heading level0 col3" >Total Revenue</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9f0986da_c129_11e7_80b0_e4a4711df334level0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_9f0986da_c129_11e7_80b0_e4a4711df334row0_col0" class="data row0 col0" >$2.93</td> 
        <td id="T_9f0986da_c129_11e7_80b0_e4a4711df334row0_col1" class="data row0 col1" >858</td> 
        <td id="T_9f0986da_c129_11e7_80b0_e4a4711df334row0_col2" class="data row0 col2" >180</td> 
        <td id="T_9f0986da_c129_11e7_80b0_e4a4711df334row0_col3" class="data row0 col3" >$2514.43</td> 
    </tr></tbody> 
</table> 



## Gender Demographics
```python
#Gender Demographics

genders = purchase_record['Gender'].unique()
gender_count, gender_percent = [], []

for x in genders:
    gender_unique = unique_players.loc[unique_players['Gender']==x,:]
    gender_count.append(len(gender_unique))

gender_percent = [round((a/total_players)*100, 2) for a in gender_count]


gender_demo_table = pd.DataFrame({"Gender":["Male","Female","Other/Non-Disclosed"],
                                  "Percentage of Players": [gender_percent[0], gender_percent[1], gender_percent[2]],
                                  "Total Count":[gender_count[0], gender_count[1], gender_count[2]]})

gender_demo_table = gender_demo_table.set_index('Gender')
gender_demo_table
```




<div>

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
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
      <td>80.88</td>
      <td>495</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.65</td>
      <td>108</td>
    </tr>
    <tr>
      <th>Other/Non-Disclosed</th>
      <td>1.47</td>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Gender)
```python
#Purchasing Analysis (Gender)

gender_pcount, gender_totvalue = [], []

for x in genders:
    gender_purchases = purchase_record.loc[purchase_record['Gender']==x,:]
    gender_pcount.append(len(gender_purchases))
    gender_totvalue.append(round(gender_purchases['Price'].sum(), 2))

gender_avgprice = [round(a/b, 2) for a,b in zip(gender_totvalue, gender_pcount)]
gender_norm = [round(a/b, 2) for a,b in zip(gender_totvalue, gender_count)]

gender_analysis_table = pd.DataFrame({"Gender":[genders[0],genders[1],genders[2]],
                                      "Purchase Count":[gender_pcount[0],gender_pcount[1],gender_pcount[2]],
                                      "Average Purchase Price":[gender_avgprice[0],gender_avgprice[1],gender_avgprice[2]],
                                      "Total Purchase Value":[gender_totvalue[0],gender_totvalue[1],gender_totvalue[2]],
                                      "Normalized Totals":[gender_norm[0],gender_norm[1],gender_norm[2]]})

gender_analysis_table = gender_analysis_table[['Gender',
                                               'Purchase Count',
                                               'Average Purchase Price',
                                               'Total Purchase Value',
                                               'Normalized Totals']]

gender_analysis_table = gender_analysis_table.set_index('Gender').style.format({'Average Purchase Price':'${:.2f}',
                                                                                'Total Purchase Value':'${:.2f}',
                                                                                'Normalized Totals':'${:.2f}'})

gender_analysis_table
```




<style  type="text/css" >
</style>  
<table id="T_92aeb6d4_c12a_11e7_86f6_e4a4711df334" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
        <th class="col_heading level0 col3" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_92aeb6d4_c12a_11e7_86f6_e4a4711df334level0_row0" class="row_heading level0 row0" >Male</th> 
        <td id="T_92aeb6d4_c12a_11e7_86f6_e4a4711df334row0_col0" class="data row0 col0" >697</td> 
        <td id="T_92aeb6d4_c12a_11e7_86f6_e4a4711df334row0_col1" class="data row0 col1" >$2.94</td> 
        <td id="T_92aeb6d4_c12a_11e7_86f6_e4a4711df334row0_col2" class="data row0 col2" >$2052.28</td> 
        <td id="T_92aeb6d4_c12a_11e7_86f6_e4a4711df334row0_col3" class="data row0 col3" >$4.15</td> 
    </tr>    <tr> 
        <th id="T_92aeb6d4_c12a_11e7_86f6_e4a4711df334level0_row1" class="row_heading level0 row1" >Female</th> 
        <td id="T_92aeb6d4_c12a_11e7_86f6_e4a4711df334row1_col0" class="data row1 col0" >149</td> 
        <td id="T_92aeb6d4_c12a_11e7_86f6_e4a4711df334row1_col1" class="data row1 col1" >$2.85</td> 
        <td id="T_92aeb6d4_c12a_11e7_86f6_e4a4711df334row1_col2" class="data row1 col2" >$424.29</td> 
        <td id="T_92aeb6d4_c12a_11e7_86f6_e4a4711df334row1_col3" class="data row1 col3" >$3.93</td> 
    </tr>    <tr> 
        <th id="T_92aeb6d4_c12a_11e7_86f6_e4a4711df334level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_92aeb6d4_c12a_11e7_86f6_e4a4711df334row2_col0" class="data row2 col0" >12</td> 
        <td id="T_92aeb6d4_c12a_11e7_86f6_e4a4711df334row2_col1" class="data row2 col1" >$3.15</td> 
        <td id="T_92aeb6d4_c12a_11e7_86f6_e4a4711df334row2_col2" class="data row2 col2" >$37.86</td> 
        <td id="T_92aeb6d4_c12a_11e7_86f6_e4a4711df334row2_col3" class="data row2 col3" >$4.21</td> 
    </tr></tbody> 
</table> 



## Age Demographics
```python
#Age Demographics

bins = [0,9,14,19,24,29,34,39,100]
age_range = ['<10','10-14','15-19','20-24','25-29','30-34','35-39','40+']

purchase_record["Age Range"] = pd.cut(purchase_record['Age'], bins, labels=age_range)
unique_players = purchase_record.drop_duplicates('SN')

age_dgroup = unique_players.groupby(['Age Range'])
age_group_count = age_dgroup['Age'].count()
age_group_percent = [round(float((a/total_players)*100), 2) for a in age_group_count]

age_demo_table = pd.DataFrame({"Percentage of Players":age_group_percent,"Total Count":age_group_count})
age_demo_table
```




<div>

</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Age Range</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.43</td>
      <td>21</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>4.08</td>
      <td>25</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.48</td>
      <td>107</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>44.44</td>
      <td>272</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>15.20</td>
      <td>93</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.33</td>
      <td>51</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>5.23</td>
      <td>32</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.80</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)
```python
#Purchase Analysis (Age)

age_agroup = purchase_record.groupby(['Age Range'])
age_pcount = age_agroup['Item ID'].count()
age_avgprice = round(age_agroup['Price'].mean(), 2)
age_totvalue = round(age_agroup['Price'].sum(), 2)
age_norm = round(age_dgroup['Price'].mean(), 2)

age_analysis_table = pd.DataFrame({"Purchase Count":age_pcount,
                                   "Average Purchase Price":age_avgprice,
                                   "Total Purchase Value":age_totvalue,
                                   "Normalized Totals":age_norm})[['Purchase Count',
                                                                   'Average Purchase Price',
                                                                   'Total Purchase Value',
                                                                   'Normalized Totals']].style.format({'Average Purchase Price':'${:.2f}',
                                                                                                       'Total Purchase Value':'${:.2f}',
                                                                                                       'Normalized Totals':'${:.2f}'})

age_analysis_table
```




<style  type="text/css" >
</style>  
<table id="T_9f201c00_c12a_11e7_870f_e4a4711df334" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
        <th class="col_heading level0 col3" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Age Range</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9f201c00_c12a_11e7_870f_e4a4711df334level0_row0" class="row_heading level0 row0" ><10</th> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row0_col0" class="data row0 col0" >33</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row0_col1" class="data row0 col1" >$2.95</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row0_col2" class="data row0 col2" >$97.28</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row0_col3" class="data row0 col3" >$3.25</td> 
    </tr>    <tr> 
        <th id="T_9f201c00_c12a_11e7_870f_e4a4711df334level0_row1" class="row_heading level0 row1" >10-14</th> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row1_col0" class="data row1 col0" >38</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row1_col1" class="data row1 col1" >$2.79</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row1_col2" class="data row1 col2" >$105.91</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row1_col3" class="data row1 col3" >$2.72</td> 
    </tr>    <tr> 
        <th id="T_9f201c00_c12a_11e7_870f_e4a4711df334level0_row2" class="row_heading level0 row2" >15-19</th> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row2_col0" class="data row2 col0" >144</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row2_col1" class="data row2 col1" >$2.89</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row2_col2" class="data row2 col2" >$416.83</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row2_col3" class="data row2 col3" >$2.91</td> 
    </tr>    <tr> 
        <th id="T_9f201c00_c12a_11e7_870f_e4a4711df334level0_row3" class="row_heading level0 row3" >20-24</th> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row3_col0" class="data row3 col0" >372</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row3_col1" class="data row3 col1" >$2.92</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row3_col2" class="data row3 col2" >$1087.66</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row3_col3" class="data row3 col3" >$2.98</td> 
    </tr>    <tr> 
        <th id="T_9f201c00_c12a_11e7_870f_e4a4711df334level0_row4" class="row_heading level0 row4" >25-29</th> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row4_col0" class="data row4 col0" >134</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row4_col1" class="data row4 col1" >$2.96</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row4_col2" class="data row4 col2" >$396.44</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row4_col3" class="data row4 col3" >$3.05</td> 
    </tr>    <tr> 
        <th id="T_9f201c00_c12a_11e7_870f_e4a4711df334level0_row5" class="row_heading level0 row5" >30-34</th> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row5_col0" class="data row5 col0" >71</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row5_col1" class="data row5 col1" >$2.97</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row5_col2" class="data row5 col2" >$211.14</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row5_col3" class="data row5 col3" >$3.10</td> 
    </tr>    <tr> 
        <th id="T_9f201c00_c12a_11e7_870f_e4a4711df334level0_row6" class="row_heading level0 row6" >35-39</th> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row6_col0" class="data row6 col0" >48</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row6_col1" class="data row6 col1" >$2.93</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row6_col2" class="data row6 col2" >$140.77</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row6_col3" class="data row6 col3" >$3.07</td> 
    </tr>    <tr> 
        <th id="T_9f201c00_c12a_11e7_870f_e4a4711df334level0_row7" class="row_heading level0 row7" >40+</th> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row7_col0" class="data row7 col0" >18</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row7_col1" class="data row7 col1" >$3.24</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row7_col2" class="data row7 col2" >$58.40</td> 
        <td id="T_9f201c00_c12a_11e7_870f_e4a4711df334row7_col3" class="data row7 col3" >$3.11</td> 
    </tr></tbody> 
</table> 



## Top Spenders
```python
#Top Spenders

top_spenders = purchase_record.groupby('SN')['Price'].agg(['sum','count','mean']).sort_values('sum', ascending=False).head()
top_spenders = top_spenders.round({'sum':2,'mean':2}).rename(columns={
    'sum':'Total Purchase Value',
    'count':'Purchase Count',
    'mean':'Average Purchase Price'})[['Purchase Count',
                                        'Average Purchase Price',
                                        'Total Purchase Value']].style.format({'Average Purchase Price':'${:.2f}',
                                                                               'Total Purchase Value':'${:.2f}'})
top_spenders
```




<style  type="text/css" >
</style>  
<table id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >SN</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334level0_row0" class="row_heading level0 row0" >Undirrala66</th> 
        <td id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334row0_col0" class="data row0 col0" >5</td> 
        <td id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334row0_col1" class="data row0 col1" >$3.41</td> 
        <td id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334row0_col2" class="data row0 col2" >$17.06</td> 
    </tr>    <tr> 
        <th id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334level0_row1" class="row_heading level0 row1" >Aerithllora36</th> 
        <td id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334row1_col0" class="data row1 col0" >4</td> 
        <td id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334row1_col1" class="data row1 col1" >$3.78</td> 
        <td id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334row1_col2" class="data row1 col2" >$15.10</td> 
    </tr>    <tr> 
        <th id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334level0_row2" class="row_heading level0 row2" >Saedue76</th> 
        <td id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334row2_col0" class="data row2 col0" >4</td> 
        <td id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334row2_col1" class="data row2 col1" >$3.39</td> 
        <td id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334row2_col2" class="data row2 col2" >$13.56</td> 
    </tr>    <tr> 
        <th id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334level0_row3" class="row_heading level0 row3" >Sondim43</th> 
        <td id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334row3_col0" class="data row3 col0" >4</td> 
        <td id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334row3_col1" class="data row3 col1" >$3.26</td> 
        <td id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334row3_col2" class="data row3 col2" >$13.02</td> 
    </tr>    <tr> 
        <th id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334level0_row4" class="row_heading level0 row4" >Mindimnya67</th> 
        <td id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334row4_col0" class="data row4 col0" >4</td> 
        <td id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334row4_col1" class="data row4 col1" >$3.18</td> 
        <td id="T_a1b6d618_c129_11e7_b6b6_e4a4711df334row4_col2" class="data row4 col2" >$12.74</td> 
    </tr></tbody> 
</table> 



## Most Popular Items
```python
#Most Popular Items

popular_items = purchase_record.groupby(['Item ID','Item Name'])['Price'].agg(['count','sum','max']).sort_values('count', ascending=False).head()
popular_items = popular_items.round({'sum':2,'max':2}).rename(
    columns={'count':'Purchase Count',
             'sum':'Total Purchase Value',
             'max':'Item Price'})[['Purchase Count',
                                   'Item Price',
                                   'Total Purchase Value']].style.format({'Item Price':'${:.2f}',
                                                                          'Total Purchase Value':'${:.2f}'})
popular_items
```




<style  type="text/css" >
</style>  
<table id="T_a24e5f86_c129_11e7_b074_e4a4711df334" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Item Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="index_name level1" >Item Name</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a24e5f86_c129_11e7_b074_e4a4711df334level0_row0" class="row_heading level0 row0" >84</th> 
        <th id="T_a24e5f86_c129_11e7_b074_e4a4711df334level1_row0" class="row_heading level1 row0" >Arcane Gem</th> 
        <td id="T_a24e5f86_c129_11e7_b074_e4a4711df334row0_col0" class="data row0 col0" >12</td> 
        <td id="T_a24e5f86_c129_11e7_b074_e4a4711df334row0_col1" class="data row0 col1" >$4.81</td> 
        <td id="T_a24e5f86_c129_11e7_b074_e4a4711df334row0_col2" class="data row0 col2" >$29.34</td> 
    </tr>    <tr> 
        <th id="T_a24e5f86_c129_11e7_b074_e4a4711df334level0_row1" class="row_heading level0 row1" >39</th> 
        <th id="T_a24e5f86_c129_11e7_b074_e4a4711df334level1_row1" class="row_heading level1 row1" >Betrayal, Whisper of Grieving Widows</th> 
        <td id="T_a24e5f86_c129_11e7_b074_e4a4711df334row1_col0" class="data row1 col0" >11</td> 
        <td id="T_a24e5f86_c129_11e7_b074_e4a4711df334row1_col1" class="data row1 col1" >$2.35</td> 
        <td id="T_a24e5f86_c129_11e7_b074_e4a4711df334row1_col2" class="data row1 col2" >$25.85</td> 
    </tr>    <tr> 
        <th id="T_a24e5f86_c129_11e7_b074_e4a4711df334level0_row2" class="row_heading level0 row2" >31</th> 
        <th id="T_a24e5f86_c129_11e7_b074_e4a4711df334level1_row2" class="row_heading level1 row2" >Trickster</th> 
        <td id="T_a24e5f86_c129_11e7_b074_e4a4711df334row2_col0" class="data row2 col0" >10</td> 
        <td id="T_a24e5f86_c129_11e7_b074_e4a4711df334row2_col1" class="data row2 col1" >$4.59</td> 
        <td id="T_a24e5f86_c129_11e7_b074_e4a4711df334row2_col2" class="data row2 col2" >$23.22</td> 
    </tr>    <tr> 
        <th id="T_a24e5f86_c129_11e7_b074_e4a4711df334level0_row3" class="row_heading level0 row3" >44</th> 
        <th id="T_a24e5f86_c129_11e7_b074_e4a4711df334level1_row3" class="row_heading level1 row3" >Bonecarvin Battle Axe</th> 
        <td id="T_a24e5f86_c129_11e7_b074_e4a4711df334row3_col0" class="data row3 col0" >9</td> 
        <td id="T_a24e5f86_c129_11e7_b074_e4a4711df334row3_col1" class="data row3 col1" >$4.36</td> 
        <td id="T_a24e5f86_c129_11e7_b074_e4a4711df334row3_col2" class="data row3 col2" >$24.04</td> 
    </tr>    <tr> 
        <th id="T_a24e5f86_c129_11e7_b074_e4a4711df334level0_row4" class="row_heading level0 row4" >154</th> 
        <th id="T_a24e5f86_c129_11e7_b074_e4a4711df334level1_row4" class="row_heading level1 row4" >Feral Katana</th> 
        <td id="T_a24e5f86_c129_11e7_b074_e4a4711df334row4_col0" class="data row4 col0" >9</td> 
        <td id="T_a24e5f86_c129_11e7_b074_e4a4711df334row4_col1" class="data row4 col1" >$4.11</td> 
        <td id="T_a24e5f86_c129_11e7_b074_e4a4711df334row4_col2" class="data row4 col2" >$23.55</td> 
    </tr></tbody> 
</table> 



## Most Profitable Items
```python
#Most Profitable Items

profitable_items = purchase_record.groupby(['Item ID','Item Name'])['Price'].agg(['count','sum','max']).sort_values('sum', ascending=False).head()
profitable_items = profitable_items.round({'sum':2,'max':2}).rename(
    columns={'count':'Purchase Count',
             'sum':'Total Purchase Value',
             'max':'Item Price'})[['Purchase Count',
                                   'Item Price',
                                   'Total Purchase Value']].style.format({'Item Price':'${:.2f}',
                                                                          'Total Purchase Value':'${:.2f}'})
profitable_items
```




<style  type="text/css" >
</style>  
<table id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Item Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="index_name level1" >Item Name</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334level0_row0" class="row_heading level0 row0" >34</th> 
        <th id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334level1_row0" class="row_heading level1 row0" >Retribution Axe</th> 
        <td id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334row0_col0" class="data row0 col0" >9</td> 
        <td id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334row0_col1" class="data row0 col1" >$4.14</td> 
        <td id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334row0_col2" class="data row0 col2" >$37.26</td> 
    </tr>    <tr> 
        <th id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334level0_row1" class="row_heading level0 row1" >107</th> 
        <th id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334level1_row1" class="row_heading level1 row1" >Splitter, Foe Of Subtlety</th> 
        <td id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334row1_col0" class="data row1 col0" >9</td> 
        <td id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334row1_col1" class="data row1 col1" >$4.15</td> 
        <td id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334row1_col2" class="data row1 col2" >$33.03</td> 
    </tr>    <tr> 
        <th id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334level0_row2" class="row_heading level0 row2" >115</th> 
        <th id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334level1_row2" class="row_heading level1 row2" >Spectral Diamond Doomblade</th> 
        <td id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334row2_col0" class="data row2 col0" >7</td> 
        <td id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334row2_col1" class="data row2 col1" >$4.25</td> 
        <td id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334row2_col2" class="data row2 col2" >$29.75</td> 
    </tr>    <tr> 
        <th id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334level0_row3" class="row_heading level0 row3" >32</th> 
        <th id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334level1_row3" class="row_heading level1 row3" >Orenmir</th> 
        <td id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334row3_col0" class="data row3 col0" >6</td> 
        <td id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334row3_col1" class="data row3 col1" >$4.95</td> 
        <td id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334row3_col2" class="data row3 col2" >$29.70</td> 
    </tr>    <tr> 
        <th id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334level0_row4" class="row_heading level0 row4" >84</th> 
        <th id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334level1_row4" class="row_heading level1 row4" >Arcane Gem</th> 
        <td id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334row4_col0" class="data row4 col0" >12</td> 
        <td id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334row4_col1" class="data row4 col1" >$4.81</td> 
        <td id="T_a2c34a3a_c129_11e7_a9e1_e4a4711df334row4_col2" class="data row4 col2" >$29.34</td> 
    </tr></tbody> 
</table> 


