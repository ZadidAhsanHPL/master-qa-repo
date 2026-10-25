**ACCEPTANCE CRITERIA**

**Project: Forecast Upload**

Version 2.0 | Full Features, User Stories, Use Cases & AC | 25 October 2026

| **Document Type**   | Acceptance Criteria – Full Specification |
| ------------------- | ---------------------------------------- |
| **Based On**        | Initiation Meeting Requirements          |
| **Target Deadline** | 15 November 2026                         |
| **Status**          | Draft – Awaiting final validation rules  |

**1\. Introduction**

This document provides the complete Acceptance Criteria for the Forecast Upload application. It covers every feature, user story (in Given-When-Then format), use case, and detailed acceptance criteria derived from the initiation meeting.

A feature is considered "Done" only when all related user stories and acceptance criteria are fully met and verified.

**2\. System Overview**

Forecast Upload is a web application that allows ~20–25 users to upload product forecast Excel files. The system validates the file against business rules and, if valid, posts the data to SAP via API. Partial uploads are not allowed.

• Primary users: Business users who prepare monthly/quarterly forecasts

• Key outcome: Eliminate manual Excel → email → SAP upload process

• Success metric: Users can self-upload validated forecasts by December 2026

**3\. Actors**

• Uploader (Business User) – Uploads Excel, views data, fixes errors, submits

• System – Validates data, displays errors, calls SAP API, logs actions

• SAP System – Receives validated forecast data via API

• Admin / Support (optional) – Views logs, supports users

**4\. Feature 1: Authentication & Session Management**

**Feature F1 – User Login & Session**

Description: Secure access to the application for authorised users only.

**User Story US-1.1 – Successful Login**

**Given:** the user is on the login page and has valid credentials

**When:** the user enters username and password and clicks Login

**Then:** the system authenticates the user and redirects to the main upload screen

**Acceptance Criteria**

**AC-1.1.1** Login page is accessible without authentication.

**AC-1.1.2** Valid credentials allow successful login.

**AC-1.1.3** After login, user is redirected to the Excel Upload screen.

**AC-1.1.4** User's name or identity is visible after login.

**User Story US-1.2 – Invalid Login**

**Given:** the user is on the login page

**When:** the user enters invalid credentials and clicks Login

**Then:** the system displays a clear error message and does not grant access

**Acceptance Criteria**

**AC-1.2.1** Invalid credentials show a clear, non-technical error message.

**AC-1.2.2** No indication is given whether username or password was wrong (security).

**AC-1.2.3** User remains on the login page.

**User Story US-1.3 – Logout**

**Given:** the user is logged in

**When:** the user clicks Logout

**Then:** the session is terminated and the user is redirected to the login page

**Acceptance Criteria**

**AC-1.3.1** Logout ends the session immediately.

**AC-1.3.2** After logout, protected pages are no longer accessible.

**User Story US-1.4 – Session Timeout**

**Given:** the user is logged in and has been inactive beyond the session timeout period

**When:** the user attempts any action

**Then:** the system ends the session and redirects to the login page with a timeout message

**Acceptance Criteria**

**AC-1.4.1** Session expires after a defined inactivity period.

**AC-1.4.2** User is informed that the session has expired.

**Use Cases – Authentication**

• UC-1: Successful login with valid credentials

• UC-2: Failed login with invalid credentials

• UC-3: Logout from any screen

• UC-4: Automatic session expiry due to inactivity

• UC-5: Direct URL access to protected page while unauthenticated → redirect to login

**5\. Feature 2: Excel File Upload**

**Feature F2 – Excel Upload**

Description: Users can upload an Excel file (.xlsx / .xls) containing forecast data.

**User Story US-2.1 – Successful File Upload**

**Given:** the user is logged in and on the upload screen

**When:** the user selects or drags-and-drops a valid Excel file and uploads it

**Then:** the system accepts the file, parses it, and displays the data in a preview table

**Acceptance Criteria**

**AC-2.1.1** User can select a file via file browser or drag-and-drop.

**AC-2.1.2** Only .xlsx and .xls files are accepted.

**AC-2.1.3** After upload, data is parsed and shown in a table matching Excel structure.

**AC-2.1.4** Loading indicator is shown during file parsing.

**User Story US-2.2 – Reject Invalid File Type**

**Given:** the user is on the upload screen

**When:** the user tries to upload a non-Excel file (e.g. .pdf, .csv, .docx)

**Then:** the system rejects the file and shows a clear error message

**Acceptance Criteria**

**AC-2.2.1** Non-Excel files are rejected immediately.

