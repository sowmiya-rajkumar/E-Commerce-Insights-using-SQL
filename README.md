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

```
SELECT AVG(price) average_price
FROM order_items;
```

#### **Result:**
![ord-2](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/3b68d2e2-8c34-45eb-8bfe-cf4fadb83e40)


### 1.3 Cities with most Orders

```
SELECT customer_city, COUNT(order_id) order_count
FROM orders o JOIN customers c ON o.customer_id = c.customer_id
GROUP BY customer_city
ORDER BY order_count DESC;
```

#### **Result:**
![ord-3](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/9148f396-9376-4af7-a5be-4ae73df80e2b)


### 1.4 Cities with its average order value

```
SELECT customer_city, ROUND(CAST(AVG(payment_value) AS numeric),2) AS avg_order_value
FROM orders o JOIN customers c ON o.customer_id = c.customer_id
				JOIN order_payments op ON o.order_id = op.order_id
GROUP BY customer_city
ORDER BY avg_order_value DESC;
```

#### **Result:**
![ord-4](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/460db4f2-abf2-4e00-8806-4541531037ff)


## 2. Customers


### 2.1 Total Customers

```
SELECT COUNT(*) total_customer_count
FROM customers;
```

#### **Result:**
![cust-1](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/b4182ea0-1bb2-472d-9f8b-86d1bf0ae2bc)


### 2.2 Customer segmentation based on purchase frequency

Segmented customers based on the number of times they made a purchase and categorized them into "First-time Buyers", "Moderate Buyers" and "Loyal Customers". <br />
<br />
**Query explanation:**<br />
1. Count the number of times each customer made a purchase <br />
2. Segment the customers based on the purchase frequency


```
CREATE VIEW customer_segment_purchase_frequency AS
	WITH order_count AS (SELECT customer_unique_id, COUNT(order_purchase_timestamp) total_orders_per_customer
						FROM customers c JOIN orders o ON c.customer_id=o.customer_id
						GROUP BY customer_unique_id
						ORDER BY total_orders_per_customer DESC)
	SELECT oc.customer_unique_id, total_orders_per_customer, customer_id, 
				CASE
				WHEN total_orders_per_customer > 6 THEN 'Loyal Customers'
				WHEN total_orders_per_customer > 1 AND total_orders_per_customer < 6 THEN 'Moderate Buyers'
				ELSE 'First time Buyers'
				END AS customer_purchase_frequency
	FROM order_count oc JOIN customers c ON oc.customer_unique_id = c.customer_unique_id;

SELECT *
FROM customer_segment_purchase_frequency;
```

#### **Result:**
![cust-2](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/5058e8c1-8d47-4600-bc08-17797164549d)


### 2.3 Average order price for each customer segment

```
SELECT customer_purchase_frequency, ROUND(CAST(AVG(payment_value) AS numeric), 2) average_price_per_cust_segment
FROM customer_segment_purchase_frequency seg_freq 
					JOIN orders o ON o.customer_id = seg_freq.customer_id
					JOIN order_payments op ON op.order_id = o.order_id
GROUP BY customer_purchase_frequency;
```

#### **Result:**
![cust-3](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/7173841e-d7e7-4ff7-b569-f408fcae4a9a)


### 2.4 Frequently purchased product category by "Loyal Customers"

```
SELECT pcn.product_category_name_english, COUNT(*) total_by_Loyal_Customers
FROM customer_segment_purchase_frequency cspf 
								JOIN orders o ON cspf.customer_id = o.customer_id
								JOIN order_items oi ON o.order_id = oi.order_id
								JOIN products p ON oi.product_id = p.product_id
								JOIN product_category_name pcn ON p.product_category_name = pcn.product_category_name
WHERE customer_purchase_frequency = 'Loyal Customers'
GROUP BY pcn.product_category_name_english
ORDER BY total_by_Loyal_Customers DESC LIMIT 1;
```

