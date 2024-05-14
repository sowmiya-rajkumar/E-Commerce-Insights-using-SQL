# E-Commerce-Insights: A Comprehensive Analysis of Orders, Customers, and Reviews.

## Overview

This project presents a comprehensive analysis of an e-commerce database using PostgreSQL. It covers various aspects including orders, customers and reviews. Through SQL queries and data exploration, valuable insights have been derived to understand the dynamics of the e-commerce platform.

## Database Details

The database is a Brazilian ecommerce public dataset of orders made at Olist Store which is available in kaggle (https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce/data) and it has information of 100k orders from 2016 to 2018 made at multiple marketplaces in Brazil. This dataset contains real commercial data that has undergone anonymization.

Below is the screenshot of all the tables in the dataset and how they relate to each other.

![Screen Shot 2024-05-14 at 7 10 16 PM](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/ef6d7dc8-a825-40b9-bbd3-9213cf8e889e)

## Orders


### Total orders made by customers

<img width="320" alt="Total_Orders_Made_by_customers" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/dee59631-0070-48d5-9ed6-f2a631d51f29">



### Average order price

<img width="319" alt="Average_order_price" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/2c52098f-d0e5-4245-9a8f-b18f6d43642d">



### Cities with most Orders

<img width="597" alt="Cities with most Orders" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/423047e6-62b1-49c8-9539-b375bf46c951">



### Cities with its average order value

<img width="781" alt="Cities with its average order value" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/d920c29d-4483-473f-b82d-897162ddba5f">



## Customers


### Total Customers

<img width="348" alt="Total_customers" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/4c802321-c5b3-4a52-b4a3-ec43e4c98f0f">



### Customer segmentation based on purchase frequency

<img width="1027" alt="Customer segmentation based on purchase frequency" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/389ebe46-23bd-41a8-950e-59bd0a933b76">


### Average order price for each customer segment

<img width="1040" alt="Average order price for each customer segment" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/08eee74c-4b10-4a94-a036-d6e2cb476eb8">


### Frequently purchased product category by "Loyal Customers"

<img width="1029" alt="Frequently purchased product category by  Loyal Customers" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/15b907b3-00e7-4711-8098-8f4c68e97fdf">



### Top 3 product categories across all customer segments

<img width="1276" alt="Top 3 product categories across all customer segments" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/f7ebe3d1-f484-4002-a236-d52329f12154">



### Product categories that are particularly popular with specific customer segments but not others

The above result analysis reveals that "bed_bath_table" and "sports_leisure" are popular across all customer segments, while "furniture_decor" is favored notably by loyal customers and moderate buyers.
Conversely, "health_beauty" appears to be a preference primarily among first-time buyers and is not as commonly purchased among the other two groups.


## Order reviews


### Total review_score counts by star

<img width="468" alt="Total review_score counts by star" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/0e3d1186-9c90-47cb-ac0a-510aefd7d736">


### Top 20 products with highest number of 5-star reviews

<img width="705" alt="Top 20 products with highest number of 5-star reviews" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/3d1e8cfc-c2d2-4feb-81ea-71cfe24d9cf8">



### Top 20 products with highest number of 1-star reviews

<img width="740" alt="Top 20 products with highest number of 1-star reviews" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/cacae23f-edd3-456e-9e0f-ff24d132cd10">



### Top 20 sellers with highest number of 5 star rating

<img width="682" alt="Top 20 sellers with highest number of 5 star rating" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/2b7a2c4b-ca63-457e-bcf5-a6ce7ea15ca4">



### Top 20 sellers with highest number of 1 star rating

<img width="691" alt="Top 20 sellers with highest number of 1 star rating" src="https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/65cc0581-8422-4f08-b539-895ecf24be49">



### Breakdown of Star Ratings Based on Delivery Timeliness: No Delay, 1-5 Days Delay, 6-10 Days Delay, and Over 10 Days Delay.

![Breakdown of Star Ratings Based on Delivery Timeliness](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/32ae1b12-0047-4d87-88e6-2003883129f2)


As observed in the results, an increase in the gap between the actual delivery date and the estimated date corresponds to a higher count of negative reviews, while a decrease in this gap leads to fewer negative reviews.