**AC-2.2.2** Error message clearly states that only Excel files are allowed.

**User Story US-2.3 – Reject Empty or Oversized File**

**Given:** the user is on the upload screen

**When:** the user uploads an empty file or a file exceeding the size limit

**Then:** the system rejects the file with an appropriate message

**Acceptance Criteria**

**AC-2.3.1** Empty files are rejected.

**AC-2.3.2** Files exceeding the defined size limit are rejected.

**AC-2.3.3** Clear error messages are shown for both cases.

**Use Cases – Excel Upload**

• UC-6: Upload valid Excel file → data preview shown

• UC-7: Attempt to upload non-Excel file → rejected

• UC-8: Upload empty file → rejected

• UC-9: Upload oversized file → rejected

• UC-10: Upload Excel with wrong structure → proceeds to validation (errors shown later)

**6\. Feature 3: Data Preview & Quantity Editing**

**Feature F3 – Preview & Edit**

Description: After upload, users can view the data and optionally edit Quantity values before submission.

**User Story US-3.1 – View Uploaded Data**

**Given:** the user has successfully uploaded a valid Excel file

**When:** the system finishes parsing the file

**Then:** the data is displayed in a table with the same columns and order as the Excel file

**Acceptance Criteria**

**AC-3.1.1** Preview table shows all rows and columns from the Excel.

**AC-3.1.2** Column order and headers match the uploaded file.

**AC-3.1.3** Material/Product Code and all month columns are visible.

**User Story US-3.2 – Edit Quantity**

**Given:** the uploaded data is displayed in the preview table

**When:** the user changes a Quantity value in an editable cell

**Then:** the new value is accepted in the UI and will be used during final validation and submission

**Acceptance Criteria**

**AC-3.2.1** Quantity cells are editable.

**AC-3.2.2** Material and Month Name columns are not editable.

**AC-3.2.3** Edited values are retained until submission or new upload.

**AC-3.2.4** User can edit multiple quantity values before submitting.

**Use Cases – Preview & Edit**

• UC-11: View full uploaded data in table format

• UC-12: Edit one or more Quantity values

• UC-13: Attempt to edit non-quantity fields → not allowed

**7\. Feature 4: Validation Engine**

**Feature F4 – Business Validation Rules**

Description: System validates the entire file against all business rules. Any error causes full rejection (all-or-nothing).

**User Story US-4.1 – Structure Validation**

**Given:** an Excel file has been uploaded

**When:** the system runs structure validation

**Then:** the system verifies exact expected columns and headers; any mismatch is reported as an error

**Acceptance Criteria**

**AC-4.1.1** System checks for the exact expected number of columns.

**AC-4.1.2** System validates column header names.

**AC-4.1.3** Missing or extra columns cause validation failure.

**User Story US-4.2 – Material / Product Code Validation**

**Given:** an Excel file has been uploaded

**When:** the system validates the Material/Product Code column

**Then:** every row must have a valid Material/Product Code; missing or invalid codes are reported with row numbers

**Acceptance Criteria**

**AC-4.2.1** Material/Product Code is mandatory for every row.

**AC-4.2.2** Format rules (to be confirmed) are enforced.

**AC-4.2.3** Errors reference exact row numbers.

**User Story US-4.3 – Month Name Validation**

**Given:** an Excel file has been uploaded

**When:** the system validates month name columns

**Then:** only exact short forms (e.g. Jan, Feb) are accepted; any variation is reported as an error with row number

**Acceptance Criteria**

**AC-4.3.1** Month names must match exactly the expected short forms (Jan, Feb, Mar, etc.).

**AC-4.3.2** Full names (January), misspellings (Jann), or wrong abbreviations are rejected.

**AC-4.3.3** Error message includes row number and the incorrect value.

**User Story US-4.4 – Year / Month Combination Validation**

**Given:** an Excel file has been uploaded and contains year-month data

**When:** the system validates year-month combinations against current date rules

**Then:** illogical combinations (e.g. forecasting Jan 2028 while current year is 2026) are rejected with clear errors

**Acceptance Criteria**

**AC-4.4.1** Year-month combination must be logical relative to the current date.

**AC-4.4.2** Final detailed rules will be provided by business and must be implemented.

**AC-4.4.3** Errors clearly explain why the year-month is invalid.

**User Story US-4.5 – Quantity Validation**

**Given:** an Excel file has been uploaded (or quantities edited)

**When:** the system validates Quantity values

**Then:** only integer values are accepted; decimals or non-numeric values are rejected with row numbers

