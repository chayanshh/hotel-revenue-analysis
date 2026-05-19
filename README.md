<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1b2a,50:1b4332,100:40916c&height=210&section=header&text=Hotel%20Revenue%20Analysis&fontSize=46&fontColor=d8f3dc&fontAlignY=38&desc=Hospitality%20Intelligence%20%7C%20Power%20BI%20%7C%20Star%20Schema&descAlignY=58&descSize=15&descColor=95d5b2&animation=fadeIn" width="100%"/>

<p>
  <img src="https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black"/>
  <img src="https://img.shields.io/badge/Data%20Model-Star%20Schema-40916c?style=for-the-badge&logo=databricks&logoColor=white"/>
  <img src="https://img.shields.io/badge/Tables-5%20CSVs-0d1b2a?style=for-the-badge&logo=files&logoColor=white"/>
  <img src="https://img.shields.io/badge/Domain-Hospitality%20Analytics-52b788?style=for-the-badge"/>
</p>

<p>
  <a href="#-overview">Overview</a> •
  <a href="#-data-model">Data Model</a> •
  <a href="#-kpis--metrics">KPIs</a> •
  <a href="#-project-structure">Structure</a> •
  <a href="#-getting-started">Getting Started</a> •
  <a href="#-dashboard">Dashboard</a>
</p>

</div>

---

## 🏨 Overview

> **Turn hotel booking data into revenue intelligence — property by property, week by week.**

This project delivers an end-to-end **hospitality revenue analysis** built on a clean star schema data model and visualized through an interactive **Power BI dashboard**. It answers the questions hotel managers and revenue teams care about most: occupancy trends, room-type performance, cancellation impact, and booking platform effectiveness.

```
5 CSVs (Star Schema)  →  Power BI Data Model  →  KPI Dashboard  →  Revenue Decisions
```

### Core Business Questions Answered
- 🏷️ Which room types and properties generate the highest revenue?
- 📉 What is the cancellation rate and its revenue impact?
- 📅 How does occupancy fluctuate across weeks and seasons?
- 🌐 Which booking platforms drive the most realized revenue?
- ⭐ How do guest ratings correlate with occupancy and revenue?

---

## 🗄️ Data Model

This project follows a **Star Schema** design — industry-standard for analytical workloads.

```
                        ┌─────────────────┐
                        │   dim_date      │
                        │─────────────────│
                        │ date            │
                        │ mmm yy          │
                        │ week no         │
                        │ day type        │
                        └────────┬────────┘
                                 │
              ┌──────────────────┼──────────────────┐
              │                  │                  │
   ┌──────────▼──────┐  ┌────────▼────────┐  ┌─────▼──────────────────┐
   │   dim_hotels    │  │   dim_rooms     │  │   fact_bookings        │
   │─────────────────│  │─────────────────│  │────────────────────────│
   │ property_id     │  │ room_id         │  │ booking_id             │
   │ property_name   │  │ room_class      │  │ property_id            │
   │ category        │  └─────────────────┘  │ room_category          │
   │ city            │                       │ check_in_date          │
   └─────────────────┘                       │ checkout_date          │
                                             │ no_guests              │
                                             │ booking_platform       │
                                             │ ratings_given          │
                                             │ booking_status         │
                                             │ revenue_generated      │
                                             │ revenue_realized       │
                                             └────────────────────────┘
                                                        │
                                             ┌──────────▼──────────────┐
                                             │ fact_aggregated_bookings│
                                             │─────────────────────────│
                                             │ property_id             │
                                             │ check_in_date           │
                                             │ room_category           │
                                             │ successful_bookings     │
                                             │ capacity                │
                                             └─────────────────────────┘
```

### Table Reference

| Table | Type | Description |
|-------|------|-------------|
| `dim_hotels.csv` | Dimension | Hotel properties — name, category (Luxury/Business), city |
| `dim_rooms.csv` | Dimension | Room types mapped to class (Standard, Elite, Premium, Presidential) |
| `dim_date.csv` | Dimension | Date spine with week number and day type (weekday/weekend) |
| `fact_bookings.csv` | Fact | ~130K individual booking transactions with revenue & status |
| `fact_aggregated_bookings.csv` | Fact | Pre-aggregated bookings by property, date, and room category |

---

## 📊 KPIs & Metrics

<details>
<summary><b>💰 Revenue Metrics</b></summary>

| Metric | Definition |
|--------|-----------|
| **Total Revenue** | Sum of `revenue_realized` across all confirmed bookings |
| **Revenue per Available Room (RevPAR)** | Revenue ÷ Total Available Room Nights |
| **Average Daily Rate (ADR)** | Revenue Realized ÷ Total Rooms Sold |
| **Revenue Realization %** | `revenue_realized` ÷ `revenue_generated` × 100 |
| **Revenue by Property** | Breakdown of realized revenue per hotel |
| **Revenue by Room Class** | Standard vs. Elite vs. Premium vs. Presidential |
| **Revenue by Booking Platform** | Direct, OTA, corporate, and other channels |
| **WoW Revenue Change %** | Week-over-week revenue growth/decline |

</details>

<details>
<summary><b>🛏️ Occupancy & Capacity</b></summary>

| Metric | Definition |
|--------|-----------|
| **Occupancy %** | Successful Bookings ÷ Total Capacity × 100 |
| **Total Capacity** | Aggregate of available room-nights per period |
| **Successful Bookings** | Count of checked-out + checked-in bookings |
| **Weekday vs. Weekend Occupancy** | Split by `day_type` from `dim_date` |
| **Occupancy by City** | Property performance grouped by location |
| **Occupancy by Room Class** | Demand signals per room tier |
| **WoW Occupancy Change %** | Week-over-week occupancy trend |

