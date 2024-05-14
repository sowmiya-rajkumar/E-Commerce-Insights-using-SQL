# E-Commerce-Insights: A Comprehensive Analysis of Orders, Customers, and Reviews.

## Overview

This project presents a comprehensive analysis of an e-commerce database using PostgreSQL. It covers various aspects including orders, customers and reviews. Through SQL queries and data exploration, valuable insights have been derived to understand the dynamics of the e-commerce platform.

## Database Details

The database is a Brazilian ecommerce public dataset of orders made at Olist Store which is available in kaggle (https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce/data) and it has information of 100k orders from 2016 to 2018 made at multiple marketplaces in Brazil. This dataset contains real commercial data that has undergone anonymization.

Below is the screenshot of all the tables in the dataset and how they relate to each other.

![Screen Shot 2024-05-14 at 7 10 16 PM](https://github.com/sowmiya-rajkumar/E-Commerce-Insights-using-SQL/assets/98767488/ef6d7dc8-a825-40b9-bbd3-9213cf8e889e)

## Orders


### Total orders made by customers

SELECT COUNT(*) total_orders
FROM orders;


### Average order price

SELECT AVG(price) average_price
FROM order_items;


### Cities with most Orders

SELECT customer_city, COUNT(order_id) order_count
FROM orders o JOIN customers c ON o.customer_id = c.customer_id
GROUP BY customer_city
ORDER BY order_count DESC;


### Cities with its average order value

SELECT customer_city, ROUND(CAST(AVG(payment_value) AS numeric),2) AS avg_order_value
FROM orders o JOIN customers c ON o.customer_id = c.customer_id
				JOIN order_payments op ON o.order_id = op.order_id
GROUP BY customer_city
ORDER BY avg_order_value DESC;


## Customers


### Total Customers

SELECT COUNT(*) total_customer_count
FROM customers;


### Customer segmentation based on purchase frequency

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


### Average order price for each customer segment

SELECT customer_purchase_frequency, ROUND(CAST(AVG(payment_value) AS numeric), 2) average_price_per_cust_segment
FROM customer_segment_purchase_frequency seg_freq 
					JOIN orders o ON o.customer_id = seg_freq.customer_id
					JOIN order_payments op ON op.order_id = o.order_id
GROUP BY customer_purchase_frequency;


### Frequently purchased product category by "Loyal Customers"

SELECT pcn.product_category_name_english, COUNT(*) total_by_Loyal_Customers
FROM customer_segment_purchase_frequency cspf 
								JOIN orders o ON cspf.customer_id = o.customer_id
								JOIN order_items oi ON o.order_id = oi.order_id
								JOIN products p ON oi.product_id = p.product_id
								JOIN product_category_name pcn ON p.product_category_name = pcn.product_category_name
WHERE customer_purchase_frequency = 'Loyal Customers'
GROUP BY pcn.product_category_name_english
ORDER BY total_by_Loyal_Customers DESC LIMIT 1;


### Top 3 product categories across all customer segments

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
WHERE counts_rank <=3


### Product categories that are particularly popular with specific customer segments but not others

The above result analysis reveals that "bed_bath_table" and "sports_leisure" are popular across all customer segments, while "furniture_decor" is favored notably by loyal customers and moderate buyers.
Conversely, "health_beauty" appears to be a preference primarily among first-time buyers and is not as commonly purchased among the other two groups.


## Order reviews


### Total review_score counts by star

SELECT review_score as star, COUNT(*) AS star_count
FROM order_reviews
GROUP BY review_score
ORDER bY review_score DESC;


### Top 20 products with highest number of 5-star reviews

SELECT p.product_id, COUNT(*) counts
FROM order_items oite JOIN order_reviews orev ON oite.order_id = orev.order_id
						JOIN products p ON oite.product_id = p.product_id
WHERE review_score = 5
GROUP BY p.product_id
ORDER BY counts DESC LIMIT 20;


### Top 20 products with highest number of 1-star reviews

SELECT p.product_id, COUNT(*) counts
FROM order_items oite JOIN order_reviews orev ON oite.order_id = orev.order_id
						JOIN products p ON oite.product_id = p.product_id
WHERE review_score = 1
GROUP BY p.product_id
ORDER BY counts DESC LIMIT 20;


### Top 20 sellers with highest number of 5 star rating

SELECT s.seller_id, COUNT(review_score) tot_5_star_review
FROM order_reviews orev JOIN order_items oi ON orev.order_id = oi.order_id
						JOIN sellers s ON s.seller_id = oi.seller_id
WHERE review_score = 5
GROUP BY s.seller_id
ORDER BY tot_5_star_review DESC LIMIT 20;


### Top 20 sellers with highest number of 1 star rating

SELECT s.seller_id, COUNT(review_score) tot_1_star_review
FROM order_reviews orev JOIN order_items oi ON orev.order_id = oi.order_id
						JOIN sellers s ON s.seller_id = oi.seller_id
WHERE review_score = 1
GROUP BY s.seller_id
ORDER BY tot_1_star_review DESC LIMIT 20;


### Breakdown of Star Ratings Based on Delivery Timeliness: No Delay, 1-5 Days Delay, 6-10 Days Delay, and Over 10 Days Delay.

CREATE VIEW star_review_count_on_actual_estimate_diff AS
	WITH actual_estimation_diff AS(SELECT *, order_delivered_customer_date - order_estimated_delivery_date 
																AS diff_estimation_delivered
								FROM orders),
		hours_conversion AS (SELECT orev.order_id, customer_id, diff_estimation_delivered, review_score, 
										(EXTRACT(epoch FROM diff_estimation_delivered)/3600) :: int AS tot_hours_difference
							FROM actual_estimation_diff aed JOIN order_reviews orev ON aed.order_id = orev.order_id),
		grouped_diff AS (SELECT *, CASE
										WHEN tot_hours_difference <= 0 THEN 'No delay'
										WHEN tot_hours_difference > 0 AND tot_hours_difference <= 120 THEN 'Delayed 1 - 5 days'
										WHEN tot_hours_difference > 120 AND tot_hours_difference <= 240 THEN 'Delayed 6 - 10 days'
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

As observed in the results, an increase in the gap between the actual delivery date and the estimated date corresponds to a higher count of negative reviews, while a decrease in this gap leads to fewer negative reviews.
