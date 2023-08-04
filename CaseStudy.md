```python
%load_ext sql
%sql mysql+pymysql://root:rootroot@localhost:3306/Target
```

    The sql extension is already loaded. To reload it, use:
      %reload_ext sql
    'Connected: root@Target'



### 1. Import the dataset and do usual exploratory analysis steps like checking the structure & characteristics of the dataset

#### 1.1 Data type of all columns in the "customers" table.


```sql
%%sql
Describe Target.customers
```

     * mysql+pymysql://root:***@localhost:3306/Target
    5 rows affected.





<table>
    <thead>
        <tr>
            <th>Field</th>
            <th>Type</th>
            <th>Null</th>
            <th>Key</th>
            <th>Default</th>
            <th>Extra</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>customer_id</td>
            <td>varchar(60)</td>
            <td>YES</td>
            <td></td>
            <td>None</td>
            <td></td>
        </tr>
        <tr>
            <td>customer_unique_id</td>
            <td>varchar(60)</td>
            <td>YES</td>
            <td></td>
            <td>None</td>
            <td></td>
        </tr>
        <tr>
            <td>customer_zip_code_prefix</td>
            <td>mediumint</td>
            <td>YES</td>
            <td></td>
            <td>None</td>
            <td></td>
        </tr>
        <tr>
            <td>customer_city</td>
            <td>varchar(50)</td>
            <td>YES</td>
            <td></td>
            <td>None</td>
            <td></td>
        </tr>
        <tr>
            <td>customer_state</td>
            <td>char(2)</td>
            <td>YES</td>
            <td></td>
            <td>None</td>
            <td></td>
        </tr>
    </tbody>
</table>



#### 1.2 Get the time range between which the orders were placed.


```sql
%%sql
SELECT MIN(order_purchase_timestamp) first_date, MAX(order_purchase_timestamp) last_date
from Target.orders;
```

     * mysql+pymysql://root:***@localhost:3306/Target
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>first_date</th>
            <th>last_date</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2016-09-04 21:15:19</td>
            <td>2018-10-17 17:30:18</td>
        </tr>
    </tbody>
</table>



#### 1.3. Count the Cities & States of customers who ordered during the given period.


```sql
%%sql
SELECT
  count(DISTINCT c.customer_state) states,
  count(DISTINCT c.customer_city) cities
from
  Target.orders
  join Target.customers c  USING(customer_id)

```

     * mysql+pymysql://root:***@localhost:3306/Target
    1 rows affected.





<table>
    <thead>
        <tr>
            <th>states</th>
            <th>cities</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>27</td>
            <td>4119</td>
        </tr>
    </tbody>
</table>



### 2. In-depth Exploration

#### 2.1. Is there a growing trend in the no. of orders placed over the past years?


```sql
%%sql
select
  EXTRACT( year from order_purchase_timestamp) AS year,
  count(*) order_count
from
  Target.orders
GROUP BY year
ORDER BY year

```

     * mysql+pymysql://root:***@localhost:3306/Target
    3 rows affected.





<table>
    <thead>
        <tr>
            <th>year</th>
            <th>order_count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2016</td>
            <td>329</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>45101</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>54011</td>
        </tr>
    </tbody>
</table>




```sql
%%sql
select
  EXTRACT(year from order_purchase_timestamp)  year,
  EXTRACT(month from order_purchase_timestamp) month,
  count(*) order_count
from
  Target.orders
GROUP by 
  year, month
order by 
  year, month

```

     * mysql+pymysql://root:***@localhost:3306/Target
    25 rows affected.





<table>
    <thead>
        <tr>
            <th>year</th>
            <th>month</th>
            <th>order_count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2016</td>
            <td>9</td>
            <td>4</td>
        </tr>
        <tr>
            <td>2016</td>
            <td>10</td>
            <td>324</td>
        </tr>
        <tr>
            <td>2016</td>
            <td>12</td>
            <td>1</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>1</td>
            <td>800</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>2</td>
            <td>1780</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>3</td>
            <td>2682</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>4</td>
            <td>2404</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>5</td>
            <td>3700</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>6</td>
            <td>3245</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>7</td>
            <td>4026</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>8</td>
            <td>4331</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>9</td>
            <td>4285</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>10</td>
            <td>4631</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>11</td>
            <td>7544</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>12</td>
            <td>5673</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>1</td>
            <td>7269</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>2</td>
            <td>6728</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>3</td>
            <td>7211</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>4</td>
            <td>6939</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>5</td>
            <td>6873</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>6</td>
            <td>6167</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>7</td>
            <td>6292</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>8</td>
            <td>6512</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>9</td>
            <td>16</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>10</td>
            <td>4</td>
        </tr>
    </tbody>
