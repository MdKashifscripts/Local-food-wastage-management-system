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
