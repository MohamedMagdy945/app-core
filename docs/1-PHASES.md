# Project Name

A general-purpose platform that connects service providers with customers.

Providers can register and manage their businesses and offerings,
while customers can discover providers, browse offerings, place orders,
and review providers.

---

# Project Status

**Current Phase:** Phase 1 - Foundation

**Status:** In Development

---

# Architecture

This project is designed as a modular monolith using:

- ASP.NET Core
- Vertical Slice Architecture
- Clean Architecture principles
- Entity Framework Core
- SQL Server

The system is currently designed as a single deployable application.
Business features are organized by feature/use case rather than by
technical layers.

---

# Solution Structure

```text
ProjectName/
│
├── docs/
│
├── src/
│   ├── ProjectName.Api/
│   ├── ProjectName.Application/
│   ├── ProjectName.Domain/
│   └── ProjectName.Infrastructure/
│
├── tests/
│   ├── ProjectName.UnitTests/
│   └── ProjectName.IntegrationTests/
│
├── .editorconfig
├── .gitignore
├── Directory.Build.props
├── Directory.Packages.props
├── ProjectName.sln
└── README.md
```

## Architecture Dependencies

```
                 ┌──────────────────┐
                 │      Domain      │
                 └──────────────────┘
                          ▲
                          │
                 ┌──────────────────┐
                 │   Application    │
                 └──────────────────┘
                    ▲           ▲
                    │           │
          ┌─────────┘           └─────────┐
          │                               │
┌──────────────────┐             ┌────────────────────┐
│       API        │             │   Infrastructure   │
└──────────────────┘             └────────────────────┘
```

**Dependency rules:**

| Project | Depends On |
|---|---|
| Domain | *(no project dependencies)* |
| Application | Domain |
| Infrastructure | Application, Domain |
| Api | Application, Infrastructure |

---

# Development Phases

## Phase 1 — Foundation

**Goal:** Prepare the solution and establish the project architecture.

- [ ] Create solution
- [ ] Create projects
- [ ] Create test projects
- [ ] Configure project references
- [ ] Configure dependency direction
- [ ] Add .gitignore
- [ ] Add .editorconfig
- [ ] Configure Directory.Build.props
- [ ] Configure Directory.Packages.props
- [ ] Configure nullable reference types
- [ ] Configure implicit usings
- [ ] Configure code analysis
- [ ] Configure Application dependency injection
- [ ] Configure Infrastructure dependency injection
- [ ] Configure API dependency injection
- [ ] Create base configuration structure
- [ ] Verify solution builds successfully

## Phase 2 — Database & Persistence

**Goal:** Prepare the database infrastructure.

- [ ] Select database provider
- [ ] Configure SQL Server
- [ ] Install EF Core packages
- [ ] Create DbContext
- [ ] Configure connection strings
- [ ] Configure EF Core
- [ ] Configure entity mappings
- [ ] Configure relationships
- [ ] Configure indexes
- [ ] Configure constraints
- [ ] Configure audit fields
- [ ] Configure concurrency where required
- [ ] Create initial migration
- [ ] Apply database migration
- [ ] Verify database connection
- [ ] Verify migration workflow

## Phase 3 — API Foundation

**Goal:** Prepare common API infrastructure before implementing business features.

- [ ] Configure controllers/endpoints
- [ ] Configure OpenAPI
- [ ] Configure Swagger
- [ ] Configure ProblemDetails
- [ ] Configure global exception handling
- [ ] Define API error response format
- [ ] Configure request validation
- [ ] Configure response conventions
- [ ] Configure API versioning strategy
- [ ] Configure health checks
- [ ] Configure CORS
- [ ] Configure HTTPS
- [ ] Configure API security defaults

## Phase 4 — Logging & Observability

**Goal:** Make the application observable in development and production.