</table>



**Judging from above data we can see that there was a increase in order count from 2016 to 2017, but the growth was minimal from 2017 to 2018**

#### 2.2 Can we see some kind of monthly seasonality in terms of the no. of orders being placed?

**No seasonality is observed. This may be due to the size of dataset**



#### 2.3. During what time of the day, do the Brazilian customers mostly place their orders? (Dawn, Morning, Afternoon or Night)
- 0-6 hrs : Dawn
- 7-12 hrs : Mornings
- 13-18 hrs : Afternoon
- 19-23 hrs : Night


```sql
%%sql
select case
           when extract( hour from order_purchase_timestamp) BETWEEN 0 and 6 THEN 'Dawn'
           when extract( hour from order_purchase_timestamp) BETWEEN 7 and 12 THEN 'Mornings'
           when extract( hour from order_purchase_timestamp) BETWEEN 13 and 18 THEN 'Afternoon'
           else 'Night'
       end as order_time,
    count(*) order_count
from Target.orders
GROUP by order_time
ORDER BY order_count desc
```

     * mysql+pymysql://root:***@localhost:3306/Target
    4 rows affected.





<table>
    <thead>
        <tr>
            <th>order_time</th>
            <th>order_count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Afternoon</td>
            <td>38135</td>
        </tr>
        <tr>
            <td>Night</td>
            <td>28331</td>
        </tr>
        <tr>
            <td>Mornings</td>
            <td>27733</td>
        </tr>
        <tr>
            <td>Dawn</td>
            <td>5242</td>
        </tr>
    </tbody>
</table>



**Majority of customers order during the afternoon time**

### 3. Evolution of E-commerce orders in the Brazil region

#### 3.1 Get the month on month no. of orders placed in each state


```sql
%%sql
SELECT customer_state,
    EXTRACT(year from order_purchase_timestamp)  year,
    EXTRACT(month from order_purchase_timestamp) month,
    count(DISTINCT order_id) order_count
from Target.orders
    JOIN Target.customers  USING(customer_id) 
GROUP BY customer_state,
         year,month
ORDER BY customer_state,
         year,month
LIMIT 10
```

     * mysql+pymysql://root:***@localhost:3306/Target
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>customer_state</th>
            <th>year</th>
            <th>month</th>
            <th>order_count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>AC</td>
            <td>2017</td>
            <td>1</td>
            <td>2</td>
        </tr>
        <tr>
            <td>AC</td>
            <td>2017</td>
            <td>2</td>
            <td>3</td>
        </tr>
        <tr>
            <td>AC</td>
            <td>2017</td>
            <td>3</td>
            <td>2</td>
        </tr>
        <tr>
            <td>AC</td>
            <td>2017</td>
            <td>4</td>
            <td>5</td>
        </tr>
        <tr>
            <td>AC</td>
            <td>2017</td>
            <td>5</td>
            <td>8</td>
        </tr>
        <tr>
            <td>AC</td>
            <td>2017</td>
            <td>6</td>
            <td>4</td>
        </tr>
        <tr>
            <td>AC</td>
            <td>2017</td>
            <td>7</td>
            <td>5</td>
        </tr>
        <tr>
            <td>AC</td>
            <td>2017</td>
            <td>8</td>
            <td>4</td>
        </tr>
        <tr>
            <td>AC</td>
            <td>2017</td>
            <td>9</td>
            <td>5</td>
        </tr>
        <tr>
            <td>AC</td>
            <td>2017</td>
            <td>10</td>
            <td>6</td>
        </tr>
    </tbody>
</table>



#### 3.2. How are the customers distributed across all the states?


```sql
%%sql
SELECT customer_state,
    count(distinct customer_id) customer_count
from Target.customers
GROUP BY customer_state
ORDER BY customer_count desc
```

     * mysql+pymysql://root:***@localhost:3306/Target
    27 rows affected.





