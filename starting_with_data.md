**Question 1: Which products do have the higher stock level?**

SQL Queries:

```
SELECT "ProductSKU", "Name", "StockLevel"
FROM "Products" 
ORDER BY "StockLevel" DESC 
LIMIT 5
``` 
Answer: 

![Cities and countries with the highest level of transaction revenues](Q4-1.png)
![Cities and countries with the highest level of transaction revenues](Q4-1.csv)


**Question 2: What are the 3 most visited pages ordered from visitors of each city and country?**

SQL Queries:
```
SELECT "Country", "City", "PageTitle", "PageViews"
FROM (
		SELECT "Country", "City", "PageTitle", "PageViews", ROW_NUMBER() OVER (
		PARTITION BY "Country", "City" 
		ORDER BY "PageViews" DESC) AS Most_Vis_Sites_rank 
   		FROM "AllSessions"
		WHERE "City" != 'not available in demo dataset'AND "City" != '(not set)' 
		) top3
WHERE Most_Vis_Sites_rank <= 3

```

Answer:

![Top 3 most visited pages ordered by city and country](Q4-2.png)
![Top 3 most visited pages ordered by city and country](Q4-2.csv)


**Question 3: What is the page that visitors from different cities spend most of time? (order it by city and country).**

SQL Queries:
```
SELECT "Country", "City", "PageTitle", "TimeOnSite"
FROM (
		SELECT "Country", "City", "PageTitle", "TimeOnSite", ROW_NUMBER() OVER (
		PARTITION BY "Country", "City" 
		ORDER BY "TimeOnSite" DESC) AS Time_rank 
   		FROM "AllSessions"
		WHERE "City" != 'not available in demo dataset'AND "City" != '(not set)' AND "TimeOnSite" IS NOT NULL
		) Sites
WHERE Time_rank <= 1

```

Answer:
![Pages that visitor spend most time per city and country](Q4-3.png)
![Pages that visitor spend most time per city and country](Q4-3.csv)

