MWANGAZA MICROFINANCE
LOAN APPLICATION AUTOMATION
Project Type
Simulated client project
Tools
n8n
Google Forms
Google Sheets
REST API
Gmail








1. PROJECT OVERVIEW
2. BUSINESS PROBLEM
3. BUSINESS REQUIREMENTS
4. SOLUTION
5. WORKFLOW ARCHITECTURE
6. HOW THE AUTOMATION WORKS
7. VALIDATION RULES
8. BUSINESS RULES
9. API INTEGRATION
10. ERROR HANDLING
11. TESTING
12. RESULTS
13. SCREENSHOTS
14. WHAT I LEARNED
15. FUTURE IMPROVEMENTS
16. PROJECT LIMITATIONS

1. PROJECT OVERVIEW 
Mwangaza Microfinance — Loan Application Automation is a simulated end-to-end business automation project built with n8n.
The automation receives loan applications submitted through an online form, prepares and validates the application data, applies business rules, submits valid applications to a simulated loan-management API, checks the API response, and sends automated notifications to customers and management.
The project was designed to demonstrate practical automation skills including workflow orchestration, data transformation, validation, business logic, REST API integration, response handling, email automation and error handling.

2. BUSINESS PROBLEM 
Mwangaza Microfinance receives loan applications through an online form.
The business needs to ensure that applications contain the required information before they are submitted to its loan-management system.
It also needs customers to receive confirmation after a successful submission and management to be alerted when applications require additional review.
Without automation, these steps could require manual checking, data entry, notifications and follow-up.
The goal of this automation is therefore to connect the application intake process with validation, the loan-management system and automated notifications.

3. BUSINESS REQUIREMENTS
The automation should:
1. Receive new loan applications.
2. Capture the customer's name, email address, phone number and requested loan amount.
3. Prepare and validate the application data.
4. Prevent incomplete or invalid applications from being submitted to the loan-management API.
5. Submit valid applications to the loan-management system.
6. Confirm that the API successfully created the application.
7. Send a confirmation email to the customer after successful API submission.
8. Notify management when the requested loan amount is KSh 50,000 or more.
9. Notify management when an application contains invalid information.
10. Handle API failures without sending a false success confirmation.

4. SOLUTION
The solution uses n8n as the automation and orchestration layer.

Google Forms provides the application interface and Google Sheets provides the intake/storage layer.

n8n receives new application records, prepares the data, validates the information and applies the business rules.

Valid applications are submitted through an HTTP POST request to a simulated loan-management API.

The API response is then evaluated before the workflow determines whether customer and management notifications should be sent.









5. WORKFLOW ARCHITECTURE 
Customer
   ↓
Google Form
   ↓
Google Sheets
   ↓
n8n Google Sheets Trigger
   ↓
Prepare Loan Data
   ↓
Validate Loan Application
   ├── Invalid → Validation Notification → Manager

   │

   └── Valid
         ↓
   Submit Loan Application to API
         ↓
   API Submission Successful?
         ├── Failure → API Error Notification → Manager

         │

         └── Success
               ↓
        Customer Confirmation
               ↓
        Loan Requires Manager Review?
               ├── No → End

               │

               └── Yes → Manager Review Notification


6. HOW THE AUTOMATION WORKS 

1. A customer submits a loan application through Google Forms.

2. The response is recorded in Google Sheets.

3. The Google Sheets Trigger starts the n8n workflow when a new application is received.

4. The Prepare Loan Data step maps and prepares the application information for use throughout the workflow.

5. The Validate Loan Application step checks whether the required application information is valid.

6. Invalid applications are stopped before reaching the loan-management API and management is notified.

7. Valid applications are sent to the loan-management API using an HTTP POST request.

8. The API response is evaluated.

9. A successful API response results in a confirmation email being sent to the customer.

10. The workflow then checks the requested loan amount.

11. Applications requesting KSh 50,000 or more trigger a management review notification.

12. API failures trigger a technical error notification instead of a customer success confirmation.


7. VALIDATION RULES 
The workflow validates the application before API submission.

Validation includes:

• Customer name is present.
• Email address is present and follows the expected email structure.
• Phone number is present and follows the defined phone-number validation.
• Loan amount is present and greater than zero.

