# End-to-End Architecture

## Overview
This document explains the end-to-end architecture of an enterprise HR analytics platform, from source system ingestion through curated DataHub processing, 
ETL transformation, dimensional modeling, security governance, semantic consumption, and business outcomes.

The architecture follows a layered source-to-consumption model:
> Source Systems → Curated DataHub → ETL / Processing → Dimensional Warehouse → Security & Governance → Semantic / BI Consumption → Business Users → Business Outcomes

This is a sanitized portfolio artifact created to demonstrate enterprise data platform architecture, dimensional modeling, governance, and analytics design thinking. It is not client documentation and does not include proprietary data, production code, credentials, internal system names, or real employee information.

## Architecture Diagram

[View End-to-End Architecture Diagram](../diagrams/hr-analytics-end-to-end-architecture-detailed.png)

## 1. Source Systems / Source Feeds
The platform starts with multiple enterprise HR and workforce-related source feeds. These source systems provide data required for workforce analytics, planning, 
compliance, and business reporting.

Representative source domains include:
- Core HR / HCM
- Sensitive HR feed
- Future-dated workforce feed
- Finance / labor data
- Benefits and payroll data
- Partner administration data

Common source patterns include:
- API-based extraction
- Secure file-based feeds
- Incremental feeds
- Future-dated snapshots
- Sensitive data feeds with restricted handling

The purpose of this layer is to collect operational HR data while preserving source traceability, lineage, and data sensitivity boundaries.

## 2. Curated DataHub Layer
The Curated DataHub layer acts as an integration-first source of truth. It stores curated source-aligned data that can be used by downstream integrations and by the analytics warehouse.

This layer typically includes:
- Current HR data
- Historical HR data
- Future-dated workforce data
- Reference and lookup data
- Sensitive HR data separated by security needs
- Operational metadata and audit attributes

### Current / Historical Data
Current and historical datasets support workforce analytics, downstream distribution, and longitudinal analysis.

**Example capabilities:**
- Current workforce state
- Employment history
- Job and organization history
- Worker movement history
- Payroll / hours / benefit-related history
- Partner or retiree-related history

### Future-Dated Data
Future-dated data supports workforce planning and forward-looking analytics.

**Example capabilities:**
- Planned hires
- Planned terminations
- Planned transfers
- Future positions
- Planned compensation
- Rolling workforce planning windows

The DataHub layer is optimized for curated integration and source alignment, not direct dashboard consumption.

## 3. ETL / Processing Layer
The ETL layer transforms curated DataHub data into a dimensional analytics model.

Representative processing capabilities include:
- Watermark-based incremental extraction
- CDC / delta detection
- SCD Type 1 and Type 2 processing
- Surrogate key lookups
- Unknown member handling
- Bridge table population
- Fact table loading
- Aggregation rebuilds
- Data quality validation
- Audit and operational logging
- Metadata and lineage capture
- SLA monitoring

### 3.1 Extraction Layer
The extraction layer identifies and processes changed data where possible.

**Typical patterns:**
- Watermark-based incremental processing
- Modified-date driven extraction
- Delta comparison
- Future-dated truncate/reload pattern
- Reduced source movement by processing only changed records

### 3.2 Transformation Layer
The transformation layer applies business rules and dimensional modeling logic.

**Typical transformations:**
- SCD Type 2 historical dimension processing
- SCD Type 1 reference dimension updates
- Type 0 static dimension handling
- MD5/checksum-based change detection
- Surrogate key assignment
- Unknown member handling
- Source-type discrimination for multi-source facts
- Bridge table relationship resolution

### 3.3 Load Orchestration
The load process is organized into dependency-based tiers. This allows parallel processing where possible while preserving referential integrity.