**Acceptance Criteria**

**AC-4.5.1** Quantity must be an integer (whole number).

**AC-4.5.2** Decimal or fractional values (e.g. 100.5) are rejected.

**AC-4.5.3** Error message shows exact row number and invalid value.

**User Story US-4.6 – All-or-Nothing Validation**

**Given:** validation has been run on the uploaded (and possibly edited) data

**When:** one or more validation errors exist

**Then:** the entire file is rejected; no data is sent to SAP; all errors are displayed together

**Acceptance Criteria**

**AC-4.6.1** If any error exists (even in one row), the whole file is rejected.

**AC-4.6.2** No partial data is ever sent to SAP.

**AC-4.6.3** All errors are collected and shown in a single list.

**AC-4.6.4** Each error includes: Row number + Field + Specific message.

**User Story US-4.7 – Display Validation Errors**

**Given:** validation has failed

**When:** the system presents the results to the user

**Then:** a clear, structured list of all errors is shown so the user can fix the Excel and re-upload

**Acceptance Criteria**

**AC-4.7.1** Errors are displayed in a readable list or table.

**AC-4.7.2** Format example: "Row 23 – Month name is incorrect".

**AC-4.7.3** User can easily identify problems and correct the source Excel.

**Use Cases – Validation**

• UC-14: Valid file with correct structure, months, years, and integer quantities → passes

• UC-15: File with misspelled month name → rejected with row-level error

• UC-16: File with decimal quantity → rejected with row-level error

• UC-17: File with future year beyond allowed range → rejected

• UC-18: File with missing Material Code → rejected

• UC-19: File with multiple different errors → all errors listed, nothing posted

• UC-20: User edits quantity to invalid value then submits → validation catches it

**8\. Feature 5: Submit & SAP Integration**

**Feature F5 – Submit to SAP**

Description: After successful validation, user submits data; system calls SAP API.

**User Story US-5.1 – Successful Submit**

**Given:** the uploaded data has passed all validations

**When:** the user clicks Submit and confirms

**Then:** the system calls the SAP API with the clean data and shows a success confirmation

**Acceptance Criteria**

**AC-5.1.1** Submit button is available only after data is previewed.

**AC-5.1.2** User must confirm before final submission.

**AC-5.1.3** On success, a clear success message/report is shown.

**AC-5.1.4** Success report includes date/time, number of rows, and user.

**User Story US-5.2 – Submit with Validation Errors**

**Given:** the data contains one or more validation errors

**When:** the user clicks Submit

**Then:** the system blocks submission, shows all errors, and does not call the SAP API

**Acceptance Criteria**

**AC-5.2.1** Submit is blocked when validation fails.

**AC-5.2.2** All current errors are displayed.

**AC-5.2.3** No API call is made to SAP.

**User Story US-5.3 – SAP API Failure**

**Given:** validation has passed and the system calls the SAP API

**When:** the SAP API returns an error or is unavailable

**Then:** the system shows a clear failure message, keeps the data available for retry, and logs the error

**Acceptance Criteria**

**AC-5.3.1** API failure shows a user-friendly error message.

**AC-5.3.2** Uploaded data remains available so the user can retry.

**AC-5.3.3** Failure is logged with sufficient technical detail.

**User Story US-5.4 – Prevent Double Submission**

**Given:** a successful submission has just been completed

**When:** the user attempts to submit the same data again

**Then:** the system prevents duplicate submission

**Acceptance Criteria**

**AC-5.4.1** After successful post, the same data cannot be re-submitted without a new upload.

**AC-5.4.2** Submit button is disabled or protected after success.

**Use Cases – Submit & SAP**

• UC-21: Valid data → Submit → SAP API success → success report

• UC-22: Invalid data → Submit → blocked with errors

• UC-23: Valid data → SAP API fails → error shown, data retained for retry

• UC-24: Attempt to re-submit already successfully posted data → prevented

**9\. Feature 6: Success Report**

**Feature F6 – Post-Upload Confirmation**

Description: After successful SAP posting, user sees a clear confirmation.

**User Story US-6.1 – View Success Report**

**Given:** the SAP API call has succeeded

**When:** the system navigates to the success screen

**Then:** a confirmation report is displayed showing that the forecast was successfully posted

**Acceptance Criteria**

**AC-6.1.1** Success screen clearly states that data has been posted to SAP.

**AC-6.1.2** Report shows upload date/time, number of rows, and user name.

**AC-6.1.3** User can start a new upload from the success screen.

**Use Cases – Success Report**

