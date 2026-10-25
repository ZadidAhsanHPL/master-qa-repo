**ACCEPTANCE CRITERIA**

**Project: Forecast Upload**

Version 1.0 | 19 October 2026

| **Document Type**   | Acceptance Criteria                                   |
| ------------------- | ----------------------------------------------------- |
| **Based On**        | Initiation Meeting Requirements     |
| **Target Deadline** | 15 November 2026 (Training in November)               |
| **Status**          | Draft – Awaiting final validation rules from business |

**1\. Introduction**

This document defines the Acceptance Criteria for the Forecast Upload application. A feature is considered "Done" only when all related criteria listed below are fully met and verified.

The application allows users to upload product forecast Excel files, validates the data against defined business rules, and posts valid data to SAP via an API. Partial uploads are not allowed.

**2\. Authentication & Login**

**AC-2.1 Login Screen**

• User can access a dedicated login page.

• User can successfully log in with valid credentials.

• Invalid credentials display a clear, user-friendly error message.

• After successful login, the user is redirected to the main upload screen.

• Logout functionality is available and securely ends the session.

**AC-2.2 Session Management**

• Session expires after a defined period of inactivity.

• Unauthenticated users cannot access any protected pages or features.

**3\. Excel Upload**

**AC-3.1 Upload Interface**

• User can select a file or drag-and-drop an Excel file (.xlsx / .xls).

• System rejects non-Excel files with a clear error message.

• System rejects empty files or files exceeding the defined size limit.

• After a successful upload, the system parses the file and displays the data.

**AC-3.2 File Parsing**

• System correctly reads the fixed set of columns (Material/Product Code + month columns).

• System preserves the original structure and order of the uploaded Excel.

**4\. Data Preview & Editing**

**AC-4.1 Data Visualization**

• After successful upload, the system displays the Excel data in a table.

• The table uses the same structure and column order as the original Excel file.

• All rows and columns are clearly visible and readable.

**AC-4.2 Quantity Editing**

• User can edit Quantity values directly in the preview table.

• Other columns (Material, Month names, etc.) remain non-editable.

• Any changes made in the UI are reflected before final submission.

**5\. Validation Rules**

**AC-5.1 Structure Validation**

• System validates that the Excel contains the exact expected number of columns.

• System validates that column headers match the expected names.

• Missing or extra columns cause the entire file to fail validation.

**AC-5.2 Material / Product Code Validation**

• Material/Product Code is mandatory for every row.

• Format rules (to be confirmed by business) are enforced.

**AC-5.3 Month Name Validation**

• Month names must match exactly the expected short forms (e.g., "Jan", "Feb").

• Full names ("January"), misspellings ("Jann"), or incorrect abbreviations are rejected.

• Error message clearly indicates the row number and the incorrect month name.

**AC-5.4 Year / Month Combination Validation**

• System validates that the year-month combination is logical relative to the current date.

• Example: Uploading a forecast for January 2028 while the current year is 2026 is rejected.

• Final detailed year rules will be provided by the business and must be implemented.

**AC-5.5 Quantity Validation**

• Quantity must be an integer (whole number only).

• Decimal or fractional values (e.g., 100.5) are rejected.

• Error message indicates the exact row number and the invalid quantity.

**AC-5.6 All-or-Nothing Rule**

• If any validation error exists in the file (even in a single row), the entire file is rejected.

• No partial data is ever sent to SAP.

• All errors are collected and displayed together in one view.

**AC-5.7 Error Display**

• System shows a clear, structured list of all validation errors.

• Each error includes: Row number + Column/Field + Specific error message.

• Example format: "Row 23 – Month name is incorrect".

• User can easily identify and fix the problems in the original Excel file.

**6\. Submit Process**

**AC-6.1 Submit Action**

• A clear "Submit" button is available after the data is previewed.

• User must explicitly confirm before final submission.

• Submit button is disabled while the system is processing.

**AC-6.2 Validation on Submit**

• On Submit, the system re-runs all validation rules on the current data (including any edited quantities).

• Only fully valid data proceeds to the SAP API call.

**7\. SAP Integration**

**AC-7.1 API Call**

• When all validations pass, the system calls the provided SAP API.

• Correct and complete data is sent to the designated location in SAP.

**AC-7.2 Success Handling**

• On successful API response, the system displays a clear success confirmation.

• User is informed that the forecast data has been successfully posted to SAP.

**AC-7.3 Failure Handling**

• If the SAP API fails, the system shows a clear, actionable error message.

• The uploaded data remains available so the user can retry.

• The failure is logged with sufficient detail for technical investigation.

**AC-7.4 Double Submission Prevention**

• After a successful post, the same data cannot be submitted again without a fresh upload.

**8\. Success Report**

**AC-8.1 Confirmation Screen**

• After successful submission, a dedicated success screen/report is shown.

• The report clearly states that the upload was successful and data has been posted to SAP.

• Basic information is displayed: upload date/time, number of rows, and user name.

**9\. Logging & Audit**

**AC-9.1 Application Logging**

• Every upload attempt is logged (both success and failure).

• Validation failures, API calls, and API responses are logged with sufficient context.

**AC-9.2 Audit Trail**

• Successful posts record: Who uploaded, when, file name, and number of rows.

• Logs are accessible for support and troubleshooting purposes.

**10\. General / Non-Functional Criteria**

**AC-10.1 User Experience**

• Loading indicators are shown during file parsing, validation, and API calls.

• Error and success messages are clear and user-friendly.

• The application works reliably on standard desktop browsers.

**AC-10.2 Security**

• Only authenticated users can upload files.

• No sensitive data is exposed in error messages or logs beyond what is necessary.

**AC-10.3 Performance**

• The system can handle Excel files containing at least 100–500 rows without significant delay.

**AC-10.4 No Manual Entry**

• There is no option for manual data entry. Only Excel upload is supported.

**11\. Out of Scope (Explicitly Confirmed)**

• Viewing existing forecasts already stored in SAP.

• Packing / material availability / three-month rolling reports.

• Partial upload of only valid rows.

• Manual data entry screens.

**12\. Notes**

• Final detailed validation rules (especially year logic and exact Material Code rules) will be provided by the business and must be incorporated before UAT.

• All acceptance criteria listed in this document must be met for a feature to be considered "Done".

• Any change to these criteria requires formal agreement from the project stakeholders.

**13\. Approval & Signatories**

By signing below, the undersigned confirm that they have reviewed and agree with the Acceptance Criteria defined in this document.

| \_**\_**\_**\_**\_**\_**\_**\_**\_**\_**<br><br>_Signature_<br><br>**Md. Rashed Karim**<br><br>DGM, IT<br><br>Date: \_**\_**\_**\_**\___    | \_**\_**\_**\_**\_**\_**\_**\_**\_**\_**<br><br>_Signature_<br><br>**Md. Najim Uddin**<br><br>Manager, IT<br><br>Date: \_**\_**\_**\_**\___             |
| ------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
|                                                                                                                                             |                                                                                                                                                         |
| \_**\_**\_**\_**\_**\_**\_**\_**\_**\_**<br><br>_Signature_<br><br>**Md. Khaled Imam**<br><br>Manager, IT<br><br>Date: \_**\_**\_**\_**\___ | \_**\_**\_**\_**\_**\_**\_**\_**\_**\_**<br><br>_Signature_<br><br>**Ainul Hossain Tushar**<br><br>Deputy Manager, IT<br><br>Date: \_**\_**\_**\_**\___ |

_— End of Document —_