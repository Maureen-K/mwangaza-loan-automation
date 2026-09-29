# Mwangaza Microfinance — Loan Application Automation

> End-to-end business process automation built with n8n, Google Forms, Google Sheets, REST APIs and Gmail.

## Overview

Mwangaza Microfinance is a simulated Kenyan microfinance company that receives loan applications through an online form.

The goal of this project was to automate the process from customer submission through validation, API submission, customer communication and management review.

The workflow was designed to handle both normal business scenarios and failure conditions.

---

## Business Problem

A loan application process can require several manual steps:

- Checking whether customer information is complete
- Validating submitted data
- Entering application information into another system
- Confirming receipt with the customer
- Identifying applications requiring management review
- Handling failed system submissions

Manual handling increases repetitive work and creates opportunities for missed information or incorrect communication.

This automation demonstrates how those steps can be orchestrated into a single workflow.

---

## Solution

The automation connects:

**Customer → Google Forms → Google Sheets → n8n → REST API → Email Notifications**

n8n acts as the orchestration layer that validates information, applies business rules, communicates with the external API and controls the notification paths.

### Architecture

```text
Customer
   ↓
Google Form
   ↓
Google Sheets
   ↓
n8n
   ↓
Prepare & Validate Application
   ↓
REST API
   ↓
Check API Response
   ├── Failure → Technical Error → Manager
   │
   └── Success
          ↓
     Customer Confirmation
          ↓
     Check Loan Amount
          ├── < KSh 50,000 → End
          │
          └── ≥ KSh 50,000 → Manager Review
