# Assignment 5.1: Will the Customer Accept the Coupon?

This ReadMe file aims to provide an overview of my work on Assignment 5.1.

## Description

This assignment aims to study the proportional differences between target groups who receive a coupon while driving and trying to predict which groups will accept the coupon and those who will decline it. The dataset contains information on the delivery of five different coupons and various demographic and point-of-time values on drivers who recived the coupon offer.

## Dependencies

+ The course supplied Jupyter file with completed assignments 
+ The datafile coupons.csv.

## Assignment Summary

The assignment has three parts
  1. Prepare the dataset coupons.csv.
  2. Complete and research the scoped assignment on those who accept a bar coupon.
  3. Complete unscoped research.

### Part 1 Prepare the DataSet coupons.csv
I reviewed the data ```data.info()``` and noted there were ```12684``` records and a number of values were objects when they would be more useful as integers. I then checked for unique records that contained missing values.
```python
missing_data = data.isnull().sum()
print(missing_data[missing_data > 0])
```
```python
car                     12576
Bar                       107
CoffeeHouse               217
CarryAway                 151
RestaurantLessThan20      130
Restaurant20To50          189
```
Considering the number of missing 'car' values, I chose to drop the car column.
```python
data = data.drop('car', axis=1)
```
Considering the total number of records, the records with missing values seem few, so I calculated the percentage of unique records with missing values.

```python
unique_rows_missing_values = round(data.isnull().any(axis=1).mean() * 100, 1                                      )
```
As the number was below 5% (```4.8%```) I decdided to drop the records.
```python
data.dropna(inplace=True)
```
I then calculated the net total of records (```12079```)and assigned it to the variable ```total_records```.
```python
total_records = len(data)
```
### Addressing Dataset
As noted above, there are a number of columns where the object values would be better suited for exploration as integers. To make the DataSet more user-friendly, I changed column and value names and converting a number of objects to int64s.

