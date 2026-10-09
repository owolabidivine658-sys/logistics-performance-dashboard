# Logistics Performance Analytics Dashboard

**Tools:** Excel, Power Query, DAX, Power BI

**Business problem:** A logistics company wants visibility into fleet maintenance costs, vehicle utilization, driver performance, and customer revenue so it can cut downtime and improve on-time delivery.

## Questions I set out to answer

1. Which vehicle makes and maintenance types drive the highest costs and downtime?
2. How much of the fleet is idle, and what does that cost?
3. Why is the on-time rate only 44.6%, and which drivers perform best?
4. Which customer types and load types generate the most revenue?

## Data source

Dataset provided through my data analytics course training.

## Process

1. **Excel:** Combined multiple sheets into one workbook and converted each dataset into an Excel Table.
2. **Power Query:** Cleaned and transformed the data. Fixed data types, removed 26 duplicate rows, and fixed date formats.
3. **Data model:** Modeled 14 related tables, with loads and trips at the center, linked to driver, truck, trailer, customer, and route tables (details below).
4. **DAX:** Created measures including On-time Rate, Active Drivers, Preventive Maintenance Count, and Preventive Rate (formulas below).
5. **Dashboard:** Built four pages: Fleet Maintenance, Fleet Performance, Driver Performance, and Customer Analysis.

## Data model

The model has 14 tables: 6 reference tables, 6 transaction tables, and 2 monthly summary tables. Loads and trips are the core of it. Each load belongs to one customer and one route, and each trip links a load to a driver, a truck, and a trailer.

| Table | Type | Key | Contains |
| --- | --- | --- | --- |
| drivers | Reference | driver_id | Demographics, employment history, license info |
| trucks | Reference | truck_id | Equipment details, acquisition info, status |
| trailers | Reference | trailer_id | Trailer inventory, types, status |
| customers | Reference | customer_id | Accounts, contract types, revenue potential |
| facilities | Reference | facility_id | Terminal and warehouse locations, capacity |
| routes | Reference | route_id | Origin-destination pairs, distances, rate structures |
| loads | Transaction | load_id | Shipment details, revenue, booking type |
| trips | Transaction | trip_id | Trip performance, fuel consumption, duration |
| fuel_purchases | Transaction | fuel_purchase_id | Fuel transactions, prices, locations |
| maintenance_records | Transaction | maintenance_id | Service history, costs, downtime |
| delivery_events | Transaction | event_id | Pickup and delivery timestamps, detention, on-time status |
| safety_incidents | Transaction | incident_id | Accidents, violations, damage costs |
| driver_monthly_metrics | Monthly summary | driver_id + month | Monthly performance per driver |
| truck_utilization_metrics | Monthly summary | truck_id + month | Monthly equipment utilization |

**Key relationships**

| From | To | Type |
| --- | --- | --- |
| loads | customers | Many-to-one |
| loads | routes | Many-to-one |
| trips | loads | One-to-one |
| trips | drivers | Many-to-one |
| trips | trucks | Many-to-one |
| trips | trailers | Many-to-one |
| fuel_purchases | trips | Many-to-one |
| maintenance_records | trucks | Many-to-one |
| delivery_events | trips | Many-to-one |
| safety_incidents | trips | Many-to-one |

## Key DAX measures

**On-time Rate:** the average monthly on-time delivery rate across driver records, updating with any filter.

```dax
On-time Rate = AVERAGE('driver_monthly_metrics'[on_time_delivery_rate])
```

**Active Drivers:** CALCULATE overrides the filter context so only drivers with an Active employment status are counted.

```dax
Active Drivers =
CALCULATE(
    COUNTROWS('drivers'),
    'drivers'[employment_status] = "Active"
)
```

**Preventive Maintenance Count:** the number of maintenance records of the preventive type.

```dax
Preventive Maintenance Count =
CALCULATE(
    COUNTROWS('maintenance_records'),
    'maintenance_records'[maintenance_type] = "preventive"
)
```

**Preventive Rate:** the share of maintenance records that are preventive. DIVIDE returns blank instead of an error when there are no records.

```dax
Preventive Rate =
DIVIDE([Preventive Maintenance Count], COUNTROWS('maintenance_records'))
```

*Note:* On-time Rate is an average of monthly rates, so each driver-month counts equally regardless of delivery volume. The dataset has no delivery-count columns, so a weighted version wasn't possible.

## Data validation

I noticed fleet revenue by asset (262M) didn't match total customer revenue (538M). I traced each figure to its source table, confirmed they use different definitions, then relabeled the measures and added footnotes to prevent misreading. I also corrected an Average MPG measure that was summing instead of averaging. Finally, I checked the data model by comparing load and trip counts and by cross-checking the on-time rate against delivery event records.

## Key insights and recommendations

- **On-time rate is 44.6%, so more than half of deliveries are late.** Break late deliveries down by driver, load type, and customer to find where delays concentrate. Then set an on-time target (e.g. 70%) and have lower-performing drivers learn from the top performers' routes and scheduling.
- **Fleet utilization is 76.67%, leaving 28 of 120 vehicles inactive.** First find out why they're idle (under maintenance, no demand, or poor scheduling). If it's low demand, reassign, lease out, or retire the extra vehicles to cut carrying costs. If it's maintenance, fix the downtime problem first.
- **Contract customers generate the most revenue (about $207M).** Protect this segment with renewal reminders and dedicated account management. Also look at converting high-volume spot customers to contracts, since that makes revenue more predictable.
- **Preventive maintenance is only 14.45% of maintenance records, while preventive and repair costs are nearly equal and downtime totals 72.23K hours.** Treat this as a hypothesis: increase scheduled preventive maintenance, starting with the vehicle makes with the highest costs (Freightliner and Peterbilt), then track whether repair costs and downtime fall over the next few months.

## Dashboard preview

Screenshots of all four dashboard pages are included in this repository.

### Fleet Maintenance
![Fleet Maintenance Dashboard](fleet-maintenance-dashboard.png.png)
### Fleet Performance
![Fleet Performance Dashboard](images/fleet-performance-dasboard.png.png)

### Driver Performance
![Driver Performance Dashboard](images/driver-performance-dashboard.png.png)

### Customer Analysis
![Customer Analysis Dashboard](images/customer-analysis-dashboard.png.png)
