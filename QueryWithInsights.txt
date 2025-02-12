WITH base AS (
SELECT 
  t.*,
  CASE 
    WHEN t.TransactionDate =  MIN(TransactionDate) OVER (PARTITION BY CustomerID) THEN 'New Customer'
    ELSE 'Returning Customer'
  END AS CustomerType

  ,CASE 
    WHEN CustomerAge < 25 THEN 'Under 25'
    WHEN CustomerAge BETWEEN 25 AND 40 THEN '25-40'
    WHEN CustomerAge BETWEEN 41 AND 60 THEN '41-60'
    ELSE '60+' 
  END AS AgeGroup

  ,CASE 
    WHEN LoyaltyPoints >= 1000 THEN 'High Loyalty'
    WHEN LoyaltyPoints BETWEEN 500 AND 999 THEN 'Medium Loyalty'
    ELSE 'Low Loyalty'
  END AS LoyaltyCategory

  ,CASE 
    WHEN FeedbackScore >= 4 THEN 'Positive'
    WHEN FeedbackScore = 3 THEN 'Neutral'
    ELSE 'Negative'
  END AS FeedbackCategory

  ,CASE 
    WHEN StoreType IN ('Online', 'E-commerce') THEN 'Online Store'
    ELSE 'Physical Store'
  END AS StoreType

  ,CASE 
    WHEN EXTRACT(HOUR FROM TransactionDate) BETWEEN 6 AND 11 THEN 'Morning'
    WHEN EXTRACT(HOUR FROM TransactionDate) BETWEEN 12 AND 17 THEN 'Afternoon'
    WHEN EXTRACT(HOUR FROM TransactionDate) BETWEEN 18 AND 23 THEN 'Evening'
    ELSE 'Late Night'
  END AS PurchaseTimeCategory

  ,CASE 
    WHEN COUNT(TransactionID) OVER (PARTITION BY CustomerID, DATE(TransactionDate)) > 1 THEN 'Frequent Buyer'
    ELSE 'Occasional Buyer'
  END AS BuyingFrequencyCategory

  ,CASE 
    WHEN ProductName IN ('Premium Laptop', 'Luxury Watch', 'Smartphone Pro') THEN 'High-Value Product'
    ELSE 'Regular Product'
  END AS ProductCategory

 from strivebigquery.digi_reports_dev.test_experiment_dataset t

)
/*
--Repeat vs. New Customer Spend
  SELECT 
    CustomerType, 
    AVG(TransactionAmount) AS AvgTransactionAmount,
    COUNT(TransactionID) AS TransactionCount
  FROM base
  GROUP BY CustomerType;
*/

/*
--spending by gender & age group.
  SELECT 
    CustomerGender, 
    AgeGroup,
    COUNT(DISTINCT CustomerID) AS TotalCustomers,
    SUM(TransactionAmount) AS TotalRevenue,
    AVG(TransactionAmount) AS AvgTransactionAmount
  FROM strivebigquery.digi_reports_dev.test_experiment_dataset
  GROUP BY CustomerGender, AgeGroup
  ORDER BY TotalRevenue DESC;
*/

/*
--Top-Selling Products
  SELECT 
    ProductName, 
    SUM(Quantity) AS TotalQuantitySold, 
    SUM(TransactionAmount) AS TotalRevenue
  FROM strivebigquery.digi_reports_dev.test_experiment_dataset
  GROUP BY ProductName
  ORDER BY TotalQuantitySold DESC
*/
/*
--Return Rate by Product
  SELECT 
    ProductName, 
    COUNT(CASE WHEN Returned = TRUE THEN TransactionID END) AS TotalReturns,
    COUNT(TransactionID) AS TotalTransactions,
    ROUND((COUNT(CASE WHEN Returned = TRUE THEN TransactionID END) * 100.0 / COUNT(TransactionID)), 2) AS ReturnRate
  FROM strivebigquery.digi_reports_dev.test_experiment_dataset
  GROUP BY ProductName
  ORDER BY ReturnRate DESC;
*/

/*
--City/Region-wise Delivery Performance
  SELECT 
    City, 
    Region, 
    AVG(DeliveryTimeDays) AS AvgDeliveryTime
  FROM strivebigquery.digi_reports_dev.test_experiment_dataset
  GROUP BY City, Region
  ORDER BY AvgDeliveryTime DESC;
*/



/*
-- total revenue per customer.
  SELECT 
    CustomerID, 
    COUNT(TransactionID) AS TotalTransactions, 
    SUM(TransactionAmount) AS TotalRevenue, 
    AVG(TransactionAmount) AS AvgTransactionValue
  FROM strivebigquery.digi_reports_dev.test_experiment_dataset
  GROUP BY CustomerID
  ORDER BY TotalRevenue DESC
*/

/*
--cities & store types with the most revenue
  SELECT 
    City, 
    StoreType, 
    COUNT(TransactionID) AS TotalTransactions, 
    SUM(TransactionAmount) AS TotalRevenue, 
    AVG(TransactionAmount) AS AvgTransactionValue
  FROM strivebigquery.digi_reports_dev.test_experiment_dataset
  GROUP BY City, StoreType
  ORDER BY TotalRevenue DESC
*/

/*
SELECT 
  StoreType, 
  Region, 
  CustomerGender,
  COUNT(DISTINCT CustomerID) AS TotalCustomers, 
  SUM(TransactionAmount) AS TotalRevenue, 
  AVG(TransactionAmount) AS AvgTransactionValue
FROM strivebigquery.digi_reports_dev.test_experiment_dataset
GROUP BY StoreType, Region, CustomerGender
ORDER BY TotalRevenue DESC;
*/