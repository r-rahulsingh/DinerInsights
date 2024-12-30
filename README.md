# DinerInsights

![image](https://github.com/user-attachments/assets/80726aeb-9ccf-46ef-b8b3-398b2fffc157)

## Introduction
Danny, a fan of Japanese cuisine, took a leap of faith in early 2021 and opened a restaurant called Danny’s Diner. His menu features his three favorite dishes: 

*	Sushi
*	Curry
*	Ramen

## Problem Statement
However, Danny’s Diner is facing challenges and seeks your help to navigate the complexities of running a restaurant. 

Despite collecting some basic data during their short operational period, they lack the expertise to interpret and utilize this information effectively to improve their business performance.

To help him deliver a better and more personalized experience for his loyal customers. We can answer a few simple questions about his customers, listed below, to help him with his business. Also, he intends to use these insights to help him decide whether to expand the existing customer loyalty program.

1.	What is the total amount each customer spent at the restaurant?
2.	How many days has each customer visited the restaurant?
3.	What was the first item from the menu purchased by each customer?
4.	What is the most purchased item on the menu and how many times was it purchased by all customers?
5.	Which item was the most popular for each customer?
6.	Which item was purchased first by the customer after they became a member?
7.	Which item was purchased just before the customer became a member?
8.	What are the total items and amount spent for each member before they became a member?
9.	If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
10.	In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?
11.	Join the tables above to create some basic datasets that his team can easily examine without resorting to SQL.
12.	Danny needs product rankings for loyalty program members.

Danny has shared with you 3 key datasets for this case study:

* Sales
* Menu
*	Members

**Entity Relationship diagram:**

![image](https://github.com/user-attachments/assets/38720e61-6965-4543-9383-e64798fa0ebd)

## Dataset schema: dannys_diner

![image](https://github.com/user-attachments/assets/4bf60119-d959-4059-b5b3-749c3b53d65e)

![image](https://github.com/user-attachments/assets/ddda6b6e-75d5-484c-8121-1b9f2e853979)

## Solution 

**Link:** https://www.db-fiddle.com/f/2rM8RAnq7h5LLDTzZiRWcd/8791

**1.	What is the total amount each customer spent at the restaurant?**

![image](https://github.com/user-attachments/assets/0ac5a5d8-c6bc-4a6e-aba9-5cf2c74a8e73)

 
**Solution:**
-	Use an aggregate query to sum the prices of each product ordered by customers.
-	Join the sales and menu tables on product_id to calculate the total for each customer.
-	Group the results by customer_id to get the total amount spent by each customer.

**Key Points:**
-	Aggregation: SUM() is used to total the spent amount.
-	Join: The sales and menu tables are joined based on product_id.
-	Grouping: Group by customer_id to calculate totals per customer.

**2.	How many days has each customer visited the restaurant?**

![image](https://github.com/user-attachments/assets/8ef1c749-c7df-4739-8acc-f2734f18483b)


**Solution:**
-	Use COUNT(DISTINCT) to count the number of unique days each customer visited.
-	Join the sales table with members to filter by customer visits.
-	Group by customer_id to get the visit count for each customer.

**Key Points:**
-	Distinct Dates: COUNT(DISTINCT) ensures counting only unique visit days.
-	Join: The sales table is used to track the visit dates.
-	Grouping: Group by customer_id for visits per customer.

**3.	What was the first item from the menu purchased by each customer?**

![image](https://github.com/user-attachments/assets/6957b17c-c00d-4d0c-b81c-d356e4728072)


**Solution:**
-	Use DISTINCT ON to select the first item ordered based on the earliest order date.
-	Filter by customer_id and order by order_date to get the first purchase.

**Key Points:**
-	DISTINCT ON: Ensures only the first purchase is selected for each customer.
-	Order Date: Orders are sorted by order_date to get the earliest.
-	Selection: Use product_name from the menu to identify the first item.

**4.	What is the most purchased item on the menu and how many times was it purchased by all customers?**

![image](https://github.com/user-attachments/assets/30aa9361-4eee-4264-a212-1eed8e235e26)


**Solution:**
-	Aggregate the data with COUNT() to find the most purchased item.
-	Group by product_name and order by the purchase count to get the most popular item.


**Key Points:**
-	COUNT: Used to count the occurrences of each item purchased.
-	Grouping and Ordering: Group by product_name and order by purchase count.
-	Most Popular: The item with the highest count is selected.

**5.	Which item was the most popular for each customer?**

![image](https://github.com/user-attachments/assets/40f631d2-86ce-4b6b-9f97-fdd8e1fe3921)


**Solution:**
-	Use COUNT() and RANK() to rank the products based on how often they were purchased by each customer.
-	Partition by customer_id and order by purchase count to find the most popular item.

**Key Points:**
-	COUNT: Calculates how many times each item was purchased.
-	RANK: Used to rank the most purchased item per customer.
-	Partition: Ensures ranking happens per customer, not globally.

**6.	Which item was purchased first by the customer after they became a member?**

![image](https://github.com/user-attachments/assets/667cebde-395e-4c45-ad16-2341065a59b2)


**Solution:**
-	Filter the sales data where the order_date is after the join_date for each customer.
-	Use ROW_NUMBER() to rank purchases by date and select the first product purchased.

**Key Points:**
-	Filter: Only consider purchases made after the customer joined the program.
-	ROW_NUMBER: Ranks the purchases by date to get the first item.
-	Joining: Join with the members table to filter based on join_date.

**7.	Which item was purchased just before the customer became a member?**

![image](https://github.com/user-attachments/assets/3ab0ea26-3c11-4335-b94b-705cca0c487f)


**Solution:**
-	Filter purchases made before the join_date of each customer.
-	Use ROW_NUMBER() with ORDER BY to get the last product purchased before joining.

**Key Points:**
-	Filter: Only consider purchases before the membership date.
-	ROW_NUMBER: Used to rank purchases by date to find the most recent one before joining.
-	Join: The sales table is joined with the members table for filtering.

**8.	What are the total items and amount spent for each member before they became a member?**

![image](https://github.com/user-attachments/assets/ba858ad9-aa5b-4b58-9ff5-85103b58eea2)


**Solution:**
-	Aggregate data for purchases made before the customer joined, summing item counts and amounts.
-	Filter by order_date before the join_date.

**Key Points:**
-	Aggregation: Use COUNT() for item totals and SUM() for the amount spent.
-	Filtering: Ensure purchases are made before join_date.
-	Group: Group by customer_id to calculate the totals per member.

**9.	If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?**

![image](https://github.com/user-attachments/assets/912ff5c2-edb6-44ec-80f2-a3fe78291138)


**Solution:**
-	Calculate points based on the price of each item.
-	Apply the multiplier for sushi and calculate points accordingly.

**Key Points:**
-	Points Calculation: Points are based on the price, with a 2x multiplier for sushi.
-	Multiplier: Special rule for sushi to award double points.
-	Aggregation: Sum points for each customer to get total points.

**10.	In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?**

![image](https://github.com/user-attachments/assets/d7c88bd3-e5a8-4d14-9941-55e0b9e617b6)


**Solution:**
-	Calculate points, applying a 2x multiplier for the first week after joining.
-	Apply standard points for other purchases made outside the first week.

**Key Points:**
-	Points Multiplier: Apply the 2x multiplier for the first week after the join date.
-	First Week Calculation: Filter purchases within the first 7 days of membership.
-	Standard Points: Use regular points calculation for purchases after the first week.

**11.	Join the tables above to create some basic datasets that his team can easily examine without resorting to SQL.**

_CODE Output, Recreated the table. Link provided above._

![image](https://github.com/user-attachments/assets/48cd016a-d71e-4cbf-b57e-9f5bef78256a)


**Solution:**
-	Use JOIN to combine sales, menu, and members tables into a comprehensive dataset.
-	Ensure necessary fields are included (e.g., customer_id, order_date, product_name).

**Key Points:**
-	JOINs: Combine the necessary tables based on relevant keys.
-	Comprehensive Data: Include all relevant data for analysis (order, customer, and product details).
-	Readable Dataset: Create a simple dataset that can be easily analyzed by the team.

**12.	Danny needs product rankings for loyalty program members.**

_CODE Output, Recreated the table. Link provided above._

![image](https://github.com/user-attachments/assets/b4f832c0-22f1-4203-bbbb-75c577fccf76)


**Solution:**
-	Rank products based on how often they are purchased by loyalty program members.
-	Use RANK() or DENSE_RANK() to rank products for each customer.

**Key Points:**
-	Ranking: Use window functions like RANK() to rank products.
-	Loyalty Members: Only consider purchases from members who are part of the loyalty program.
-	Aggregation: Group by product to calculate its ranking based on purchase frequency.

# About Me!
Follow me on [Medium](https://medium.com/@r-rahulsingh). I’ve also shared some interesting stuff on Image Segmentation, Neural Networks, case studies and many. Check it out! 

Feel free to reach out to me on [LinkedIn](https://www.linkedin.com/in/r-rahulsingh/) for any collaboration, work or simply talk over a virtual coffee. ☕️





