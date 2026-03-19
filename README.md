# Jumbotail_assignment
Overview
This project implements a SKU-level replenishment planning system that generates optimal purchase order (PO) suggestions. The goal is to maintain sufficient inventory to meet demand while respecting constraints such as storage capacity, case sizes, and existing pipeline inventory.
 Approach
1. Data Processing
•	Loaded the dataset using Python (pandas).
•	Handled missing values in key columns such as:
o	current_inventory
o	orderedquantity
o	max_drr
•	Parsed relevant fields and ensured correct data types.

2. Inventory Calculation Logic
a) Total Available Inventory
•	Combined on-hand and pipeline stock:
total_inventory = current_inventory + orderedquantity
b) Target Inventory
•	Calculated required inventory using demand and safety buffer:
target_inventory = max_drr × (inv_norm + safety_stock)
c) Required Units
•	Determined additional units needed:
required_units = target_inventory − total_inventory
•	If negative → no ordering required.

3. Constraints Applied
a) Space Constraint
•	Ensured total inventory does not exceed storage capacity:
max_possible = max_allocated_space − total_inventory
b) Case Rounding
•	Orders were rounded up to full cases:
final_cases = ceil(required_units / case_size)
final_units = final_cases × case_size
•	Revalidated to ensure space constraints are not violated after rounding.

4. Output Metrics Computed
For each SKU, the following were calculated:
•	final_suggestion → Units to order
•	final_cases_suggestion → Cases to order
•	final_value → Total purchase cost (units × cp)
•	final_days_of_inventory →
•	(current_inventory + orderedquantity + final_suggestion) / max_drr
•	final_tonnage → units × deadweight

5. SQL Implementation
•	Loaded processed data into SQLite database.
•	Executed analytical queries:
Query 1:
Vendor-level summary:
•	Total order value
•	Total cases
•	Count of SKUs below safety stock
Query 2:
Top 10 riskiest SKUs:
•	Based on lowest days of inventory
•	Filtered for active demand (max_drr > 0)

6. Visualization
•	Created a horizontal bar chart for:
o	Top 10 riskiest SKUs
o	Based on lowest inventory coverage

 Use of AI
•	Used AI assistance to:
o	Structure the replenishment logic
o	Debug errors (e.g., missing columns, SQL execution)
o	Optimize code structure and handling of edge cases
•	All logic and outputs were manually verified for correctness.

 Assumptions
•	Demand (max_drr) is constant.
•	All incoming orders (orderedquantity) will be fulfilled.
•	Safety stock is expressed in days.
•	Case size rounding is mandatory.
•	If max_drr = 0, no replenishment is required.

 Improvements (Future Work)
•	Prioritize SKUs using sales_band (A > B > C).
•	Build an interactive dashboard (Streamlit).