**Representative tier model:**
| Tier | Purpose |
|------|---------|
| Tier 0 | Utility setup, watermark read, cache warm-up, batch start logging |
| Tier 1 | Independent Type 1 / static dimensions |
| Tier 2 | Core SCD2 dimensions such as Worker, Job, Company, Location, Department, Line of Service, Supervisory Organization |
| Tier 3 | Dependent dimensions such as Position, Worker Sensitive, Sensitive Identifier, Future Worker, Planned Position |
| Tier 4 | Bridge tables such as worker-to-matrix organization, worker-to-organization role, market-to-region |
| Tier 5 | Fact tables loaded in parallel fact groups |
| Tier 6 | Aggregation rebuilds, DQ validation, audit logging, watermark update |

### 3.4 Data Quality, Audit, and Metadata
The ETL layer includes platform controls for reliability and traceability.

**Data quality examples:**
- Referential integrity checks
- Completeness checks
- Uniqueness checks
- Source-to-target reconciliation
- Volume anomaly checks
- SCD2 current-row validation

**Audit and metadata examples:**
- Batch logs
- Error logs
- Watermark tracking
- DQ check history
- Source-to-target mapping
- Model version history
- Source schema registry
- User security mapping
- Lineage and impact analysis

## 4. HR Analytics Dimensional Warehouse
The dimensional warehouse is designed using Kimball methodology and follows a constellation schema pattern.

**The warehouse includes:**
- Conformed dimensions
- Business-process fact tables
- Periodic snapshot facts
- Transaction facts
- Accumulating snapshot facts where appropriate
- Factless facts where appropriate
- Bridge tables
- Pre-aggregated facts
- Restricted and highly restricted objects
- Utility, audit, metadata, and security tables

### 4.1 Warehouse Schema Layout
**A representative warehouse layout includes:**

| Schema / Area | Purpose |
|---|---|
| Dimension schema | General dimensions and reference dimensions |
| Fact schema | General business-process facts |
| Aggregation schema | Pre-aggregated facts for performance |
| Bridge schema | Many-to-many relationship resolution |
| Restricted schema | Sensitive HR data |
| Highly restricted schema | Compensation, payroll, sensitive identifier, legal-sensitive data |
| Utility schema | ETL, DQ, audit, metadata, and security support tables |

### 4.2 Core Conformed Dimensions
**Representative conformed dimensions include:**
- Date
- Worker
- Position
- Job
- Company
- Location
- Department
- Line of Service
- Supervisory Organization
- Pay Group
- Termination Reason
- Action Reason

These dimensions provide consistent slicing across multiple HR fact tables.

### 4.3 Central Fact Tables
**Representative fact subject areas include:**
- Worker headcount snapshot
- Worker lifecycle events
- Worker termination
- Worker leave
- Worker hours
- Planned workforce
- Performance review
- International assignment
- Compensation
- Bonus
- Rewards
- Payroll balance
- Partner
- Retiree

### 4.4 Aggregation Layer
Aggregation tables support faster dashboard performance by pre-computing common metrics at business-friendly grains.

**Representative aggregation domains include:**
- Monthly headcount trends
- Monthly worker event trends
- Monthly hours trends
- Attrition trends
- Workforce planning trends
- Security-aware compensation summaries

Aggregation tables reduce repeated scans over detailed fact tables while preserving the ability to drill through to transaction-level detail when needed.

## 5. Dimensional Model Design Patterns
The dimensional model uses several Kimball and enterprise data warehouse design patterns.

**Key patterns include:**
- Kimball constellation schema
- Multiple fact tables at different grains
- Conformed dimensions shared across facts
- Role-playing dimensions
- SCD Type 2 historical tracking
- SCD Type 1 reference dimensions
- Type 0 static dimensions
- Bridge tables for many-to-many relationships
- Junk dimensions for grouped flags
- Snapshot context dimensions
- Unknown member pattern
- Future-dated dimensions and facts
- Pre-aggregated performance tables

These patterns support historical accuracy, reuse, performance, and governed analytics consumption.

## 6. Security Layer
The security layer protects HR data based on sensitivity.

The model separates data into three broad security tiers:

### General
Used for broadly authorized HR analytics.

**Examples:**
- Headcount
- Events
- Leave
- Performance
- Hours
- Planning

### Restricted
Used for sensitive HR data and leadership-level analytics.

