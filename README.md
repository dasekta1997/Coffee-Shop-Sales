# 📦 Coffee Shop Sales — Repo Upgrade Pack


* `README.md`
* `dax/measures.md`
* `data/README.md` (data dictionary)
* `powerquery/transformations.m` (starter M script for export/extend)

---

## README.md 

````markdown
# ☕ Coffee Shop Sales — Power BI Dashboard

> Analyze a coffee chain’s sales to answer 10 business questions: peak hours, product mix, top stores, and seasonality. Built in Power BI with DAX + Power Query.

![Dashboard Overview](docs/overview.png)
<img width="480" height="269" alt="{ADC32B74-7FE3-42C3-AD1D-0DF09625DCE1}" src="https://github.com/user-attachments/assets/63137cd4-efeb-4038-ad5d-57f4977dda96" />
<img width="481" height="271" alt="{14ED89E2-449B-46D2-B668-63A34EACC2B1}" src="https://github.com/user-attachments/assets/c58cf2fa-9a83-4b11-8ad6-ac94b16ff5e1" />


<p align="left">
  <img alt="license" src="https://img.shields.io/badge/License-MIT-green" />
  <img alt="powerbi" src="https://img.shields.io/badge/Power%20BI-Desktop-yellow" />
  <img alt="status" src="https://img.shields.io/badge/Status-v1.0-blue" />
</p>

## 🔎 Results at a glance
- **Total Revenue (Jan–Jun 2023):** $698.81K  
- **Average Daily Sales:** $1,185  
- **Top Store:** Hell’s Kitchen ($237K, ~34% of revenue)  
- **Other Stores:** Astoria $232K (steady MoM growth), Lower Manhattan $230K  
- **Top Category:** Coffee ($270K), followed by Tea ($196K)  
- **Top Product Type:** Barista Espresso ($91K), Brewed Chai Tea ($77K), Hot Chocolate ($72K)  
- **Peak Sales Hours:** 8–10 AM (10 AM = $89K, 9 AM = $85K, 8 AM = $83K)  
- **Average Transaction Value:** $4.81 at Lower Manhattan (highest), $4.66 Hell’s Kitchen, $4.59 Astoria  
- **Monthly Growth:** Lowest: Feb $76K → Highest: Jun $166K (2× growth)  
- **Weekday Trend:** Saturday busiest (21.7K txns), Sunday weakest (20.5K)  
- **High-Value Item:** Civet Cat Coffee at $45/unit (luxury outlier)

## 🧱 Data & Model
- **Grain:** Transaction (one row per item/receipt line)
- **Period:** Jan 2023 → Jun 2023
- **Schema:** Star (Fact `Transactions` → Dim `Date`, `Product`, `Store`)
- **Key columns:** `TxnID`, `TxnDateTime`, `Qty`, `UnitPrice`, `ProductCategory`, `ProductType`, `StoreLocation`

## 🔧 Transformations (Power Query)
- Type fixes, trim/clean text
- Derive `Date`, `Time`, `Hour`, `DayOfWeek`, `Month`, `Year`
- Compute `Line Revenue = UnitPrice * Qty`
- Optional: filter out negative/zero prices; handle missing values

Power Query M script: see [`/powerquery/transformations.m`](powerquery/transformations.m)

## 🧮 Measures (DAX)
Key measures used across visuals:

```DAX
Total Revenue := SUMX(Transactions, Transactions[UnitPrice] * Transactions[Qty])

Transactions Count := DISTINCTCOUNT(Transactions[TxnID])

AOV := DIVIDE([Total Revenue], [Transactions Count])

Items per Transaction := DIVIDE(SUM(Transactions[Qty]), [Transactions Count])

Weekend Revenue := CALCULATE([Total Revenue], 'Date'[IsWeekend] = TRUE())

Weekend Revenue % := DIVIDE([Weekend Revenue], [Total Revenue])

MoM Revenue % :=
VAR Curr = [Total Revenue]
VAR Prev = CALCULATE([Total Revenue], DATEADD('Date'[Date], -1, MONTH))
RETURN DIVIDE(Curr - Prev, Prev)

YoY Revenue % :=
VAR Curr = [Total Revenue]
VAR Prev = CALCULATE([Total Revenue], DATEADD('Date'[Date], -1, YEAR))
RETURN DIVIDE(Curr - Prev, Prev)

Top Category Revenue :=
CALCULATE([Total Revenue], TOPN(1, ALL('Product'[Category]), [Total Revenue], DESC))
````

> Full list with descriptions in [`/dax/measures.md`](dax/measures.md).

## 📊 Visuals

* KPI cards: Revenue, AOV, Items/Txn, MoM %, YoY %
* Area/line: Revenue by Date (with slicers for Month/Year)
* Column chart: Revenue by Hour (peak hours)
* Bar: Revenue by Product Category / Product Type
* Map/Bar: Revenue by Store Location
* Matrix: Category × Month (seasonality)
* Donut: Transactions by Store
* Funnel: Revenue per Unit Sold (Top 10 items)
All Visuals can be find in the given link below
[Google Drive Folder](https://drive.google.com/file/d/1wxssjyjRxa-DcGt73XKMI7eobDpS2V4A/view?usp=drive_link)

## ▶️ Reproduce / Run

1. Clone repo
2. Open `powerbi/CoffeeShopSales.pbix` in **Power BI Desktop** (or Power BI Service)
3. If using your own data, map columns as described in `/data/README.md`
4. Click **Refresh** — all visuals and measures recalc from Power Query/DAX

## 📁 Repo Structure

```
/data/Coffee_Shop_Sales_sample.csv    # sample or anonymized data
/dax/measures.md                      # complete list of DAX measures
/powerbi/CoffeeShopSales.pbix         # Power BI report file
/powerquery/transformations.m         # exported Power Query steps
/docs/overview.png                    # screenshots (add more if needed)
README.md
LICENSE
```

## 🧠 Insights

* **Morning sales (8–10 AM)** drive the highest revenue; staff and inventory should be aligned here.
* **Hell’s Kitchen** earns the most revenue (\$237K) despite similar transaction volume to Astoria → higher ATV.
* **Coffee & Tea** dominate (\$466K combined) → prioritize marketing/promotions in these categories.
* **Premium products (Civet Cat, \$45)** show strong margin opportunities.
* **Weekend sales** are higher, but **Sunday underperforms**; consider promotions to boost traffic.
* **Q2 revenue doubled vs Q1** → strong seasonality; prepare campaigns for Apr–Jun.


MIT — use freely with attribution.

---

## LICENSE (MIT) — copy this into a new file named `LICENSE` in the repo root

```
MIT License

Copyright (c) 2025 Ekta Das

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