#### **Result:**
![cust-4](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/deb7d241-5be3-4982-ba39-8ef843af2f55)


### 2.5 Top 3 product categories across all customer segments

```
WITH customer_seg_prod_counts AS (SELECT customer_purchase_frequency, pcn.product_category_name_english, COUNT(*) counts
								FROM customer_segment_purchase_frequency cspf 
																JOIN orders o ON cspf.customer_id = o.customer_id
																JOIN order_items oi ON o.order_id = oi.order_id
																JOIN products p ON oi.product_id = p.product_id
																JOIN product_category_name pcn ON p.product_category_name = pcn.product_category_name
								GROUP BY customer_purchase_frequency, pcn.product_category_name_english
								ORDER BY counts DESC),
	prod_count_ranking AS (SELECT *, DENSE_RANK() OVER(PARTITION BY customer_purchase_frequency ORDER BY counts DESC) as counts_rank
				FROM customer_seg_prod_counts)
SELECT *
FROM prod_count_ranking
WHERE counts_rank <=3;
```

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

```
SELECT review_score as star, COUNT(*) AS star_count
FROM order_reviews
GROUP BY review_score
ORDER bY review_score DESC;
```

#### **Result:**
![rev-1](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/901712dd-8cd8-425f-8541-e2fbb31104fc)


### 3.2 Top 20 products with highest number of 5-star reviews

```
SELECT p.product_id, COUNT(*) counts
FROM order_items oite JOIN order_reviews orev ON oite.order_id = orev.order_id
						JOIN products p ON oite.product_id = p.product_id
WHERE review_score = 5
GROUP BY p.product_id
ORDER BY counts DESC LIMIT 20;
```

#### **Result:**
![rev-2](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/0acbb3b5-3e94-43aa-ad03-69be40c38432)


### 3.3 Top 20 products with highest number of 1-star reviews

```
SELECT p.product_id, COUNT(*) counts
FROM order_items oite JOIN order_reviews orev ON oite.order_id = orev.order_id
						JOIN products p ON oite.product_id = p.product_id
WHERE review_score = 1
GROUP BY p.product_id
ORDER BY counts DESC LIMIT 20;
```

#### **Result:**
![rev-3](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/7c9de283-06f6-4768-888e-8a9a590d5e47)


### 3.4 Breakdown of Star Ratings Based on Delivery Timeliness: No Delay, 1-5 Days Delay, 6-10 Days Delay, and Over 10 Days Delay.

```
CREATE VIEW star_review_count_on_actual_estimate_diff AS
	WITH actual_estimation_diff AS(SELECT *, order_estimated_delivery_date - order_delivered_customer_date 
																AS diff_estimation_delivered
								FROM orders),
		hours_conversion AS (SELECT orev.order_id, customer_id, diff_estimation_delivered, review_score, 
										(EXTRACT(epoch FROM diff_estimation_delivered)/3600) :: int AS tot_hours_difference
							FROM actual_estimation_diff aed JOIN order_reviews orev ON aed.order_id = orev.order_id),
		grouped_diff AS (SELECT *, CASE
										WHEN tot_hours_difference >= 0 THEN 'No delay'
										WHEN tot_hours_difference < 0 AND tot_hours_difference >= -120 THEN 'Delayed 1 - 5 days'
										WHEN tot_hours_difference < -120 AND tot_hours_difference >= -240 THEN 'Delayed 6 - 10 days'
										ELSE 'Greater than 10 days'
										END AS delivered_difference
							FROM hours_conversion),
		segmented_days AS (SELECT delivered_difference, review_score, COUNT(*) total
								FROM grouped_diff
								GROUP BY delivered_difference, review_score)
	SELECT delivered_difference,
		MAX (CASE WHEN review_score = 5 THEN total END) AS Five_star,
		MAX (CASE WHEN review_score = 4 THEN total END) AS Four_star,
		MAX (CASE WHEN review_score = 3 THEN total END) AS Three_star,
		MAX (CASE WHEN review_score = 2 THEN total END) AS Two_star,
		MAX (CASE WHEN review_score = 1 THEN total END) AS One_star
	FROM segmented_days
	GROUP BY delivered_difference
	ORDER BY CASE
				WHEN delivered_difference = 'No delay' THEN 1
				WHEN delivered_difference = 'Delayed 1 - 5 days' THEN 2
				WHEN delivered_difference = 'Delayed 6 - 10 days' THEN 3
				ELSE 4
			END;


SELECT delivered_difference AS delay_difference,
		CAST(ROUND((five_star*1.0/(five_star+four_star+three_star+two_star+one_star))*100, 2) AS varchar(5)) ||'%' AS Five_star,
		CAST(ROUND((four_star*1.0/(five_star+four_star+three_star+two_star+one_star))*100, 2) AS varchar(5)) ||'%' AS Four_star,
		CAST(ROUND((three_star*1.0/(five_star+four_star+three_star+two_star+one_star))*100, 2) AS varchar(5)) ||'%' AS Three_star,
		CAST(ROUND((two_star*1.0/(five_star+four_star+three_star+two_star+one_star))*100, 2) AS varchar(5)) ||'%' AS Two_star,
		CAST(ROUND((one_star*1.0/(five_star+four_star+three_star+two_star+one_star))*100, 2) AS varchar(5)) ||'%' AS One_star,
		CAST(100 AS varchar(5)) ||'%' AS Total
FROM star_review_count_on_actual_estimate_diff;
```

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