**Examples:**
- Demographics
- Diversity-related data
- Sensitive worker attributes
- Restricted HR leadership analytics

### Highly Restricted
Used for highly sensitive or legal-sensitive data.

**Examples:**
- Individual compensation
- Bonus
- Payroll balances
- Sensitive personal identifiers
- Planned compensation
- Pay equity analysis

**Security controls include:**
- Schema-level separation
- Role-based access control
- Explicit GRANT / DENY
- Power BI row-level security
- Separate workspaces or datasets for sensitive reporting
- User security mapping
- Audit logging
- Least-privilege access

## 7. Semantic and Consumption Layer
The semantic and BI layer transforms the dimensional warehouse into business-friendly analytics products.

**Representative components include:**
- SQL semantic views
- Business-friendly names
- Pre-joined fact and dimension views
- Security-aware views
- Power BI semantic models
- Certified datasets
- Composite models
- Import-mode dimensions and aggregation facts
- DirectQuery drill-through for large detail facts
- Row-level security roles
- Governed KPI definitions

### Analytics Domains
**Representative dashboard and analytics domains include:**
- Workforce headcount
- Attrition
- Leave management
- Performance review tracking
- International mobility
- Diversity and inclusion
- Compensation
- Workforce planning
- Planned compensation
- Data quality scorecard
- Executive workforce summary

## 8. Business Users
The platform supports multiple stakeholder groups.

**Representative users include:**
- HR business partners
- People analytics teams
- Finance / FP&A
- Compensation teams
- Diversity and inclusion teams
- Compliance / legal teams
- HR operations
- Global mobility teams
- Executives
- Data engineering and operations teams

Each group consumes governed analytics through dashboards, certified datasets, semantic views, or operational monitoring views based on access rights.

## 9. Business Outcomes
The architecture is designed to deliver measurable business and platform outcomes.

**Representative outcomes include:**
- Governed self-service analytics
- Faster workforce reporting
- Consistent KPI definitions
- Point-in-time workforce history
- Secure access to sensitive HR data
- Improved data quality and reconciliation
- SLA-driven data availability
- Scalable dashboard performance
- Reduced repeated manual reporting
- Improved trust in enterprise HR analytics

## Architecture Principles

### 1. Separation of Concerns
**Each layer has a clear responsibility:**
- Source systems provide operational data
- Curated DataHub organizes source-aligned data
- ETL transforms data into analytics-ready structures
- Dimensional warehouse supports reporting and analytics
- Security layer enforces access controls
- Semantic layer provides business-friendly consumption

### 2. Conformed Dimensions
Conformed dimensions ensure consistent analytics across multiple HR business processes.

### 3. Historical Accuracy
SCD Type 2, snapshot facts, and effective dating support point-in-time workforce reporting.

### 4. Performance by Design
Aggregation tables, semantic models, and drill-through patterns balance performance and detail.

### 5. Security by Design
Sensitive HR data is separated into restricted and highly restricted layers.

### 6. Data Quality as a Platform Capability
DQ checks, reconciliation, and audit logs are part of the platform architecture.

### 7. Governed Self-Service
Semantic views, certified datasets, KPI definitions, and access controls enable safe self-service analytics.

## Related Artifacts
- [High-Level Architecture](../diagrams/hr-analytics-high-level-architecture.png)
- [End-to-End Architecture Diagram](../diagrams/hr-analytics-end-to-end-architecture-detailed.png)
- [Constellation Schema](../diagrams/hr-analytics-constellation-schema.png)
- [Bus Matrix Summary](../diagrams/hr-analytics-bus-matrix-summary.png)
- [Detailed Bus Matrix](../diagrams/hr-analytics-bus-matrix-detailed.png)
- [Security Model](../diagrams/hr-analytics-security-model.png)
- [Aggregation Strategy](../diagrams/hr-analytics-aggregation-strategy.png)

## Confidentiality Note
This document is a sanitized portfolio artifact created for data architecture demonstration purposes. It does not include proprietary client documentation, 
confidential data, production code, credentials, internal system names, or real employee/customer data.
