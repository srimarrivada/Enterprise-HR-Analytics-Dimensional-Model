# Bridge Table Design

## Purpose
Bridge tables are used when a fact or dimension has a many-to-many relationship with another dimension.  

In HR analytics, examples include:
- Worker to multiple organizations
- Worker to multiple roles
- Worker to multiple regions
- Organization to multiple hierarchy paths

## Example Bridge Tables

### BRIDGE_WORKER_ORG_ROLE
**Purpose:** Represents workers assigned to multiple organization roles.

**Example columns:**
- Worker_SK
- Organization_SK
- Role_Type
- Effective_Start_Date
- Effective_End_Date
- Allocation_Percent
- Is_Current

### BRIDGE_ORG_HIERARCHY
**Purpose:** Supports multi-level organization rollups.

**Example columns:**
- Child_Organization_SK
- Parent_Organization_SK
- Hierarchy_Level
- Hierarchy_Path
- Effective_Start_Date
- Effective_End_Date

## Design Considerations
- Avoid double-counting when using bridge tables
- Use allocation percentage where needed
- Add effective dates for historical accuracy
- Clearly document grain and join rules
