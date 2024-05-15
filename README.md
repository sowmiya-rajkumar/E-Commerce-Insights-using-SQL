# E-Commerce-Insights: A Comprehensive Analysis of Orders, Customers, and Reviews.

## Overview

This project presents a comprehensive analysis of an e-commerce database using PostgreSQL. It covers various aspects including orders, customers and reviews. Through SQL queries and data exploration, valuable insights have been derived to understand the dynamics of the e-commerce platform.

## Database Details

The database is a Brazilian ecommerce public dataset of orders made at Olist Store which is available in kaggle (https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce/data) and it has information of 100k orders from 2016 to 2018 made at multiple marketplaces in Brazil. This dataset contains real commercial data that has undergone anonymization.

Below is the screenshot of all the tables in the dataset and how they relate to each other.

![Screen Shot 2024-05-14 at 7 10 16 PM](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/ef6d7dc8-a825-40b9-bbd3-9213cf8e889e)

## 1. Orders


### 1.1 Total orders made by customers

```
SELECT COUNT(*) total_orders
FROM orders;
```

#### **Result:**
![ord-1](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/307c186f-f532-41e8-b513-42f7f2a36881)


### 1.2 Average order price

<img width="319" alt="Average_order_price" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/2c52098f-d0e5-4245-9a8f-b18f6d43642d">

#### **Result:**
![ord-2](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/3b68d2e2-8c34-45eb-8bfe-cf4fadb83e40)


### 1.3 Cities with most Orders

<img width="597" alt="Cities with most Orders" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/423047e6-62b1-49c8-9539-b375bf46c951">

#### **Result:**
![ord-3](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/9148f396-9376-4af7-a5be-4ae73df80e2b)


### 1.4 Cities with its average order value

<img width="781" alt="Cities with its average order value" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/d920c29d-4483-473f-b82d-897162ddba5f">

#### **Result:**
![ord-4](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/460db4f2-abf2-4e00-8806-4541531037ff)


## 2. Customers


### 2.1 Total Customers

<img width="348" alt="Total_customers" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/4c802321-c5b3-4a52-b4a3-ec43e4c98f0f">

#### **Result:**
![cust-1](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/b4182ea0-1bb2-472d-9f8b-86d1bf0ae2bc)


### 2.2 Customer segmentation based on purchase frequency

Segmented customers based on the number of times they made a purchase and categorized them into "First-time Buyers", "Moderate Buyers" and "Loyal Customers". <br />
<br />
**Query explanation:**<br />
1. Count the number of times each customer made a purchase <br />
2. Segment the customers based on the purchase frequency


<img width="1027" alt="Customer segmentation based on purchase frequency" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/389ebe46-23bd-41a8-950e-59bd0a933b76">

#### **Result:**
![cust-2](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/5058e8c1-8d47-4600-bc08-17797164549d)


### 2.3 Average order price for each customer segment

<img width="1040" alt="Average order price for each customer segment" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/08eee74c-4b10-4a94-a036-d6e2cb476eb8">

#### **Result:**
![cust-3](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/7173841e-d7e7-4ff7-b569-f408fcae4a9a)


### 2.4 Frequently purchased product category by "Loyal Customers"

<img width="1029" alt="Frequently purchased product category by  Loyal Customers" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/15b907b3-00e7-4711-8098-8f4c68e97fdf">

#### **Result:**
![cust-4](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/deb7d241-5be3-4982-ba39-8ef843af2f55)


### 2.5 Top 3 product categories across all customer segments

<img width="1276" alt="Top 3 product categories across all customer segments" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/f7ebe3d1-f484-4002-a236-d52329f12154">

**Query explanation:** <br />
1. Count the number of times a product category is bought by each customer segment. <br />
2. Using DENSE_RANK() get the top 3 product categories.
   
#### **Result:**
![cust-5](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/6a7b8d57-d31c-4851-865c-fba9136ecbd9)


### 2.6 Product categories that are particularly popular with specific customer segments but not others

The above result analysis reveals that "bed_bath_table" and "sports_leisure" are popular across all customer segments, while "furniture_decor" is favored notably by loyal customers and moderate buyers.
Conversely, "health_beauty" appears to be a preference primarily among first-time buyers and is not as commonly purchased among the other two groups.


## 3. Reviews


### 3.1 Total review_score counts by star

<img width="468" alt="Total review_score counts by star" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/0e3d1186-9c90-47cb-ac0a-510aefd7d736">

#### **Result:**
![rev-1](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/901712dd-8cd8-425f-8541-e2fbb31104fc)


### 3.2 Top 20 products with highest number of 5-star reviews

<img width="705" alt="Top 20 products with highest number of 5-star reviews" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/3d1e8cfc-c2d2-4feb-81ea-71cfe24d9cf8">

#### **Result:**
![rev-2](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/0acbb3b5-3e94-43aa-ad03-69be40c38432)


### 3.3 Top 20 products with highest number of 1-star reviews

<img width="740" alt="Top 20 products with highest number of 1-star reviews" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/cacae23f-edd3-456e-9e0f-ff24d132cd10">

#### **Result:**
![rev-3](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/7c9de283-06f6-4768-888e-8a9a590d5e47)


### 3.4 Breakdown of Star Ratings Based on Delivery Timeliness: No Delay, 1-5 Days Delay, 6-10 Days Delay, and Over 10 Days Delay.

![Breakdown of Star Ratings Based on Delivery Timeliness](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/32ae1b12-0047-4d87-88e6-2003883129f2)

