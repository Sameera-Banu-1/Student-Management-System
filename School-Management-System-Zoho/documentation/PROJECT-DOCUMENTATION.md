# STUDENT MANAGEMENT & FEE AUTOMATION SYSTEM

**Complete Zoho CRM Project Documentation**

**Technology:** Zoho CRM • Deluge • Workflow Automation • Reports • Dashboards

## 1. Executive Summary

The Student Management & Fee Automation System is a school-focused CRM solution developed using Zoho CRM to centralise student, fee and payment information and reduce manual financial processing.

The core implementation uses Students as the master record, Fees as the financial obligation layer, and Payments as the transaction layer. Multiple payment transactions can be associated with a fee, allowing the system to calculate the amount collected, outstanding balance and current payment status.

Workflow automation and Deluge scripting are used to process payment transactions and maintain the related fee information. Reports and dashboards provide management-level visibility into fee collection, outstanding amounts, payment status and payment modes.

The project demonstrates practical CRM customisation, relational data modelling, workflow automation, Deluge development, email notifications, reporting and dashboard analytics, while providing a foundation that can be extended to admissions, academics, attendance, examinations and a Zoho Creator parent-facing application.

## 2. Project Objective

The objective of the project is to build a structured school management solution using Zoho CRM, with particular focus on student, fee and payment management.

The implementation aims to:

- Centralise student information.
- Maintain structured fee records.
- Record individual payment transactions.
- Automatically calculate collected and outstanding amounts.
- Automatically determine payment status.
- Reduce manual financial updates.
- Send relevant payment-related notifications.
- Provide useful reports and dashboards.
- Demonstrate Deluge-based business automation.
- Provide an extensible foundation for broader school-management requirements.

## 3. Problem Statement

Schools manage large amounts of student and financial information. When fee information is maintained manually, staff may face incorrect balances, missed updates, duplicate work, delayed reminders, and limited management visibility.

Payments may also be split across multiple transactions, making manual calculation of the total collected amount and outstanding balance error-prone.

This project addresses these challenges by creating a connected CRM data model and implementing automation for fee calculations, payment reconciliation, status updates, notifications, reporting, and dashboard-based monitoring.


## 4. Assignment Requirements Mapping

| Requirement                        | Implementation                                                                                                             |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| **CRM Modules**                    | Students, Fees, and Payments                                                                                               |
| **Creator Structure**              | Parent-facing application concept designed as an extension layer for CRM data                                              |
| **Working CRM Webform**            | Controlled entry point for student/admission data; final published URL can be added at submission                          |
| **Data Structure & Relationships** | `Student → Fees → Payments` relational model                                                                               |
| **CRM–Creator Integration**        | Designed for controlled retrieval and updating of CRM information through supported Zoho integration/API mechanisms        |
| **Workflows**                      | Student creation, payment-to-fee update, payment confirmation, and outstanding-fee reminder                                |
| **Deluge Functions**               | `Update_Fee_Amounts1` custom function                                                                                      |
| **Validations**                    | Required fields and business-rule validations to maintain data accuracy                                                    |
| **Reports**                        | Fee collection summary, payment details, and payment-status reporting                                                      |
| **Dashboards**                     | Total Fees, Total Amount Collected, Outstanding Amount, status distribution, monthly collection, and payment-mode analysis |
| **Additional Feature**             | Automated fee reconciliation and Payment Status classification                                                             |
| **Optimisation & Scalability**     | Transaction-based aggregation, null handling, and clear separation between transaction-level and summary-level data        |


## 5. Project Objectives

* Centralise student information in Zoho CRM.
* Create a structured and reliable fee-management process.
* Record individual payment transactions.
* Automatically calculate the total amount collected and outstanding balance.
* Automatically classify fee status as **Pending**, **Partially Paid**, or **Paid**.
* Send payment confirmation and outstanding-fee notifications.
* Generate meaningful financial and operational reports.
* Build dashboards for effective financial decision-making.
* Demonstrate Deluge-based CRM automation.
* Provide an extensible architecture for future modules such as admissions, attendance, academics, examinations, and parent access.


## 6. Business Process

