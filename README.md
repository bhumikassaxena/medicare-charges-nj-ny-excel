# Medicare Charges vs Payments: NJ and NY Hospitals (2024)

An Excel + Power Query project that measures how much New Jersey and New York hospitals bill (charges) compared with what they actually receive (payments) for Original Medicare inpatient stays, and where that gap is largest.

<img width="1899" height="1040" alt="Medicare_charges vs Payment_summary_excel_dashboard" src="https://github.com/user-attachments/assets/3bee751d-1997-4373-aace-d0fc909a0528" />


<img width="3200" height="2000" alt="Medicare_charges vs Payments_summary" src="https://github.com/user-attachments/assets/e2d0f1c0-f057-4da3-b136-d9cdda3f1d55" />


## Business question

How big is the gap between hospital charges and payments in NJ and NY, and where is it largest: which hospitals, which ownership types, which state, and does it relate to CMS quality (star) ratings?

**Charge-to-payment ratio** = total charges ÷ total payments. A ratio of 5.42x means hospitals billed about $5.42 for every $1 they received. A high ratio means high list prices (chargemaster), not higher Medicare revenue: Medicare pays a fixed, predetermined rate per DRG.

## Data

| Source | What it is |
|---|---|
| CMS Medicare Inpatient Hospitals: by Provider and Service (data year 2024) | One row per hospital + DRG. 145,879 rows nationally, filtered to NJ and NY (13,103 rows, 186 hospitals) |
| CMS Provider Data Catalog: Hospital General Information | Hospital ownership and overall star rating, joined on CCN (CMS Certification Number) |

## Method

1. **Power Query**: loaded the CSV, set CCN, ZIP and DRG code as text (keeps leading zeros and keeps joins from breaking), removed unused columns, filtered to NJ and NY.
2. **Weighted totals**: the file only has per-patient averages, so I multiplied each average by discharges to get `Total_Charges` and `Total_Payments`. Every ratio in the workbook is sum of charges ÷ sum of payments, never an average of row-level ratios.
3. **Join**: joined star rating and ownership with `XLOOKUP` on CCN. Missing ratings were set to null (not zero) and shown as "No Rating". 7 of 13,103 rows (one hospital, Claxton-Hepburn) had no match and were flagged "Not Found", not dropped.
4. **Pivot tables and charts**: hospital, DRG, ownership and star-rating pivots, a dashboard with KPI cards, pivot charts and a State slicer.

## Key findings

- **Overall:** hospitals billed $61.19B and received $11.30B across 505,777 discharges, a ratio of **5.42x**.
- **NJ vs NY:** New Jersey **6.54x** vs New York **4.97x**, about 32% higher. Compare ratios, not total dollars, because NY has more hospitals and patients.
- **By hospital:** ratios range from **1.22x** (Queens Hospital Center, lowest among hospitals with 500+ discharges) to **22.68x** (Capital Health Medical Center - Hopewell). The top five are all NJ hospitals from two systems (Capital Health and CarePoint), which suggests pricing is set at the system level. This is a pattern, not proven.
- **By ownership:** Government - Local hospitals are lowest at 1.76x (7 hospitals); other types range from 4.30x to 5.95x.
- **By star rating (read with care):** overall the ratio rises from 3.74x (1 star) to 5.87x (5 stars). But within New Jersey it falls (6.87x to 5.84x) and within New York it rises (2.96x to 5.87x), so the overall pattern mostly reflects the mix of states, not the rating itself. Use the State slicer to see this. It is a correlation, not a cause.
- **By condition:** among the 160 DRGs with 500+ combined discharges, high-volume conditions cluster in a narrow band (roughly 4.6x to 6.6x). Each DRG pools every hospital, so DRG ratios look tighter than hospital ratios do.
- **Top DRGs by ratio (500+ discharges):** kidney transplant is highest at 7.17x, followed by intracranial hemorrhage / cerebral infarction without CC/MCC (6.81x) and dysequilibrium (6.76x).

## Limitations

- Original Medicare fee-for-service patients at acute care (IPPS) hospitals only. No Medicare Advantage, private insurance or uninsured patients, so this measures hospital pricing behaviour, not what patients owe.
- "Payments" is Medicare's share plus patient co-pay plus secondary insurance; the data does not split these.
- CMS hides hospital-DRG groups with 10 or fewer discharges, so small hospitals and rare conditions are underrepresented.
- One year (2024): a comparison, not a trend. Averages, not patient-level data.
- DRG analysis uses a 500-discharge cutoff that I chose to avoid noisy ratios.
- Some hospitals have no CMS star rating; the star-rating chart excludes them.
- KPI cards are typed values from the `Dash Data` sheet and do not respond to the slicer. The Top 10 chart uses values copied from `Hospital_Pivot`. Small hospitals can appear in the Top 10 (for example, Carepoint Health-Hoboken has only 90 discharges).
- Charges are a hospital's list prices; high charges do not mean high revenue.

## Files

- `Medicare_Charges_NJ_NY_2024.xlsx`: tabs `Medicare_Data`, `Hospital_Info`, `DRG_Volume`, `Hospital_Pivot`, `DRG_Pivot`, `Ownership_Pivot`, `Rating_Pivot`, `Dash Data`, `Dashboard`
- `CMS_Medicare_Inpatient_NJ_NY_2024_raw_filtered.csv`: the raw CMS file filtered to NJ and NY (13,103 rows), before any calculations. The full national file (about 38 MB) is too large for GitHub's browser upload; download it from CMS.
- `Hospital_General_Information.csv`: CMS hospital ownership and star ratings (national)
- Dashboard images: the Excel dashboard screenshot and the summary graphic are shown at the top of this README and are also saved as image files in this repository.


## Tools

Excel (Power Query, PivotTables, PivotCharts, slicer, XLOOKUP).

## AI usage note

I used AI tools as an assistant for formula syntax, chart setup and documentation drafting. The analysis decisions and verification of every figure against my pivot tables are my own.