```
SELECT s.seller_id, COUNT(review_score) tot_5_star_review
FROM order_reviews orev JOIN order_items oi ON orev.order_id = oi.order_id
						JOIN sellers s ON s.seller_id = oi.seller_id
WHERE review_score = 5
GROUP BY s.seller_id
ORDER BY tot_5_star_review DESC LIMIT 20;
```

#### **Result:**
![rev-4](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/a4546fc9-2872-4eb3-9da2-7a7b10b1dafb)


### 4.2 Top 20 sellers with highest number of 1 star rating

```
SELECT s.seller_id, COUNT(review_score) tot_1_star_review
FROM order_reviews orev JOIN order_items oi ON orev.order_id = oi.order_id
						JOIN sellers s ON s.seller_id = oi.seller_id
WHERE review_score = 1
GROUP BY s.seller_id
ORDER BY tot_1_star_review DESC LIMIT 20;
```

#### **Result:**
![rev-5](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/ecb5cbeb-b6f1-4012-8300-68d56b6aeb0b)


### 4.3 From these Top 20 sellers with highest number of 1 star rating, find the top 3 products that gives them the highest negative rating
```
CREATE VIEW Top20sellers_top3products_1starrating AS
	WITH low_rating_sellers AS (SELECT s.seller_id, COUNT(review_score) tot_1_star_review
								FROM order_reviews orev JOIN order_items oi ON orev.order_id = oi.order_id
														JOIN sellers s ON s.seller_id = oi.seller_id
								WHERE review_score = 1
								GROUP BY s.seller_id
								ORDER BY tot_1_star_review DESC LIMIT 20),
		seller_products_lowrate AS (SELECT lrs.seller_id, tot_1_star_review, orev.order_id, product_id, review_score
									FROM low_rating_sellers lrs JOIN order_items oi ON lrs.seller_id = oi.seller_id
																JOIN order_reviews orev ON oi.order_id = orev.order_id
									WHERE review_score = 1),
		ranking_products AS (SELECT seller_id, product_id, COUNT(*) counts
							FROM seller_products_lowrate
							GROUP BY seller_id, product_id
							ORDER BY seller_id, counts DESC),
		ranking AS (SELECT *, 
							DENSE_RANK() OVER(PARTITION BY seller_id ORDER BY counts DESC) as ranks
					FROM ranking_products)
	SELECT *
	FROM ranking
	WHERE ranks <=3 ;

SELECT *
FROM Top20sellers_top3products_1starrating;
```

