# API Contracts

This document describes the planned API contracts for Smart Forms.

## Overview
TODO: describe the API architecture and conventions.

## Conventions
- Base URL: /api
- Authentication: JWT or ASP.NET Identity based flow
- Content type: application/json
- Error responses: structured validation / authorization / not-found responses

## Authentication endpoints
### POST /api/auth/register
TODO: request and response contract.

### POST /api/auth/login
TODO: request and response contract.

## Form endpoints
### GET /api/forms
TODO: list current user forms.

### GET /api/forms/{id}
TODO: get one form by id.

### POST /api/forms
TODO: create form.

### PUT /api/forms/{id}
TODO: update form.

### DELETE /api/forms/{id}
TODO: delete form.

### POST /api/forms/{id}/publish
TODO: publish form.

### POST /api/forms/{id}/unpublish
TODO: unpublish form.

## Submission endpoints
### GET /api/forms/{id}/submissions
TODO: list submissions for a form.

### GET /api/submissions/{id}
TODO: get a single submission.

### POST /api/forms/{id}/submit
TODO: submit answers to a published form.

### GET /api/forms/{id}/export/csv
TODO: export submissions as CSV.

## DTOs
### CreateFormRequest
TODO: fields, validation rules, and example payload.

### UpdateFormRequest
TODO: fields and validation rules.

### FormResponse
TODO: shape of returned form model.

### SubmissionRequest
TODO: payload for submitted answers.

### SubmissionResponse
TODO: payload for returned submissions.

## Validation rules
TODO: describe required fields, expected types, and auth rules.

## Notes
TODO: add versioning strategy, error contract, and pagination details.