• UC-25: Successful post → success report displayed

• UC-26: User starts new upload after viewing success report

**10\. Feature 7: Logging & Audit Trail**

**Feature F7 – Logging**

Description: All important actions are logged for support and audit purposes.

**User Story US-7.1 – Log Upload Attempts**

**Given:** a user uploads a file (success or failure)

**When:** the system processes the upload

**Then:** the attempt is logged with timestamp, user, file name, and result

**Acceptance Criteria**

**AC-7.1.1** Every upload attempt is logged.

**AC-7.1.2** Validation failures are logged with error summary.

**AC-7.1.3** Successful posts record who, when, file name, and row count.

**AC-7.1.4** SAP API calls and responses are logged.

**Use Cases – Logging**

• UC-27: Successful upload and post → full audit record created

• UC-28: Failed validation → logged with error details

• UC-29: SAP API failure → logged for technical investigation

**11\. Feature 8: Non-Functional Requirements**

**Feature F8 – Quality Attributes**

**User Story US-8.1 – Usability & Feedback**

**Given:** the user is performing any operation (upload, validate, submit)

**When:** the system is processing

**Then:** appropriate loading indicators and clear success/error messages are shown

**Acceptance Criteria**

**AC-8.1.1** Loading indicators appear during parse, validation, and API calls.

**AC-8.1.2** All messages are clear and user-friendly.

**AC-8.1.3** Application works on standard desktop browsers.

**User Story US-8.2 – Performance**

**Given:** a user uploads an Excel file containing 100–500 rows

**When:** the system processes the file

**Then:** the operation completes without significant delay

**Acceptance Criteria**

**AC-8.2.1** System handles files with at least 100–500 rows acceptably.

**User Story US-8.3 – Security**

**Given:** an unauthenticated user tries to access any protected function

**When:** the request is made

**Then:** access is denied and the user is redirected to login

**Acceptance Criteria**

**AC-8.3.1** Only authenticated users can upload files.

**AC-8.3.2** No sensitive data is exposed in error messages beyond necessity.

**User Story US-8.4 – No Manual Entry**

**Given:** the user is on the application

**When:** the user looks for a way to enter data manually

**Then:** no manual data entry option exists; only Excel upload is available

**Acceptance Criteria**

**AC-8.4.1** There is no manual data entry screen or form.

**AC-8.4.2** Only Excel file upload is supported.

**12\. Out of Scope (Explicitly Confirmed)**

• Viewing existing forecasts already stored in SAP

• Packing / material availability / three-month rolling reports

• Partial upload of only valid rows

• Manual data entry screens

• Admin UI for managing validation rules (future consideration)

**13\. Summary of Features & User Stories**

F1 Authentication: US-1.1 to US-1.4

F2 Excel Upload: US-2.1 to US-2.3

F3 Preview & Edit: US-3.1 to US-3.2

F4 Validation Engine: US-4.1 to US-4.7

F5 Submit & SAP: US-5.1 to US-5.4

F6 Success Report: US-6.1

F7 Logging & Audit: US-7.1

F8 Non-Functional: US-8.1 to US-8.4

**14\. Notes**

• Final detailed validation rules (especially year logic and Material Code format) will be provided by the business and must be incorporated before UAT.

• All acceptance criteria must be met for a feature to be considered "Done".

• Any change to these criteria requires formal agreement from stakeholders.

**15\. Approval & Signatories**

By signing below, the undersigned confirm that they have reviewed and agree with the Acceptance Criteria defined in this document.

| \_**\_**\_**\_**\_**\_**\_**\_**\_**\_**<br><br>_Signature_<br><br>**Md. Rashed Karim**<br><br>DGM, IT<br><br>Date: \_**\_**\_**\_**\___    | \_**\_**\_**\_**\_**\_**\_**\_**\_**\_**<br><br>_Signature_<br><br>**Md. Najim Uddin**<br><br>Manager, IT<br><br>Date: \_**\_**\_**\_**\___             |
| ------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
|                                                                                                                                             |                                                                                                                                                         |
| \_**\_**\_**\_**\_**\_**\_**\_**\_**\_**<br><br>_Signature_<br><br>**Md. Khaled Imam**<br><br>Manager, IT<br><br>Date: \_**\_**\_**\_**\___ | \_**\_**\_**\_**\_**\_**\_**\_**\_**\_**<br><br>_Signature_<br><br>**Ainul Hossain Tushar**<br><br>Deputy Manager, IT<br><br>Date: \_**\_**\_**\_**\___ |

_— End of Document —_