- [ ] Configure application logging
- [ ] Configure structured logging
- [ ] Define log levels
- [ ] Define logging conventions
- [ ] Avoid sensitive data in logs
- [ ] Add request logging
- [ ] Add error logging
- [ ] Add correlation/request identifiers
- [ ] Configure health checks
- [ ] Evaluate metrics
- [ ] Evaluate distributed tracing
- [ ] Evaluate OpenTelemetry

## Phase 5 — Validation

**Goal:** Establish consistent validation across the application.

- [ ] Select validation approach
- [ ] Configure FluentValidation if required
- [ ] Configure request validation
- [ ] Define validation error format
- [ ] Separate input validation from business rules
- [ ] Add domain-level business rule validation
- [ ] Ensure invalid requests return appropriate HTTP responses
- [ ] Add validation tests

## Phase 6 — Authentication & Authorization

**Goal:** Secure the application and establish user access rules.

**Actors:**
- Customer
- Provider Owner
- Admin

- [ ] Select authentication strategy
- [ ] Configure authentication
- [ ] Configure JWT if applicable
- [ ] Configure token validation
- [ ] Configure authorization
- [ ] Define roles
- [ ] Define policies
- [ ] Configure resource-based authorization
- [ ] Implement provider ownership authorization
- [ ] Protect customer operations
- [ ] Protect provider owner operations
- [ ] Protect admin operations
- [ ] Add authorization tests

## Phase 7 — Testing Foundation

**Goal:** Establish a reliable automated testing strategy.

**Unit Tests**
- [ ] Configure xUnit
- [ ] Configure assertions
- [ ] Test domain behavior
- [ ] Test application use cases
- [ ] Test business rules
- [ ] Test validators

**Integration Tests**
- [ ] Configure integration test host
- [ ] Configure test database
- [ ] Test API endpoints
- [ ] Test database persistence
- [ ] Test authentication
- [ ] Test authorization
- [ ] Test complete use cases

**Future**
- [ ] Testcontainers
- [ ] End-to-end testing
- [ ] CI test execution

## Phase 8 — Provider Management

**Goal:** Implement the provider lifecycle.

**Features**
- [ ] Register Provider
- [ ] View Provider
- [ ] Update Provider
- [ ] Approve Provider
- [ ] Reject Provider
- [ ] Activate Provider
- [ ] Deactivate Provider
- [ ] Provider ownership
- [ ] Provider visibility rules

**Business Rules**
- Pending providers are not visible to customers
- Provider Owner can manage only their own providers
- Only authorized Admin users can approve providers
- Provider status transitions are validated

## Phase 9 — Offering Management

**Goal:** Allow providers to manage their offerings.

**Domain**

```
Provider
    │
    └── Offering
          │
          └── OfferingItem
```

**Features**
- [ ] Create Offering
- [ ] Update Offering
- [ ] Create Offering Item
- [ ] Update Offering Item
- [ ] Change Offering Item availability
- [ ] View Offering
- [ ] View Offering Items

**Business Rules**
- Offering belongs to a provider
- Offering Item belongs to an offering
- Provider Owner can manage only their own offering
- Offering Item must be available to be ordered
- Offering Item price must be valid

## Phase 10 — Cart

**Goal:** Allow customers to prepare orders before checkout.

**Features**
- [ ] Create Cart
- [ ] Add Item to Cart
- [ ] Remove Item from Cart
- [ ] Update Item Quantity
- [ ] View Cart
- [ ] Clear Cart

**Business Rules**
- A customer owns their cart
- Cart can contain items from multiple providers
- Each Cart Item belongs to an Offering Item
- Provider information is derived from the Offering Item
- Quantity must be greater than zero
- Cart does not represent a confirmed order

## Phase 11 — Checkout & Orders

**Goal:** Convert cart items into provider-specific orders.

**Checkout Flow**

