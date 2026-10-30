# Procurement Spend Analysis (Washington DC POs)

An Excel-based analysis of 46K+ government purchase orders (FY2024-26) to understand where procurement money goes: which agencies, suppliers and categories drive spend, how much of it is contract-backed, and how spend changes year to year.

**Tools:** Excel (Power Query, Pivot Tables)

---

## 1. Business Questions

1. How much was spent, and how did it change across fiscal years?
2. Which agencies and commodity categories account for most of the spend?
3. How much spend is backed by a contract vs. non-contract?
4. How clean is the PO data, and what needs fixing before it can be trusted for reporting?

## 2. Dataset

| Item | Detail |
|---|---|
| Source | Government of the District of Columbia, Office of Contracting and Procurement (PASS system), published on Open DC Data |
| Link | https://hub.arcgis.com/datasets/28fffe702af44277956763e50c35f577_16 |
| Raw size | 280,476 rows |
| Final size | 46,270 POs (FY2024-26) |
| Currency | USD |

Key fields used: PO number, agency, supplier, commodity, contract number, PO total, order date, fiscal year.

## 3. Data Cleaning (Power Query)

| Step | What was done | Why |
|---|---|---|
| 1 | Removed system timestamp columns and the requester name column | Not needed for analysis; avoids showing individual employee names |
| 2 | Set correct data types (amount, dates, fiscal year) | Reliable sums and date filters |
| 3 | Removed rows with a blank PO number | Not a valid PO record |
| 4 | Created `BasePO` and `Version` columns from the PO number (e.g. `PO590924-V3`), sorted by version and kept only the latest version of each PO | PO revisions appear as separate rows; keeping all would double-count spend |
| 5 | Standardized supplier names into `SupplierClean` (trimmed, upper-cased, removed punctuation and extra spaces) | Same supplier written in different ways was being counted separately |
| 6 | Created `ContractType` from the contract number field. Blank, zero and placeholder values (such as `0`, `N/A`, `NONE`, `QUOTATION`) were treated as non-contract | Contract number field contained placeholder text instead of real blanks |
| 7 | Replaced errors in the PO title column with "Not Available" | Prevent load errors |
| 8 | Filtered to fiscal years 2024, 2025 and 2026 | Recent data, manageable size for Excel |

Row count: 280,476 raw rows → 256,457 after removing duplicate PO versions and blank POs → 46,270 after the FY2024-26 filter.

## 4. Analysis (Pivot Tables)

Six Pivot Tables were built in the workbook:

1. Top suppliers by spend
2. Total spend
3. Contract vs. non-contract spend
4. Agency-wise spend (top 10)
5. Commodity-wise spend (top 10)
6. Yearly trend

## 5. Key Findings

**Total spend:** about **$11.68B** across 46,270 POs (FY2024-26).

**Contract coverage**

| | Spend | POs | Share of spend |
|---|---|---|---|
| Contract | $11,317.9M | 30,830 | 96.9% |
| Non-contract | $361.6M | 15,440 | 3.1% |

About one-third of POs have no contract, but they make up only about 3% of spend. This is mostly small, low-value "tail" spend.

**Agencies:** the top 10 agencies account for about **77%** of spend. Department of General Services alone is about **30%**. DC Public Schools has the most POs (8,729) but much lower spend than agencies with fewer, larger POs, such as the Office of the Deputy Mayor for Planning and Economic Development (281 POs).

**Commodities:** the top 10 categories make up about **40%** of spend. Construction Management Services is the largest (about $1.64B, ~14%) with only 688 POs, while software implementation consulting has 4,586 POs for $623M.

**Yearly trend**

| Fiscal year | Spend | POs | Avg. PO value (approx.) |
|---|---|---|---|
| FY2024 | $3,397M | 16,911 | ~$201K |
| FY2025 | $4,238M | 15,107 | ~$281K |
| FY2026 | $4,044M | 14,252 | ~$284K |

Spend rose about 25% in FY2025. The number of POs fell each year while the average PO value rose.

## 6. Assumptions & Limitations

- **PO versions:** `-V2`, `-V3` suffixes are treated as revisions of the same PO, and the latest version is assumed to hold the final amount.
- **Contract flag is rule-based:** it depends on the contract number field, which contains placeholder values. Some entries (for example, blanket purchase agreement references) are counted as contract.
- **Supplier names:** standardized for spacing, case and punctuation only. Some names are truncated in the source data, and joint ventures are kept as separate suppliers.
- **Commodity and agency names** are used as provided in the source.
- **Public data:** this analysis uses publicly available government data and is for learning and portfolio purposes. The requester name column was removed.
- Average PO values are rough figures (total spend divided by PO count).

## 7. Repository Contents

> Update these names to match your files.

```
├── data/
│   └── cleaned PO data (requester column removed)
├── Procurement_Spend_Analysis.xlsx
└── README.md
```

## 8. How to Reproduce

1. Download the dataset from the link above.
2. Load it into Excel using Power Query (Data → From Table/Range).
3. Apply the cleaning steps in Section 3.
4. Load the cleaned table and insert Pivot Tables for supplier, agency, commodity, contract type and fiscal year.

## 9. Author

**Abhishek Kumar Mishra**
MCA graduate | Procurement & Data Analysis
[LinkedIn](https://www.linkedin.com/in/abhishek-kumar-mishra6/) | [Portfolio](https://abhishek-mishra-portfolio.netlify.app/)
