# Paint Company Sales & Customer Analytics

Welcome to the Paint Company Sales & Customer Analytics project! This repository contains the database schema, sample data, and SQL queries for analyzing sales, customer behavior, and campaign effectiveness for a paint company operating across multiple Indian cities.

---

## 🗂️ Project Structure

- **Database:** `CAMPAIGN`
- **Tables:**
  - `City` 🏙️: City details, state, and tier classification
  - `Customer` 👤: Customer demographics and contact info
  - `Item` 🛒: Paint products and categories
  - `Campaign` 📣: Marketing campaign details
  - `CouponMapping` 🏷️: Coupon types, values, and item mapping
  - `CustomerTransactionData` 🧾: All transactions, linking customers, items, campaigns, and coupons

---

## 🚀 Features & Analysis

- **Cardinality Checks:**
  - Number of paint categories
  - Coupon types offered
  - States served
  - Order types

- **Sales Analytics:**
  - Transactions by year, quarter, and month
  - Purchase amounts by product category, order type, and city tier

- **Customer Segmentation:**
  - Analyze sales by city tier and income bracket

---

## 💻 Database Schema

```sql
CREATE DATABASE CAMPAIGN;
USE CAMPAIGN;

CREATE TABLE City (
  City_Id int PRIMARY key,
  City_Name varchar(64) NOT NULL,
  State varchar(64) NOT NULL,
  CityTier int NOT NULL
);

CREATE TABLE Customer (
  Customer_Id varchar(8) PRIMARY KEY,
  Name varchar(128) NOT NULL,
  Gender char NOT NULL,
  City_Id int,
  Pincode int,
  Birthdate date,
  income_bracket float,
  emailId varchar(256),
  PhoneNo char(14),
  FOREIGN KEY (City_Id) REFERENCES City(City_Id)
);

CREATE TABLE Campaign (
  campaign_id varchar(32) PRIMARY KEY,
  campaignType varchar(128) NOT NULL,
  start_date date NOT NULL,
  end_date date NOT NULL
);

CREATE TABLE Item (
  Item_Id varchar(32) PRIMARY KEY,
  Item_Name varchar(45) NOT NULL,
  Item_Category varchar(128) NOT NULL,
  Price int NOT NULL
);

CREATE TABLE CouponMapping (
  coupon_id varchar(32) PRIMARY KEY,
  item_id varchar(32),
  couponType varchar(128) NOT NULL,
  Value float NOT NULL,
  Min_Purchase float NOT NULL,
  Expiry int NOT NULL,
  FOREIGN KEY (item_id) REFERENCES Item (Item_Id)
);

CREATE TABLE CustomerTransactionData (
  Trans_Id varchar(32) PRIMARY KEY,
  Cust_Id varchar(8),
  item_id varchar(32),
  campaign_id varchar(32),
  coupon_id varchar(32),
  quantity int NOT NULL,
  PurchasingAmt float NOT NULL,
  PurchaseDate date NOT NULL,
  OrderType varchar(45) NOT NULL,
  FOREIGN KEY (campaign_id) REFERENCES Campaign (campaign_id),
  FOREIGN KEY (coupon_id) REFERENCES CouponMapping (coupon_id),
  FOREIGN KEY (Cust_Id) REFERENCES Customer (Customer_Id),
  FOREIGN KEY (item_id) REFERENCES Item (Item_Id)
);
```

---

## 📊 Sample SQL Queries

- **Cardinality Example:**
  ```sql
  SELECT COUNT(DISTINCT(Item_Category)) AS Category_count FROM Item;
  SELECT COUNT(DISTINCT couponType) AS Coupon_Count FROM CouponMapping;
  SELECT COUNT(DISTINCT State) AS City_Count FROM City;
  SELECT COUNT(DISTINCT OrderType) AS OrderType_Count FROM CustomerTransactionData;
  ```

- **Sales by Year:**
  ```sql
  SELECT YEAR(PurchaseDate) AS Year, COUNT(Trans_Id) AS Total_Trans
  FROM CustomerTransactionData
  GROUP BY YEAR(PurchaseDate);
  ```

- **Sales by Product Category:**
  ```sql
  SELECT I.Item_Category, SUM(PurchasingAmt) AS Total_Sales
  FROM CustomerTransactionData AS C
  JOIN Item AS I ON C.item_id = I.item_id
  GROUP BY I.Item_Category;
  ```

- **Sales by City Tier:**
  ```sql
  SELECT CityTier, ROUND(SUM(PurchasingAmt), 0) AS Total_Sales
  FROM CustomerTransactionData AS CT
  JOIN Customer AS C ON C.Customer_Id = CT.Cust_Id
  JOIN City ON City.City_Id = C.City_Id
  GROUP BY CityTier;
  ```

---

## 📈 Use Cases

- Track sales trends over time
- Identify top-performing product categories
- Analyze marketing campaign effectiveness
- Segment customers by city tier and income

---

## 🏷️ Hashtags

#DataAnalytics #SQL #DatabaseDesign #SalesAnalysis #CustomerAnalytics #BusinessIntelligence #PaintIndustry #IndianMarket

---

## 🤝 Contributing

Pull requests are welcome! For major changes, please open an issue first to discuss what you would like to change.

---

## 📬 Contact

For questions or collaboration, please open an issue or contact the project maintainer.

---

*Built with ❤️ for data-driven business insights!*

