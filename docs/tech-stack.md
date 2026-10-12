Goal of the project
Smart Forms is a SaaS-style application for creating and managing web forms. Users can define fields, publish forms, collect responses, and review results.

The product should support:

user authentication
dashboard for forms
form creation and editing
form publishing
response collection
submission viewing
CSV export
basic analytics
Technology stack
Frontend

Angular
TypeScript
RxJS
Angular Material or custom design system
Angular Router
Formly or custom dynamic form builder
Backend

.NET 9
ASP.NET Core Minimal API
Entity Framework Core
FluentValidation
Serilog
Swagger / OpenAPI
Database

SQL Server locally
Azure SQL Database in production
EF Core migrations
Infrastructure

Docker + Docker Compose
Azure App Service or Azure Container Apps
Azure SQL Database
Azure Key Vault
Application Insights
GitHub Actions for CI/CD
Security

JWT or Azure AD / Entra ID
HTTPS only
secret management through Key Vault
input validation and sanitization
Architecture
We use a modular backend + SPA frontend approach.

Frontend:

Angular app with pages:
login
dashboard
form builder
form preview
submissions list
settings
Backend:

ASP.NET Core Minimal API exposes REST endpoints
business logic isolated into services
database access via EF Core repositories
DTOs for API contracts
Core principle:

frontend communicates only with backend API
backend owns business rules and persistence
database is controlled only through backend
