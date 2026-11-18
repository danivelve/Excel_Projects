# Data cleaning notes

The raw data is in the `Bike_Buyers` sheet of the workbook and in `data/bike_buyers_raw.csv`. The cleaned version is in the `Working Sheet` and in `data/bike_buyers_clean.csv`.

## Steps

| # | Step | Detail | Rows affected |
|---|---|---|---|
| 1 | Remove duplicates | Exact duplicate rows were removed with Excel's Remove Duplicates. | 1,026 → 1,000 |
| 2 | Expand marital status | `M` → `Married`, `S` → `Single` (Find & Replace) | All rows |
| 3 | Expand gender | `M` → `Male`, `F` → `Female` | All rows |
| 4 | Add age brackets | New `Age Brackets` column created with a nested `IF` formula | All rows |
| 5 | Fix age bracket bug | See below | 25 rows |

## Age bracket bug

The original formula was:

```excel
=IF(L2>54,"Old",IF(L2>31,"Middle Age",IF(L2<31,"Adolescent","Invalid")))
```

It checks `>31` and `<31`, so an age of exactly 31 matches neither condition and falls through to `"Invalid"`. That affected **25 customers**.

The corrected formula, which places 31-year-olds in Middle Age:

```excel
=IF(L2>54,"Old",IF(L2>=31,"Middle Age","Adolescent"))
```

| Bracket | Ages | Customers |
|---|---|---|
| Adolescent (Young) | 25-30 | 110 |
| Middle Age | 31-54 | 701 |
| Old (Senior) | 55+ | 189 |

`data/bike_buyers_clean.csv` uses the corrected brackets. In the workbook, the **Bike Analysis** sheet counts age groups straight from the `Age` column, so it is correct even though the original `Age Brackets` column is unchanged.

## Checks

- No missing values in any column.
- Ages range from 25 to 89. Incomes range from $10,000 to $170,000.
- `Purchased Bike` contains only `Yes`/`No`: 481 Yes, 519 No.
