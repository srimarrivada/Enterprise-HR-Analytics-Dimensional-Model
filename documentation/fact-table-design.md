# Fact Table Design

## Fact Types Used
This model uses multiple fact types:
- Periodic snapshot facts
- Transaction facts
- Accumulating snapshot facts
- Factless facts
- Aggregated facts

## Core Fact Tables

### FACT_HEADCOUNT_SNAPSHOT
**Grain:** One row per worker per snapshot month.

**Purpose:** Supports monthly headcount, workforce trend, organization, location, job, and level analytics.

**Measures:**
- Headcount count
- FTE
- Active worker indicator
- Tenure months
- Span of control

**Foreign Keys:**
- Worker_SK
- Date_SK
- Organization_SK
- Position_SK
- Job_SK
- Location_SK
- Company_SK
- Manager_Worker_SK

### FACT_WORKER_EVENT
**Grain:** One row per worker event.

**Purpose:** Tracks worker lifecycle events such as hire, job change, transfer, promotion, manager change, leave, and termination.

**Measures:**
- Event count
- Event sequence number
- Days since previous event

**Foreign Keys:**
- Worker_SK
- Event_Date_SK
- Event_Type_SK
- Organization_SK
- Job_SK
- Location_SK

### FACT_HIRE
**Grain:** One row per hire event.  
**Purpose:** Supports hiring, onboarding, source, recruiting, and workforce growth analytics.

### FACT_TERMINATION
**Grain:** One row per termination event.  
**Purpose:** Supports attrition, turnover, voluntary/involuntary exit, tenure-based attrition, and organization-level attrition analysis.

### FACT_COMPENSATION
**Grain:** One row per worker per compensation plan/effective period.  
**Purpose:** Supports compensation analysis with restricted access.

### FACT_PERFORMANCE
**Grain:** One row per worker per performance review cycle.  
**Purpose:** Supports performance distribution, rating trends, and talent analytics.

### FACT_LEARNING_ENROLLMENT
**Grain:** One row per worker per learning session/enrollment.  
**Purpose:** Supports training coverage, enrollment, completion, and learning effectiveness analytics.
