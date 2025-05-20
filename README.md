# 🚴 Ingesting Real-Time Bike Data with Eventstream in Microsoft Fabric

## 🎯 Purpose of the Lab

This lab demonstrates how to use **Microsoft Fabric’s Eventstream** feature to capture, transform, and analyze real-time data without writing any code. The real-time data used in this lab comes from a sample source simulating bicycle collection points in a city bike-share system.  
The goal is to route streaming data into an Eventhouse database, apply transformations like aggregation, and visualize the results using **Kusto Query Language (KQL)**.

---

## 🛠️ Step-by-Step Process

### 1. Creating the Workspace

- Created a new **workspace** with Fabric capacity enabled
- Verified that the workspace was created and empty

---

### 2. Creating the Eventhouse

- Created a new **Eventhouse** inside the workspace
- Noted the automatic creation of a **KQL database**
- Opened the KQL database (initially empty)

---

### 3. Creating the Eventstream

- Navigated to the KQL database and selected **Get data**
- Chose **Eventstream > New eventstream** and named it `Bicycle-data`

---

### 4. Adding a Data Source

- Selected **Use sample data**
- Added the **Bicycles** sample stream as a source
- Verified the stream appeared in the Eventstream canvas

---

### 5. Adding a Destination

- Added **Eventhouse** as a destination for raw data
- Configured:
  - Destination Name: `bikes-table`
  - Table Name: `bikes`
  - Input Format: JSON
- Published the Eventstream
- Verified ingested data using the **Data Preview** and **Data Insights** tabs

---

### 6. Querying Captured Data

- Opened the **KQL database**
- Queried the `bikes` table using built-in KQL query:
  ```kusto
  bikes
  | where ingestion_time() between (now(-1d) .. now())
  ```
---

## 7. Transforming the Event Data 🔁 

- Edited the **Eventstream** to group data by `Street`

- Configured the transformation:

  - **Operation:** `GroupByStreet`  
  - **Aggregation:** `SUM(No_Bikes)`  
  - **Group by:** `Street`  
  - **Time Window:** Tumbling (every **5 seconds**)  

- Added Eventhouse destination:

  - **Destination Name:** `bikes-by-street-table`  
  - **Table Name:** `bikes-by-street`

- Published the stream and verified grouped output

---

## 8. Querying Transformed Data 🔍 

Used the following KQL query on the `bikes-by-street` table:

```kusto
['bikes-by-street']
| summarize TotalBikes = sum(tolong(SUM_No_Bikes)) by Window_End_Time, Street
| sort by Window_End_Time desc, Street asc
```
- Displayed total bikes per street for each 5-second window

---

## 📘 What I Learned

- Ingesting and routing real-time streaming data via **Eventstream**
- Setting up **Eventhouse** and connecting to **KQL databases**
- Performing real-time data transformation using **no-code UI**
- Writing and running **KQL queries** for time-based analysis


---

## Screenshots

<img width="1197" alt="1" src="https://github.com/user-attachments/assets/67a667fe-6ad2-495d-adcf-ad3befd927b5" />

<img width="923" alt="2" src="https://github.com/user-attachments/assets/e2755a9c-a222-4fb9-a408-9b14fb2cfc06" />

<img width="1423" alt="3" src="https://github.com/user-attachments/assets/6a613d29-e7f2-4883-807d-64d1a658c07a" />

<img width="1400" alt="4" src="https://github.com/user-attachments/assets/34a61a0a-6e01-4b93-86be-41b0010a9dd3" />

<img width="1447" alt="5" src="https://github.com/user-attachments/assets/d720162b-56b6-4cbe-a385-3ad0fdd5efb8" />

<img width="1490" alt="6" src="https://github.com/user-attachments/assets/7e7f77cc-7d69-4999-90b0-938eeffad596" />

<img width="1324" alt="7" src="https://github.com/user-attachments/assets/0ceb0457-e06d-4b04-8993-978a464976c9" />

<img width="1451" alt="8" src="https://github.com/user-attachments/assets/8038e1a7-c78f-47f1-8ca9-d371be3999c1" />

<img width="1443" alt="9" src="https://github.com/user-attachments/assets/ace9648b-99aa-498c-b1a7-318e5e28ca0e" />

<img width="1227" alt="10" src="https://github.com/user-attachments/assets/d0cda7cb-551a-4a2f-8070-8bc9963769ed" />
