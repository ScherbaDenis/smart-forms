# Requirements

Project tracking: [Smart Forms Project Board](https://github.com/users/ScherbaDenis/projects/4)

## 1. Overview

Smart Forms is a web application for creating, managing, and analyzing digital forms. It allows users to design structured questionnaires, publish them for responses, review submissions, and export results for downstream analysis.

The product is designed for teams and organizations that need to collect structured data without writing custom code or using fragmented tools such as spreadsheets or static forms.

## 2. Product goals

- Allow users to create forms quickly and without technical knowledge
- Support common form field types used in business workflows
- Make form publishing simple and predictable
- Collect responses in a safe and structured way
- Provide a dashboard for review and reporting
- Support CSV export for data analysis and reporting
- Keep the architecture maintainable and extensible for future features

## 3. Target users

### 3.1 Authenticated user
An authenticated user is the main actor in the system. They can create, update, publish, and manage their own forms.

### 3.2 Respondent
A respondent is an external user who fills out a published form and submits answers.

### 3.3 Administrator
An administrator manages platform-level operations, configuration, and monitoring. This role can be added later and is not required for the MVP.

## 4. Core user scenarios

### Scenario 1: Create a form
A user signs in, creates a new form, adds fields, configures validation, and saves the draft.

### Scenario 2: Publish a form
A user finalizes the form, validates the configuration, and publishes it to make it available for responses.

### Scenario 3: Submit data
A respondent opens the public link, fills out the form, and submits responses.

### Scenario 4: Review submissions
The form owner opens the submissions view and reviews all submitted responses.

### Scenario 5: Export results
The form owner exports the collected data as CSV for analysis or external processing.

## 5. Functional requirements

### 5.1 Authentication and user access
- FR-01: A user can register with an email and password.
- FR-02: A registered user can sign in using their credentials.
- FR-03: An authenticated user can sign out.
- FR-04: A user can access only their own forms and their own submissions.
- FR-05: A user cannot modify or view another user's forms without explicit ownership.

### 5.2 Form management
- FR-06: An authenticated user can create a new form.
- FR-07: A user can edit an existing form they own.
- FR-08: A user can delete a form they own.
- FR-09: A user can view the list of their forms in a dashboard.
- FR-10: A user can view a single form with its metadata and fields.
- FR-11: A user can duplicate a form if this feature is needed in the future, but this is not required for the MVP.

### 5.3 Form design and fields
- FR-12: A user can add a text field.
- FR-13: A user can add an email field.
- FR-14: A user can add a number field.
- FR-15: A user can add a date field.
- FR-16: A user can add a select field with multiple options.
- FR-17: A user can add a checkbox field.
- FR-18: A user can add a file upload field.
- FR-19: A user can add a textarea field.
- FR-20: A user can mark a field as required.
- FR-21: A user can rename a field.
- FR-22: A user can reorder form fields.
- FR-23: A user can remove a field from a form.
- FR-24: A user can define field validation rules for supported field types.
- FR-25: A user can preview the form before publishing.

### 5.4 Form publishing and access
- FR-26: A user can publish a form when the form passes validation.
- FR-27: A published form receives a public URL for respondents.
- FR-28: A user can unpublish a form if needed.
- FR-29: A form can be public or private depending on product configuration.
- FR-30: A form cannot be published without valid fields and a valid configuration.

### 5.5 Submission handling
- FR-31: A respondent can open a published form using a public URL.
- FR-32: A respondent can submit answers to the form.
- FR-33: A submission must validate against the field rules of the form.
- FR-34: A submission is stored securely and associated with the correct form.
- FR-35: A user can view all submissions for a form they own.
- FR-36: A user can inspect each submission individually.

### 5.6 Dashboard and reporting
- FR-37: A user can view a dashboard with all their forms.
- FR-38: A user can see total form count.
- FR-39: A user can see total submission count.
- FR-40: A user can see recent submissions or recent activity.
- FR-41: A user can filter form submissions by date or form.
- FR-42: A user can view basic summary statistics for a form.

### 5.7 Export
- FR-43: A user can export form submissions as CSV.
- FR-44: Exported data must include submission metadata and field values.
- FR-45: CSV export should maintain a predictable column format for each form.

## 6. Business rules

- BR-01: Only an authenticated user can create or manage a form.
- BR-02: Only the owner of a form can edit, publish, or delete it.
- BR-03: A form must contain at least one valid field before publication.
- BR-04: Required fields cannot be empty during submission.
- BR-05: A field value must respect its configured type and validation constraints.
- BR-06: A submission is rejected if required values are missing or invalid.
- BR-07: Form field names must be valid and unique within a form.
- BR-08: A respondent must not be able to access internal dashboard or management screens.
- BR-09: The form owner should be able to inspect all responses tied to their form.
- BR-10: Public form URLs must not expose admin or internal data.

## 7. Non-functional requirements

### 7.1 Performance
- NFR-01: Basic dashboard and list views should load quickly for typical datasets.
- NFR-02: Form submission should complete within an acceptable response time for standard operations.
- NFR-03: CSV export should handle typical submission volumes without unacceptable delays.

### 7.2 Security
- NFR-04: All input must be validated on the server-side.
- NFR-05: Authentication and authorization must be enforced in the backend.
- NFR-06: Secrets must not be committed to source code.
- NFR-07: Public form submission must not allow arbitrary access to management APIs.
- NFR-08: File uploads must be validated and restricted to allowed types and safe use cases.

### 7.3 Reliability and quality
- NFR-09: Critical form and submission flows must be covered by automated tests.
- NFR-10: The system must support Docker-based local development.
- NFR-11: The system must be deployable to Azure using modern CI/CD practices.
- NFR-12: The backend must provide predictable error responses for validation and authorization failures.

### 7.4 Maintainability
- NFR-13: Code must be organized by layers and domain responsibilities.
- NFR-14: The architecture must remain simple enough for MVP delivery without over-engineering.
- NFR-15: The project should use CQRS and MediatR patterns to keep logic maintainable.

## 8. MVP scope

The MVP should include the following:

- user registration and sign in
- form creation and editing
- support for common field types
- validation rules for required fields
- form publishing
- public form link for respondents
- form submission storage
- submissions review page
- dashboard with form summary and response totals
- CSV export for submission data

## 9. Out of scope for MVP

The following items are intentionally excluded from the first version:

- AI-assisted form generation
- advanced analytics dashboards
- team collaboration and shared form ownership
- advanced role management
- payment or billing flow
- notification system
- real-time collaboration
- complex workflow automation
- integration with external CRM or ERP systems

## 10. Future roadmap candidates

These can be considered after the MVP is stable:

- multi-user team workspaces
- reusable templates library
- advanced conditional logic
- dashboard widgets and charts
- workflow automation
- webhooks and external integrations
- rich analytics and reporting
- multilingual forms
- advanced user permissions

## 11. Acceptance criteria examples

### Form creation
- Given a signed-in user, when they create a form with a valid name, then the form is saved and appears in the dashboard.

### Validation
- Given a required field is empty, when the form is submitted, then the API returns a validation error.

### Publishing
- Given a form contains valid fields, when the user publishes it, then the form is available through a public link.

### Submission
- Given a published form, when a respondent submits valid answers, then the data is stored and associated with the correct form.

### Export
- Given a form has submissions, when the user exports them, then a valid CSV file is generated with the expected structure.

## 12. Definition of done

A feature is considered complete when:

- the business requirement is implemented
- validation and authorization rules are enforced
- the logic is covered by relevant automated tests
- the API contract matches the frontend usage
- the feature works in the local environment
- documentation is updated if the behavior changes

## 13. Summary

This requirements document defines the Smart Forms MVP and establishes the baseline for implementation. It keeps the project focused on the essential value of the product: creating forms, collecting responses, reviewing submissions, and exporting data while staying simple enough to build and maintain efficiently.