The fee-management process follows a transaction-based automation model:

1. **Create Student**
   Student information is entered into the **Students** module.

2. **Create Fee Record**
   A Fee record is created and associated with the relevant student.

3. **Record Payment**
   A Payment record is created against the related Fee.

4. **Trigger Automation**
   The Payment workflow triggers the custom Deluge function `Update_Fee_Amounts1`.

5. **Retrieve Related Payments**
   The function retrieves all payment transactions associated with the Fee.

6. **Calculate Total Collection**
   All valid payment amounts are aggregated to determine the total amount collected.

7. **Update Fee Summary**
   The Fee record is updated with the calculated financial information.

8. **Calculate Outstanding Balance**
   The remaining balance is calculated based on the total fee and amount collected.

9. **Update Payment Status**
   The system automatically determines the appropriate status:

   * **Pending** — No payment has been collected.
   * **Partially Paid** — Some amount has been collected, but the full fee is not paid.
   * **Paid** — The complete fee amount has been collected.

10. **Send Notifications**
    Automated email notifications communicate payment confirmations and outstanding-fee reminders.

### Process Flow

```text
Student Created
      ↓
Fee Record Created
      ↓
Payment Recorded
      ↓
Payment Workflow Triggered
      ↓
Update_Fee_Amounts1
      ↓
Retrieve Related Payments
      ↓
Calculate Total Collected
      ↓
Update Fee Amounts
      ↓
Calculate Outstanding Amount
      ↓
Determine Payment Status
      ↓
Send Notifications
      ↓
Reports & Dashboard
```

## 7. Solution Architecture

The architecture separates **master data, financial obligations, and payment transactions** to maintain data consistency and scalability.

* **Students** represent the people and their school-related information.
* **Fees** represent financial obligations associated with students.
* **Payments** represent individual payment transactions.
* A single Fee can have multiple Payments, which supports **partial-payment and multiple-transaction scenarios**.
* Automation keeps the Fee summary synchronized with its related Payment records.

### Architecture Layers

| Layer                    | Component           | Purpose                                                                         |
| ------------------------ | ------------------- | ------------------------------------------------------------------------------- |
| **Master**               | Students            | Stores student identity and related school information.                         |
| **Financial Obligation** | Fees                | Stores total fee, amount collected, outstanding amount, and payment status.     |
| **Transaction**          | Payments            | Stores individual payment transactions and payment details.                     |
| **Automation**           | Workflow + Deluge   | Automatically synchronizes payment transactions with the Fee financial summary. |
| **Communication**        | Email Notifications | Confirms successful payments and sends reminders for outstanding balances.      |
| **Analytics**            | Reports + Dashboard | Converts CRM data into meaningful management and financial insights.            |
| **Extension**            | Zoho Creator        | Provides the foundation for a potential parent-facing application layer.        |

### High-Level Architecture

```text
                    ┌──────────────────┐
                    │     Students     │
                    │   Master Data    │
                    └────────┬─────────┘
                             │ 1 : Many
                             ▼
                    ┌──────────────────┐
                    │       Fees       │
                    │ Financial        │
                    │ Obligation       │
                    └────────┬─────────┘
                             │ 1 : Many
                             ▼
                    ┌──────────────────┐
                    │     Payments     │
                    │  Transactions    │
                    └────────┬─────────┘
                             ▼
                    ┌──────────────────┐
                    │ Workflow +       │
                    │ Deluge Function  │
                    └────────┬─────────┘
              ┌──────────────┴──────────────┐
              ▼                             ▼
     ┌──────────────────┐         ┌──────────────────┐
     │ Email            │         │ Reports &        │
     │ Notifications    │         │ Dashboard        │
     └──────────────────┘         └──────────────────┘
                    Zoho Creator
                         │
                         ▼
              Parent-Facing Application
```

## 8. Data Model & Relationships

The system uses a relational structure where payment transactions are linked to their corresponding Fee records.