Below is the result after making changest (NOTE: the changes are rather extensive, so I've left them out of this ReadMe file)

### Driver Demographic Information
* `gender` Male, Female
* `age` 20, 21, 26, 31, 36, 41, 46
* `marital_tatus` single, partner, married, divorced, widowed
* `has_children` 0, 1
* `education`	1, 2, 3, 4, 5, 6
* `occupation` A list of various profession
* `income` 	12499, 12500, 25000, 37500, 62500, 75000, 50000, 87500, 100000
* `monthly_bar_visits` -1, 0, 3, 8, 9
* `monthly_coffeehouse_visits` -1, 0, 3, 8, 9
* `monthly_takeaway_visits` -1, 0, 3, 8, 9
* `monthly_restaurant_visits_lt20` -1, 0, 3, 8, 9
* `monthly_restaurant_visits_btw20_50` -1, 0, 3, 8, 9

New Column
* `professional_status`	student, unemployed, white collar, blue collar, retired, other
  <br>Based on `occupation` and using a standar classificaiton of job roles 

### Point-of-Time Information
* `time` 700, 1000, 1200, 1400, 1800, 2200 (Note: Call as a number, ex. 7:30 as 750, 1045 as 1075, etc.)
* `destination` home, work, other
* `passenger` alone, friend, child, partner
* `weather` sunny, rainy, snowy
* `temperature`	30, 55, 80 (°F)
* `same_direction`	0, 1

New Columns
* `same_direction` 0, 1
  <br>Based on `direction_opp` & 'direction_same'
* `to_coupon_geq` 5, 15, 25
  <br>Based on `toCoupon_GEQ5min` & `toCoupon_GEQ15min` & `toCoupon_GEQ25min`

### Coupon Information
* `coupon`	restaurant < $20, coffeehouse, bar, takeaway, restaurant $20 to 50
* `expiration`	2, 24
* `to_coupon_geq`	5, 15, 25
* `accepted`	0, 1
---
### What proportion of the total observations chose to accept the coupon?
![Image](https://github.com/Michael-Stout/Assignment_5_1/blob/main/images/bar_plot_coupon.png)

| Coupon Type | Coupon Offered | Coupons Accepted | Proportion Accepted |
|---------------|---------------|---------------|---------------|
| All Coupons | 12079 | 6877 | ```0.569``` |<br>

---
### Use a bar plot to visualize the 'coupon' column.
![Image](https://github.com/Michael-Stout/Assignment_5_1/blob/main/images/bar_plot_coupon_column.png)

The proportion of acceptance of the 'coupon' column is as follows<br>

| Coupon | Count | Acceptance |
|---------------|---------------|---------------|
| takeaway | 1682 of 2280 | ```0.738```<br>
| restaurant < $20 | 1881 of 2653 | ```0.709```<br>
| coffeehouse | 1894 of 3816 | ```0.496```<br>
| restuarant $20 to 50 |632 of 1417 | ```0.446```<br>
| bar | 788 of 1913 | ```0.412```<br>

---
### Use a histogram to visualize the 'temperature' column.
![Image](https://github.com/Michael-Stout/Assignment_5_1/blob/main/images/hist_temperature.png)

| Temperature | Coupons Offered | Coupons Accepted | Proportion Accepted |
|---------------|---------------|---------------|---------------|
| 30 | ```2195``` | 1179 | 0.537 |<br>
| 55 | ```3668``` | 1967 | 0.537 |<br>
| 80 | ```6222``` | 3737 | 0.600 |<br>

---

### Part 2
### Create a new DataFrame that contains just the bar coupons.
```python
bar_df = data.loc[data['coupon'] == 'bar']
```
I used .loc to filter rows by a boolean condition for efficiency.

---
### What proportion of bar coupons were accepted?
![Imange](https://github.com/Michael-Stout/Assignment_5_1/blob/main/images/bar_plot_coupon_accepted.png)

| Coupon Type | Bar Coupon Offered | Coupons Accepted | Proportion Accepted |
|---------------|---------------|---------------|---------------|
| Bar Coupons | 1913 | 788 | ```0.412``` |<br>

---
### Compare the acceptance rate between those who went to a bar 3 or fewer times a month to those who went more.

| Group | Bar Coupon Offered | Coupons Accepted | Proportion Accepted |
|---------------|---------------|---------------|---------------|
| 3 or Less | 1913 | 641 | ```0.373``` |<br>
| 4 or More | 1913 | 147 | ```0.760``` |<br>

---
### Compare the acceptance rate between drivers who go to a bar more than once a month and are over the age of 25 to the all others. Is there a difference?

| Group | Bar Coupon Offered | Coupons Accepted | Proportion Accepted |
|---------------|---------------|---------------|---------------|
| Target Group | 403 | 278 | ```0.690``` |<br>
| All Others | 1510 | 510 | ```0.338``` |<br>

---
### Use the same process to compare the acceptance rate between drivers who go to bars more than once a month and had passengers that were not a kid and had occupations other than farming, fishing, or forestry.

| Group | Bar Coupon Offered | Coupons Accepted | Proportion Accepted |
|---------------|---------------|---------------|---------------|
| Target Group | 572 | 392 | ```0.685``` |<br>
| All Others | 1341 | 396 | ```0.295``` |<br>

---
### Compare the acceptance rates between those drivers who:
* go to bars more than once a month, had passengers that were not a kid, and were not widowed OR
* go to bars more than once a month and are under the age of 30 OR
* go to cheap restaurants more than 4 times a month and income is less than 50K.

| Group | Bar Coupon Offered | Coupons Accepted | Proportion Accepted |
|---------------|---------------|---------------|---------------|
| Target Group 1 | 1913 | 392 | ```0.205``` |<br>
| Target Group 2 | 1913 | 236 | ```0.123``` |<br>
| Target Group 3 | 1913 | 152 | ```0.080``` |<br>

---
### Based on these observations, what do you hypothesize about drivers who accepted the bar coupons?

* The overall proportion of bar coupons accepted is 41.2% (788 of 1913), suggesting half of drivers do not accept bar coupons while driving.
* Those who go to a bar four or more times a month are more likely to accept bar coupons (76%) than those who go three times a month or less (37.3%).
* Drivers who go to a bar more than once a month and are over 25 have a higher acceptance rate (69%) than all others (33.8%).
* When considering drivers who go to bars more than once a month and have passengers who are not kids and have occupations other than farming, fishing, or forestry, the acceptance rate is 68.5%, which is significantly higher than the acceptance rate for all others (29.5%).
* Comparing the acceptance rates among different groups of drivers offered a bar coupon:
  * Those who go to bars more than once a month have passengers who are not under 18 and are not widowed; the acceptance rate is 20.5%.
  * The acceptance rate is 12.3% for those who go to bars more than once a month and are under 30.
  * If one goes to cheap restaurants more than four times a month and earns less than $50,000/year, this indicates they would not accept a bar coupon;  the acceptance rate is 8.0%.

### Part 3

## The Takeaway Coupon

## Percentage of Takeaway Coupon Acceptance by Professional Status

White color professionals represent almost half of those accepting the takeaway coupon.

![Image](https://github.com/Michael-Stout/Assignment_5_1/blob/main/images/professional_status_pie.png)

## Acceptance Proportion by Professional Status

While only third in when it comes to acceptance proportion ```0.732```, the white color professionals numbers provide the most data to explore ```822/1123```.

| Group       | Takeaway Coupon Offered | Coupons Accepted | Proportion Accepted |
|-------------|--------------------|------------------|---------------------|
| Blue Collar | 267                | 226              | ```0.846```               |
| Unemployed  | 347                | 261              | ```0.752```               |
| White Collar| 1123               | 822              | ```0.732```               |
| Retired     | 84                 | 59               | ```0.702```               |
| Other       | 164                | 115              | ```0.701```               |
| Student     | 295                | 199              | ```0.675```               |

![Image](https://github.com/Michael-Stout/Assignment_5_1/blob/main/images/proportion_accepted_chart.png)

## Proportion of White Collar Professionals who Accepted Coupons by Marital Status

It would seem marital status has little effect on accepting a coupon as Single ```075```, Marrie dan dDivice all fal within two points of each other, with those with partners not far behind.

| Group    | Takeaway Coupon Offered | Coupons Accepted | Proportion Accepted |
|----------|--------------------|------------------|---------------------|
| Single   | 388                | 292              | ```0.753```                |
| Married  | 522                | 388              | ```0.743```                |
| Divorced | 60                 | 43               | ```0.717```                |
| Partner  | 153                | 99               | ```0.647```                |

![Image](https://github.com/Michael-Stout/Assignment_5_1/blob/main/images/marital_status_proportion_accepted_chart.png)

## Proportion of White Collor Professionals who Accepte Coupons by Age

Clearly, younger while collar professional from the ages 21 to 35 are safe best to target a coupon as they are all at ```1.000``` acceptance. And acceptance rates fall as age increases.

| Group     | Takeaway Coupon Offered | Coupons Accepted | Proportion Accepted |
|-----------|--------------------|------------------|---------------------|
| age 21-25 | 5                  | 5                | 1.000               |
| age 26-30 | 13                 | 13               | 1.000               |
| age 31-35 | 3                  | 3                | 1.000               |
| age 46-65 | 15                 | 13               | 0.867               |
| age 36-40 | 5                  | 3                | 0.600               |
| age 41-45 | 16                 | 9                | 0.562               |
| age 16-20 | 0                  | 0                | 0.000               |

![Image](https://github.com/Michael-Stout/Assignment_5_1/blob/main/images/age_group_proportion_accepted_chart.png)

### Discussion
* Blue-collar workers have the highest acceptance rate for takeaway coupons at 84.6%, followed closely by students at 67.5%.
* White-collar professionals have a lower acceptance rate (73.2%) compared to blue-collar workers and the unemployed but higher numbers of acceptance.
* The acceptance rate decreases with age.
* White-collar professionals aged 21-25 and 26-35 have a 100% acceptance rate and a steep decline with increased age.
* Marital status does not appear to influence coupon acceptance among white-collar professionals
* White-collar professionals, regardless of marital status, are likely to accept a takeaway coupon if they are between the ages of 21 and 35.

### Next Steps & Recommendations

It would be nice to write a script to interrogate the data with NLP
Some data could be improved such as giving the actual time, tempreature, and age rather than implied ranges.
All in all it was a good excecise and I would like to spend more time with it.
## Author

Michael Stout<br>
michael.stout@gmail.com

## Version History

* 0.1
    * Initial Release
