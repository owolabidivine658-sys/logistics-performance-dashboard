 # logistics-performance-dashboard
Logistics performance analytics dashboard in Power BI covering fleet maintenance, fleet utilization, driver performance, and customer revenue. Built with Excel, Power Query, and DAX.
Logistics Performance Analytics Dashboard
Tools: Excel, Power Query, DAX, Power BI
Business problem: A logistics company wants visibility into fleet maintenance costs, vehicle utilization, driver performance, and customer revenue so it can cut downtime and improve on-time delivery.
**Questions I set out to answer**
1. Which vehicle makes and maintenance types drive the highest costs and downtime?
2. How much of the fleet is idle, and what does that cost?
3. Why is the on-time rate only 44.6%, and which drivers perform best?
4. Which customer types and load types generate the most revenue?
**Data source**
Dataset provided through my data analytics course training.
**Process**
1. Excel: Combined multiple sheets into one workbook and converted each dataset into an Excel Table.
2. Power Query: Cleaned and transformed the data. [fixed data types, removed duplicates, handled null values, renamed columns.]
3. Data model: [Describe your tables and relationships, e.g. fact table linked to vehicle, driver, and customer tables.]
4. DAX: Created measures including [Fleet Utilization, Preventive Rate, On-Time Rate, etc.].
5. Dashboard: Built four pages: Fleet Maintenance, Fleet Performance, Driver Performance, and Customer Analysis.
Data validation
I noticed fleet revenue by asset (262M) didn't match total customer revenue (538M). I traced each figure to its source table, confirmed they use different definitions, then relabeled the measures and added footnotes to prevent misreading. I also corrected an Average MPG measure that was summing instead of averaging.
**Key insights and recommendations**
• On-time rate is 44.6%,so more than half of deliveries are late. Break late deliveries down by driver, load type, and customer to find where delays concentrate. Then set an on-time target (e.g. 70%) and have lower-performing drivers learn from the top performers' routes and scheduling.
• Fleet utilization is 76.67%, leaving 28 vehicles idle, so more than half of deliveries are late. Break late deliveries down by driver, load type, and customer to find where delays concentrate. Then set an on-time target (e.g. 70%) and have lower-performing drivers learn from the top performers' routes and scheduling..
• Contract customers generate the most revenue (about $207M). Protect this segment with renewal reminders and dedicated account management. Also look at converting high-volume spot customers to contracts, since that makes revenue more predictable.
• Preventive maintenance is only 14.45% of the total, while repairs cost almost as much as preventive work and downtime totals 72.23K hours. Shift more spend toward scheduled preventive maintenance, starting with the vehicle makes with the highest costs (Freightliner and Peterbilt). Then track whether repair costs and downtime fall over the next few months.
Link
[published report or screen recording]
Two things to fix in the draft
• Each recommendation needs a "so what." The blanks are there on purpose, because they should be your reasoning. Aim for one concrete action each.
• Specifics beat generalities. "Cleaned the data" means nothing. "Removed 340 duplicate rows and fixed date formats" is memorable.
Next step
Send your coach two things to finish this README:
1. The 3-4 main cleaning steps you did in Power Query
2. Your 3-5 favorite DAX measures (paste the formulas)
Then practice explaining each one in plain language, which is exactly what you'll need in an interview.
