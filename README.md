# Expense Tracker API

A REST API for managing personal expenses. Users can create an account, log in securely, and manage their own expenses and categories.

The project is built using **ASP.NET Core 8**, **Entity Framework Core**, **SQL Server**, and **JWT authentication**.

![.NET](https://img.shields.io/badge/.NET-8.0-512BD4)

![Auth](https://img.shields.io/badge/auth-JWT-black)

![SQL Server](https://img.shields.io/badge/SQL%20Server-EF%20Core-CC2927)

![Status](https://img.shields.io/badge/status-active%20development-blue)

## Features

* User registration and login using JWT authentication
* Passwords are stored using BCrypt hashing
* Each user can only access their own expenses and categories
* Default expense categories are created when a user registers
* Users can create and delete their own categories
* Add, view, update and delete expenses
* Filter expenses by date and category
* Pagination for expense lists
* Monthly expense summary with category-wise totals
* Request validation using data annotations
* Global exception handling for consistent API errors
* Swagger UI for testing the API

## Tech Stack

* **C# / .NET 8**
* **ASP.NET Core Web API**
* **Entity Framework Core**
* **SQL Server / LocalDB**
* **JWT Bearer Authentication**
* **BCrypt.Net**
* **Swagger / Swashbuckle**

## Project Structure

```text
ExpenseTracker/
├── Controllers/     # API endpoints
├── Services/        # Business logic
├── Models/          # User, Category and Expense entities
├── DTOs/            # Request and response models
├── Data/            # DbContext, relationships and indexes
├── Middleware/      # Global error handling
└── SQL/             # Reporting queries
```

The API follows a simple layered structure:

```text
Controllers → Services → DbContext → SQL Server
```

Controllers handle incoming HTTP requests, services contain the business logic, and Entity Framework Core handles database operations.

## Authentication and Data Isolation

The API uses **JWT authentication** for protected endpoints.

After logging in, the user receives a token which is used to access expense and category endpoints.

Each expense and category is associated with a user. The user ID is taken from the authenticated JWT, so users can only access their own data.

For example, one user cannot view, update or delete another user's expenses.

## Getting Started

### Requirements

You will need:

* .NET 8 SDK
* SQL Server or SQL Server LocalDB
* Entity Framework Core CLI

If EF Core CLI is not installed:

```bash
dotnet tool install --global dotnet-ef
```

### Run the Project

Restore the required packages:

```bash
dotnet restore
```

Create the database:

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

Start the application:

```bash
dotnet run
```

Open Swagger using the URL shown in the terminal:

```text
https://localhost:<port>/swagger
```

## JWT Configuration

The project uses a JWT secret for signing authentication tokens.

For local development, configure the key using .NET User Secrets instead of committing the secret to GitHub:

```bash
dotnet user-secrets init
dotnet user-secrets set "Jwt:Key" "<your-long-random-secret>"
```

Use a strong secret and keep it out of source control.

## API Endpoints

| Method | Endpoint                        | Auth | Description                              |
| ------ | ------------------------------- | ---- | ---------------------------------------- |
| POST   | `/api/auth/register`            | No   | Create a new account                     |
| POST   | `/api/auth/login`               | No   | Login and receive a JWT                  |
| GET    | `/api/categories`               | Yes  | Get your categories                      |
| POST   | `/api/categories`               | Yes  | Create a category                        |
| DELETE | `/api/categories/{id}`          | Yes  | Delete a category                        |
| GET    | `/api/expenses`                 | Yes  | Get expenses with filters and pagination |
| GET    | `/api/expenses/{id}`            | Yes  | Get an expense                           |
| POST   | `/api/expenses`                 | Yes  | Add an expense                           |
| PUT    | `/api/expenses/{id}`            | Yes  | Update an expense                        |
| DELETE | `/api/expenses/{id}`            | Yes  | Delete an expense                        |
| GET    | `/api/expenses/summary/monthly` | Yes  | Get monthly expense totals               |

### Expense Filtering

The expense list supports filtering and pagination:

```text
/api/expenses?from=&to=&categoryId=&page=&pageSize=
```

You can filter expenses by:

* Date range
* Category
* Page number
* Page size

## Monthly Summary

The API also provides a monthly summary showing the total amount spent and the amount spent in each category.

Example:

```text
GET /api/expenses/summary/monthly?year=2026&month=9
```

The summary uses LINQ `GroupBy()` to group expenses by category before generating the results.

## Quick Walkthrough

1. Register a new account using `/api/auth/register`.
2. Log in using `/api/auth/login`.
3. Copy the JWT returned by the API.
4. Click **Authorize** in Swagger and enter the token.
5. Check the default categories using `/api/categories`.
6. Add an expense using `/api/expenses`.
7. Use the monthly summary endpoint to view spending by category.

Example expense:

```json
{
  "title": "Lunch",
  "amount": 250,
  "date": "2026-09-20",
  "categoryId": 1
}
```

## Validation and Error Handling

The API validates incoming requests before processing them.

Some examples include:

* Invalid or missing required fields
* Negative expense amounts
* Duplicate email during registration
* Invalid login credentials
* Accessing another user's expense
* Using a category that doesn't belong to the logged-in user
* Deleting a category that is already being used

A global exception middleware is also used to handle unexpected errors and return consistent API responses.

## Testing

The API can currently be tested manually through Swagger.

Some scenarios to test:

* Registering with an existing email
* Logging in with an incorrect password
* Calling protected endpoints without a JWT
* Trying to access another user's expenses
* Creating an expense with a negative amount
* Using another user's category
* Testing pagination and filters
* Checking monthly summary calculations
* Trying to delete a category that has expenses

Automated unit and integration tests are planned for a future update.

## Future Improvements

Planned features include:

* Unit and integration tests using xUnit
* Refresh tokens
* Monthly budgets and spending alerts
* Export expenses to CSV
* Structured logging with Serilog
* Docker support
* GitHub Actions CI pipeline

## Author

**Riya**

LinkedIn: https://www.linkedin.com/in/riya-sharma-48bb34253/
