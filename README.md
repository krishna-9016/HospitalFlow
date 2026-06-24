<div align="center">

# 🏥 HospitalFlow

### Patient OPD Management & Hospital Services Automation on ServiceNow

[![ServiceNow](https://img.shields.io/badge/ServiceNow-Healthcare-green?style=for-the-badge&logo=servicenow&logoColor=white)](https://www.servicenow.com/)
[![Domain](https://img.shields.io/badge/Domain-Healthcare-red?style=for-the-badge)]()
[![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)]()
[![Developer](https://img.shields.io/badge/Developer-Krishna%20Chaurasiya-orange?style=for-the-badge)](https://github.com/krishna-9016)

---

> **HospitalFlow** is a ServiceNow-based hospital management system that automates the complete patient journey — from OPD appointment booking to discharge — eliminating manual paperwork, reducing wait times, and improving the overall healthcare experience through smart workflows and a self-service patient portal.

---

</div>

## 📌 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [ServiceNow Modules Used](#-servicenow-modules-used)
- [Database Tables](#-database-tables)
- [Workflows & Flows](#-workflows--flows)
- [Notifications & Automation](#-notifications--automation)
- [Dashboard & Reports](#-dashboard--reports)
- [Patient Self-Service Portal](#-patient-self-service-portal)
- [How to Build — Step by Step](#-how-to-build--step-by-step)
- [Screenshots](#-screenshots)
- [Future Enhancements](#-future-enhancements)
- [Developer](#-developer)

---

## 🔍 Overview

**HospitalFlow** digitizes the day-to-day operations of a hospital's outpatient department (OPD). The system handles:

- Online appointment booking with real-time doctor availability
- Patient registration and insurance verification
- Doctor assignment and consultation tracking
- Lab test ordering, result upload, and report delivery
- Billing and payment management
- Discharge summary generation
- Support ticket system for patient queries

This project showcases ServiceNow's **Service Catalog**, **Flow Designer**, **Service Portal**, and **Business Rules** in a real-world healthcare context — demonstrating how ServiceNow extends beyond IT into enterprise service management.

---

## ✨ Features

### 📅 1. OPD Appointment Booking
- Patients book appointments via the **Self-Service Portal**
- Dynamic doctor availability slots based on department and day
- Departments supported: General Medicine, Cardiology, Orthopedics, Neurology, Pediatrics, Dermatology
- Appointment statuses: `Requested → Confirmed → In Consultation → Completed → Cancelled`
- Auto-confirmation email with appointment ID, doctor name, time slot, and room number
- **Reschedule & Cancel** options available up to 2 hours before the appointment

### 🧾 2. Patient Registration & Onboarding
Automated patient onboarding after first appointment confirmation:

**Onboarding Checklist:**
- ✅ Patient ID Generation (unique `PAT-XXXXXX`)
- ✅ Medical Record Number (MRN) Assignment
- ✅ Insurance Verification
- ✅ Emergency Contact Registration
- ✅ Patient Portal Account Activation
- ✅ Welcome Email with Portal Credentials

### 👨‍⚕️ 3. Doctor & Consultation Management
- Doctor profiles with specialization, availability schedule, and max daily patients
- Auto-assignment of doctor based on department and availability
- Consultation notes form for doctors to record:
  - Chief Complaint
  - Diagnosis
  - Prescription
  - Follow-up date
  - Referral to specialist (if needed)
- Consultation lifecycle: `Scheduled → In Progress → Completed`

### 🧪 4. Lab & Diagnostics Management
- Doctor orders lab tests directly from consultation form
- Tests routed to respective lab departments: Pathology, Radiology, Microbiology
- Lab technician uploads results with reference ranges
- Auto-notification to doctor and patient when results are ready
- Reports archived in patient's medical history

**Supported Tests:**
- Blood CBC, LFT, KFT, Lipid Profile
- X-Ray, MRI, CT Scan, Ultrasound
- Urine Routine, Culture & Sensitivity
- ECG, Echo

### 💳 5. Billing & Payment Management
- Auto-generation of bill after consultation + lab orders
- Bill components: Consultation Fee + Lab Charges + Medication Charges
- Payment statuses: `Pending → Partially Paid → Paid → Waived`
- Insurance claim submission workflow
- Receipt generation on payment confirmation
- Outstanding balance alerts sent to patient

### 🏨 6. IPD (Inpatient) Admission
- Doctor can recommend IPD admission from consultation
- Ward and bed allocation workflow: `Request → Ward Manager Approval → Bed Assigned`
- Ward types: General, Semi-Private, Private, ICU
- Nurse assignment to admitted patient
- Daily progress notes by attending doctor

### 🚪 7. Discharge Management
- Discharge initiated by treating doctor
- Discharge summary auto-generated including:
  - Diagnosis summary
  - Treatment given
  - Medications prescribed
  - Follow-up instructions
  - Lab reports attached
- Final bill generated on discharge
- Feedback form sent to patient post-discharge

### 🎫 8. Patient Help Desk
- Patients raise support tickets for:
  - Appointment issues
  - Billing queries
  - Lab report delays
  - Feedback & complaints
- Ticket routing to respective department staff
- SLA-based resolution tracking

---

## 🛠️ ServiceNow Modules Used

| Module | Purpose |
|---|---|
| **ServiceNow Studio** | Application development environment |
| **Service Catalog** | Appointment booking, lab test requests, IPD admission requests |
| **Flow Designer** | Patient onboarding, appointment confirmation, discharge workflows |
| **Service Portal** | Patient self-service portal |
| **Business Rules** | Auto-assignment, bill calculation, MRN generation |
| **Client Scripts** | Dynamic slot availability, form field control |
| **UI Policies** | Show/hide fields based on appointment type and patient status |
| **Notifications** | Appointment confirmations, lab results, discharge summaries |
| **Reports & Dashboards** | OPD stats, doctor utilization, revenue reports |
| **SLA Management** | Lab report turnaround time, help desk resolution |
| **Scheduled Jobs** | Daily appointment reminders, follow-up alerts |

---

## 🗄️ Database Tables

| Table Name | Description |
|---|---|
| `x_hf_patient` | Patient master records |
| `x_hf_appointment` | OPD appointment bookings |
| `x_hf_doctor` | Doctor profiles and schedules |
| `x_hf_consultation` | Consultation notes and prescriptions |
| `x_hf_lab_order` | Lab test orders linked to consultation |
| `x_hf_lab_result` | Lab results uploaded by technicians |
| `x_hf_billing` | Patient bills and payment tracking |
| `x_hf_ipd_admission` | Inpatient admission records |
| `x_hf_discharge` | Discharge summaries |
| `x_hf_support_ticket` | Patient help desk tickets |
| `x_hf_doctor_schedule` | Doctor availability slots by day/time |

---

## 🔁 Workflows & Flows

### Patient Journey — End to End
```
Patient Books Appointment (Portal)
             ↓
   Auto Slot Confirmation (Flow)
             ↓
   Patient Arrives → Registration
             ↓
   Doctor Consultation
       ↓           ↓
  Lab Orders    Prescription
       ↓
  Lab Results Ready → Notified
       ↓
   Follow-up / IPD Admission?
       ↓               ↓
   Discharged      Bed Allocated
       ↓
  Discharge Summary + Final Bill
       ↓
    Feedback Sent
```

### Appointment Booking Flow
```
Patient Submits Booking (Catalog)
             ↓
Check Doctor Availability (Business Rule)
             ↓
      ┌──────┴──────┐
  Available      Not Available
      ↓                ↓
Slot Confirmed    Suggest Next Slot
      ↓
Confirmation Email + Calendar Invite
```

### Lab Test Flow
```
Doctor Orders Lab Test (Consultation Form)
             ↓
Lab Order Record Created → Routed to Lab Dept
             ↓
Lab Technician Processes Sample
             ↓
Results Uploaded with Reference Ranges
             ↓
Auto-Notify Doctor + Patient
             ↓
Report Archived in Patient History
```

### IPD Admission & Discharge Flow
```
Doctor Recommends Admission
             ↓
Admission Request → Ward Manager Approval
             ↓
Bed Allocated → Nurse Assigned
             ↓
Daily Progress Notes by Doctor
             ↓
Doctor Initiates Discharge
             ↓
Discharge Summary Auto-Generated
             ↓
Final Bill Created → Payment Collected
             ↓
Feedback Form Sent to Patient
```

---

## 🔔 Notifications & Automation

| Trigger | Notification Sent To |
|---|---|
| Appointment Confirmed | Patient — confirmation with slot details |
| Appointment Reminder | Patient — 24 hours before appointment |
| Lab Results Ready | Doctor + Patient |
| Bill Generated | Patient — itemized bill |
| Payment Received | Patient — receipt |
| IPD Bed Allocated | Patient + Attendant |
| Discharge Initiated | Patient — discharge summary + instructions |
| Follow-up Due | Patient — 1 day before follow-up date |
| Help Desk Ticket Resolved | Patient |

**Scheduled Jobs:**
- Daily 8 AM — send appointment reminders for the day
- Daily 6 PM — flag overdue lab reports (SLA breach)
- Weekly — generate OPD utilization report for admin

---

## 📊 Dashboard & Reports

### Hospital Admin Dashboard

| Widget | Metric |
|---|---|
| 📅 Today's Appointments | Count by department |
| 🧪 Pending Lab Reports | Overdue vs. in-progress |
| 💰 Revenue Today | Consultation + Lab + IPD |
| 🛏️ Bed Occupancy | % of beds occupied by ward |
| 👨‍⚕️ Doctor Utilization | Patients seen per doctor |
| 🎫 Open Support Tickets | Count by category |

### Report Types Built
- **Daily OPD Count** — appointments per department per day
- **Doctor Workload** — consultations per doctor per week
- **Lab TAT (Turnaround Time)** — avg hours from order to result
- **Revenue Breakdown** — pie chart by service type
- **Patient Satisfaction** — avg feedback score per doctor
- **Appointment No-Show Rate** — % of booked vs. attended

---

## 🌐 Patient Self-Service Portal

The patient-facing portal (`/hospitalflow`) allows patients to:

| Action | Description |
|---|---|
| 📅 Book Appointment | Select department, doctor, date, time slot |
| 🔍 Track Appointment | View status and doctor details |
| 🧪 View Lab Reports | Download results as PDF |
| 💳 View & Pay Bills | Online payment with receipt download |
| 📋 Medical History | Past consultations, prescriptions, reports |
| 🎫 Raise Support Ticket | Query for billing, reports, feedback |
| 📝 Give Feedback | Rate doctor and hospital experience |

---

## 🏗️ How to Build — Step by Step

> Build this on your **ServiceNow PDI** at [developer.servicenow.com](https://developer.servicenow.com)

### Step 1 — Create the Application
1. Open **ServiceNow Studio** → `Create Application`
2. Name: `HospitalFlow`, Scope: `x_hf`
3. Type: **Classic Application**

### Step 2 — Create Tables
Go to Studio → `Create Application File` → `Table` for each:

| Table | Extends | Important Fields |
|---|---|---|
| Patient | None | patient_id, name, dob, blood_group, insurance_id, emergency_contact |
| Appointment | None | patient, doctor, department, slot_date, slot_time, status |
| Doctor | None | name, specialization, department, max_daily_patients |
| Consultation | None | appointment, diagnosis, prescription, lab_ordered, follow_up_date |
| Lab Order | None | consultation, test_name, department, status, ordered_by |
| Lab Result | None | lab_order, result_value, reference_range, uploaded_by, report_file |
| Billing | None | patient, appointment, consultation_fee, lab_charges, total, status |

### Step 3 — MRN Auto-Generation (Business Rule)

```javascript
// Business Rule: Before Insert on x_hf_patient
// Name: Generate Patient MRN

(function executeRule(current, previous) {
    var prefix = 'PAT-';
    var counter = new GlideRecord('x_hf_patient');
    counter.orderByDesc('sys_created_on');
    counter.setLimit(1);
    counter.query();
    var nextNum = 1000;
    if (counter.next()) {
        var lastId = counter.patient_id.toString();
        var lastNum = parseInt(lastId.replace(prefix, ''));
        nextNum = lastNum + 1;
    }
    current.patient_id = prefix + nextNum;
})(current, previous);
```

### Step 4 — Auto Bill Calculation (Business Rule)

```javascript
// Business Rule: Before Insert/Update on x_hf_billing
// Name: Calculate Total Bill

(function executeRule(current, previous) {
    var consultFee = parseFloat(current.consultation_fee) || 0;
    var labCharges = parseFloat(current.lab_charges) || 0;
    var medCharges = parseFloat(current.medication_charges) || 0;
    current.total_amount = consultFee + labCharges + medCharges;

    if (current.amount_paid >= current.total_amount) {
        current.payment_status = 'paid';
    } else if (current.amount_paid > 0) {
        current.payment_status = 'partially_paid';
    } else {
        current.payment_status = 'pending';
    }
})(current, previous);
```

### Step 5 — Doctor Availability Check (Client Script)

```javascript
// Client Script: onChange on Department field in Appointment form
// Name: Load Doctor Slots

function onChange(control, oldValue, newValue, isLoading) {
    if (isLoading || newValue === '') return;

    g_form.clearOptions('doctor');
    g_form.clearOptions('slot_time');

    var ga = new GlideAjax('HospitalFlowAjax');
    ga.addParam('sysparm_name', 'getDoctorsByDept');
    ga.addParam('sysparm_dept', newValue);
    ga.getXMLAnswer(function(response) {
        var doctors = JSON.parse(response);
        doctors.forEach(function(doc) {
            g_form.addOption('doctor', doc.sys_id, doc.name);
        });
    });
}
```

### Step 6 — Appointment Confirmation Flow (Flow Designer)

1. **Trigger:** Record Created → `x_hf_appointment` where `status = Requested`
2. **Action 1:** Check doctor availability (inline script)
3. **Action 2:** Set `status = Confirmed`, assign slot
4. **Action 3:** Send Email notification to patient
5. **Action 4:** Create onboarding checklist if new patient

### Step 7 — Lab Result Notification Flow

1. **Trigger:** Record Updated → `x_hf_lab_result` where `status = Uploaded`
2. **Action 1:** Update linked `x_hf_lab_order` status → `Completed`
3. **Action 2:** Send Email to Patient with report attached
4. **Action 3:** Send Email to Doctor with result summary

### Step 8 — Discharge Summary Flow

1. **Trigger:** Record Updated → `x_hf_ipd_admission` where `status = Discharge Initiated`
2. **Action 1:** Create `x_hf_discharge` record, auto-populate from admission + consultation
3. **Action 2:** Generate final bill (trigger billing Business Rule)
4. **Action 3:** Send discharge summary email to patient
5. **Action 4:** Send feedback form link to patient (after 1 hour delay)

### Step 9 — Service Catalog Items
Create these catalog items under **Service Catalog → Maintain Items:**

| Catalog Item | Variables |
|---|---|
| Book OPD Appointment | Department, Doctor, Date, Time Slot, Reason |
| Request Lab Test | Test Name, Urgency, Patient ID |
| IPD Admission Request | Department, Ward Type, Reason, Insurance |
| Raise Support Ticket | Category, Description, Preferred Contact |

### Step 10 — Service Portal Setup
1. **Service Portal → Portals** → New → URL: `hospitalflow`
2. Create pages: Home, Book Appointment, My Records, Lab Reports, Billing, Support
3. Add **search bar** widget for finding doctors by name/specialization
4. Add **announcement widget** for hospital news and OPD hours

---

## 📸 Screenshots

> Add screenshots here after building on your PDI

| Screen | Description |
|---|---|
| `screenshots/portal-home.png` | Patient self-service portal homepage |
| `screenshots/appointment-booking.png` | OPD appointment booking catalog form |
| `screenshots/consultation-form.png` | Doctor's consultation notes form |
| `screenshots/lab-order.png` | Lab test order and result upload view |
| `screenshots/billing.png` | Patient bill with itemized charges |
| `screenshots/discharge-flow.png` | Flow Designer — discharge workflow |
| `screenshots/dashboard.png` | Hospital Admin KPI dashboard |
| `screenshots/patient-history.png` | Patient medical history view |

---

## 🚀 Future Enhancements

- [ ] QR Code-based patient check-in at OPD counter
- [ ] AI-powered symptom checker on the patient portal
- [ ] Telemedicine / video consultation integration
- [ ] Pharmacy management module with prescription auto-fulfillment
- [ ] Ambulance request and tracking system
- [ ] Parent/guardian portal for pediatric patients
- [ ] Integration with government health schemes (Ayushman Bharat)
- [ ] Mobile app via ServiceNow Mobile Agent

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

*Built with Krishna Chaurasiya ❤️ on ServiceNow PDI*

</div>