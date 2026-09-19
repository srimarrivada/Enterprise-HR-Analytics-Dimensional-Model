# Data Quality Framework

## Purpose
The data quality framework validates data across ingestion, transformation, modeling, and consumption layers.

## DQ Categories
- Completeness
- Uniqueness
- Referential integrity
- Valid value/range checks
- Consistency checks
- Source-to-target reconciliation
- Volume anomaly checks
- SCD2 current-row validation

## Example DQ Rules
| Rule | Description | Severity |
|---|---|---|
| Worker_ID not null | Worker business key must exist | Critical |
| One current row per Worker_ID | SCD2 dimensions must have one active row | Critical |
| Fact foreign keys valid | Fact rows must resolve to dimensions | Critical |
| Headcount variance threshold | Daily/monthly variance should be within expected range | Warning |
| Compensation amount range | Compensation values must fall within valid range | Critical |
| Duplicate worker event check | Prevent duplicate lifecycle events | Warning |

## Severity Model
- Critical: Stop pipeline and investigate
- Warning: Continue with alert
- Informational: Log for trend analysis

## DQ Output
DQ results can be stored in a control table:

```text
DQ_Check_ID
Check_Name
Check_Category
Run_Date
Result_Status
Failed_Record_Count
Severity
Comments
```
    