**Query explanation:** <br />
1. cte - actual_estimation_diff -> created a column that shows the difference between the estimated and actual delivery date. <br />
2. cte - hours_conversion -> converted the difference from seconds to hours. <br />
3. cte - grouped_diff -> Categorizing the delivery time as "No delay", "Delayed 1 - 5 days", "Delayed 6 - 10 days" and "Greater than 10 days". <br />
4. cte - segmented_days -> Counts the total number of reviews for each combination of review score and delivery difference category. <br />
5. Main Query -> The main query aggregates the counts of reviews for each combination of delivery difference category and review score. <br />
              -> It uses conditional aggregation (MAX(CASE...)) to pivot the data, organizing it by delivery difference category and review score. <br />
              -> The results are ordered based on the delivery difference categories. <br />
6. Outer Query -> The outer query retrieves the results from the view . and calculates the percentage distribution of each review score within each delivery difference category.

**Result:**
![rev-6](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/c07725a1-961b-4587-8862-f3a341d9061c)


As observed in the results, an increase in the gap between the actual delivery date and the estimated date corresponds to a higher count of negative reviews, while a decrease in this gap leads to fewer negative reviews.


## 4. Sellers  


### 4.1 Top 20 sellers with highest number of 5 star rating

<img width="682" alt="Top 20 sellers with highest number of 5 star rating" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/2b7a2c4b-ca63-457e-bcf5-a6ce7ea15ca4">

#### **Result:**
![rev-4](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/a4546fc9-2872-4eb3-9da2-7a7b10b1dafb)


### 4.2 Top 20 sellers with highest number of 1 star rating

<img width="691" alt="Top 20 sellers with highest number of 1 star rating" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/65cc0581-8422-4f08-b539-895ecf24be49">

#### **Result:**
![rev-5](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/ecb5cbeb-b6f1-4012-8300-68d56b6aeb0b)


### 4.3 From these Top 20 sellers with highest number of 1 star rating, find the top 3 products that gives them the highest negative rating

![tot20-top3](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/3b9ed6e6-f711-4122-a9aa-70693ed7298a)

**Query explanation:** <br />
1. cte - low_rating_sellers -> Identifies the top 20 sellers with the highest number of 1-star reviews by counting the occurrences of 1-star reviews for each seller. <br />
2. cte - seller_products_lowrate -> Retrieves the order details (order_id, product_id, review_score) for each product associated with the sellers identified in the previous CTE.<br />
3. cte - ranking_products -> Counts the occurrences of each product associated with the low-rated sellers<br />
4. cte - ranking -> Assigns ranks to the products within each seller group based on their counts using the DENSE_RANK() window function.<br />
5. Main query - a View is created that selects all the columns from the cte "ranking" and a "WHERE" clause to filter only the top 3 products for each seller based on their ranks.

   
#### **Result:**
![sell3](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/4cdf8018-6afa-4d31-beda-2d7c45db8e64)


### 4.4 Analysing the reasons behind the negative reviews received by these products from the specific sellers

![analysing reasons](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/237e11a0-1b1b-483b-b247-8aeee5ace84d)

This script sets up a view that provides information on the difference between estimated and actual delivery dates, freight values, price, etc for specific seller-product combinations, facilitating further analysis and decision-making based on delivery performance and freight value metrics.

#### **Result**
![sell4](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/4d63a6cb-205e-4192-a6d4-c60e4185e51e)


### 4.5 Analysing the negative reviews from delayed delivery perspective

![delay-del](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/7e68770f-f72c-427c-90ad-ee8cc887cd2d)

#### **Result**
![sell5](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/ba5d3337-f04d-486c-99b1-a1aae8988a4b)


The findings indicate that the top 3 products from each top 20 sellers receiving 1-star reviews are not primarily attributed to delivery delays, as the average difference between the estimated and delivered dates are all positive.

### 4.6 Analysing the negative reviews from shipping cost(freight_value) perspective

![freight](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/e2870cda-92a5-410a-b92a-faf51677fb37)

**Query Explanation:** <br />
1. cte - prod_from_lowrating_sellers_freight -> Calculates the average freight value for products associated with sellers receiving low ratings. <br />
2. cte - same_prod_freightval_when_5star - > Calculates the average freight value for the same products where it received 5-star ratings. <br />
3. Main query -> selects the product_id, average freight value, and average freight value when the product received 5-star ratings from the two CTEs.  <br />

In summary, this query aims to identify any discrepancies in freight values for products associated with low-rated sellers compared to the same products when they received high ratings. This analysis can help uncover whether freight value contributes to the low ratings received by these sellers' products.

#### **Result**
![sell6](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/7e1a67b7-be91-451f-aa66-a92d221cb31d)

- The findings suggest that negative reviews cannot be solely attributed to freight values, as the same products with 5-star ratings show similar freight values. <br />
- Thus, neither delivery delays nor freight values appear to be significant factors contributing to the negative ratings of these products. However, with access to additional data    such as product quality, review descriptions, customer service issues, or any other potential reasons for dissatisfaction, a deeper analysis could be conducted to identify the root cause of the negative reviews.
- Then the findings can be shared with relevant teams within the e-commerce platform, such as product management, customer support, or fulfillment teams. This information can help    them address specific issues and improve overall customer satisfaction.


Thank you for taking the time to explore this project! <br />

If you have any questions, suggestions, or just want to say hello, feel free to reach out to me via [email](sowmiya.rajkumar2021@gmail.com).
