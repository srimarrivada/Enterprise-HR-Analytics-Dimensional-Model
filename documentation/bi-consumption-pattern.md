# BI Consumption Pattern

## Purpose
The BI consumption layer is designed to provide fast, governed, and business-friendly analytics access.

## Consumption Patterns
- Power BI semantic model
- DirectQuery for large facts
- Import mode for small dimensions
- Composite model for performance balance
- Aggregation tables for trend dashboards
- Semantic views for business-friendly naming
- Row-level security for user-based filtering

## Performance Strategy
Large detailed facts can be expensive for dashboard queries. To improve performance, pre-aggregated tables are created at common analysis grains.

**Example aggregation grains:**
- Month
- Organization
- Location
- Job family
- Management level
- Company
- Department

**Example Aggregation Tables:**
- AGG_HEADCOUNT_MONTHLY
- AGG_ATTRITION_MONTHLY
- AGG_HIRING_MONTHLY
- AGG_LEARNING_COVERAGE

## Benefits
- Faster dashboard performance
- Reduced scan volume
- Consistent KPI calculations
- Improved user experience
- Better support for self-service analytics
