# SCD Type 2 Strategy

## Purpose
SCD Type 2 is used to preserve historical changes in key HR dimensions so analytics can answer point-in-time questions accurately.

**Example question:**
> What was headcount by organization and location as of January 2024?

This requires historical dimension values, not only current values.

## Design Pattern
Each SCD2 dimension includes:
- Surrogate key
- Business key
- Effective start date
- Effective end date
- Current row indicator
- Change hash/checksum
- Insert/update audit columns

## Example Columns
```sql
Worker_SK
Worker_ID
Worker_Name
Organization_ID
Job_ID
Location_ID
Effective_Start_Date
Effective_End_Date
Is_Current
Change_Hash
Created_Date
Updated_Date