<table>
    <thead>
        <tr>
            <th>customer_state</th>
            <th>customer_count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>SP</td>
            <td>41746</td>
        </tr>
        <tr>
            <td>RJ</td>
            <td>12852</td>
        </tr>
        <tr>
            <td>MG</td>
            <td>11635</td>
        </tr>
        <tr>
            <td>RS</td>
            <td>5466</td>
        </tr>
        <tr>
            <td>PR</td>
            <td>5045</td>
        </tr>
        <tr>
            <td>SC</td>
            <td>3637</td>
        </tr>
        <tr>
            <td>BA</td>
            <td>3380</td>
        </tr>
        <tr>
            <td>DF</td>
            <td>2140</td>
        </tr>
        <tr>
            <td>ES</td>
            <td>2033</td>
        </tr>
        <tr>
            <td>GO</td>
            <td>2020</td>
        </tr>
        <tr>
            <td>PE</td>
            <td>1652</td>
        </tr>
        <tr>
            <td>CE</td>
            <td>1336</td>
        </tr>
        <tr>
            <td>PA</td>
            <td>975</td>
        </tr>
        <tr>
            <td>MT</td>
            <td>907</td>
        </tr>
        <tr>
            <td>MA</td>
            <td>747</td>
        </tr>
        <tr>
            <td>MS</td>
            <td>715</td>
        </tr>
        <tr>
            <td>PB</td>
            <td>536</td>
        </tr>
        <tr>
            <td>PI</td>
            <td>495</td>
        </tr>
        <tr>
            <td>RN</td>
            <td>485</td>
        </tr>
        <tr>
            <td>AL</td>
            <td>413</td>
        </tr>
        <tr>
            <td>SE</td>
            <td>350</td>
        </tr>
        <tr>
            <td>TO</td>
            <td>280</td>
        </tr>
        <tr>
            <td>RO</td>
            <td>253</td>
        </tr>
        <tr>
            <td>AM</td>
            <td>148</td>
        </tr>
        <tr>
            <td>AC</td>
            <td>81</td>
        </tr>
        <tr>
            <td>AP</td>
            <td>68</td>
        </tr>
        <tr>
            <td>RR</td>
            <td>46</td>
        </tr>
    </tbody>
</table>



**From above data we can infer that majority of the customer are from Sao Paulo**

### 4. Impact on Economy: Analyze the money movement by e-commerce by looking at order prices, freight and others. 

#### 4.1. Get the % increase in the cost of orders from year 2017 to 2018 (include months between Jan to Aug only). You can use the "payment_value" column in the payments table to get the cost of orders.


```sql
%%sql
with
  cte
  as
  (
    SELECT
      EXTRACT(month FROM order_purchase_timestamp) month,
      SUM(case when EXTRACT(YEAR FROM order_purchase_timestamp) = 2017 AND EXTRACT(month FROM order_purchase_timestamp) BETWEEN 2 AND 7 then payment_value else 0 end) data_17,
      SUM(case when EXTRACT(YEAR FROM order_purchase_timestamp) = 2018 AND EXTRACT(month FROM order_purchase_timestamp) BETWEEN 2 AND 7 then payment_value else 0 end) data_18
    FROM target.payments
      JOIN target.orders
  USING (order_id) 
    WHERE 
    EXTRACT(YEAR FROM order_purchase_timestamp) BETWEEN 2017 AND 2018 AND EXTRACT(month FROM order_purchase_timestamp) BETWEEN 2 AND 7
    GROUP BY month
    ORDER BY month
  )
SELECT
  month,
  round(((data_18 - data_17)/ data_17)*100,2) pct_chg
from cte 
```

     * mysql+pymysql://root:***@localhost:3306/Target
    6 rows affected.





<table>
    <thead>
        <tr>
            <th>month</th>
            <th>pct_chg</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2</td>
            <td>239.99</td>
        </tr>
        <tr>
            <td>3</td>
            <td>157.78</td>
        </tr>
        <tr>
            <td>4</td>
            <td>177.84</td>
        </tr>
        <tr>
            <td>5</td>
            <td>94.63</td>
        </tr>
        <tr>
            <td>6</td>
            <td>100.26</td>
        </tr>
        <tr>
            <td>7</td>
            <td>80.04</td>
        </tr>
    </tbody>
</table>



**From above data we can say that February month showed highest change in order cost while July showed the lowest**

