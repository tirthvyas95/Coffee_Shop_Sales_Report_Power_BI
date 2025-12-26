# **Coffee Shop Sales Report in Power BI**
![Screenshot_1](https://github.com/tirthvyas95/Coffee_Shop_Sales_Report_Power_BI/blob/c9347f72c60bf7dfd9f5ad928259110957e1b65f/Screenshots/SS_1.png)
## Introduction

This a Sample report I made to learn Power BI processes and features using a walktrough published on a Youtube Channel called [Data Tutorials](https://www.youtube.com/@datatutorials1). In this report aims to visualize the per day and per hour sales patterns that occur in multiple coffee shops in New York. The data set used is also available and you can follow the same [Walkthrough](https://www.youtube.com/watch?v=_JwAryp3STM) to recreate the report in Power BI desktop. Power BI Desktop is available free of cost on the [Microsoft Power BI Download Page](https://www.microsoft.com/en-us/download/details.aspx?id=58494) although you must purchase Microsoft Fabric License or other payed plans to pubish the report and utilize other advanced features available on the [Microsoft Fabric Platform](https://www.microsoft.com/en-us/microsoft-fabric). Following is the Metadata of the dataset and the list of measures used to realize the visualization along with the features like interations between visuals that enable you to sort and filter through all the visuals enabling stakeholders to explore how to store are performing. In this report an emphasis was made on the custom tooltip function which allows report authors to make a custom tooltip which can be used instead of the standard to create another layer of immersion.

## Metadata

### Transactions Table
1. transaction_id = Unique ID of each transaction
2. transaction_date = Date when the transaction was made(i.e: Customer paid the bill)
3. transaction_time = Time when the transaction was recorded
4. transaction_qty = Number of Items in the transaction
5. store_id = Unique ID of multiple store situated throughout the city of New York
6. store_location = The location name of the store
7. product_id = Unique ID of products
8. unit_price = Unit price of the product
9. product_category = The category in which the product belongs to
10. product_type = Type of product
11. product_detail = Additional details of product

### Date Table
1. Date = The date
2. Day Name = Name of the day
3. Day Number = Day number of the week(1-7)
4. Month = Month of the year
5. Month Number = Monther number of the year(1-12)

## Measures
1. Total Sales = Number of transactions/Invoices/Customers
```
Total Sales = SUM(Transactions[Sales])
```
2. Total Quantity Sold = Total quantity sold
```
Total Quantity Sold = SUM(Transactions[transaction_qty])
```
3. Total Orders = Number of distinct transactions
```
Total Orders = DISTINCTCOUNT(Transactions[transaction_id])
```
4. PM Sales = Previous Month sales, made using DATEADD() function
```
PM Sales = CALCULATE([CM Sales], DATEADD('Date Table'[Date], -1, MONTH))
```
5. PM Quantity = Previous Month Quantity Sold
```
PM Quantity = CALCULATE([CM Quantity], DATEADD('Date Table'[Date], -1, MONTH))
```
6. PM Orders = Previous Month amount of orders
```
PM Orders = CALCULATE([CM Orders], DATEADD('Date Table'[Date], -1, MONTH))
```
7. CM Sales = Current month sales
```
CM Sales = VAR selected_month = SELECTEDVALUE('Date Table'[Month])
            RETURN
            TOTALMTD(CALCULATE([Total Sales], 'Date Table'[Month] = selected_month), 'Date Table'[Date])
```
8. CM Quantity = Current month quantity
```
CM Quantity = VAR selected_month = SELECTEDVALUE('Date Table'[Month])
            RETURN
            TOTALMTD(CALCULATE([Total Quantity Sold], 'Date Table'[Month] = selected_month), 'Date Table'[Date])
```
9. CM Orders = Current month Orders, made using the TOTALMTD() function
```
CM Orders = VAR selected_month = SELECTEDVALUE('Date Table'[Month])
            RETURN
            TOTALMTD(CALCULATE([Total Orders], 'Date Table'[Month] = selected_month), 'Date Table'[Date])
```
10. Daily Avg Sales = Daily average sales
```
Daily Avg Sales = AVERAGEX(ALLSELECTED(Transactions[transaction_date]), [Total Sales])
```
11. MoM Growth & Diff Sales = To calculate the month on month Growth KPI of total sales
```
MoM Growth & Diff Sales = 
    VAR month_diff = [CM Sales] - [PM Sales]
    VAR mom = ([CM Sales] - [PM Sales]) / [PM Sales]
    VAR _sign = IF(month_diff > 0, "+", "")
    VAR _sign_trend = IF(month_diff > 0, "▲", "▼")
    RETURN
        _sign_trend & " " & _sign & FORMAT(mom, "#0.0%") & " | " & _sign & FORMAT(month_diff/1000, "0.0K") & " " & "vs LM"
```
12. MoM Growth & Diff Quantity = To calculate the month on month Growth KPI of quantity sold
```
MoM Growth & Diff Quantity = 
    VAR month_diff = [CM Quantity] - [PM Quantity]
    VAR mom = ([CM Quantity] - [PM Quantity]) / [PM Quantity]
    VAR _sign = IF(month_diff > 0, "+", "")
    VAR _sign_trend = IF(month_diff > 0, "▲", "▼")
    RETURN
        _sign_trend & " " & _sign & FORMAT(mom, "#0.0%") & " | " & _sign & FORMAT(month_diff/1000, "0.0K") & " " & "vs LM"
```
13. MoM Growth & Diff Orders = To calculate the month on month growth KPI of Orders, made using DAX Variables, IF() in DAX and FORMAT() function
```
MoM Growth & Diff Orders = 
    VAR month_diff = [CM Orders] - [PM Orders]
    VAR mom = ([CM Orders] - [PM Orders]) / [PM Orders]
    VAR _sign = IF(month_diff > 0, "+", "")
    VAR _sign_trend = IF(month_diff > 0, "▲", "▼")
    RETURN
        _sign_trend & " " & _sign & FORMAT(mom, "#0.0%") & " | " & _sign & FORMAT(month_diff/1000, "0.0K") & " " & "vs LM"
```

## Features and Interactions
### Filtering by Date
![Screenshot_1](https://github.com/tirthvyas95/Coffee_Shop_Sales_Report_Power_BI/blob/c9347f72c60bf7dfd9f5ad928259110957e1b65f/Screenshots/SS_2.png)
### Custom Tool Tip for Calender Chart
![Screenshot_1](https://github.com/tirthvyas95/Coffee_Shop_Sales_Report_Power_BI/blob/c9347f72c60bf7dfd9f5ad928259110957e1b65f/Screenshots/SS_3.png)
### Filtering by Weekdays and Weekends
![Screenshot_1](https://github.com/tirthvyas95/Coffee_Shop_Sales_Report_Power_BI/blob/c9347f72c60bf7dfd9f5ad928259110957e1b65f/Screenshots/SS_4.png)
### Filtering by Store Location
![Screenshot_1](https://github.com/tirthvyas95/Coffee_Shop_Sales_Report_Power_BI/blob/c9347f72c60bf7dfd9f5ad928259110957e1b65f/Screenshots/SS_5.png)
### Filering by Product Category
![Screenshot_1](https://github.com/tirthvyas95/Coffee_Shop_Sales_Report_Power_BI/blob/c9347f72c60bf7dfd9f5ad928259110957e1b65f/Screenshots/SS_6.png)
The same type of filtering is available with 'Sales by Product' visual as well
### Custom Tool Tip for Day and Hour Chart
![Screenshot_1](https://github.com/tirthvyas95/Coffee_Shop_Sales_Report_Power_BI/blob/9ccf40ad438d8acdb0a653a0e73bb1ae4abd360e/Screenshots/SS_7.png)

## References
- Data Tutorials, Data Tutorials's Youtube Channel. Retrieved December 11, 2025, from https://www.youtube.com/@datatutorials1
- Microsoft Learn, Microsoft Learn's Data Analyst Career Path. Retrieved December 11, 2025, from https://learn.microsoft.com/en-us/training/career-paths/data-analyst

