# Database Schema

This document describes the current data model for Smart Forms.

## Purpose
TODO: describe the data model and why it is structured this way.

## Core entities

### User
- Id
- Email
- PasswordHash
- CreatedAt
- UpdatedAt

### Form
- Id
- OwnerUserId
- Title
- Description
- IsPublished
- CreatedAt
- UpdatedAt

### FormField
- Id
- FormId
- FieldType
- Name
- Label
- IsRequired
- SortOrder
- ConfigurationJson
- CreatedAt
- UpdatedAt

### Submission
- Id
- FormId
- SubmittedBy
- SubmittedAt
- CreatedAt

### SubmissionAnswer
- Id
- SubmissionId
- FormFieldId
- ValueJson
- CreatedAt

## Relationships
- User -> many Form
- Form -> many FormField
- Form -> many Submission
- Submission -> many SubmissionAnswer
- FormField -> many SubmissionAnswer

## Notes
TODO: add the final schema decisions, indexes, constraints, and migration strategy.

## Considerations
- Use SQL Server / Azure SQL
- Use EF Core and migrations
- Keep ownership rules aligned with product requirements
