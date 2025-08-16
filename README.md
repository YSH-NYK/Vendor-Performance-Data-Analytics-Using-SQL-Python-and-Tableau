# Vendor-Performance-Data-Analytics

## 📌 Problem Statement  
Effective inventory and sales management are critical for optimizing profitability in the retail and wholesale industry. Companies must avoid losses caused by:  

- Inefficient pricing strategies  
- Poor inventory turnover  
- Vendor dependency  

This project analyzes retail & wholesale data to provide insights into:  

1. **Identifying underperforming brands** that require promotion or pricing adjustment  
2. **Top vendors contributing to gross profit**  
3. **Impact of bulk purchasing** on cost savings  
4. **Inventory turnover analysis** to reduce holding cost & improve efficiency  

---

## 📂 Dataset  (https://drive.google.com/drive/folders/1erbLbZfkdrBo5fBNuPR1sFVMkdXnivg7)
The dataset contains purchase, sales, and vendor information:  

- **purchase_price** → Price at which brands were purchased from vendors (actual brand price).  
- **purchases** → Transaction-level details of purchases (quantities, amounts, dates).  
- **vendor_invoice** → Aggregate purchase data per vendor (quantity, total spent).  
- **begin_inventory / end_inventory** → Store & brand-wise inventory snapshots.  
- **sales** → Sales transaction details (quantity sold, selling price).  

---

## ⚙️ Workflow  

### **Step 1 – Data Collection**  
Collected raw CSV data (purchases, purchase_price, sales, vendor_invoice).  

### **Step 2 – Database Ingestion**  
Stored all CSVs into an SQLite database:  
```python
engine = create_engine('sqlite:///Data/inventory.db')
df.to_sql(table_name, con=engine, if_exists='replace', index=False)
```

### **Step 3 – Logging**  
Implemented logging for tracking script execution.  
```python
logging.basicConfig(
    filename="logs/Overall_logs.log",
    level=logging.DEBUG,
    format="%(asctime)s-%(levelname)s - %(message)s",
    filemode="a"
)
```

### **Step 4 – Initial EDA (SQL)**  
- Explored all 4 tables  
- Understood relationships between sales, purchase & vendor data  

### **Step 5 – Feature Engineering**  
Created a **consolidated table** with derived features:  
- `GrossProfit = TotalSalesDollars - TotalPurchaseDollars`  
- `ProfitMargin = (GrossProfit / TotalSalesDollars) * 100`  
- `StockTurnover = TotalSalesQuantity / TotalPurchaseQuantity`  
- `SalesToPurchaseRatio = TotalSalesDollars / TotalPurchaseDollars`  

### **Step 6 – Final EDA**  
- Identified **loss-making products** (negative gross profit)  
- Detected **zero-sale items** (slow-moving / obsolete stock)  
- Highlighted **outliers** (premium products, abnormal freight costs)  
- Correlation insights:  
  - Purchase price had weak effect on profit & sales  
  - High correlation between purchase quantity & sales quantity (~1.0)  
  - Higher sales price → lower profit margin (competitive pricing effect)  
  - Stock turnover ≠ guaranteed profitability  

### **Step 7 – Insights & Solutions**  

✅ **1. Brands requiring promotion or price adjustment**  
- Low sales + High profit margin → candidates for price drops / promotional campaigns.  

✅ **2. Top-performing vendors & brands**  
- Ranked vendors & brands by total sales revenue.  

✅ **3. Vendor purchase contribution**  
- Identified vendors contributing most to overall purchase dollars.  
- Visualized via **pie chart (Top 10 vendors + Others)**.  

✅ **4. Impact of bulk purchase**  
- Grouped purchase sizes into **Small / Medium / Large**.  
- Found that **bulk purchases reduce unit cost** → encourage bulk buying.  

✅ **5. Inventory turnover**  
- Calculated **unsold inventory value (capital locked)**:  
```python
df["UnsoldInventoryValue"] = (df["TotalPurchaseQuantity"] - df["TotalSalesQuantity"]) * df["PurchasePrice"]
```
- Identified vendors with highest unsold stock.  

---

## 📊 Dashboard (Tableau)  

All insights & KPIs were visualized in a **Tableau Dashboard**:  
<img width="1919" height="997" alt="Dashboard" src="https://github.com/user-attachments/assets/28e77cfc-9aec-4af2-a825-52b16faff83c" />

  

---

## 🚀 Tech Stack  
- **Python** → Data ingestion, preprocessing, feature engineering  
- **SQLite** → Database storage  
- **Pandas / NumPy** → Data manipulation  
- **Matplotlib / Seaborn** → Visual EDA  
- **Tableau** → Final Dashboard & storytelling  
- **Logging** → Monitoring script execution  

---

## 📌 Key Business Outcomes  
- Identified **brands needing promotion or price cuts**  
- Highlighted **top vendors driving profit**  
- Showed that **bulk purchasing reduces costs**  
- Quantified **capital locked in unsold inventory**  
- Provided data-driven strategies for **improving turnover & profitability**  