| Relationship      | Type        | Explanation                                                                                                  |
| ----------------- | ----------- | ------------------------------------------------------------------------------------------------------------ |
| **Student → Fee** | One-to-Many | A student can have multiple fee obligations, such as tuition, examination, hostel, or other applicable fees. |
| **Fee → Payment** | One-to-Many | A single fee can be settled through multiple payment transactions.                                           |
| **Payment → Fee** | Many-to-One | Each payment transaction belongs to one specific Fee record.                                                 |


### Transaction-Based Financial Model

Payments are **not treated as manually entered totals** inside the Fee record. Instead, each payment is stored as an individual transaction.

This approach provides:

* Accurate payment tracking
* Support for multiple transactions
* Partial-payment handling
* Reduced manual calculation
* Better auditability
* Reliable outstanding-balance calculation
* Scalable financial reporting

## 9. Students Module

The **Students** module stores master student information and connects students with their fees and payments.

| Field / Concept     | Purpose                  |
| ------------------- | ------------------------ |
| Student Name        | Student identification   |
| Email               | Communication            |
| Student Information | Required student details |
| Related Fees        | Connected fee records    |
| Related Payments    | Payment activity         |

**Screenshot:** `01_students_module.png`


## 10. Fees Module

The **Fees** module manages the financial status of each student.

| Field                   | API Name                  | Purpose                         |
| ----------------------- | ------------------------- | ------------------------------- |
| Total Fees              | `Total_Fees`              | Total amount due                |
| Amount Collected        | `Amount_Collected1`       | Amount collected                |
| Collected Amount Helper | `Collected_Amount_Helper` | Calculated collection           |
| Outstanding Amount      | `outstanding_amount1`     | Remaining balance               |
| Payment Status          | `payment_status`          | Pending / Partially Paid / Paid |

Fee details are automatically updated when payments are recorded.

**Screenshot:** `02_fees_module.png`


## 11. Payments Module

The **Payments** module stores individual transactions linked to a Fee.

| Field / Concept   | Purpose                    |
| ----------------- | -------------------------- |
| Payment Name / ID | Transaction identification |
| Student           | Student reference          |
| Fee               | Related Fee                |
| Payment Date      | Transaction date           |
| Amount Paid       | Amount received            |
| Payment Mode      | Cash / UPI / Bank Transfer |
| Payment Owner     | Record ownership           |

**Screenshot:** `03_payments_module.png`


## 12. Important API Names

| Module   | API Name                  | Purpose               |
| -------- | ------------------------- | --------------------- |
| Fees     | `Total_Fees`              | Total fee             |
| Fees     | `Amount_Collected1`       | Collected amount      |
| Fees     | `Collected_Amount_Helper` | Calculated collection |
| Fees     | `outstanding_amount1`     | Outstanding balance   |
| Fees     | `payment_status`          | Payment status        |
| Payments | `Fee`                     | Fee lookup            |
| Payments | `Amount_Paid`             | Payment amount        |


##  Workflow Automation

Payment-related workflows automatically trigger **Deluge-based fee reconciliation**, updating the collected amount, outstanding balance, and payment status.

## 12.1 Payment Update Fee Amount

When a Payment is created or edited, `Update_Fee_Amounts1`:

1. Retrieves the related Fee.
2. Fetches all Payments for that Fee.
3. Calculates total collected using `Amount_Paid`.
4. Calculates the outstanding amount.
5. Determines the payment status.
6. Updates the Fee record.

**Screenshot:** `05_payment_update_workflow.png`

## 12.2 Automatic Payment Status

| Condition                         | Status             |
| --------------------------------- | ------------------ |
| Collected = 0                     | **Pending**        |
| Collected > 0 and Outstanding > 0 | **Partially Paid** |
| Outstanding = 0                   | **Paid**           |

Tested successfully with a partial-payment scenario.

## 12.3 Payment Confirmation

Automatically sends payment confirmation with relevant payment and student/fee details.

**Screenshot:** `07_payment_confirmation.png`

## 12.4 Outstanding Fee Reminder

Automatically notifies users when the **Outstanding Amount > 0**, supporting timely fee collection.


## 13. Deluge Custom Function

**Function:** `automation.Update_Fee_Amounts1`

The function reconciles Payments with the related Fee by:

