# Smart Forms

## 1. Goal of the project

Smart Forms is a SaaS-style application for creating and managing web forms. Users can define custom fields, share forms, collect responses, and review results in one place.

The product should support:
- user authentication
- dashboard for forms
- form creation and editing
- form publishing
- submission collection
- result viewing
- CSV export
- basic analytics

## 2. Technology stack

### Frontend
- Angular
- TypeScript
- RxJS
- Angular Router
- Angular Material or a custom design system

### Backend
- .NET 9
- ASP.NET Core Minimal API
- Entity Framework Core
- FluentValidation
- Serilog
- Swagger / OpenAPI

### Database
- SQL Server locally
- Azure SQL Database in production
- EF Core migrations

### Infrastructure
- Docker + Docker Compose
- Azure App Service or Azure Container Apps
- Azure Key Vault
- Application Insights
- GitHub Actions for CI/CD

### Security
- JWT or Microsoft Entra ID / Azure AD
- HTTPS only
- secret management via Key Vault
- validation and sanitization of user input

## 3. Architecture

We use a modular architecture with a separate frontend and backend.

### Frontend
The Angular application will include pages for:
- login
- dashboard
- form builder
- form preview
- submissions list
- settings

### Backend
The ASP.NET Core Minimal API will expose REST endpoints and own the business logic and persistence layer.

### Design principles
- frontend communicates only with the API
- backend owns validation, business rules, and database access
- database access is handled only through the backend
- the system is structured for future scaling and Azure deployment

This is the technical foundation for the Smart Forms project.
