# 🚢 Automotive Importation & Landing Cost Optimization Pipeline

An end-to-end data analytics project designed to evaluate container shipping logistics, port landing costs, and clearing agent operational efficiency for automotive importation.

---

## 📊 Live Interactive Dashboard
You can explore the live, interactive Tableau Public dashboard here:  
👉 **[View Dashboard on Tableau Public](https://public.tableau.com/app/profile/clinton.ubah4498/viz/AutomotiveImportationLandingCostOptimization/Dashboard1)**

---

## 🔍 Project Overview & Objectives
Managing international automotive supply chains involves tracking complex cost layers—including CIF value, customs duties, terminal handling fees, and demurrage penalties. This project builds a complete data pipeline from a relational MySQL database to an interactive Tableau dashboard to answer core business questions:
* *Which clearing agents deliver the highest operational volume while minimizing demurrage costs and clearance turnaround times?*
* *How do total landing costs break down across various ports of origin (Antwerp, Hamburg, Jebel Ali, Shanghai)?*
* *What are the monthly expenditure trends and seasonal volume fluctuations in container shipments?*

---

## 🛠️ Tech Stack & Architecture
* *Database & Querying:* MySQL (container_logistics_db)
* *Data Cleansing & Modeling:* SQL JOIN operations across relational tables (shipments, clearing_agents, landing_costs) exported to logistics_master_data.csv
* *Visualization & BI:* Tableau Public (featuring KPI cards, stacked bar charts, and continuous time-series trend lines)

---

## 📈 Key Insights & Findings
1. *Top-Performing Clearing Agent:* *Vanguard Port Service* emerged as the most efficient agent, handling the highest volume (*212 containers), maintaining a fast **11.4-day clearance average, and securing the lowest average demurrage fees ($507.17*).
2. *Cost Drivers:* *CIF Value* and *Customs Duty* account for the overwhelming majority of total landing costs across all destination ports at Tin Can Island.
3. *Temporal Trends:* Monthly landing cost fluctuations closely track container arrival volumes, highlighting peak shipping windows throughout the year.

---

## 🗄️ Database Schema & SQL Query Sample
The relational database utilizes three core tables linked via foreign keys:
* shipments (tracks bill of lading, container ID, shipping lines, origin ports, and arrival/clearance dates)
* clearing_agents (stores agent names, agency tiers, and ID mappings)
* landing_costs (records financial components including CIF value, customs duty, terminal handling, and demurrage fees)

```sql
-- Sample Master Data Extraction Query
SELECT 
    s.container_id,
    s.bill_of_lading,
    c.agent_name,
    c.agency_tier,
    s.port_of_origin,
    s.destination_port,
    s.arrival_date,
    s.clearance_date,
    DATEDIFF(s.clearance_date, s.arrival_date) AS clearance_duration_days,
    l.cif_value_usd,
    l.customs_duty_usd,
    l.terminal_handling_usd,
    l.demurrage_days,
    l.demurrage_fee_usd,
    (l.cif_value_usd + l.customs_duty_usd + l.terminal_handling_usd + l.demurrage_fee_usd) AS total_landing_cost_usd
FROM shipments s
JOIN clearing_agents c ON s.agent_id = c.agent_id
JOIN landing_costs l ON s.container_id = l.container_id;
