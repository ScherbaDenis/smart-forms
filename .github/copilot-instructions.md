# Copilot Instructions for Smart Forms

## Project context
- Project: Smart Forms
- Frontend: Angular + TypeScript
- Backend: .NET 9 + ASP.NET Core Minimal API
- Database: SQL Server locally, Azure SQL in production
- Architecture: Clean Architecture with CQRS + MediatR
- Containerization: Docker and Docker Compose
- Cloud: Microsoft Azure
- Goal: build a maintainable SaaS-style form management product with a clear MVP and room to scale without over-engineering.

## Core architecture principles
This project should follow Microsoft Clean Architecture principles:

- Do not mix business rules with HTTP concerns.
- Keep the API layer thin and focused on transport concerns.
- Keep business behavior in the application layer and domain layer.
- Keep persistence and infrastructure concerns separate from business logic.
- Prefer explicit dependencies flowing inward.

## Layering rules

### Domain layer
- Contains entities, value objects, and business rules.
- Must not depend on infrastructure, API, or UI projects.
- Should be the most stable layer.
- Use domain models and domain logic here.

### Application layer
- Contains use cases, commands, queries, validators, and orchestration logic.
- Implements business workflows and coordinates domain logic.
- Depends on the Domain layer only.
- Use CQRS, MediatR, and handlers here.
- Keep handlers small, focused, and testable.

### Infrastructure layer
- Contains database access, EF Core configuration, repositories, integrations, and external service implementations.
- Implements interfaces defined by the Application layer.
- Does not contain business rules.

### API layer
- ASP.NET Core Minimal API project.
- Responsible only for HTTP concerns, request validation, route mapping, and responses.
- Should delegate business logic to application handlers.
- Should not contain business rules or domain logic.
- Keep controllers or endpoint handlers thin.

## Coding rules
- Prefer clear, readable code over clever code.
- Favor explicit, maintainable solutions over abstraction for abstraction's sake.
- Keep methods and classes focused and small.
- Do not add libraries or frameworks unless they are clearly justified.
- Use strong typing in both Angular and .NET.
- Prefer DTOs for API contracts instead of exposing internal models.
- Keep Angular components focused on UI behavior and not business logic.
- Keep backend business logic in the application and domain layers.
- Avoid placing database logic in the API layer.
- Do not over-engineer into microservices too early.
- Keep the architecture modular and simple, but scalable.

## CQRS and MediatR rules
- Use CQRS as the default backend pattern.
- Separate commands and queries.
- Use MediatR for in-process dispatch of commands, queries, and notifications.
- Keep handlers focused on one responsibility.
- Business rules must be implemented in the application/domain layers, not in endpoint handlers.
- Do not use MediatR as a replacement for API Gateway routing.
- MediatR is for internal application flow, not external HTTP routing.

## API Gateway and async messaging rules
- API Gateway is for routing external traffic.
- MediatR is for internal process-level messaging.
- Do not confuse these concerns.
- Do not add RabbitMQ, Azure Service Bus, or a gateway unless the architecture genuinely requires it.
- If async messaging is introduced later, add it only after the domain boundaries are clear and the need is proven.

## TDD and testing rules
- Follow Test-Driven Development for business-critical features.
- Write failing tests first for new domain behavior.
- Implement the minimum code required to satisfy the test.
- Refactor carefully while keeping tests green.
- Prioritize tests for:
  - form creation
  - validation
  - publishing
  - submissions
  - dashboard queries
  - export logic
  - ownership and authorization checks

## Testing stack
- Backend: xUnit, FluentAssertions, Moq or NSubstitute, Testcontainers
- Frontend: Angular tests and Playwright for E2E
- Use unit tests for most logic
- Use integration tests for API and database behavior
- Use a small set of high-value end-to-end tests for critical user journeys

## Security rules
- Never trust client-side validation alone.
- Enforce authorization and ownership checks in the backend.
- Validate all inputs server-side.
- Use HTTPS in all environments.
- Keep secrets in Azure Key Vault or environment variables, never in source code.
- Apply least privilege principles in Azure and application access.

## Frontend rules
- Use Angular + TypeScript.
- Keep components focused and readable.
- Keep API calls in services.
- Centralize error handling and loading states.
- Do not duplicate backend validation logic in the frontend as the source of truth.
- Prefer typed, explicit UI state over implicit behavior.

## Database and persistence rules
- Use EF Core for relational persistence.
- Keep persistence concerns in the Infrastructure layer.
- Use migrations for schema evolution.
- Design data ownership clearly.
- Keep database access behind repository or data-access abstractions.

## Documentation rules
- Update docs when architecture, API contracts, or behavior changes.
- Keep README and docs clear and concise.
- Document major patterns and the reason they were introduced.
- Do not add speculative architecture that the project does not need yet.

## Git and quality rules
- Keep commits small and focused.
- Do not mix unrelated refactors with feature work.
- Keep PRs scoped to one goal.
- Ensure tests pass before finishing work.
- Prefer maintainable, explicit code over clever optimization.

## Default approach for this project
- Start with a clean, modular backend and Angular frontend.
- Use CQRS + MediatR in the backend.
- Keep service boundaries simple.
- Add API Gateway only if routing complexity requires it.
- Add message brokers only when async cross-service communication is truly needed.
- Keep MVP lean, test-driven, and maintainable.

## What to avoid
- Do not build a large distributed system too early.
- Do not split into microservices before there is a real need.
- Do not add RabbitMQ or Azure Service Bus without a clear domain requirement.
- Do not add unnecessary abstractions.
- Do not skip tests for critical form workflows.
- Do not put business logic in endpoint handlers.
- Do not allow API contracts to drift from actual behavior.

This repository should remain aligned with Microsoft Clean Architecture principles while staying pragmatic and product-focused.
