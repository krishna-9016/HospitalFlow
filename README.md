<div align="center">

# 🖥️ ITServiceDesk Pro

### Enterprise IT Service Management System on ServiceNow

[![ServiceNow](https://img.shields.io/badge/ServiceNow-ITSM-green?style=for-the-badge&logo=servicenow&logoColor=white)](https://www.servicenow.com/)
[![ITIL](https://img.shields.io/badge/ITIL-Aligned-blue?style=for-the-badge)](https://www.axelos.com/certifications/itil-service-management)
[![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)]()
[![Developer](https://img.shields.io/badge/Developer-Krishna%20Chaurasiya-orange?style=for-the-badge)](https://github.com/krishna-9016)

---

> **ITServiceDesk Pro** is a fully functional IT Service Management (ITSM) application built on ServiceNow, covering Incident Management, Asset & Configuration Management (CMDB), and Change Request workflows — all aligned with ITIL best practices.

---

</div>

## 📌 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [ServiceNow Modules Used](#-servicenow-modules-used)
- [Database Tables](#-database-tables)
- [Workflows & Flows](#-workflows--flows)
- [SLA Configuration](#-sla-configuration)
- [REST API Integration](#-rest-api-integration)
- [Dashboard & Reports](#-dashboard--reports)
- [Knowledge Base](#-knowledge-base)
- [How to Build — Step by Step](#-how-to-build--step-by-step)
- [Screenshots](#-screenshots)
- [Future Enhancements](#-future-enhancements)
- [Developer](#-developer)

---

## 🔍 Overview

**ITServiceDesk Pro** simulates a real-world enterprise IT helpdesk system. It enables IT teams to:

- Log, assign, escalate, and resolve **incidents**
- Track all IT **assets** (laptops, servers, licenses) with full lifecycle management
- Submit, review, and approve **change requests** through a CAB (Change Advisory Board) process
- Monitor team performance via **SLA tracking** and **KPI dashboards**
- Reduce ticket volume via an integrated **Knowledge Base**

This project demonstrates core ServiceNow ITSM capabilities that are directly tested in **CSA** and **CAD** certification exams.

---

## ✨ Features

### 🚨 1. Incident Management
- Raise incidents via Service Portal or direct form entry
- Auto-assignment to IT groups based on **category** (Network, Hardware, Software, Security)
- **Priority matrix** auto-calculation based on Impact × Urgency
- Escalation rules — auto-escalate P1 incidents to IT Manager if unresolved within 1 hour
- Full incident lifecycle: `New → Assigned → In Progress → Resolved → Closed`
- Link incidents to **Problem records** for root cause analysis

### 🗃️ 2. Asset & CMDB Management
- Track IT assets: Laptops, Desktops, Servers, Network Devices, Software Licenses
- Asset lifecycle states: `Ordered → In Stock → In Use → In Maintenance → Retired`
- Auto-populate CMDB Configuration Items (CIs) when asset is assigned to a user
- Relationship mapping: Asset → User → Department → Location
- View full **assignment history** for any asset

### 🔄 3. Change Request Management (ITIL CAB)
- Three change types: **Standard**, **Normal**, **Emergency**
- Risk assessment form with impact analysis fields
- Multi-level approval workflow:
  ```
  Requester → IT Manager → CAB Approval → Implementation → Review → Closed
  ```
- Rollback plan mandatory for high-risk changes
- Blackout window configuration — block changes during critical business periods

### 📋 4. Problem Management
- Link multiple incidents to a single Problem record
- Root Cause Analysis (RCA) documentation
- Known Error Database (KEDB) integration
- Problem lifecycle: `New → Assess → Root Cause Analysis → Fix in Progress → Resolved`

---

## 🛠️ ServiceNow Modules Used

| Module | Purpose |
|---|---|
| **ServiceNow Studio** | Application development environment |
| **Flow Designer** | Approval workflows, escalation, automation |
| **Service Catalog** | Self-service incident/request submission portal |
| **Service Portal** | Employee-facing IT helpdesk portal |
| **Business Rules** | Auto-assignment, priority calculation, status updates |
| **Client Scripts** | Real-time form validation, dynamic field visibility |
| **UI Policies** | Show/hide fields based on incident category |
| **SLA Management** | Define, track, and breach SLA targets |
| **CMDB** | Asset and configuration item tracking |
| **Notifications** | Email alerts for assignments, SLA breach, approvals |
| **Reports & Dashboards** | KPI visibility for IT managers |
| **Knowledge Management** | Self-service article library |
| **REST API / Scripted REST** | Inbound webhook for auto-incident creation |

---

## 🗄️ Database Tables

| Table Name | Description |
|---|---|
| `x_itsd_incident` | Core incident records |
| `x_itsd_asset` | IT asset master data |
| `x_itsd_cmdb_ci` | CMDB Configuration Items |
| `x_itsd_change_request` | Change request records |
| `x_itsd_problem` | Problem records linked to incidents |
| `x_itsd_sla_definition` | SLA rules and breach thresholds |
| `x_itsd_knowledge` | Knowledge Base articles |
| `x_itsd_assignment_group` | IT team groups for routing |
| `x_itsd_audit_log` | Full audit trail of all changes |

---

## 🔁 Workflows & Flows

### Incident Flow
```
Employee Submits Incident (Portal)
          ↓
Auto-Assignment to Group (Business Rule)
          ↓
Agent Picks Up → Status: In Progress
          ↓
   ┌──────┴──────┐
Resolved      Escalated (SLA Breach)
   ↓                  ↓
Closed         IT Manager Notified
```

### Change Request Flow
```
Requester Submits Change Request
          ↓
IT Manager Review (Approve / Reject)
          ↓
CAB (Change Advisory Board) Vote
          ↓
Implementation Window Scheduled
          ↓
Change Implemented → Post-Implementation Review
          ↓
         Closed
```

### Asset Lifecycle Flow
```
Asset Ordered
     ↓
Received → In Stock
     ↓
Assigned to User → In Use
     ↓
Issue Raised → In Maintenance
     ↓
End of Life → Retired
```

---

## ⏱️ SLA Configuration

| SLA Name | Priority | Response Time | Resolution Time |
|---|---|---|---|
| P1 - Critical | 1 | 15 minutes | 1 hour |
| P2 - High | 2 | 30 minutes | 4 hours |
| P3 - Medium | 3 | 2 hours | 8 hours |
| P4 - Low | 4 | 4 hours | 24 hours |

- **Warning threshold**: 75% of resolution time elapsed → email alert sent
- **Breach**: SLA breached → auto-escalate + notify IT Manager
- SLA pauses when incident is in **Pending** state (waiting on user)

---

## 🔗 REST API Integration

ITServiceDesk Pro exposes and consumes REST APIs to simulate real-world monitoring tool integrations.

### Inbound — Auto Incident Creation
A monitoring tool (e.g., Nagios, Datadog) sends an alert via webhook → ServiceNow auto-creates an incident.

```json
POST /api/x_itsd/incident/create
{
  "short_description": "Server CPU usage exceeded 95%",
  "category": "Hardware",
  "impact": "1",
  "urgency": "1",
  "source": "Nagios Monitor"
}
```

**Business Rule** processes the payload and creates the incident record automatically with priority P1.

### Outbound — Notify External Teams
When a P1 incident is raised, ServiceNow sends a REST call to an external Slack/Teams webhook to alert the on-call engineer.

---

## 📊 Dashboard & Reports

### IT Manager Dashboard
| Widget | Metric |
|---|---|
| 🔴 Open Incidents | Count by priority |
| 🟡 SLA At Risk | Incidents approaching breach |
| 🟢 Resolved Today | Count |
| 📈 MTTR | Mean Time to Resolve (avg in hours) |
| 🖥️ Asset Utilization | % of assets currently in use |
| 🔄 Pending Changes | Changes awaiting CAB approval |

### Report Types Built
- **Incident Trend** — bar chart, incidents per week
- **SLA Compliance** — % of incidents resolved within SLA per month
- **Top Incident Categories** — pie chart (Network / Hardware / Software)
- **Change Success Rate** — successful vs. failed changes
- **Asset Distribution** — assets per department

---

## 📚 Knowledge Base

- Articles linked to incident **categories** (e.g., "How to reset VPN" linked to Network incidents)
- Agents can **suggest articles** to users during incident resolution
- Users can rate articles (👍 / 👎) — low-rated articles flagged for review
- Reduces repeat tickets by enabling **self-service resolution**

**Sample Articles:**
- How to connect to VPN remotely
- Steps to request a new software license
- Laptop not booting — first-level troubleshooting guide
- How to reset your Active Directory password

---

## 🏗️ How to Build — Step by Step

> Follow this guide on your **ServiceNow PDI** (Personal Developer Instance) at [developer.servicenow.com](https://developer.servicenow.com)

### Step 1 — Create the Application
1. Go to **ServiceNow Studio** → `Create Application`
2. Name: `ITServiceDesk Pro`, Scope: `x_itsd`
3. Choose **Classic Application**

### Step 2 — Create Tables
Create the following tables in Studio → `Create Application File` → `Table`:

| Table | Extends | Key Fields |
|---|---|---|
| Incident | Task | category, priority, assignment_group, state |
| Asset | None | asset_tag, model, assigned_to, state, serial_number |
| Change Request | Task | type, risk, cab_required, implementation_date |
| Problem | Task | related_incidents, root_cause, workaround |

> **Pro tip:** Extending `Task` gives you built-in fields like assigned_to, state, short_description, and SLA support for free.

### Step 3 — Build Forms
For each table, open **Form Designer** and arrange:
- Header section: Number, Short Description, State
- Details section: Category, Priority, Impact, Urgency
- Assignment section: Assignment Group, Assigned To
- Resolution section: Resolution Notes, Close Code

### Step 4 — Priority Auto-Calculation (Business Rule)
```javascript
// Business Rule: Before Insert/Update on Incident table
// Name: Auto Calculate Priority

(function executeRule(current, previous) {
    var impact = parseInt(current.impact);
    var urgency = parseInt(current.urgency);
    var matrix = {
        '1,1': '1', '1,2': '2', '1,3': '3',
        '2,1': '2', '2,2': '3', '2,3': '4',
        '3,1': '3', '3,2': '4', '3,3': '5'
    };
    current.priority = matrix[impact + ',' + urgency] || '4';
})(current, previous);
```

### Step 5 — Auto Assignment (Business Rule)
```javascript
// Business Rule: After Insert on Incident table
// Name: Auto Assign to Group

(function executeRule(current, previous) {
    var groupMap = {
        'network':  'Network Support',
        'hardware': 'Hardware Support',
        'software': 'Software Support',
        'security': 'Security Team'
    };
    var groupName = groupMap[current.category.toString().toLowerCase()];
    if (groupName) {
        var gr = new GlideRecord('sys_user_group');
        gr.addQuery('name', groupName);
        gr.query();
        if (gr.next()) {
            current.assignment_group = gr.sys_id;
        }
    }
})(current, previous);
```

### Step 6 — SLA Definitions
1. Go to **SLA → SLA Definitions** → New
2. Name: `P1 Resolution SLA`
3. Table: `x_itsd_incident`
4. Conditions: `Priority = 1`
5. Duration: `1 Hour`
6. Pause conditions: `State = Pending`
7. Add **Workflow** for breach notification

### Step 7 — Flow Designer Workflows
**Incident Escalation Flow:**
1. Trigger: SLA Breach on Incident
2. Action: Update `state` → `Escalated`
3. Action: Send Email to IT Manager
4. Action: Create Task for IT Manager follow-up

**Change Approval Flow:**
1. Trigger: Change Request created with `type = Normal`
2. Action: Ask for Approval → IT Manager
3. If Approved → Ask for Approval → CAB Group
4. If Approved → Set state = `Scheduled`
5. If Rejected → Set state = `Cancelled`, notify requester

### Step 8 — Service Portal
1. Go to **Service Portal → Portals** → New
2. URL Suffix: `itsd`
3. Add pages: Home, Submit Incident, My Tickets, Knowledge Base
4. Add catalog items: `Report an Incident`, `Request New Asset`, `Submit Change Request`

### Step 9 — REST API (Scripted REST)
1. Go to **Scripted REST APIs** → New
2. Name: `Incident Inbound API`, Base API path: `/incident`
3. Add Resource: `POST /create`
4. Script:
```javascript
(function process(request, response) {
    var body = request.body.data;
    var gr = new GlideRecord('x_itsd_incident');
    gr.initialize();
    gr.short_description = body.short_description;
    gr.category = body.category;
    gr.impact = body.impact;
    gr.urgency = body.urgency;
    gr.insert();
    response.setStatus(201);
    response.setBody({ status: 'created', incident_number: gr.number.toString() });
})(request, response);
```

### Step 10 — Dashboard
1. Go to **Reports** → Create reports for each KPI
2. Go to **Dashboards** → New → Add widgets
3. Pin reports as widgets on the IT Manager Dashboard

---

## 📸 Screenshots

> Add screenshots here after building on your PDI

| Screen | Description |
|---|---|
| `screenshots/incident-list.png` | Incident list view with priority color coding |
| `screenshots/incident-form.png` | Incident form with all sections |
| `screenshots/change-flow.png` | Flow Designer — Change Approval workflow |
| `screenshots/dashboard.png` | IT Manager KPI Dashboard |
| `screenshots/portal-home.png` | Service Portal home page |
| `screenshots/sla-config.png` | SLA definition configuration |

---

## 🚀 Future Enhancements

- [ ] AI-powered incident categorization using ML (predict category from description)
- [ ] Chatbot integration on Service Portal for first-level support
- [ ] Mobile app integration via ServiceNow Mobile Agent
- [ ] Automated root cause detection using event correlation
- [ ] Integration with Active Directory for auto-user provisioning
- [ ] Shift scheduling for IT support agents

---

## 👨‍💻 Developer

**Krishna Omkar Chaurasiya**
B.Tech Computer Science & Engineering (AI & Data Sciences)
Parul University, Vadodara — 2027

[![GitHub](https://img.shields.io/badge/GitHub-krishna--9016-black?style=flat-square&logo=github)](https://github.com/krishna-9016)
[![Portfolio](https://img.shields.io/badge/Portfolio-Visit-orange?style=flat-square&logo=google-chrome)](https://codolio.com/profile/krishna_9016)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Krishna%20Chaurasiya-blue?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/krishnachaurasiya)

> 🏆 ServiceNow CSA & CAD Certified &nbsp;|&nbsp; Oracle OCI Certified &nbsp;|&nbsp; LeetCode Knight (1850+ rating)

---

<div align="center">

**⭐ Star this repo if you found it helpful!**

*Built with ❤️ on ServiceNow PDI*

</div>