```
Cart
  │
  ├── Provider A
  │      ├── Item
  │      └── Item
  │
  └── Provider B
         ├── Item
         └── Item

            ↓

        Checkout

            ↓

    ┌───────────────┐
    │   Order A     │
    │  Provider A   │
    └───────────────┘

    ┌───────────────┐
    │   Order B     │
    │  Provider B   │
    └───────────────┘
```

**Features**
- [ ] Checkout
- [ ] Create Order
- [ ] Create Order Items
- [ ] Calculate order total
- [ ] Store item price at order creation
- [ ] View customer orders
- [ ] View provider orders
- [ ] Update order status
- [ ] Cancel order

**Business Rules**
- Provider must be Active
- Offering Item must be Available
- Order must contain at least one item
- Quantity must be greater than zero
- Order belongs to exactly one provider
- Customer cannot modify the order total
- Order Item stores the price at order creation
- Provider manages only its own orders

## Phase 12 — Delivery

**Goal:** Support provider-specific delivery.

- [ ] Define delivery model
- [ ] Define delivery ownership
- [ ] Define delivery status
- [ ] Define delivery address
- [ ] Define delivery fee
- [ ] Define delivery driver model if required
- [ ] Connect delivery with order
- [ ] Define delivery lifecycle
- [ ] Add delivery authorization rules

## Phase 13 — Reviews

**Goal:** Allow customers to review providers.

**Features**
- [ ] Create Review
- [ ] View Reviews
- [ ] Update Review
- [ ] Delete Review
- [ ] Calculate provider rating

**Business Rules**
- Define who can review
- Define whether an order is required
- Define review ownership
- Define whether multiple reviews are allowed
- Define review moderation rules

## Phase 14 — Notifications

**Goal:** Notify users about important system events.

**Potential Events**
- [ ] Provider approved
- [ ] Provider rejected
- [ ] Order confirmed
- [ ] Order cancelled
- [ ] Order preparing
- [ ] Order ready
- [ ] Order delivered

**Channels** *(actual channels will depend on business requirements)*
- [ ] In-app
- [ ] Push notifications
- [ ] Email
- [ ] SMS

## Phase 15 — Payments

**Goal:** Integrate payment processing if required.

- [ ] Select payment provider
- [ ] Define payment model
- [ ] Define payment status
- [ ] Create payment abstraction
- [ ] Implement provider integration
- [ ] Handle payment success
- [ ] Handle payment failure
- [ ] Handle payment cancellation
- [ ] Handle refunds
- [ ] Handle webhooks
- [ ] Verify webhook signatures
- [ ] Prevent duplicate payment processing
- [ ] Add payment integration tests

## Phase 16 — Performance & Scalability

**Goal:** Optimize the application based on actual usage.

- [ ] Analyze slow queries
- [ ] Add required database indexes
- [ ] Optimize EF Core queries
- [ ] Use pagination
- [ ] Use projections
- [ ] Evaluate caching
- [ ] Evaluate Redis
- [ ] Evaluate response caching
- [ ] Evaluate background processing
- [ ] Monitor database performance
- [ ] Monitor API performance

> Performance decisions should be based on actual measurements.

## Phase 17 — Security Hardening

**Goal:** Prepare the application for production security requirements.

- [ ] HTTPS
- [ ] Secure authentication
- [ ] Authorization policies
- [ ] Input validation
- [ ] CORS restrictions
- [ ] Rate limiting
- [ ] Security headers
- [ ] Secret management
- [ ] Password security if passwords are managed by the system
- [ ] Token security
- [ ] Sensitive data protection
- [ ] Audit important actions
- [ ] Dependency vulnerability scanning
- [ ] Review OWASP API security risks

## Phase 18 — CI/CD

**Goal:** Automate build, test, and deployment.

**Pipeline**

```
Push / Pull Request
        ↓
Restore
        ↓
Build
        ↓
Unit Tests
        ↓
Integration Tests
        ↓
Code Analysis
        ↓
Publish
        ↓
Deploy
```