</details>

<details>
<summary><b>❌ Cancellation & Booking Status</b></summary>

| Metric | Definition |
|--------|-----------|
| **Cancellation Rate %** | Cancelled Bookings ÷ Total Bookings × 100 |
| **Total Cancelled Bookings** | Count of `booking_status = Cancelled` |
| **No-Show Rate %** | No-shows ÷ Total Bookings × 100 |
| **Booking Status Mix** | Proportion of Checked Out / Checked In / Cancelled / No Show |
| **Cancellation by Platform** | Which channels have the highest cancellation rates |
| **Cancellation by Room Class** | Room tier with highest drop-off |
| **WoW Cancellation Change %** | Week-over-week cancellation trend |

</details>

<details>
<summary><b>⭐ Guest Satisfaction</b></summary>

| Metric | Definition |
|--------|-----------|
| **Average Rating** | Mean of `ratings_given` across all rated bookings |
| **Rating by Property** | Property-level guest satisfaction scores |
| **Rating by Room Class** | Does room tier affect satisfaction? |
| **Rating vs. Occupancy Correlation** | Do higher-rated properties fill faster? |
| **Rating vs. ADR Correlation** | Premium pricing vs. guest experience |
| **Unrated Booking %** | Proportion of bookings with no rating submitted |

</details>

<details>
<summary><b>🌐 Booking Platform Analysis</b></summary>

| Metric | Definition |
|--------|-----------|
| **Bookings by Platform** | Volume distribution across all channels |
| **Revenue by Platform** | Which channel drives most realized revenue |
| **ADR by Platform** | Average rate achieved per booking source |
| **Cancellation Rate by Platform** | Channel reliability metric |
| **Platform Revenue Realization %** | Quality of revenue per channel |

</details>

---

## 📁 Project Structure

```
hotel-revenue-analysis/
│
├── 📊 Hotel Revenue.pbix               # Power BI dashboard (main deliverable)
├── 📄 Business kpis for hotel.pdf      # KPI definitions & business logic
│
├── 📂 Data Files (Star Schema)
│   ├── dim_hotels.csv                  # Hotel master data (25 properties)
│   ├── dim_rooms.csv                   # Room type to class mapping
│   ├── dim_date.csv                    # Date dimension with week & day type
│   ├── fact_bookings.csv               # ~130K booking transactions (12.5 MB)
│   └── fact_aggregated_bookings.csv    # Pre-aggregated booking summary
│
└── 📖 README.md                        # You are here
```

---

## 🚀 Getting Started

### Prerequisites
```
Power BI Desktop (latest)   |   Windows 10/11
```

### 1. Clone the repository
```bash
git clone https://github.com/chayanshh/hotel-revenue-analysis.git
cd hotel-revenue-analysis
```

### 2. Open the dashboard
Launch **Power BI Desktop** and open:
```
Hotel Revenue.pbix
```

### 3. Refresh data (if needed)
If Power BI prompts for a data source path:
- Go to **Home → Transform Data → Data Source Settings**
- Update file paths to point to your local CSV folder
- Click **Close & Apply** to reload all 5 tables

### 4. Explore the KPIs
Navigate through the dashboard pages — each page focuses on a different business lens:

| Page | Focus |
|------|-------|
| Executive Summary | High-level revenue, occupancy, ADR, RevPAR |
| Property Performance | Hotel-by-hotel breakdown |
| Booking Analysis | Platform, status, and cancellation trends |
| Room Analysis | Room class and category performance |
| Trend Analysis | Week-over-week patterns |

---

## 📈 Dashboard

The Power BI report (`Hotel Revenue.pbix`) includes:

- **KPI Cards** — Revenue, Occupancy %, ADR, RevPAR, Cancellation Rate, Avg Rating
- **Time Series** — Weekly trend lines for all core metrics
- **Property Matrix** — Sortable table comparing all hotels side by side
- **Platform Funnel** — Booking volume → realized revenue by channel
- **Day Type Split** — Weekday vs. weekend performance comparison
- **Slicers** — Filter by city, room class, hotel category, booking platform, and date range

---

## 🧩 Data Pipeline

```mermaid
flowchart LR
    A[📁 5 CSV Files] --> B[🗃️ Star Schema]
    B --> C[📊 Power BI Model]
    C --> D[🔢 DAX Measures]
    D --> E[📈 KPI Dashboard]
    E --> F[💡 Revenue Decisions]

    style A fill:#1b4332,color:#d8f3dc
    style F fill:#40916c,color:#d8f3dc
```

---

## 💡 Key Insights Enabled

| Business Question | Metric Used |
|---|---|
| Which city drives the most revenue? | Revenue by City |
| Are weekends worth premium pricing? | ADR & Occupancy by Day Type |
| Which OTA has the worst cancellation rate? | Cancellation % by Platform |
| Which room class underperforms vs. capacity? | Occupancy % by Room Class |
| Is guest satisfaction trending up? | WoW Avg Rating Change |

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/new-metric`
3. Add your DAX measure or data enhancement
4. Open a Pull Request with context on what was added

---

## 👤 Author

**Chayansh**
- GitHub: [@chayanshh](https://github.com/chayanshh)

---

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:40916c,50:1b4332,100:0d1b2a&height=120&section=footer" width="100%"/>

*Built with 🏨 Power BI · Star Schema · DAX*

</div>
