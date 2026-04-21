# 🍽️ Food Wastage Management System

## 📌 Project Overview
This project is an end-to-end data analytics solution designed to reduce food wastage by connecting food providers (restaurants, supermarkets, etc.) with receivers (NGOs, individuals).  

It enables tracking, analysis, and efficient redistribution of surplus food using SQL analytics and an interactive dashboard.

---

## 🎯 Problem Statement
Food wastage is a major global issue, while many people still suffer from food shortages.  

There is a lack of systems that:
- Track surplus food efficiently  
- Connect donors with receivers  
- Provide insights into food distribution and wastage  

---

## 💡 Solution
This project solves the problem by building a **data-driven system** that:

- Tracks food donations from providers  
- Manages claims from receivers  
- Identifies expiring and unclaimed food  
- Provides analytical insights using SQL  
- Visualizes data through an interactive dashboard  

---

## 🏗️ Project Architecture

---

## 🛠️ Technologies Used

- 🐍 Python (Pandas, MySQL Connector)
- 🗄️ MySQL (Database + SQL Queries)
- 📊 Streamlit (Interactive Dashboard)
- 💻 VS Code (Development)

---

## 🧱 Database Design

The system consists of 4 main tables:

1. **Providers** – Stores food donors  
2. **Receivers** – Stores food receivers  
3. **Food Listings** – Tracks available food  
4. **Claims** – Tracks food requests  

✔ Implemented Primary Keys and Foreign Keys  
✔ Maintained relational integrity  

---

## 🧹 Data Cleaning & Processing

- Removed missing values  
- Standardized text fields (city, food type)  
- Converted data types  
- Fixed invalid dates  
- Ensured consistency for analysis  

---

## 📥 Data Loading

- Loaded CSV data using Pandas  
- Inserted data into MySQL  
- Handled:
  - Duplicate entries  
  - Data type mismatches  
  - Missing columns  

---

## 📊 SQL Analysis

Performed 15+ analytical queries:

- Providers & receivers per city  
- Provider contribution analysis  
- Claims status percentage  
- Most common food types  
- Expiring food detection  
- Unclaimed food identification  
- Top providers & receivers  

---

## 🖥️ Streamlit Dashboard Features

- 🔍 Filters (city, food type, provider type)  
- 📊 KPI Metrics  
- 📈 Charts (EDA)  
- ⚠️ Expiry Alerts  
- 📞 Contact Providers  
- 📂 SQL Query Results  
- ⚙️ CRUD Operations  

---

## ⚙️ CRUD Operations

- ➕ Add food listings  
- ✏️ Update records  
- ❌ Delete entries  

---

## 📈 Key Insights

- Identified high food wastage areas  
- Analyzed provider contributions  
- Tracked claim patterns  
- Highlighted expiring food  

---

## 🚀 Future Enhancements

- Authentication system  
- Multi-page dashboard  
- Cloud deployment  
- Real-time notifications  

---

