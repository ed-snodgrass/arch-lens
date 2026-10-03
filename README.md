# ArchLens

**Agentic Architectural Review for Software Components**

ArchLens is an agentic software factory that analyzes software components to identify architectural boundaries and recommend improvements to testing and component design.

The goal of ArchLens is to help developers understand how a component interacts with external systems and how those interactions can be made more reliable and maintainable.

ArchLens focuses on three areas:

- **Contract Tests** — Identify external dependencies and recommend tests that verify the contracts between the component and those dependencies.
- **Behavior Tests** — Identify important user-facing behaviors and recommend tests that validate those behaviors.
- **Adapters** — Identify areas of tight coupling and recommend adapters or ports that can better isolate the application from external implementations.

## Motivation

Modern applications frequently depend on external APIs, SDKs, databases, message brokers, and other services. Over time, these dependencies can become tightly coupled to application logic and difficult to test safely.

Developers reviewing an unfamiliar codebase must answer questions such as:

- What external systems does this component depend on?
- Where are the boundaries between application logic and external systems?
- Which boundaries should be protected by contract tests?
- What user behaviors should be verified through behavior tests?
- Where would an adapter or port reduce coupling?
- What evidence in the code supports these recommendations?

ArchLens explores whether an agentic workflow can assist with this type of architectural review.

## How It Works

ArchLens analyzes a source repository through a set of specialized agentic workflows.

```text
Repository
    │
    ▼
Codebase Discovery
    │
    ├── Components
    ├── Entry Points
    ├── External Dependencies
    └── User Workflows
    │
    ▼
Architecture Analysis
    │
    ├───────────────┬────────────────┐
    ▼               ▼                ▼
Contract Test    Behavior Test     Coupling
Analysis         Analysis          Analysis
    │               │                │
    ▼               ▼                ▼
Suggested        Suggested         Suggested
Tests            Scenarios         Adapters
    └───────────────┴────────────────┘
                    │
                    ▼
             Architecture Review
```

### 1. Codebase Discovery

ArchLens first builds context about the component being analyzed.

The discovery workflow identifies:

- Application components
- Entry points
- External APIs and services
- SDK dependencies
- Persistence boundaries
- Important user workflows
- Relationships between application code and external systems

The resulting context is provided to the architectural analysis workflows.

### 2. Contract Test Analysis

External dependencies represent important architectural boundaries.

ArchLens identifies these boundaries and recommends where contract tests could reduce integration risk.

For example:

```text
Finding:
PaymentService communicates with the Stripe API.

Recommendation:
Introduce a contract test verifying the expected payment creation
request and response.

Suggested Contract:
PaymentService expects Stripe to return a payment identifier,
status, and amount for a successful payment.
```

Where appropriate, ArchLens may also generate a test skeleton demonstrating how the contract could be verified.

### 3. Behavior Test Analysis

ArchLens identifies important behaviors exposed to users or other consumers of the component.

These behaviors can be expressed as scenarios such as:

```gherkin
Given a customer has a valid order
When the customer successfully completes payment
Then the order should transition to PAID
```

Behavior recommendations focus on validating outcomes rather than implementation details.

### 4. Coupling Analysis

ArchLens examines how application logic interacts with external implementations.

For example:

```text
OrderService
    │
    ▼
Stripe SDK
```

ArchLens may identify this as an opportunity to introduce a port:

```text
OrderService
    │
    ▼
PaymentGateway
    │
    ▼
StripePaymentGateway
    │
    ▼
Stripe SDK
```

The architectural review explains why the adapter may be useful and can provide a suggested interface or implementation skeleton.

## Example Finding

An ArchLens recommendation might look like:

```text
Finding
-------
PaymentService directly depends on StripeClient.

Evidence
--------
PaymentService creates and invokes StripeClient as part of the
payment workflow.

Architectural Concern
---------------------
Application payment logic is directly coupled to the Stripe SDK.

Recommendation
--------------
Introduce a PaymentGateway abstraction between PaymentService
and Stripe.

Suggested Adapter
-----------------
PaymentGateway
    └── StripePaymentGateway

Suggested Contract Test
-----------------------
Verify that StripePaymentGateway correctly maps the application's
payment request to Stripe and maps the Stripe response back to the
application's payment model.

Suggested Behavior Test
-----------------------
Given a valid order
When payment succeeds
Then the order transitions to PAID.
```

## Design Principles

ArchLens follows several principles when generating recommendations.

### Evidence-Based Recommendations

Recommendations should reference evidence discovered in the repository rather than relying solely on generic architectural guidance.

### Human-in-the-Loop

ArchLens provides recommendations rather than automatically restructuring an application.

Developers remain responsible for deciding whether a proposed test, adapter, or architectural change is appropriate.

### Focus on Boundaries

ArchLens prioritizes interactions between components and external systems because these boundaries frequently introduce integration risk and coupling.

### Actionable Output

Recommendations should result in something a developer can act upon, such as:

- A contract test
- A behavior scenario
- A test skeleton
- A proposed interface
- An adapter skeleton
- An architectural recommendation

## Capstone Scope

ArchLens is being developed as a four-week capstone project for a course on building **Software Factories using Agentic Workflows**.

The capstone focuses on demonstrating the agentic workflow rather than building a universal static-analysis platform.

The initial implementation will therefore target a constrained set of technologies and architectural patterns.

### In Scope

- Repository exploration
- External dependency identification
- Architectural boundary identification
- Contract test recommendations
- Behavior test recommendations
- Coupling analysis
- Adapter/port recommendations
- Evidence-backed architectural review
- Generation of selected test or adapter skeletons

### Out of Scope

The initial version does not attempt to:

- Support every programming language or framework
- Replace traditional static-analysis tools
- Automatically refactor production applications
- Automatically approve architectural changes
- Guarantee that every recommendation is appropriate
- Perform a comprehensive enterprise architecture assessment

## Project Goals

The capstone will evaluate whether an agentic software factory can:

1. Understand enough of a software component to identify meaningful architectural boundaries.
2. Discover external dependencies that may benefit from contract testing.
3. Identify user-facing workflows that may benefit from behavior testing.
4. Identify coupling that could potentially be reduced through ports or adapters.
5. Produce recommendations supported by evidence from the repository.
6. Generate useful engineering artifacts from those recommendations.

## Success Criteria

A successful ArchLens analysis should be able to take a sample repository and produce an architectural review containing:

```text
Repository
     │
     ▼
  ArchLens
     │
     ├── External Dependencies
     │      └── Suggested Contract Tests
     │
     ├── User Behaviors
     │      └── Suggested Behavior Tests
     │
     └── Coupling Opportunities
            └── Suggested Ports / Adapters
```

Most importantly, an engineer reviewing the output should be able to understand **what ArchLens found, why it matters, what evidence supports the finding, and what action ArchLens recommends**.

## Status

🚧 **Capstone Project — Under Development**

ArchLens is currently being developed as a proof of concept for exploring architectural review through agentic software development workflows.
