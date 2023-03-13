What are your risk areas? Identify and describe them.



QA Process:
Describe your QA process and include the SQL queries used to execute it.

My QA process was planed in 2 stages: 1) manually spot check and 2) systematic validation

Particularly, in the manually spot check there are 2 modules: i) Excel and ii)SQL. Within the Excel module, counting rows, nulls, and repeated parameters, along with a visually identifying abnormal behaviours were the methods implemented. On the other hand, in the SQL module, logic validation by testing spot cases and grouping results by different variables were the methods applied.

Regarding the systematic validation, this stage was proposed for a future work.

Through Excel: 

FILES:
All_session: by using the counta() function, columns productRefundAmount(N), itemQuantity (W), itemRevenue (X), and searchKeyword (AB) with all empty cells, were deleted. Additionally, columns productRevenue (Q) transactionRevenue (Y) with 4 cells not empty, and transactionId (Z) with 9 cells not empty, were deleted.
Analytics: using counta() it was found that usedid(F) column had all its cells empty. With only removing the userdID cell, file size was reduced from 438 to 74.2 MBs. Candidate for future inspection: revenue (M).
CurrentCurrency USD (Irrelevant).

Queries:

Below, provide the SQL queries you used to clean your data.

1. Verifying uniqueness: Checking that ProductSKUs are unique in products, sales_report and sales_by_SKU to potentially select it as primary key of tables products, sales_report, and sales_by_SKU.

SELECT DISTINCT "ProductSKU"
FROM products 
ORDER BY "ProductSKU" ASC
--Result got it for products: 1092 rows, that is equal to the number of SKU rows in the Excel spreadsheet 

SELECT DISTINCT "ProductSKU"
FROM sales_report 
ORDER BY "ProductSKU" ASC
--Result got it for sales_report: 454 rows, that is equal to the number of SKU rows in the Excel spreadsheet 

SELECT DISTINCT "ProductSKU"
FROM sales_by_SKU 
ORDER BY "ProductSKU" ASC
--Result got it for sales_by_SKU: 462 rows, that is equal to the number of SKU rows in the Excel spreadsheet 

2. Lines between other queries to deal with data issues:

* WHERE "TotalTransactionRevenue" IS NOT NULL AND "City" != 'not available in demo dataset'
* WHERE pr."ProductSKU" IS NOT NULL AND al."City" != 'not available in demo dataset' AND al."City" != '(not set)'
* WHERE pr."OrderedQuantity" IS NOT NULL AND al."City" != 'not available in demo dataset' AND al."City" != '(not set)' AND al."v2ProductCategory" != '(not set)'
* WHERE "City" != 'not available in demo dataset'AND "City" != '(not set)' AND "TimeOnSite" IS NOT NULL