# Contributing to Commerce Link

Thank you for your interest in contributing to Commerce Link! This guide will help you get started.

## Local Development Setup

The full local setup (prerequisites, Docker-based infrastructure, seeded users, run instructions) lives in the main application repository so we keep it in one place. Follow the README there:

- [commerce-link/app — README](https://github.com/commerce-link/app#readme)

Once your local environment is up, return here for contribution guidelines, repository structure, and coding conventions.

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

### Creating a New Integration

This is one of the best ways to contribute. To add a new payment, shipping, invoicing, or marketplace provider:

1. Create a new Maven module (e.g., `payments-newprovider`)
2. Add a dependency on the corresponding API module (e.g., `payments-api`)
3. Implement the provider interface (e.g., `PaymentProvider`)
4. Implement the descriptor (e.g., `PaymentProviderDescriptor`) with `name()`, `displayName()`, `configurationFields()`, and `create(Map)`
5. Register via `META-INF/services/` for `ServiceLoader` discovery
6. Add a `README.md` describing configuration requirements
7. Add a `LICENSE` file (MIT)

Use the [Integration](https://github.com/commerce-link/.github/issues/new?template=integration.yml) issue template to propose new integrations.

## Coding Conventions

- **No Logger statements** — all logging goes to Sentry automatically. Use `System.out`/`System.err` only in rare cases.
- **No comments** — code should be self-explanatory. Refactor instead of commenting.
- **DTOs** — use factory methods: `OrderDto.from(Order order)`
- **Localization** — implement `LocalizedEnum` interface. Polish is the primary language.
- **CSV** — use `CSVLoader`, `CSVWriter`, `CSVReady` from `commercelink-starter`
- **Error handling** — `GlobalExceptionHandler` catches common exceptions. Sentry logs errors automatically.

### Naming Conventions

**DynamoDB Tables**: PascalCase plural (e.g., `Orders`, `Products`)
- Acronyms stay uppercase: `RMA` (not `Rma`)
- No version suffixes (no `V2`)
- Name reflects what the table stores

**DynamoDB Attributes**: camelCase with specific patterns:
- Attribute names match Java field names (no shortening)
- Manufacturer codes: `mfn`
- Quantities: `qty`
- Unit prices/costs: `unitPrice`, `unitCost`
- Totals without prefix: `totalPrice`, `totalCost`
- Timestamps: `{action}At` (e.g., `createdAt`, `orderedAt`, `shippedAt`)
- British spelling: `fulfilment` (not fulfillment)
- Nested documents: Java field without domain prefix (e.g., `events` not `deliveryEvents`)

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
