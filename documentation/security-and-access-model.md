# Security and Access Model

## Purpose
HR analytics requires strict access control because data may include sensitive personal, compensation, demographic, or legal information.  
<br/>
![Enterprise HR Analytics Security Model](../diagrams/hr-analytics-security-model.png)

This security model applies defense-in-depth principles to enterprise HR analytics data. The design separates data into general, restricted, and highly restricted layers based on sensitivity and user authorization.

## Security Tiers

### General Layer
Accessible to authorized HR analytics users.  
Includes workforce analytics such as headcount, hiring, termination, movement, learning, and organization analytics.

### Restricted Layer
Accessible to limited HR leadership or approved groups. 
Includes sensitive demographics, diversity-related data, restricted worker attributes, and HR leadership analytics.

### Highly Restricted Layer
Accessible only to highly approved users. 
Includes compensation, sensitive personal identifiers, pay equity analysis, and legal-sensitive data.

## Security Patterns / Controls
- Schema-level separation
- Role-based access control (RBAC)
- Row-level security
- Column masking
- Explicit DENY rules for restricted attributes
- Audit logging
- Least-privilege access

**Example Roles:**
- HR_Analytics_General
- HR_Leadership_Restricted
- Compensation_Highly_Restricted
- Data_Engineering_Admin
- Audit_ReadOnly
