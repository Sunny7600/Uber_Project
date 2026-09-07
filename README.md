# 🚗 Uber Ride Analytics — End-to-End Data Analytics Project

An end-to-end Data Analytics project analyzing over **40,000+ ride booking records** for Uber in July 2024. This project covers data cleaning in Excel, relational querying and view creation in MySQL, and interactive dashboard design using Power BI.

---

## 📌 Project Overview

The objective of this project is to analyze ride booking trends, revenue distribution, ride cancellation reasons (by customers and drivers), and driver/customer rating performance to extract actionable business insights for operational efficiency.

### 🛠️ Tech Stack & Tools
* **Excel:** Initial data cleaning and data structure validation.
* **MySQL Workbench:** Database schema setup, SQL queries execution, and SQL View creation for business KPIs.
* **Power BI:** Data modeling, DAX measures creation, and multi-page interactive dashboard visualization.

---

## 📊 Key Dashboard Metrics & Business Insights

### 1. Overall Performance
* **Total Bookings:** 40.54K
* **Total Booking Value (Gross Revenue):** ₹13.76 Million
* **Success Rate:** **62.18%** (~25.21K rides completed successfully).
* **Cancellation Breakdown:**
  * **Canceled by Driver:** 17.79% (7.21K rides)
  * **Canceled by Customer:** 10.06% (4.08K rides)
  * **Driver Not Found:** 9.97% (4.04K rides)

### 2. Revenue & Payment Analysis
* **Cash & UPI** are the primary payment channels, generating over 85%+ of total booking values.
* **Credit Card & Debit Card** payments account for a lower volume of completed transactions.

### 3. Cancellation Reasons
* **Driver Cancellations:** Personal & car-related issues (34.66%) and customer-related issues (29.45%) were the main drivers.
* **Customer Cancellations:** Driver not moving towards pickup location (29.88%) and driver asking to cancel (25.94%) were the top reasons.

### 4. Vehicle Type & Ratings
* **Fleet Coverage:** Auto, Bike, E-Bike, Mini, Prime Sedan, Prime SUV, and Prime Plus.
* **Average Ratings:** Ratings remain balanced across vehicle categories (~3.98 to 4.02 for drivers and ~3.98 to 4.01 for customers).

---

## 🗄️ SQL Views & Business Queries

All key analytical requirements were answered using SQL Views in MySQL:

```sql
-- 1. Retrieve all successful bookings
CREATE VIEW Successful_Bookings AS
SELECT * FROM bookings WHERE Booking_Status = 'Success';

-- 2. Find the average ride distance for each vehicle type
CREATE VIEW ride_distance_for_each_vehicle AS
SELECT Vehicle_Type, AVG(Ride_Distance) AS avg_distance 
FROM bookings GROUP BY Vehicle_Type;

-- 3. Total number of canceled rides by customers
CREATE VIEW canceled_rides_by_customers AS
SELECT COUNT(*) FROM bookings WHERE Booking_Status = 'canceled by Customer';

-- 4. Top 5 customers by number of rides
CREATE VIEW Top_5_Customers AS
SELECT Customer_ID, COUNT(Booking_ID) AS total_rides
FROM bookings GROUP BY Customer_ID ORDER BY total_rides DESC LIMIT 5;

-- 5. Rides canceled by drivers due to personal/car issues
CREATE VIEW Rides_canceled_by_Drivers_P_C_Issues AS
SELECT COUNT(*) FROM bookings
WHERE canceled_Rides_by_Driver = 'Personal & Car related issue';

-- 6. Max & Min driver ratings for Prime Sedan
CREATE VIEW Max_Min_Driver_Rating AS
SELECT MAX(Driver_Ratings) AS max_rating, MIN(Driver_Ratings) AS min_rating
FROM bookings WHERE Vehicle_Type = 'Prime Sedan';

-- 7. All rides paid via UPI
CREATE VIEW UPI_Payment AS
SELECT * FROM bookings WHERE Payment_Method = 'UPI';

-- 8. Average customer rating per vehicle type
CREATE VIEW AVG_Cust_Rating AS
SELECT Vehicle_Type, AVG(Customer_Rating) AS avg_customer_rating
FROM bookings GROUP BY Vehicle_Type;

-- 9. Total booking value of successful rides
CREATE VIEW total_successful_ride_value AS
SELECT SUM(Booking_Value) AS total_successful_ride_value
FROM bookings WHERE Booking_Status = 'Success';

-- 10. List all incomplete rides along with the reason
CREATE VIEW Incomplete_Rides_Reason AS
SELECT Booking_ID, Incomplete_Rides_Reason
FROM bookings WHERE Incomplete_Rides = 'Yes';
```

##Key DAX Measures Used in Power BI

```sql // Total Bookings
TotalBookings = COUNTROWS(july)

// Total Canceled Bookings
CanceledBookings = 
CALCULATE(
    COUNTROWS(july),
    july[Booking_Status] IN {"Canceled by Driver", "Canceled by Customer"}
)

// Cancellation Percentage Rate
CanceledPercentage = 
DIVIDE([CanceledBookings], [TotalBookings], 0) * 100
```

## Dashboard Structure (Power BI)

The dashboard contains 5 dedicated analytical view pages:

Overall View: High-level KPIs, Ride Volume over time, and Booking Status pie chart breakdown.
Vehicle Type View: Vehicle performance comparison, average distance, and booking values.
Revenue View: Payment method revenue breakdown and top customer spending.
Cancellation View: Deep dive into customer and driver cancellation reasons.
Ratings View: Comparative rating matrix across customer and driver dimensions.