1. Retrieving the Fee and related Payments.
2. Calculating total collected.
3. Calculating the outstanding amount.
4. Determining the payment status.
5. Updating the Fee record.

**Source:** `deluge/Update_Fee_Amounts1.deluge`

**Screenshot:** `06_deluge_function.png`


## 14. Optimisation & Scalability

* Supports multiple payments per Fee.
* Uses null-safe payment calculations.
* Aggregates Payments using the Fee lookup.
* Automatically derives Payment Status.
* Reports and dashboards use calculated data.
* Can be extended with logging, bulk processing, and monitoring.


## 15. Email Notifications

| Notification                 | Trigger                | Purpose                |
| ---------------------------- | ---------------------- | ---------------------- |
| **Payment Confirmation**     | Payment recorded       | Confirms transaction   |
| **Outstanding Fee Reminder** | Outstanding amount > 0 | Supports fee follow-up |

**Screenshot:** `12_email_notifications.png`


## 16. Reports

### Fee Collection Summary

Displays total fees, collected amount, outstanding amount, student, and payment status.

### Payment Details

Displays payment, fee, student, date, mode, and amount.

### Fee/Payment Status Summary

Tracks **Pending, Partially Paid, and Paid** records.

**Screenshot:** `09_reports.png`


## 17. Dashboard & Analytics

### KPIs

| KPI                    | Purpose                      |
| ---------------------- | ---------------------------- |
| **Total Fees**         | Overall financial obligation |
| **Amount Collected**   | Total received               |
| **Outstanding Amount** | Remaining balance            |

### Charts

* **Payment Status Distribution**
* **Monthly Payment Collection**
* **Payment Mode Analysis**

**Tested Dashboard Values:** Total Fees ₹2,20,000 | Collected ₹90,000 | Outstanding ₹1,20,000.

**Screenshot:** `11_dashboard.png`


## 18. Business Value

* **Total Fees** → Measures financial obligation.
* **Collected Amount** → Measures collection performance.
* **Outstanding Amount** → Identifies pending collections.
* **Payment Status** → Highlights collection risk.
* **Monthly Collection** → Shows trends.
* **Payment Mode** → Shows transaction patterns.


## 19. Testing & Validation

| Test              | Expected Result                  | Status      |
| ----------------- | -------------------------------- | ----------- |
| Partial Payment   | Partially Paid + correct balance | ✅ Verified  |
| Full Payment      | Paid + Outstanding = ₹0          | ✅ Verified  |
| No Payment        | Pending                          | ✅ Verified  |
| Multiple Payments | Correct total aggregation        | ✅ Supported |
| Dashboard         | KPIs/charts reflect CRM data     | ✅ Verified  |


## What Makes the Project Different

* **Clear data model:** Student → Fee → Payment
* **Automatic reconciliation:** Payment transactions update Fee balances.
* **Smart status tracking:** Pending, Partially Paid, or Paid.
* **Automated communication:** Payment confirmations and reminders.
* **Management analytics:** Focused KPIs, reports, and dashboards.
* **Scalable design:** Ready for future Zoho Creator parent access.
* **Maintainable architecture:** Designed with optimisation and scalability in mind.
* **Business-focused documentation:** Explains both implementation and business value.


## 20. Future Enhancements

* Admission and application tracking
* Attendance and academic performance management
* Parent portal using Zoho Creator
* Online payment gateway and receipt generation
* SMS/WhatsApp notifications
* Automated due-date reminders
* Refund and payment reversal management
* Role-based dashboards
* Bulk reconciliation and exception monitoring
* Integration and automation monitoring


## Final Project Statement

This project demonstrates a practical **School Management System built with Zoho CRM**, focusing on data integrity, automation, and efficient fee management.
By connecting **Students, Fees, and Payments** with Deluge-based reconciliation, automated payment status, notifications, reports, and dashboards, the solution reduces manual effort and provides a scalable foundation for future school-management and parent-facing features.


## 🔗 Live Demo Webform

📋 [School Admission Inquiry Form] (<form ... action=' https://crm.zoho.in/crm/WebToLeadForm '>)
https://crm.zoho.in/crm/org60083676102/settings/webform?module=Leads

