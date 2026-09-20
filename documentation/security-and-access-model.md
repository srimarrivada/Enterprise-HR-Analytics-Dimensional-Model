# Security and Access Model

## Purpose

HR analytics requires strict access control because data may include sensitive personal, compensation, demographic, or legal information.

## Security Tiers

### General Layer
Accessible to authorized HR analytics users.

**Examples:**
- Headcount
- Hiring
- Termination
- Organization
- Location
- Job
- Learning

### Restricted Layer
Accessible to limited HR leadership or approved groups.

**Examples:**
- Sensitive demographics
- Diversity-related attributes
- Worker-sensitive attributes

### Highly Restricted Layer
Accessible only to highly approved users.

**Examples:**
- Compensation
- National identifiers

## Security Patterns
- Schema-level separation
- Role-based access control
- Row-level security
- Column masking
- Explicit deny rules for restricted attributes
- Audit logging
- Least-privilege access

## Example Roles
- HR_Analytics_General
- HR_Leadership_Restricted
- Compensation_Highly_Restricted
- Data_Engineering_Admin
- Audit_ReadOnly
