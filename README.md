# E-Commerce-Insights: A Comprehensive Analysis of Orders, Customers, and Reviews.

## Overview

This project presents a comprehensive analysis of an e-commerce database using PostgreSQL. It covers various aspects including orders, customers and reviews. Through SQL queries and data exploration, valuable insights have been derived to understand the dynamics of the e-commerce platform.

## Database Details

The database is a Brazilian ecommerce public dataset of orders made at Olist Store which is available in kaggle (https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce/data) and it has information of 100k orders from 2016 to 2018 made at multiple marketplaces in Brazil. This dataset contains real commercial data that has undergone anonymization.

Below is the screenshot of all the tables in the dataset and how they relate to each other.

![Screen Shot 2024-05-14 at 7 10 16 PM](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/ef6d7dc8-a825-40b9-bbd3-9213cf8e889e)

## 1. Orders


### 1.1 Total orders made by customers

<img width="320" alt="Total_Orders_Made_by_customers" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/dee59631-0070-48d5-9ed6-f2a631d51f29">


![ord-1](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/307c186f-f532-41e8-b513-42f7f2a36881)


### 1.2 Average order price

<img width="319" alt="Average_order_price" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/2c52098f-d0e5-4245-9a8f-b18f6d43642d">


![ord-2](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/3b68d2e2-8c34-45eb-8bfe-cf4fadb83e40)


### 1.3 Cities with most Orders

<img width="597" alt="Cities with most Orders" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/423047e6-62b1-49c8-9539-b375bf46c951">


![ord-3](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/9148f396-9376-4af7-a5be-4ae73df80e2b)


### 1.4 Cities with its average order value

<img width="781" alt="Cities with its average order value" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/d920c29d-4483-473f-b82d-897162ddba5f">


![ord-4](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/460db4f2-abf2-4e00-8806-4541531037ff)


## 2. Customers


### 2.1 Total Customers

<img width="348" alt="Total_customers" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/4c802321-c5b3-4a52-b4a3-ec43e4c98f0f">


![cust-1](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/b4182ea0-1bb2-472d-9f8b-86d1bf0ae2bc)


### 2.2 Customer segmentation based on purchase frequency

<img width="1027" alt="Customer segmentation based on purchase frequency" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/389ebe46-23bd-41a8-950e-59bd0a933b76">


![cust-2](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/5058e8c1-8d47-4600-bc08-17797164549d)


### 2.3 Average order price for each customer segment

<img width="1040" alt="Average order price for each customer segment" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/08eee74c-4b10-4a94-a036-d6e2cb476eb8">


![cust-3](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/7173841e-d7e7-4ff7-b569-f408fcae4a9a)


### 2.4 Frequently purchased product category by "Loyal Customers"

<img width="1029" alt="Frequently purchased product category by  Loyal Customers" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/15b907b3-00e7-4711-8098-8f4c68e97fdf">


![cust-4](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/deb7d241-5be3-4982-ba39-8ef843af2f55)


### 2.5 Top 3 product categories across all customer segments

<img width="1276" alt="Top 3 product categories across all customer segments" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/f7ebe3d1-f484-4002-a236-d52329f12154">


![cust-5](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/6a7b8d57-d31c-4851-865c-fba9136ecbd9)


### 2.6 Product categories that are particularly popular with specific customer segments but not others

The above result analysis reveals that "bed_bath_table" and "sports_leisure" are popular across all customer segments, while "furniture_decor" is favored notably by loyal customers and moderate buyers.
Conversely, "health_beauty" appears to be a preference primarily among first-time buyers and is not as commonly purchased among the other two groups.


## 3. Order reviews


### 3.1 Total review_score counts by star

<img width="468" alt="Total review_score counts by star" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/0e3d1186-9c90-47cb-ac0a-510aefd7d736">


![rev-1](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/901712dd-8cd8-425f-8541-e2fbb31104fc)


### 3.2 Top 20 products with highest number of 5-star reviews

<img width="705" alt="Top 20 products with highest number of 5-star reviews" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/3d1e8cfc-c2d2-4feb-81ea-71cfe24d9cf8">


![rev-2](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/0acbb3b5-3e94-43aa-ad03-69be40c38432)


### 3.3 Top 20 products with highest number of 1-star reviews

<img width="740" alt="Top 20 products with highest number of 1-star reviews" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/cacae23f-edd3-456e-9e0f-ff24d132cd10">


![rev-3](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/7c9de283-06f6-4768-888e-8a9a590d5e47)


### 3.4 Top 20 sellers with highest number of 5 star rating

<img width="682" alt="Top 20 sellers with highest number of 5 star rating" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/2b7a2c4b-ca63-457e-bcf5-a6ce7ea15ca4">


![rev-4](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/a4546fc9-2872-4eb3-9da2-7a7b10b1dafb)


### 3.5 Top 20 sellers with highest number of 1 star rating

<img width="691" alt="Top 20 sellers with highest number of 1 star rating" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/65cc0581-8422-4f08-b539-895ecf24be49">


![rev-5](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/ecb5cbeb-b6f1-4012-8300-68d56b6aeb0b)


### 3.6 Breakdown of Star Ratings Based on Delivery Timeliness: No Delay, 1-5 Days Delay, 6-10 Days Delay, and Over 10 Days Delay.

![Breakdown of Star Ratings Based on Delivery Timeliness](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/32ae1b12-0047-4d87-88e6-2003883129f2)


![rev-6](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/c07725a1-961b-4587-8862-f3a341d9061c)


As observed in the results, an increase in the gap between the actual delivery date and the estimated date corresponds to a higher count of negative reviews, while a decrease in this gap leads to fewer negative reviews.
