# Dimensional Model Overview

## Purpose
This model is designed to support enterprise HR analytics across workforce, organization, hiring, termination, movement, compensation, performance, and learning subject areas.
The design follows Kimball dimensional modeling principles using a constellation schema with shared conformed dimensions and multiple fact tables.

## Modeling Principles
- Facts represent measurable business events or snapshots
- Dimensions provide descriptive context for analysis
- Conformed dimensions are reused across facts
- SCD Type 2 is used where historical tracking is required
- Role-playing dimensions support multiple analytical perspectives
- Bridge tables handle many-to-many relationships
- Aggregation tables improve dashboard performance
- Security tiers separate general, restricted, and highly restricted data

## Core Subject Areas
- Workforce headcount
- Worker movement
- Hiring and onboarding
- Termination and attrition
- Organization hierarchy
- Position and job structure
- Compensation
- Performance
- Learning and development
- Diversity and sensitive HR analytics

## Core Design Patterns
- Monthly snapshot facts for headcount and workforce planning
- Transaction facts for hires, terminations, movements, and events
- SCD2 dimensions for worker, organization, job, position, and location history
- Conformed dimensions for consistent reporting across facts
- Pre-aggregated facts for high-volume dashboard queries

## Bus Matrix
The bus matrix is used to define how facts and dimensions relate across HR analytics subject areas. It ensures that shared conformed dimensions are consistently reused across business processes.

This model uses conformed dimensions such as:
- Date
- Worker
- Position
- Job
- Company
- Location
- Department
- Line of Service
- Business Unit
- Supervisory Organization

The bus matrix helps validate:
- Fact grain
- Dimension reuse
- Conformed dimension strategy
- Subject-area coverage
- Security-tiered facts
- Optional/nullable relationships

The detailed bus matrix is available here:
[View Detailed Bus Matrix](diagrams/bus-matrix-detailed.png)
