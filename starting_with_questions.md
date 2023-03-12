Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
```
SELECT "City", "Country", "TotalTransactionRevenue"
FROM "AllSessions" 
WHERE "TotalTransactionRevenue" IS NOT NULL AND "City" != 'not available in demo dataset'
ORDER BY "TotalTransactionRevenue" DESC 
LIMIT 50
```
Answer: The 5 cities with the highest level of the transaction revenues are Atlanta, Sunnyvale, and Los Angeles in the US, Tel-Avid in Israel, and Sydney in Australia.

![Cities and countries with the highest level of transaction revenues](Q3-1.png)
![Cities and countries with the highest level of transaction revenues](Q3-1.csv)


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
```
SELECT Al."City", Al."Country", AVG ("OrderedQuantity") AS AV_Prod_Ordered
FROM "AllSessions" Al
INNER JOIN "Products" Pr
ON Al."ProductSKU" = Pr."ProductSKU"
WHERE "City" != 'not available in demo dataset' AND "City" != '(not set)'
GROUP BY Al."Country", Al."City"
ORDER BY "Country"
```

Answer: The 5 cities with the highest average number of products ordered are Council Bluffs, Bellflower, and Bellingham in the US, Cork in Ireland, and Santiago in Chile.

![Average number of products ordered from visitors in each city and country](Q3-2.png)
![Average number of products ordered from visitors in each city and country](Q3-2.csv)


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
```
SELECT Al."Country", Al."City", "v2ProductCategory", "OrderedQuantity" 
FROM "AllSessions" Al
INNER JOIN "Products" Pr
ON Al."ProductSKU" = Pr."ProductSKU"
WHERE Pr."OrderedQuantity" IS NOT NULL AND "City" != 'not available in demo dataset' AND "City" != '(not set)' AND al."v2ProductCategory" != '(not set)'
GROUP BY Al."Country", Al."City", "v2ProductCategory", "OrderedQuantity" 
ORDER BY "Country"
```


Answer: No, I didn't identify any pattern. Fun (Home - Accessories) and Sport&Fitness (Home - Accessories) were the categories with the highest amounts of quantities of ordered products.

![Product categories of products ordered from visitors in each city and country](Q3-3.png)
![Product categories of products ordered from visitors in each city and country](Q3-3.csv)


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
```
SELECT "Country", "City", "Name" AS ProductName, Num_Prod_Ordered
FROM (
		SELECT Al."Country", Al."City", Pr."Name", Pr."OrderedQuantity" AS Num_Prod_Ordered, ROW_NUMBER() OVER (
		PARTITION BY Al."Country", Al."City" 
		ORDER BY Pr."OrderedQuantity" DESC) AS Top_Sell_Prod_rank 
   		FROM "AllSessions" Al
		INNER JOIN "Products" Pr
		ON Al."ProductSKU" = Pr."ProductSKU"
		WHERE Pr."OrderedQuantity" IS NOT NULL AND Al."City" != 'not available in demo dataset'AND Al."City" != '(not set)' AND Al."v2ProductName" != '(not set)' 
		) ranks
WHERE Top_Sell_Prod_rank <= 1

```
Answer: Kick Ball and 22 oz Water Bottle are the top-selling products. Not pattern found.

![Top-selling product per city and country](Q3-4.png)
![top-selling product per city and country](Q3-4.csv)


**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
```
SELECT "City", "Country", SUM ("TotalTransactionRevenue") AS SumOfTotalTransRev
FROM "AllSessions" 
WHERE "TotalTransactionRevenue" IS NOT NULL AND "City" != 'not available in demo dataset'
GROUP BY "City", "Country" ORDER BY "City"
```

Answer: San Francisco, Sunnyvale, and Atlanta (All of them from the US) are the top 3 cities in terms of revenue generation.

![Total transaction revenue generated from each city and country](Q3-5.png)
![Total transaction revenue generated from each city and country](Q3-5.csv)