#### 4.2. Calculate the Total & Average value of order price for each state.


```sql
%%sql
SELECT customer_state,
    round(sum(payment_value)) total_value,
    round(avg(payment_value), 2) average_value
from Target.payments
    JOIN Target.orders USING(order_id)
    JOIN Target.customers USING(customer_id)  
GROUP BY customer_state
ORDER BY average_value DESC
```

     * mysql+pymysql://root:***@localhost:3306/Target
    27 rows affected.





<table>
    <thead>
        <tr>
            <th>customer_state</th>
            <th>total_value</th>
            <th>average_value</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>PB</td>
            <td>141546.0</td>
            <td>248.33</td>
        </tr>
        <tr>
            <td>AC</td>
            <td>19681.0</td>
            <td>234.29</td>
        </tr>
        <tr>
            <td>RO</td>
            <td>60866.0</td>
            <td>233.2</td>
        </tr>
        <tr>
            <td>AP</td>
            <td>16263.0</td>
            <td>232.33</td>
        </tr>
        <tr>
            <td>AL</td>
            <td>96962.0</td>
            <td>227.08</td>
        </tr>
        <tr>
            <td>RR</td>
            <td>10065.0</td>
            <td>218.8</td>
        </tr>
        <tr>
            <td>PA</td>
            <td>218296.0</td>
            <td>215.92</td>
        </tr>
        <tr>
            <td>SE</td>
            <td>75246.0</td>
            <td>208.44</td>
        </tr>
        <tr>
            <td>PI</td>
            <td>108524.0</td>
            <td>207.11</td>
        </tr>
        <tr>
            <td>TO</td>
            <td>61485.0</td>
            <td>204.27</td>
        </tr>
        <tr>
            <td>CE</td>
            <td>279464.0</td>
            <td>199.9</td>
        </tr>
        <tr>
            <td>MA</td>
            <td>152523.0</td>
            <td>198.86</td>
        </tr>
        <tr>
            <td>RN</td>
            <td>102718.0</td>
            <td>196.78</td>
        </tr>
        <tr>
            <td>MT</td>
            <td>187029.0</td>
            <td>195.23</td>
        </tr>
        <tr>
            <td>PE</td>
            <td>324850.0</td>
            <td>187.99</td>
        </tr>
        <tr>
            <td>MS</td>
            <td>137535.0</td>
            <td>186.87</td>
        </tr>
        <tr>
            <td>AM</td>
            <td>27967.0</td>
            <td>181.6</td>
        </tr>
        <tr>
            <td>BA</td>
            <td>616646.0</td>
            <td>170.82</td>
        </tr>
        <tr>
            <td>SC</td>
            <td>623086.0</td>
            <td>165.98</td>
        </tr>
        <tr>
            <td>GO</td>
            <td>350092.0</td>
            <td>165.76</td>
        </tr>
        <tr>
            <td>DF</td>
            <td>355141.0</td>
            <td>161.13</td>
        </tr>
        <tr>
            <td>RJ</td>
            <td>2144380.0</td>
            <td>158.53</td>
        </tr>
        <tr>
            <td>RS</td>
            <td>890899.0</td>
            <td>157.18</td>
        </tr>
        <tr>
            <td>MG</td>
            <td>1872257.0</td>
            <td>154.71</td>
        </tr>
        <tr>
            <td>ES</td>
            <td>325968.0</td>
            <td>154.71</td>
        </tr>
        <tr>
            <td>PR</td>
            <td>811156.0</td>
            <td>154.15</td>
        </tr>
        <tr>
            <td>SP</td>
            <td>5998227.0</td>
            <td>137.5</td>
        </tr>
    </tbody>
</table>



**From above data we can say that max average order value is 248.44 originating from PB state**

#### 4.3. Calculate the Total & Average value of order freight for each state.


```sql
%%sql
select customer_state,
    round(sum(freight_value)) total_freight_value,
    round(avg(freight_value),2) avg_freight_value
from Target.orders
    join Target.order_items using(order_id)
    join Target.customers using(customer_id)  
group by customer_state
ORDER BY avg_freight_value DESC
```

     * mysql+pymysql://root:***@localhost:3306/Target
    27 rows affected.





