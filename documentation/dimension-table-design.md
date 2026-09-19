# Dimension Table Design

## Core Dimensions

### DIM_WORKER
**Type:** SCD Type 2  
**Purpose:** Provides worker attributes for workforce analytics.

**Example attributes:**
- Worker_ID
- Employee_Type
- Worker_Status
- Hire_Date
- Termination_Date
- Management_Level
- Worker_Type
- Is_Current

### DIM_ORGANIZATION
**Type:** SCD Type 2 / Hierarchical  
**Purpose:** Supports organizational hierarchy analysis.

**Example attributes:**
- Organization_ID
- Organization_Name
- Organization_Type
- Parent_Organization_ID
- Hierarchy_Level
- Is_Current

### DIM_POSITION
**Type:** SCD Type 2  
**Purpose:** Tracks position history and workforce planning context.

### DIM_JOB
**Type:** SCD Type 2 / Hierarchical  
**Purpose:** Supports job family, job profile, and level-based analytics.

### DIM_LOCATION
**Type:** SCD Type 2  
**Purpose:** Supports geography, office, country, and region-based analytics.

### DIM_DATE
**Type:** Static / Role-playing  
**Purpose:** Used across multiple date roles.

**Common roles:**
- Snapshot Date
- Hire Date
- Termination Date
- Event Date
- Effective Date
- Review Date
- Enrollment Date

## Dimension Types Used
- Conformed dimensions
- Role-playing dimensions
- SCD Type 1 dimensions
- SCD Type 2 dimensions
- Hierarchical dimensions
- Static dimensions
- Restricted/sensitive dimensions
