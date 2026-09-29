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

---

## Workflow Architecture

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

Workflow

Key Automation Logic
1. Application Intake

Customers submit:

Customer name
Email address
Phone number
Loan amount

The application is recorded in Google Sheets and detected by the n8n Google Sheets Trigger.

2. Data Preparation & Validation

The workflow prepares the application data and checks whether the required information meets the defined validation rules.

Invalid applications are stopped before reaching the loan-management API.

3. REST API Submission

Valid applications are submitted using an HTTP POST request.

The workflow checks the API response before treating the application as successfully submitted.

A 201 Created response is treated as a successful API creation.

4. Customer Confirmation

A confirmation email is sent only after successful API submission.

5. Management Review

Applications requesting KSh 50,000 or more trigger a management-review notification.

6. Error Handling

The workflow handles two different failure classes:

Validation failure

The application contains invalid or incomplete information and is stopped before API submission.

API failure

The application passes validation, but the external API fails. The customer is not sent a false success message, and management receives a technical-error notification.

Key Results
API Integration

The workflow successfully submitted an application to the simulated REST API and handled the 201 Created response.

Customer Communication

The customer receives an automated confirmation after successful API submission.

Management Review

Applications meeting the KSh 50,000 threshold trigger an automated management notification.

Validation Handling

Invalid applications are stopped before API submission and routed to a management notification.

Failure Handling

A deliberate API failure was also tested to verify that the workflow does not send a false success message.

The API failure path triggers a technical-error notification for management.

Testing
Scenario	Expected Behaviour	Result
KSh 40,000 valid application	API submission + customer confirmation	Passed
KSh 50,000 valid application	API submission + customer confirmation + manager review	Passed
KSh 100,000 valid application	API submission + customer confirmation + manager review	Passed
Invalid application	Stop before API + validation notification	Passed
API failure	No false success + technical notification	Passed
Tools & Technologies
Tool	Purpose
n8n	Workflow orchestration
Google Forms	Customer application intake
Google Sheets	Application data storage
REST API	External system integration
HTTP POST	Application submission
Gmail	Automated notifications
JSON	Data exchanged with the API
What This Project Demonstrates

This project demonstrates practical ability to:

Translate business requirements into automation logic
Build multi-step n8n workflows
Work with triggers and conditional branches
Transform and validate data
Implement business rules
Integrate with REST APIs
Work with JSON request bodies and API responses
Interpret HTTP status codes
Handle success and failure paths
Automate customer communication
Automate internal business notifications
Test workflows using realistic business scenarios
Project Documentation

For the complete technical breakdown, see:

Project Documentation

Additional Evidence
Customer Intake Form

Application Data

Important Project Note

This is a simulated client project created to demonstrate practical business automation skills.

JSONPlaceholder was used as a simulated loan-management API for testing.

It is not a production integration with an actual Mwangaza Microfinance system.

In a real deployment, the test API would be replaced with the client's authenticated loan-management API and production security, monitoring and operational requirements would be implemented.

Future Improvements

A production version could include:

Authenticated API integration
Duplicate application detection
Unique application IDs
Database logging
Retry mechanisms
Advanced validation
Monitoring and alerting
Audit trails
Production deployment
AI-assisted document processing and application summaries
Project Status

Completed — Core MVP

The workflow has been built, tested across successful and failure scenarios, documented and prepared as a portfolio project.