<table>
    <thead>
        <tr>
            <th>customer_state</th>
            <th>total_freight_value</th>
            <th>avg_freight_value</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>RR</td>
            <td>2235.0</td>
            <td>42.98</td>
        </tr>
        <tr>
            <td>PB</td>
            <td>25720.0</td>
            <td>42.72</td>
        </tr>
        <tr>
            <td>RO</td>
            <td>11417.0</td>
            <td>41.07</td>
        </tr>
        <tr>
            <td>AC</td>
            <td>3687.0</td>
            <td>40.07</td>
        </tr>
        <tr>
            <td>PI</td>
            <td>21218.0</td>
            <td>39.15</td>
        </tr>
        <tr>
            <td>MA</td>
            <td>31524.0</td>
            <td>38.26</td>
        </tr>
        <tr>
            <td>TO</td>
            <td>11733.0</td>
            <td>37.25</td>
        </tr>
        <tr>
            <td>SE</td>
            <td>14111.0</td>
            <td>36.65</td>
        </tr>
        <tr>
            <td>AL</td>
            <td>15915.0</td>
            <td>35.84</td>
        </tr>
        <tr>
            <td>PA</td>
            <td>38699.0</td>
            <td>35.83</td>
        </tr>
        <tr>
            <td>RN</td>
            <td>18860.0</td>
            <td>35.65</td>
        </tr>
        <tr>
            <td>AP</td>
            <td>2789.0</td>
            <td>34.01</td>
        </tr>
        <tr>
            <td>AM</td>
            <td>5479.0</td>
            <td>33.21</td>
        </tr>
        <tr>
            <td>PE</td>
            <td>59450.0</td>
            <td>32.92</td>
        </tr>
        <tr>
            <td>CE</td>
            <td>48352.0</td>
            <td>32.71</td>
        </tr>
        <tr>
            <td>MT</td>
            <td>29715.0</td>
            <td>28.17</td>
        </tr>
        <tr>
            <td>BA</td>
            <td>100157.0</td>
            <td>26.36</td>
        </tr>
        <tr>
            <td>MS</td>
            <td>19144.0</td>
            <td>23.37</td>
        </tr>
        <tr>
            <td>GO</td>
            <td>53115.0</td>
            <td>22.77</td>
        </tr>
        <tr>
            <td>ES</td>
            <td>49765.0</td>
            <td>22.06</td>
        </tr>
        <tr>
            <td>RS</td>
            <td>135523.0</td>
            <td>21.74</td>
        </tr>
        <tr>
            <td>SC</td>
            <td>89660.0</td>
            <td>21.47</td>
        </tr>
        <tr>
            <td>DF</td>
            <td>50626.0</td>
            <td>21.04</td>
        </tr>
        <tr>
            <td>RJ</td>
            <td>305589.0</td>
            <td>20.96</td>
        </tr>
        <tr>
            <td>MG</td>
            <td>270853.0</td>
            <td>20.63</td>
        </tr>
        <tr>
            <td>PR</td>
            <td>117852.0</td>
            <td>20.53</td>
        </tr>
        <tr>
            <td>SP</td>
            <td>718723.0</td>
            <td>15.15</td>
        </tr>
    </tbody>
</table>



**From above data we can say that max average freight value is 42.98 originating from RR state**

### 5. Analysis based on sales, freight and delivery time

#### 5.1 Find the no. of days taken to deliver each order from the order’s purchase date as delivery time. Also, calculate the difference (in days) between the estimated & actual delivery date of an order.Do this in a single query.


```sql
%%sql
SELECT order_id,
    DATEDIFF(order_delivered_customer_date, order_purchase_timestamp) as time_to_deliver,
    DATEDIFF( order_estimated_delivery_date,  order_delivered_customer_date) as diff_estimated_delivery
from Target.orders
where
  DATEDIFF(order_delivered_customer_date, order_purchase_timestamp) IS NOT NULL
ORDER by time_to_deliver desc
LIMIT 10
```

     * mysql+pymysql://root:***@localhost:3306/Target
    10 rows affected.





