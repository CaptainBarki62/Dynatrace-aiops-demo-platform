# Dynatrace Workflow, ServiceNow Integration and RCA Automation

## Objective

Automatically perform the following actions when a problem is detected by Davis AI:

1. Detect Problem
2. Generate RCA Summary
3. Create ServiceNow Incident
4. Send Email Notification
5. Trigger Auto Remediation
6. Verify Recovery
7. Close Incident

---

# Architecture

Synthetic Failure

↓

Davis AI Problem

↓

Workflow Trigger

↓

Generate RCA

↓

Create ServiceNow Incident

↓

Email Notification

↓

Auto Remediation

↓

Verify Recovery

↓

Close Incident

---

# Prerequisites

Ensure:

* Dynatrace Workflow App Enabled
* ServiceNow Instance Available
* ServiceNow User Created
* ServiceNow API Credentials Available
* Email Integration Configured
* EasyTrade Application Running

---

# ServiceNow Integration

Navigate:

Settings

↓

Connections

↓

Connections and Credentials

↓

Add Connection

Select:

ServiceNow

---

# ServiceNow Connection Details

Provide:

Instance URL

```text
https://<instance>.service-now.com
```

Username

```text
servicenow_user
```

Password

```text
********
```

Connection Name

```text
ServiceNow-Integration
```

Test Connection

Expected Result:

```text
Connection Successful
```

Save Connection

---

# Create Workflow

Navigate:

Workflows

↓

Create Workflow

Workflow Name:

```text
EasyTrade Auto Remediation Workflow
```

Trigger Type:

```text
Problem Event Trigger
```

Event Type:

```text
Problem Opened
```

Save

---

# Step 1 - Get Problem Details

Add Action

Select:

```text
Dynatrace → Get Problem Details
```

Input:

```text
Problem ID
```

Output:

```text
Problem Title
Root Cause
Affected Services
Severity
Impact
```

---

# Step 2 - Generate RCA Report

Add Action

Select:

```text
Dynatrace → Generate Problem Summary
```

or

```text
Davis AI Problem Summary
```

Include:

* Problem Title
* Root Cause
* Impacted Services
* Start Time
* Affected Users
* Recommended Actions

Output:

```text
RCA Summary
```

---

# Step 3 - Create ServiceNow Incident

Add Action

Select:

```text
ServiceNow → Create Incident
```

Connection:

```text
ServiceNow-Integration
```

Map Fields:

Short Description

```text
{{Problem Title}}
```

Description

```text
{{RCA Summary}}
```

Priority

```text
High
```

Category

```text
Infrastructure
```

Subcategory

```text
Kubernetes
```

Assignment Group

```text
SRE Team
```

Output:

```text
Incident Number
```

Example:

```text
INC0012345
```

---

# Step 4 - Send Email Notification

Add Action

Select:

```text
Send Email
```

Recipients:

```text
operations@company.com
```

Subject:

```text
Dynatrace Problem Detected - {{Problem Title}}
```

Body:

Include:

* Incident Number
* Problem Title
* Root Cause
* RCA Summary
* Dynatrace Problem Link

---

# Step 5 - Trigger Auto Remediation

Add Action

Select:

```text
HTTP Request
```

Method:

```text
POST
```

URL:

```text
http://<remediation-api>/remediate
```

Headers:

```json
{
  "Content-Type": "application/json"
}
```

Body:

```json
{
  "issue_type": "failure"
}
```

Expected Response:

```json
{
  "status": "frontend_restored_and_restarted"
}
```

---

# Step 6 - Wait for Recovery

Add Action

Select:

```text
Wait
```

Duration:

```text
2 Minutes
```

---

# Step 7 - Verify Recovery

Add Action

Select:

```text
HTTP Request
```

Method:

```text
GET
```

Target:

```text
EasyTrade Frontend URL
```

Expected:

```text
HTTP 200
```

---

# Step 8 - Update ServiceNow Incident

Add Action

Select:

```text
ServiceNow → Update Incident
```

Status:

```text
Resolved
```

Resolution Notes:

```text
Auto remediation successfully restored service.
Verified through Dynatrace Synthetic Monitoring.
```

---

# Step 9 - Send Recovery Notification

Add Action

Select:

```text
Send Email
```

Subject:

```text
Service Restored - {{Problem Title}}
```

Body:

Include:

* Incident Number
* Resolution Summary
* Recovery Time
* Validation Result

---

# Workflow Testing

Simulate Failure:

```bash
kubectl scale deployment easytrade-frontendreverseproxy \
-n easytrade \
--replicas=0
```

Expected Flow:

Synthetic Failure

↓

Davis AI Problem

↓

Workflow Triggered

↓

RCA Generated

↓

ServiceNow Incident Created

↓

Email Sent

↓

Auto Remediation Executed

↓

Recovery Verified

↓

Incident Closed

↓

Recovery Email Sent

---

# RCA Example

Problem:

```text
EasyTrade Frontend Unavailable
```

Root Cause:

```text
Frontend Reverse Proxy Deployment Scaled To Zero
```

Impact:

```text
Application Unavailable To Users
```

Affected Service:

```text
easytrade-frontendreverseproxy
```

Resolution:

```text
Workflow executed auto-remediation API.
Deployment restarted successfully.
Application availability restored.
```

---

# Success Criteria

* Davis AI Problem Created
* Workflow Triggered Automatically
* RCA Generated
* ServiceNow Incident Created
* Email Notification Sent
* Auto Remediation Executed
* Service Recovery Verified
* Incident Automatically Resolved
* Recovery Notification Sent

End-to-End AIOps Automation Successful