**Query explanation:** <br />
1. cte - low_rating_sellers -> Identifies the top 20 sellers with the highest number of 1-star reviews by counting the occurrences of 1-star reviews for each seller. <br />
2. cte - seller_products_lowrate -> Retrieves the order details (order_id, product_id, review_score) for each product associated with the sellers identified in the previous CTE.<br />
3. cte - ranking_products -> Counts the occurrences of each product associated with the low-rated sellers<br />
4. cte - ranking -> Assigns ranks to the products within each seller group based on their counts using the DENSE_RANK() window function.<br />
5. Main query - a View is created that selects all the columns from the cte "ranking" and a "WHERE" clause to filter only the top 3 products for each seller based on their ranks.

   
#### **Result:**
![sell3](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/4cdf8018-6afa-4d31-beda-2d7c45db8e64)


### 4.4 Analysing the reasons behind the negative reviews received by these products from the specific sellers

```
CREATE VIEW estimation_actual_delivery_date_difference AS
	WITH delivered_date_diff AS (SELECT tt.seller_id, tt.product_id, counts, ranks, o.order_id, 
								 							order_item_id, price, freight_value,
										order_delivered_customer_date, order_estimated_delivery_date,
								(EXTRACT(epoch FROM (order_estimated_delivery_date - order_delivered_customer_date)/3600)) :: int 
								 																				AS actu_esti_diff_hrs
							FROM Top20sellers_top3products_1starrating tt 
													JOIN order_items oi ON tt.seller_id = oi.seller_id AND tt.product_id = oi.product_id
													JOIN orders o ON o.order_id = oi.order_id
							ORDER BY tt.seller_id, tt.product_id, counts DESC)
	SELECT *
	FROM delivered_date_diff;

SELECT *
FROM estimation_actual_delivery_date_difference;
```

This script sets up a view that provides information on the difference between estimated and actual delivery dates, freight values, price, etc for specific seller-product combinations, facilitating further analysis and decision-making based on delivery performance and freight value metrics.

#### **Result**
![sell4](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/4d63a6cb-205e-4192-a6d4-c60e4185e51e)


### 4.5 Analysing the negative reviews from delayed delivery perspective

```
SELECT seller_id, product_id, counts, ranks, AVG(actu_esti_diff_hrs) avg_actu_esti_deli_diff_hrs
FROM estimation_actual_delivery_date_difference
GROUP BY seller_id, product_id, counts, ranks
ORDER BY seller_id, ranks;
```

#### **Result**
![sell5](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/ba5d3337-f04d-486c-99b1-a1aae8988a4b)


The findings indicate that the top 3 products from each top 20 sellers receiving 1-star reviews are not primarily attributed to delivery delays, as the average difference between the estimated and delivered dates are all positive.

### 4.6 Analysing the negative reviews from shipping cost(freight_value) perspective

```
WITH prod_from_lowrating_sellers_freight AS(SELECT seller_id, product_id, counts, ranks,  
														ROUND(CAST(AVG(freight_value) AS numeric),2) avg_freight_value
											FROM estimation_actual_delivery_date_difference
											GROUP BY seller_id, product_id, counts, ranks
											ORDER BY seller_id, ranks),

	same_prod_freightval_when_5star AS (SELECT product_id, ROUND(CAST(AVG(freight_value) AS numeric),2) AS avg_freightval_5star
										FROM order_items oi JOIN order_reviews orev ON oi.order_id = orev.order_id
										WHERE product_id IN (SELECT product_id
															FROM estimation_actual_delivery_date_difference) 
																		AND review_score = 5
										GROUP BY product_id)
SELECT freilowsell.product_id, avg_freight_value, avg_freightval_5star
FROM prod_from_lowrating_sellers_freight freilowsell 
				JOIN same_prod_freightval_when_5star frei5star 
							ON freilowsell.product_id=frei5star.product_id;
```

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