## 🔗 Project Links
[Uploadinimport streamlit as st
import pandas as pd
from db import get_connection

# ================================
# PAGE CONFIG
# ================================
st.set_page_config(page_title="Food Dashboard", layout="wide")

st.title("🍽️ Food Wastage Management System")
st.success("✅ Data Loaded Successfully")

# ================================
# DB CONNECTION
# ================================
conn = get_connection()

# ================================
# LOAD DATA
# ================================
food = pd.read_sql("SELECT * FROM food_listings", conn)
food['expiry_date'] = pd.to_datetime(food['expiry_date'])

# ================================
# KPIs
# ================================
col1, col2, col3 = st.columns(3)
col1.metric("Total Food Items", len(food))
col2.metric("Total Quantity", food['quantity'].sum())
col3.metric("Unique Cities", food['location'].nunique())

# ================================
# FILTERS
# ================================
st.sidebar.header("Filters")

city = st.sidebar.selectbox("Select City", sorted(food['location'].unique()))
food_type = st.sidebar.selectbox("Select Food Type", sorted(food['food_type'].unique()))
provider = st.sidebar.selectbox("Select Provider Type", sorted(food['provider_type'].unique()))

filtered = food[
    (food['location'] == city) &
    (food['food_type'] == food_type) &
    (food['provider_type'] == provider)
]

st.subheader("📌 Filtered Data")
st.dataframe(filtered)

# ================================
# CONTACT PROVIDERS
# ================================
st.markdown("---")
st.subheader("📞 Contact Providers")

query = f"""
SELECT name, contact, city
FROM providers
WHERE city = '{city}'
"""

contacts = pd.read_sql(query, conn)
st.dataframe(contacts)

# ================================
# CHARTS
# ================================
st.markdown("---")

st.subheader("🍱 Food Type Distribution")
st.bar_chart(food['food_type'].value_counts())

st.subheader("🏢 Food Contribution by Provider Type")
provider_chart = food.groupby("provider_type")["quantity"].sum()
st.bar_chart(provider_chart)

# ================================
# EXPIRY ALERT
# ================================
today = pd.Timestamp.today()
soon = today + pd.Timedelta(days=2)

expiring = food[
    (food['expiry_date'] >= today) &
    (food['expiry_date'] <= soon)
]

st.subheader("⚠️ Expiring Soon (Next 2 Days)")
st.dataframe(expiring)

# ================================
# CRUD OPERATIONS
# ================================
st.markdown("---")
st.header("⚙️ Manage Food Listings")

# ➕ ADD
st.subheader("➕ Add Food")
food_name = st.text_input("Food Name")
quantity = st.number_input("Quantity", min_value=1)
location_input = st.text_input("Location")

if st.button("Add Food"):
    cursor = conn.cursor()
    cursor.execute("""
        INSERT INTO food_listings (food_name, quantity, location)
        VALUES (%s, %s, %s)
    """, (food_name, quantity, location_input))
    conn.commit()
    st.success("✅ Food Added")

# ✏️ UPDATE
st.subheader("✏️ Update Food Quantity")
update_id = st.number_input("Food ID", min_value=1, key="update")
new_quantity = st.number_input("New Quantity", min_value=1)

if st.button("Update Food"):
    cursor = conn.cursor()
    cursor.execute("""
        UPDATE food_listings
        SET quantity = %s
        WHERE food_id = %s
    """, (new_quantity, update_id))
    conn.commit()
    st.success("✅ Updated")

# ❌ DELETE
st.subheader("❌ Delete Food")
delete_id = st.number_input("Food ID to Delete", min_value=1, key="delete")

if st.button("Delete Food"):
    cursor = conn.cursor()
    cursor.execute("DELETE FROM food_listings WHERE food_id = %s", (delete_id,))
    conn.commit()
    st.success("✅ Deleted")

# ================================
# SQL QUERIES (BUTTONS)
# ================================
st.markdown("---")
st.header("📊 SQL Insights")

if st.button("1️⃣ Providers & Receivers per City"):
    df = pd.read_sql("""
        SELECT p.city,
               COUNT(DISTINCT p.provider_id) AS providers,
               COUNT(DISTINCT r.receiver_id) AS receivers
        FROM providers p
        LEFT JOIN receivers r ON p.city = r.city
        GROUP BY p.city
    """, conn)
    st.dataframe(df)

if st.button("2️⃣ Provider Contribution"):
    df = pd.read_sql("""
        SELECT provider_type, SUM(quantity) AS total
        FROM food_listings
        GROUP BY provider_type
        ORDER BY total DESC
    """, conn)
    st.dataframe(df)

if st.button("3️⃣ Top Receivers"):
    df = pd.read_sql("""
        SELECT receiver_id, COUNT(*) AS claims
        FROM claims
        GROUP BY receiver_id
        ORDER BY claims DESC
        LIMIT 10
    """, conn)
    st.dataframe(df)

if st.button("4️⃣ Claims Status %"):
    df = pd.read_sql("""
        SELECT status,
        COUNT(*) * 100.0 / (SELECT COUNT(*) FROM claims) AS percentage
        FROM claims
        GROUP BY status
    """, conn)
    st.dataframe(df)

if st.button("5️⃣ Most Common Food Type"):
    df = pd.read_sql("""
        SELECT food_type, COUNT(*) AS total
        FROM food_listings
        GROUP BY food_type
        ORDER BY total DESC
    """, conn)
    st.dataframe(df)

if st.button("6️⃣ Top City by Listings"):
    df = pd.read_sql("""
        SELECT location, COUNT(*) AS total
        FROM food_listings
        GROUP BY location
        ORDER BY total DESC
        LIMIT 5
    """, conn)
    st.dataframe(df)

if st.button("7️⃣ Claims per Food"):
    df = pd.read_sql("""
        SELECT food_id, COUNT(*) AS total_claims
        FROM claims
        GROUP BY food_id
        ORDER BY total_claims DESC
    """, conn)
    st.dataframe(df)

if st.button("8️⃣ Top Providers by Claims"):
    df = pd.read_sql("""
        SELECT f.provider_id, COUNT(*) AS total_claims
        FROM claims c
        JOIN food_listings f ON c.food_id = f.food_id
        GROUP BY f.provider_id
        ORDER BY total_claims DESC
    """, conn)
    st.dataframe(df)g app.py…]()

### 📁 Python Files
👉 [app.py](https://github.com/user-attachments/files/26925164/app.py)
[db.py](https://github.com/user-attachments/files/26925367/db.py)

### 📊 PPT Presentation
👉 [Food-Wastage-Management-System.pptx](https://github.com/user-attachments/files/26925417/Food-Wastage-Management-System.pptx)


### 🗄️ SQL Script
👉 [SQL Script.sql](https://github.com/user-attachments/files/26925429/SQL.Script.sql)


### 🗄️ Project Analysis Report(QUESTION ANSWERED)
👉 [SQL PROJECT ANALYSIS ANSWERS.pdf](https://github.com/user-attachments/files/26925447/SQL.PROJECT.ANALYSIS.ANSWERS.pdf)

### 🗄️ Dashboard Screenshort's(Streamlit)
👉<img width="1896" height="923" alt="Screenshot 2026-04-21 140028" src="https://github.com/user-attachments/assets/d40a9cc5-5748-4044-b5b7-cc4586066064" />
<img width="1902" height="912" alt="Screenshot 2026-04-21 140019" src="https://github.com/user-attachments/assets/079b6a2c-0f6a-417a-9081-cf4a7f490984" />
<img width="1901" height="917" alt="Screenshot 2026-04-21 135953" src="https://github.com/user-attachments/assets/f186f61c-64da-4343-9de9-36dd7bd22ed2" />
<img width="1901" height="916" alt="Screenshot 2026-04-21 135931" src="https://github.com/user-attachments/assets/3f87bedc-e3e6-4ee7-8a51-9152b578fb0b" />

---

## 💬 Conclusion

This project demonstrates how data analytics can be used to solve real-world problems like food wastage by combining:

- Data Engineering  
- SQL Analysis  
- Interactive Visualization  

---

## ⭐ If you like this project
Give it a ⭐ on GitHub!