<table>
    <thead>
        <tr>
            <th>order_id</th>
            <th>time_to_deliver</th>
            <th>diff_estimated_delivery</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ca07593549f1816d26a572e06dc1eab6</td>
            <td>210</td>
            <td>-181</td>
        </tr>
        <tr>
            <td>1b3190b2dfa9d789e1f14c05b647a14a</td>
            <td>208</td>
            <td>-188</td>
        </tr>
        <tr>
            <td>440d0d17af552815d15a9e41abe49359</td>
            <td>196</td>
            <td>-165</td>
        </tr>
        <tr>
            <td>2fb597c2f772eca01b1f5c561bf6cc7b</td>
            <td>195</td>
            <td>-155</td>
        </tr>
        <tr>
            <td>285ab9426d6982034523a855f55a885e</td>
            <td>195</td>
            <td>-166</td>
        </tr>
        <tr>
            <td>0f4519c5f1c541ddec9f21b3bddd533a</td>
            <td>194</td>
            <td>-161</td>
        </tr>
        <tr>
            <td>47b40429ed8cce3aee9199792275433f</td>
            <td>191</td>
            <td>-175</td>
        </tr>
        <tr>
            <td>2fe324febf907e3ea3f2aa9650869fa5</td>
            <td>190</td>
            <td>-167</td>
        </tr>
        <tr>
            <td>2d7561026d542c8dbd8f0daeadf67a43</td>
            <td>188</td>
            <td>-159</td>
        </tr>
        <tr>
            <td>c27815f7e3dd0b926b58552628481575</td>
            <td>188</td>
            <td>-162</td>
        </tr>
    </tbody>
</table>



#### 5.2.a Find out the top 5 states with the highest freight value


```sql
%%sql
SELECT customer_state Top_5,
    round(avg(freight_value), 2) as avg_freight_value
from Target.orders
    join Target.order_items using(order_id)
    join Target.customers using(customer_id)  
GROUP BY customer_state
order by avg_freight_value desc
limit 5
```

     * mysql+pymysql://root:***@localhost:3306/Target
    5 rows affected.





<table>
    <thead>
        <tr>
            <th>Top_5</th>
            <th>avg_freight_value</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>RR</td>
            <td>42.98</td>
        </tr>
        <tr>
            <td>PB</td>
            <td>42.72</td>
        </tr>
        <tr>
            <td>RO</td>
            <td>41.07</td>
        </tr>
        <tr>
            <td>AC</td>
            <td>40.07</td>
        </tr>
        <tr>
            <td>PI</td>
            <td>39.15</td>
        </tr>
    </tbody>
</table>



#### 5.2.b Find out the top 5 states with the  lowest average freight value


```sql
%%sql
SELECT customer_state Bottom_5,
    round(avg(freight_value), 2) as avg_freight_value
from Target.orders
    join Target.order_items using(order_id)
    join Target.customers using(customer_id)  
GROUP BY customer_state
order by avg_freight_value
limit 5
```

     * mysql+pymysql://root:***@localhost:3306/Target
    5 rows affected.





<table>
    <thead>
        <tr>
            <th>Bottom_5</th>
            <th>avg_freight_value</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>SP</td>
            <td>15.15</td>
        </tr>
        <tr>
            <td>PR</td>
            <td>20.53</td>
        </tr>
        <tr>
            <td>MG</td>
            <td>20.63</td>
        </tr>
        <tr>
            <td>RJ</td>
            <td>20.96</td>
        </tr>
        <tr>
            <td>DF</td>
            <td>21.04</td>
        </tr>
    </tbody>
</table>



#### 5.3.a Find out the top 5 states with the highest delivery time.


```sql
%%sql
SELECT customer_state Top_5,
    avg(DATEDIFF(order_delivered_customer_date, order_purchase_timestamp)) as avg_delivery_time
from Target.orders
    join Target.customers using(customer_id) 
GROUP BY customer_state
order by avg_delivery_time DESC
limit 5
```

     * mysql+pymysql://root:***@localhost:3306/Target
    5 rows affected.





<table>
    <thead>
        <tr>
            <th>Top_5</th>
            <th>avg_delivery_time</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>RR</td>
            <td>29.3415</td>
        </tr>
        <tr>
            <td>AP</td>
            <td>27.1791</td>
        </tr>
        <tr>
            <td>AM</td>
            <td>26.3586</td>
        </tr>
        <tr>
            <td>AL</td>
            <td>24.5013</td>
        </tr>
        <tr>
            <td>PA</td>
            <td>23.7252</td>
        </tr>
    </tbody>
</table>



#### 5.3.b Find out the top 5 states with the lowest average delivery time.


