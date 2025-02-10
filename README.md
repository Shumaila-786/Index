# Mortgage Loan Eligibility Checker

## Overview
This project is a **Mortgage Loan Eligibility Checker**, which allows users to input financial details and check their mortgage loan eligibility. The frontend is a static website hosted on **AWS S3**, and the backend is an **AWS Lambda function** with a **Function URL**, which processes the user input and returns eligibility results. The eligibility data is stored in **AWS DynamoDB**.

## Features
- User-friendly form to input financial details.
- Secure communication with AWS Lambda for backend processing.
- CORS-enabled for cross-origin requests between S3 and Lambda.
- Mortgage eligibility results displayed dynamically.
- Data stored in DynamoDB for reference.

---

## **Architecture**
The project consists of the following AWS services:
1. **S3** – Hosts the static HTML, CSS, and JavaScript frontend.
2. **Lambda** – Processes user data and determines mortgage eligibility.
3. **Function URL** – Public URL for frontend-to-backend communication.
4. **DynamoDB** – Stores mortgage applications for reference.
5. **IAM Roles** – Grants permissions to Lambda for accessing DynamoDB.

---

## **Setup and Deployment**
### **1. Upload Frontend to S3**
1. Create an S3 bucket and enable **Static Website Hosting**.
2. Upload the provided `index.html` and associated files (`style.css`, `script.js`).
3. Update the bucket policy to allow public access.
4. Copy the **S3 static website URL** for testing.

### **2. Deploy Lambda Function**
1. Create a new AWS Lambda function (`MortgageCalculatorLambda`).
2. Set runtime as **Python 3.x**.
3. Attach an **IAM role** with **DynamoDB PutItem permissions**.
4. Enable **Function URL** and configure **CORS**.
5. Deploy the Lambda function with the mortgage eligibility logic.

### **3. Set Up DynamoDB Table**
1. Create a table named `MortgageApplications`.
2. Set **Primary Key** as `ApplicationID` (String).
3. Ensure Lambda has the required **permissions** to write to this table.

### **4. Connect Frontend to Backend**
1. Update `script.js` with your **Lambda Function URL**.
2. Ensure `CORS` headers are correctly set in Lambda responses.
3. Test the form submission and verify results.

---

## **Usage**
1. Open the **S3 website URL** in a browser.
2. Enter financial details and click **"Check Eligibility"**.
3. The frontend sends data to Lambda via the **Function URL**.
4. Lambda processes the request and stores data in **DynamoDB**.
5. Results are displayed dynamically on the page.

---

## **Code Breakdown**
### **Frontend (`index.html`)**
- A simple form collects user input (income, credit score, debt-to-income ratio, loan amount, property value).
- JavaScript (`script.js`) handles form submission and makes a **fetch request** to the Lambda Function URL.
- Results are displayed dynamically with conditional styling.

### **Backend (AWS Lambda)**
- The Lambda function receives JSON data from the frontend.
- It processes eligibility based on income, loan amount, and credit score.
- Results are returned as JSON and displayed on the webpage.
- Validated data is stored in **DynamoDB**.

---

## **Example API Request & Response**
### **Request (Sent by Frontend)**
```json
{
  "income": 50000,
  "credit_score": 700,
  "dti": 30,
  "loan_amount": 200000,
  "property_value": 250000
}
```
### **Response (Received from Lambda)**
```json
{
  "message": "Eligible for Mortgage",
  "details": [
    "Credit Score is sufficient.",
    "DTI ratio is within acceptable limits.",
    "Loan-to-Value ratio is reasonable."
  ]
}
```

---

## **CORS Configuration**
Since the frontend (S3) and backend (Lambda) are on different origins, **CORS headers** must be included in Lambda responses:
```json
"headers": {
  "Access-Control-Allow-Origin": "*",
  "Access-Control-Allow-Methods": "OPTIONS,POST",
  "Access-Control-Allow-Headers": "Content-Type"
}
```

---

## **Troubleshooting**
- **CORS Errors?** Ensure Lambda **Function URL CORS settings** allow requests from your S3 domain.
- **403 Forbidden in S3?** Check the bucket policy allows public read access.
- **Lambda Errors?** View logs in **AWS CloudWatch** for debugging.
- **Data not saving in DynamoDB?** Verify Lambda has `dynamodb:PutItem` permission.

---

## **Future Enhancements**
- Add **user authentication** to secure submissions.
- Implement a **loan interest rate calculator**.
- Store more user data (employment details, expenses, etc.).
- Deploy frontend using **CloudFront** for faster performance.

---

## **Contributing**
Feel free to fork this repository, submit PRs, or report issues. 🚀

---

## **License**
This project is licensed under the MIT License.

