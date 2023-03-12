What issues will you address by cleaning the data?
*Missing values.
*Extreme values.
*Null values.
*Duplicate rows.
*Wrong capitalization of text.
*Existence of outliers.
*Lack of data quality.
*lack of efficiency (Very big files)
*Wrong data format or structure.

Through Excel: 

FILES:

AllSession: It was identified with the use of the counta() function that the folloing columns in the Excel file were empty: productRefundAmount(N), itemQuantity (W), itemRevenue (X), and searchKeyword (AB). Aditionally, it was identified that columns productRevenue (Q) and transactionRevenue (Y) had only 4 cells not empty (out of 15,135) and they were deleted. Similarly, it was deleted column transactionId (Z) due to it had only 9 cells not empty out of 15,135.

Analytics: It was identified with the use of the counta() function that the userid(F) column had all its cells empty. Consequently, it was deleted. Furthermore, the column SocialEngagementType has the same info in all cells. By using function COUNTIF(G2:G1048576, "Not Socially Engaged"), the result showed that the entire column has the same parameter (Result= 1.048.575). Consequently, this column was deleted. As result, by removing the previous 2 columns, the file size was reduced from 438 to 74 MBs. 

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

2. Lines to included in queries to deal with some data issues:

* WHERE "TotalTransactionRevenue" IS NOT NULL AND "City" != 'not available in demo dataset'
* WHERE pr."ProductSKU" IS NOT NULL AND al."City" != 'not available in demo dataset' AND al."City" != '(not set)'
* WHERE pr."OrderedQuantity" IS NOT NULL AND al."City" != 'not available in demo dataset' AND al."City" != '(not set)' AND al."v2ProductCategory" != '(not set)'
* WHERE "City" != 'not available in demo dataset'AND "City" != '(not set)' AND "TimeOnSite" IS NOT NULL