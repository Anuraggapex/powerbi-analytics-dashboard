# 📊 Interactive Power BI Analytics Dashboard

Welcome! This repository houses a complete, end-to-end Power BI dashboard built to turn messy, raw data into a clean, interactive story. 

Instead of just looking at spreadsheets, this project is designed to give stakeholders a clear, visual way to track KPIs, spot trends, and make smart, data-driven decisions.

---

## 🚀 Quick Preview

> 💡 **Note:** Since GitHub can't open a `.pbix` file on its own, here is a look at the main reporting canvas. To click around and interact with the data, you can download the file locally!

---

## 🛠️ What went into this project?

Here is a breakdown of the actual work done behind the scenes to take this from raw data to a finished dashboard:

### 1. Data Cleaning & Prep (Power Query)
* **The Cleanup:** Nobody likes messy data. I used Power Query to handle missing values, fix data types, and standardize dates so the model runs smoothly.
* **Smart Shaping:** Merged and shaped tables at the ingestion stage to keep the final file lightweight and fast.

### 2. Building the Engine (Data Modeling)
* **Star Schema:** Set up a clean, organized relational model using Fact and Dimension tables so filtering is fast and intuitive.
* **Calendar Control:** Built a custom, continuous Date table to unlock Power BI's time-intelligence features (like comparing this month's data to last month's).

### 3. Crunching the Numbers (DAX)
* **Custom Measures:** Wrote tailored DAX formulas to calculate key business metrics on the fly based on whatever filters the user selects.
* **Time Intelligence:** Programmed calculations to automatically track Period-over-Period growth and Year-to-Date (YTD) trends.
* **Clean Code:** Used `VAR` (variables) in complex formulas to keep the logic clean and execution speeds fast.

### 4. Designing the Experience (UI/UX)
* **Easy Navigation:** Designed the layout to look and feel like an app, using natural navigation paths so users don't get lost.
* **Deep Dives:** Set up interactive slicers, cross-filtering, and drill-downs so you can look at the big picture or zoom all the way into individual rows of data.
* **Helpful Tooltips:** Added custom hover tooltips to give extra context without crowding the main screen.

---

## 📁 What's in this Repo?

```text
├── README.md               <- This guide right here
├── dashboard-preview.png   <- A screenshot of the dashboard
├── dashboard-preview.png   <- A screenshot of the dashboard
├── dashboard-preview.png   <- A screenshot of the dashboard
├── dashboard-preview.png   <- A screenshot of the dashboard
├── dashboard-preview.png   <- A screenshot of the dashboard
├── dashboard-preview.png   <- A screenshot of the dashboard
├── dashboard-preview.png   <- A screenshot of the dashboard 
└── analytics_report.pbix   <- The core Power BI file (data, model, and visuals)