```sql
%%sql
SELECT customer_state Top_5,
    avg(DATEDIFF(order_delivered_customer_date, order_purchase_timestamp)) as avg_delivery_time
from target.orders
    join Target.customers using(customer_id) 
GROUP BY customer_state
order by avg_delivery_time
limit 5
```

     * mysql+pymysql://root:***@localhost:3306/Target
    5 rows affected.





<table>
    <thead>
        <tr>
            <th>Top_5</th>
            <th>avg_delivery_time</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>SP</td>
            <td>8.7005</td>
        </tr>
        <tr>
            <td>PR</td>
            <td>11.9380</td>
        </tr>
        <tr>
            <td>MG</td>
            <td>11.9465</td>
        </tr>
        <tr>
            <td>DF</td>
            <td>12.8990</td>
        </tr>
        <tr>
            <td>SC</td>
            <td>14.9075</td>
        </tr>
    </tbody>
</table>



#### 5.4 Find out the top 5 states where the order delivery is really fast as compared to the estimated date of delivery.


```sql
%%sql
SELECT
    customer_state,
    avg(DATEDIFF(order_estimated_delivery_date, order_purchase_timestamp) -  DATEDIFF(order_delivered_customer_date, order_purchase_timestamp)) diff
FROM target.orders
    join Target.customers using(customer_id) 
GROUP BY customer_state
ORDER BY diff
limit 5
```

     * mysql+pymysql://root:***@localhost:3306/Target
    5 rows affected.





<table>
    <thead>
        <tr>
            <th>customer_state</th>
            <th>diff</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>AL</td>
            <td>8.7078</td>
        </tr>
        <tr>
            <td>MA</td>
            <td>9.5718</td>
        </tr>
        <tr>
            <td>SE</td>
            <td>10.0209</td>
        </tr>
        <tr>
            <td>ES</td>
            <td>10.4962</td>
        </tr>
        <tr>
            <td>BA</td>
            <td>10.7945</td>
        </tr>
    </tbody>
</table>



### 6. Analysis based on the payments

#### 6.1 Find the month on month no. of orders placed using different payment types.


```python
SELECT DISTINCT payment_type from Target.payments
```


(5 row(s) affected)



Total execution time: 00:00:01.098





<table><tr><th>payment_type</th></tr><tr><td>credit_card</td></tr><tr><td>UPI</td></tr><tr><td>voucher</td></tr><tr><td>debit_card</td></tr><tr><td>not_defined</td></tr></table>




```sql
%%sql
SELECT
  EXTRACT(year from order_purchase_timestamp)  year,
  EXTRACT(month from order_purchase_timestamp) month,
  sum(case when payment_type = "credit_card" then 1 else 0 end) credit_card,
  sum(case when payment_type = "UPI" then 1 else 0 end) UPI,
  sum(case when payment_type = "voucher" then 1 else 0 end) voucher,
  sum(case when payment_type = "debit_card" then 1 else 0 end) debit_card,
  sum(case when payment_type = "not_defined" then 1 else 0 end) not_defined
from
  Target.orders
  join Target.payments using(order_id) 
GROUP BY 
  year,month
ORDER BY 
  year,month
```

     * mysql+pymysql://root:***@localhost:3306/Target
    25 rows affected.





