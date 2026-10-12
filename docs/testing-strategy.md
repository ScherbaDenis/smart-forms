# Testing Strategy

This document describes the testing approach for the Smart Forms project.

## 1. Goal of testing

The goal is to ensure that the application is reliable, maintainable, and safe to evolve. For this project, testing should focus on the most important business flows:

- form creation and editing
- validation rules
- submission handling
- dashboard and list queries
- export features
- authentication and user access

The project should use a practical testing pyramid: many unit tests, fewer integration tests, and a small number of end-to-end tests.

## 2. Testing pyramid

The recommended balance is:

- Unit tests: majority of tests
- Integration tests: focused on API and database behavior
- End-to-end tests: key user journeys only

This is the best balance for a product like Smart Forms because it gives fast feedback without too much test maintenance cost.

## 3. Backend testing

### 3.1 Frameworks

Recommended stack:
- xUnit
- FluentAssertions
- Moq or NSubstitute
- Testcontainers for SQL Server integration tests

### 3.2 What to test

The backend should be tested at the following levels:

#### Unit tests
- command handlers
- query handlers
- validators
- domain logic
- service methods
- mapping logic

Examples:
- create form command with invalid title
- publish form without required fields
- submit form with missing required answer values
- dashboard summary query returns correct counts

#### Integration tests
- API endpoints
- database access through EF Core
- request validation and responses
- authorization checks

Examples:
- POST /api/forms
- GET /api/forms/{id}
- POST /api/forms/{id}/submit
- GET /api/forms/{formId}/submissions

### 3.3 Best practices

- test business behavior, not implementation details
- prefer small and focused tests
- always test validation failures and edge cases
- verify returned status codes and payloads
- use real database behavior in integration tests
- keep test data deterministic and easy to read

## 4. Frontend testing

### 4.1 Frameworks

Recommended stack:
- Jasmine/Karma or Jest for unit tests
- Playwright for end-to-end tests

### 4.2 What to test

Frontend tests should cover:
- form builder interactions
- validation messages
- dashboard filters and sorting
- user flow between pages
- submission form rendering

Examples:
- adding a text field to a form
- required validation on a field
- saving a form draft
- publishing a form successfully
- submitting a completed form

### 4.3 Best practices

- test user behavior, not DOM implementation details
- avoid fragile tests tied to CSS selectors
- keep tests focused on critical user flows
- use page objects in E2E tests

## 5. End-to-end testing

The most important E2E scenarios for Smart Forms are:

1. Sign in
2. Create a new form
3. Add fields
4. Save and publish the form
5. Share the form link
6. Submit answers
7. View responses
8. Export results as CSV

These scenarios represent the core product value and should be verified in browser automation tests.

## 6. API contract testing

The backend and frontend should agree on shared contracts. This means:

- DTOs must be stable
- request/response shape must be predictable
- validation errors must be consistent
- status codes must be correct

Contract tests should ensure that:
- required properties are present
- invalid requests return proper errors
- API responses match expected structure

## 7. Test data strategy

Use reusable factories and builders for test data instead of ad hoc objects.

Examples:
- UserFactory
- FormFactory
- FormFieldFactory
- SubmissionFactory

This keeps tests readable and reduces duplication.

Good test data should be:
- explicit
- minimal
- realistic
- deterministic

## 8. Coverage goals

For the MVP, a reasonable target is:

- backend unit tests: 70%+ coverage on core logic
- integration tests for critical API flows
- E2E tests for the main user journeys

We should not try to cover every rare edge case in E2E. The goal is confidence in the highest-value flows.

## 9. CI/CD and automated checks

All pull requests should run the test suite before merge.

Recommended pipeline stages:

- restore dependencies
- build backend
- build frontend
- run backend unit tests
- run backend integration tests
- run frontend unit tests
- run Playwright smoke tests

If a test fails, the pipeline should stop and prevent the merge.

## 10. Best practices for this project

For Smart Forms, prioritize the following tests first:

- form creation
- form validation
- publish flow
- submission flow
- dashboard list queries
- export CSV
- permission and ownership checks

These are the most critical product behaviors and the ones users will rely on most.

## 11. Anti-patterns to avoid

- testing implementation details instead of behavior
- writing only UI tests for everything
- using production data or a real production database in tests
- skipping validation tests
- having giant tests with unrelated assertions
- writing brittle tests tied to CSS or markup structure

## 12. Recommended stack summary

### Backend
- xUnit
- FluentAssertions
- Moq or NSubstitute
- Testcontainers

### Frontend
- Jasmine/Karma or Jest
- Playwright

### CI
- GitHub Actions

## 13. Final recommendation

The Smart Forms project should adopt a pragmatic testing strategy:

- broad unit coverage for business logic
- narrow but meaningful integration coverage for API and persistence
- a small, reliable set of browser-based E2E tests for the core user journey

This gives us confidence without creating unnecessary test overhead in the MVP stage.