Applications that fail validation do not proceed to the loan-management API.

8. BUSINESS RULES
Management Review Threshold

Loan applications requesting KSh 50,000 or more require management review.

The threshold is inclusive:

KSh 49,999 → No management review notification.

KSh 50,000 → Management review notification.

KSh 100,000 → Management review notification.

9. API INTEGRATION
The workflow uses n8n's HTTP Request node to communicate with an external REST API.
The application is sent using an HTTP POST request.
The request body contains the relevant loan application information, including:
• Customer name
• Email address
• Phone number
• Loan amount
The workflow checks the HTTP response returned by the API.
A HTTP status code of 201 Created is treated as a successful application creation.
The workflow does not send the customer a success confirmation simply because the HTTP Request node executed.
The API response must indicate successful creation before the customer confirmation is sent.


10. ERROR HANDLING 
The workflow handles two major categories of failure.

1. Validation Failure
If customer information is incomplete or invalid, the application is stopped before API submission.
Management receives a notification containing the relevant validation issue.

2. API/Technical Failure
If the application passes validation but the loan-management API does not return the expected successful response, the workflow does not send a customer success confirmation.
Instead, management receives a technical error notification so that the issue can be investigated.
This prevents customers from receiving a false confirmation when the application has not actually been created successfully.



11. TESTING 

Test
Expected result
Result
Valid application — KSh 40,000
API success + customer email, no manager review
Passed
Valid application — KSh 50,000
API success + customer email + manager review
Passed
Valid application — KSh 100,000
API success + customer email + manager review
Passed
Invalid application
Stop before API + manager notification
Passed
Deliberate API failure
No customer success email + technical notification




The API failure scenario was deliberately introduced by changing the test API endpoint to an invalid endpoint.

The resulting unsuccessful API response was handled by the workflow and routed to the technical error notification path.

The API endpoint was restored after testing.


12. RESULTS 
The completed workflow successfully demonstrated an automated loan application process from customer intake through API submission and notification.

The workflow was able to:

• Receive application data automatically.
• Validate application information.
• Stop invalid applications.
• Submit valid applications through a REST API.
• Detect successful API creation.
• Send automated customer confirmation emails.
• Apply a KSh 50,000 management-review threshold.
• Notify management of applications requiring review.
• Handle API failures without generating false customer confirmations.


13. SCREENSHOTS 
The following screenshots provide evidence of the completed workflow and its test scenarios.

1. Complete n8n workflow
2. Successful API response
3. Customer confirmation email
4. Management review email
5. Validation failure notification
6. API failure and technical error notification
7. Google Form
8. Google Sheets intake
7. API failure email


14. WHAT I LEARNED 
This project helped me understand how business automation differs from simply connecting two applications.

I learned how to:

• Translate business requirements into workflow logic.
• Separate data transformation, validation and business rules.
• Work with data moving between different systems.
• Use n8n's HTTP Request node to communicate with REST APIs.
• Understand GET and POST requests.
• Work with request bodies and API responses.
• Use HTTP status codes to determine whether an API operation succeeded.
• Build conditional paths using IF nodes.
• Handle both business-data failures and technical failures.
• Test both successful and unsuccessful scenarios.
• Design an automation around a business process rather than individual n8n nodes.


15. FUTURE IMPROVEMENTS 
Potential production improvements include:

• Replace the simulated API with a real loan-management API.
• Add API authentication.
• Add duplicate application detection.
• Generate and track unique application IDs.
• Add persistent application logging.
• Add retry logic for temporary API failures.
• Add more advanced validation.
• Add monitoring and alerting.
• Add authentication and access controls.
• Deploy the workflow in a production environment.
• Add AI-assisted application summaries or document processing where appropriate.

16. PROJECT LIMITATIONS
This is a simulated client project created for portfolio and learning purposes.

The loan-management API used during development was JSONPlaceholder, a public testing API.

Therefore, the project demonstrates the architecture and automation logic of a real loan-processing integration but does not represent a production integration with an actual Mwangaza Microfinance loan-management system.

In a production implementation, the simulated API would be replaced with the client's authenticated loan-management API.