<table>
    <thead>
        <tr>
            <th>year</th>
            <th>month</th>
            <th>credit_card</th>
            <th>UPI</th>
            <th>voucher</th>
            <th>debit_card</th>
            <th>not_defined</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2016</td>
            <td>9</td>
            <td>3</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2016</td>
            <td>10</td>
            <td>254</td>
            <td>63</td>
            <td>23</td>
            <td>2</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2016</td>
            <td>12</td>
            <td>1</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>1</td>
            <td>583</td>
            <td>197</td>
            <td>61</td>
            <td>9</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>2</td>
            <td>1356</td>
            <td>398</td>
            <td>119</td>
            <td>13</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>3</td>
            <td>2016</td>
            <td>590</td>
            <td>200</td>
            <td>31</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>4</td>
            <td>1846</td>
            <td>496</td>
            <td>202</td>
            <td>27</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>5</td>
            <td>2853</td>
            <td>772</td>
            <td>289</td>
            <td>30</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>6</td>
            <td>2463</td>
            <td>707</td>
            <td>239</td>
            <td>27</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>7</td>
            <td>3086</td>
            <td>845</td>
            <td>364</td>
            <td>22</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>8</td>
            <td>3284</td>
            <td>938</td>
            <td>294</td>
            <td>34</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>9</td>
            <td>3283</td>
            <td>903</td>
            <td>287</td>
            <td>43</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>10</td>
            <td>3524</td>
            <td>993</td>
            <td>291</td>
            <td>52</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>11</td>
            <td>5897</td>
            <td>1509</td>
            <td>387</td>
            <td>70</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2017</td>
            <td>12</td>
            <td>4377</td>
            <td>1160</td>
            <td>294</td>
            <td>64</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>1</td>
            <td>5520</td>
            <td>1518</td>
            <td>416</td>
            <td>109</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>2</td>
            <td>5253</td>
            <td>1325</td>
            <td>305</td>
            <td>69</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>3</td>
            <td>5691</td>
            <td>1352</td>
            <td>391</td>
            <td>78</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>4</td>
            <td>5455</td>
            <td>1287</td>
            <td>370</td>
            <td>97</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>5</td>
            <td>5497</td>
            <td>1263</td>
            <td>324</td>
            <td>51</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>6</td>
            <td>4813</td>
            <td>1100</td>
            <td>324</td>
            <td>182</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>7</td>
            <td>4755</td>
            <td>1229</td>
            <td>281</td>
            <td>242</td>
            <td>0</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>8</td>
            <td>4985</td>
            <td>1139</td>
            <td>295</td>
            <td>277</td>
            <td>2</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>9</td>
            <td>0</td>
            <td>0</td>
            <td>15</td>
            <td>0</td>
            <td>1</td>
        </tr>
        <tr>
            <td>2018</td>
            <td>10</td>
            <td>0</td>
            <td>0</td>
            <td>4</td>
            <td>0</td>
            <td>0</td>
        </tr>
    </tbody>
</table>



**From above data we can infer that majority of users use credit card for purchasing**

#### 6.2 Find the no. of orders placed on the basis of the payment installments that have been paid.


```sql
%%sql
SELECT
  payment_installments, COUNT(order_id) AS order_count
from Target.payments
  join Target.orders using(order_id) 
where order_status != 'canceled'
GROUP BY payment_installments
ORDER BY order_count DESC;
```

     * mysql+pymysql://root:***@localhost:3306/Target
    24 rows affected.





<table>
    <thead>
        <tr>
            <th>payment_installments</th>
            <th>order_count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>52184</td>
        </tr>
        <tr>
            <td>2</td>
            <td>12353</td>
        </tr>
        <tr>
            <td>3</td>
            <td>10392</td>
        </tr>
        <tr>
            <td>4</td>
            <td>7056</td>
        </tr>
        <tr>
            <td>10</td>
            <td>5292</td>
        </tr>
        <tr>
            <td>5</td>
            <td>5209</td>
        </tr>
        <tr>
            <td>8</td>
            <td>4239</td>
        </tr>
        <tr>
            <td>6</td>
            <td>3898</td>
        </tr>
        <tr>
            <td>7</td>
            <td>1620</td>
        </tr>
        <tr>
            <td>9</td>
            <td>638</td>
        </tr>
        <tr>
            <td>12</td>
            <td>133</td>
        </tr>
        <tr>
            <td>15</td>
            <td>74</td>
        </tr>
        <tr>
            <td>18</td>
            <td>27</td>
        </tr>
        <tr>
            <td>11</td>
            <td>22</td>
        </tr>
        <tr>
            <td>24</td>
            <td>18</td>
        </tr>
        <tr>
            <td>20</td>
            <td>17</td>
        </tr>
        <tr>
            <td>13</td>
            <td>15</td>
        </tr>
        <tr>
            <td>14</td>
            <td>15</td>
        </tr>
        <tr>
            <td>17</td>
            <td>8</td>
        </tr>
        <tr>
            <td>16</td>
            <td>5</td>
        </tr>
        <tr>
            <td>21</td>
            <td>3</td>
        </tr>
        <tr>
            <td>0</td>
            <td>2</td>
        </tr>
        <tr>
            <td>22</td>
            <td>1</td>
        </tr>
        <tr>
            <td>23</td>
            <td>1</td>
        </tr>
    </tbody>
</table>



**From above data we can infer that majority of the payments are done as single installments and max installment is 23**