- [ ] Configure CI
- [ ] Restore dependencies
- [ ] Build solution
- [ ] Run unit tests
- [ ] Run integration tests
- [ ] Run code analysis
- [ ] Build production artifact
- [ ] Configure deployment
- [ ] Configure environment variables
- [ ] Configure secrets
- [ ] Configure database migration strategy

## Phase 19 — Production Readiness

**Goal:** Ensure the project is ready for production deployment.

- [ ] Production configuration
- [ ] Production database
- [ ] Database backup strategy
- [ ] Database migration strategy
- [ ] Logging
- [ ] Monitoring
- [ ] Health checks
- [ ] Error handling
- [ ] Authentication
- [ ] Authorization
- [ ] CORS
- [ ] Rate limiting
- [ ] Security review
- [ ] Performance testing
- [ ] Integration tests
- [ ] Deployment pipeline
- [ ] Rollback strategy
- [ ] Documentation

---

# Project Principles

1. **Business First** — Business requirements and rules should drive the implementation.
2. **Keep Domain Independent** — The Domain project must not depend on infrastructure or API concerns.
3. **Prefer Vertical Slices** — Organize application code around features and use cases.
4. **Don't Overengineer** — Do not introduce infrastructure, abstractions, or libraries without a real need.
5. **Validate at the Correct Boundary** — Separate input validation from domain business rules.
6. **Security by Default** — Never trust client-provided values for authorization, prices, totals, ownership, or other business-critical data.
7. **Test Business Rules** — Important business behavior must be covered by automated tests.
8. **Measure Before Optimizing** — Performance optimizations should be based on actual measurements.

---

# Current Progress

## Foundation

- [ ] Solution created
- [ ] Projects created
- [ ] .gitignore created
- [ ] Project references
- [ ] Central package management
- [ ] .editorconfig
- [ ] Directory.Build.props
- [ ] Dependency injection
- [ ] Configuration

## Infrastructure

- [ ] Database
- [ ] EF Core
- [ ] Migrations

## API

- [ ] Global exception handling
- [ ] ProblemDetails
- [ ] Logging
- [ ] Validation
- [ ] OpenAPI
- [ ] Health checks

## Security

- [ ] Authentication
- [ ] Authorization
- [ ] Roles
- [ ] Ownership authorization

## Features

- [ ] Provider
- [ ] Offering
- [ ] Offering Item
- [ ] Cart
- [ ] Checkout
- [ ] Order
- [ ] Delivery
- [ ] Review
- [ ] Payment
- [ ] Notifications

---

# Development Workflow

For every feature:

```
Requirement
    ↓
Use Case
    ↓
Business Rules
    ↓
Domain Model
    ↓
Application Use Case
    ↓
Infrastructure
    ↓
API
    ↓
Tests
    ↓
Documentation
```

A feature is considered complete when its required business behavior, implementation, tests, and documentation are complete.

---

# Getting Started

## Prerequisites

- .NET SDK
- SQL Server
- Git

*Additional requirements will be added as the project evolves.*

## Clone

```bash
git clone <repository-url>
cd ProjectName
```

## Restore

```bash
dotnet restore
```

## Build

```bash
dotnet build
```

## Test

```bash
dotnet test
```

---

# Documentation

Project documentation is maintained under `docs/`. Documentation includes:

- Product definition
- Actors
- Use cases
- Business rules
- Architecture
- Assumptions
- Open questions

---

> **Note on the README:** The Phases above are a roadmap, not a commitment to implement every phase now. As items are completed, change `[ ]` to `[x]` instead of rewriting the README each time.

**Actual execution order:**

```
Phase 1 → Project References → Central Package Management → .editorconfig
    ↓
Directory.Build.props → Configuration → Dependency Injection
    ↓
Phase 2: Database + EF Core
    ↓
Phase 3: Exception Handling + Logging + Validation + OpenAPI
    ↓
Phase 4: Authentication + Authorization
    ↓
Phase 5: Testing Foundation
    ↓
First Feature
```