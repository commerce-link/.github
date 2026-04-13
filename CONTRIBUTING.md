# Contributing to Commerce Link

Thank you for your interest in contributing to Commerce Link! This guide will help you get started.

## Prerequisites

- Java 21+
- Maven
- [AWS NoSQL Workbench](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/workbench.html) (local DynamoDB at `localhost:8000`)
- [LocalStack](https://localstack.cloud/) (local AWS services at `localhost:4566`)
- Git

## Local Development Setup

1. Clone the repository you want to work on
2. Start AWS NoSQL Workbench (DynamoDB)
3. Start LocalStack (S3, SQS, SES, EventBridge, Secrets Manager)
4. Run DynamoDB schema migration: execute `main()` in `commercelink-starter`'s `DynamoDbSchema.java`
5. Build: `mvn clean compile`
6. Run tests: `mvn test`

## Repository Structure

Commerce Link is organized as separate repositories following a plugin-based architecture:

- **Core**: `app`, `starter`, `commons`, `rest-client`
- **Plugin system**: `provider-api` defines `ProviderDescriptor<T>`, domain APIs extend it, implementations provide concrete integrations
- **Provider APIs**: `invoicing-api`, `payments-api`, `shipping-api`, `marketplace-api`, `supplier-api`, `pim-api`
- **Implementations**: e.g., `payments-stripe`, `shipping-furgonetka`, `invoicing-fakturownia`

## How to Contribute

### Good First Issues

New to CommerceLink? Look for issues labeled [`good first issue`](https://github.com/search?q=org%3Acommerce-link+label%3A%22good+first+issue%22+state%3Aopen&type=issues). These are smaller, well-scoped tasks that are a great starting point for new contributors.

### Reporting Bugs

- Use the [Bug Report](https://github.com/commerce-link/.github/issues/new?template=bug-report.yml) issue template
- Include steps to reproduce, expected vs actual behavior, and your environment details

### Suggesting Features

- Use the [Feature Request](https://github.com/commerce-link/.github/issues/new?template=feature-request.yml) issue template
- Describe the problem you're solving, not just the solution

### Submitting a Pull Request

1. Fork the repository
2. Create a feature branch from `main`
3. Make your changes following the coding conventions below
4. Ensure `mvn clean compile` passes
5. Run existing tests: `mvn test`
6. Submit a pull request using the PR template

### Creating a New Provider Implementation

This is one of the best ways to contribute. To add a new payment, shipping, invoicing, or marketplace provider:

1. Create a new Maven module (e.g., `payments-newprovider`)
2. Add a dependency on the corresponding API module (e.g., `payments-api`)
3. Implement the provider interface (e.g., `PaymentProvider`)
4. Implement the descriptor (e.g., `PaymentProviderDescriptor`) with `name()`, `displayName()`, `configurationFields()`, and `create(Map)`
5. Register via `META-INF/services/` for `ServiceLoader` discovery
6. Add a `README.md` describing configuration requirements
7. Add a `LICENSE` file (MIT)

Use the [Provider Implementation](https://github.com/commerce-link/.github/issues/new?template=provider-implementation.yml) issue template to propose new integrations.

## Coding Conventions

- **No Logger statements** — all logging goes to Sentry automatically. Use `System.out`/`System.err` only in rare cases.
- **No comments** — code should be self-explanatory. Refactor instead of commenting.
- **DTOs** — use factory methods: `OrderDto.from(Order order)`
- **Localization** — implement `LocalizedEnum` interface. Polish is the primary language.
- **CSV** — use `CSVLoader`, `CSVWriter`, `CSVReady` from `commercelink-starter`
- **Error handling** — `GlobalExceptionHandler` catches common exceptions. Sentry logs errors automatically.

### Naming Conventions

**DynamoDB Tables**: PascalCase plural (e.g., `Orders`, `Products`)

**DynamoDB Attributes**: camelCase with specific patterns:
- Manufacturer codes: `mfn`
- Quantities: `qty`
- Unit prices: `unitPrice`, `unitCost`
- Timestamps: `{action}At` (e.g., `createdAt`, `orderedAt`)
- British spelling: `fulfilment` (not fulfillment)

**SQS Queues**: `{module}-{domain}-{action}-queue[.fifo]`
- Singular nouns: `order` not `orders`
- Verb for action: `generate` not `generator`
- Example: `app-order-generate-queue`

## Licensing

- The main `commercelink` application is licensed under BSL 1.1
- All other repositories are licensed under MIT
- By submitting a pull request, you agree that your contribution will be licensed under the same license as the repository

## Questions?

- Open a [GitHub Discussion](https://github.com/orgs/commerce-link/discussions) for questions
- See [SUPPORT.md](SUPPORT.md) for more ways to get help
