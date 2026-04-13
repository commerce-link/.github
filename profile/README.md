# Commerce Link

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Java 21](https://img.shields.io/badge/Java-21-orange.svg)](https://openjdk.org/projects/jdk/21/)
[![Spring Boot 3.5](https://img.shields.io/badge/Spring%20Boot-3.5-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-blue.svg)](https://github.com/commerce-link/.github/blob/main/CONTRIBUTING.md)
[![GitHub Sponsors](https://img.shields.io/badge/sponsor-GitHub%20Sponsors-pink.svg)](https://github.com/sponsors/commerce-link)

**The open-source e-commerce platform you actually own.**

CommerceLink is a complete ecosystem for running online and onsite sales. Built with Spring Boot 3 and Java 21, it gives you everything you need to launch a store, manage inventory from multiple suppliers, process orders, handle payments, generate invoices, ship products, and sell on marketplaces — all from a single platform that you fully control.

Unlike SaaS solutions where you rent access and play by someone else's rules, CommerceLink runs on your infrastructure. No monthly fees per transaction. No vendor lock-in. No feature gates. You get the full platform, free for commercial use — see [Licensing](#licensing) for details.

### Why CommerceLink?

- **Truly modular** — every external integration (payments, shipping, invoicing, marketplaces, suppliers) is a standalone module built on the `provider-api` plugin system. Use the ones you need, ignore the rest, or build your own in hours — not weeks.
- **Multi-supplier from day one** — aggregate inventory feeds from dozens of distributors simultaneously. CommerceLink automatically matches products across suppliers by EAN and manufacturer codes, finds the best prices, and optimizes fulfilment paths.
- **Scales with your business** — start with a single store and grow to hundreds without reinstalling anything. The platform is multi-tenant by design.
- **Production-ready** — this is not a hobby project. CommerceLink powers real businesses processing real orders every day. It includes warehouse management, RMA, automated fulfilment, financial reporting, and email notifications out of the box.
- **Built on proven technology** — Java 21, Spring Boot 3.5, AWS (DynamoDB, S3, SQS, SES). No exotic dependencies. Your team already knows this stack.
- **Cheap to host** — runs on minimal AWS infrastructure. No expensive licensing, no per-seat fees. Your biggest cost is the AWS bill, and with DynamoDB's on-demand pricing you only pay for what you use.
- **Make it yours** — fork it, modify it, adapt it to your exact business needs. All provider modules are MIT-licensed with no restrictions. The core application uses BSL 1.1, which allows full customization for your own business — the only limitation is that you cannot resell it as a competing product. No obligation to publish your changes in either case.

> **Coming soon:** `storefront-api` — a provider API for publishing products to popular e-commerce platforms like WooCommerce, Shoper, and others.

## Who Is This For

CommerceLink is designed to work in two deployment models, depending on the scale of your business:

- **Single store** — use CommerceLink as an out-of-the-box platform to run your own online store. Set up your branding, connect your payment and shipping providers, configure your supplier integrations, and start selling. Everything you need is included in one installation with no additional setup required.

- **Multi-store** — configure CommerceLink to manage multiple stores from a single installation. This model is built for large retail networks that operate tens or even hundreds of stores and need shared inventory across all of them, centralized supplier integrations, coordinated fulfilment, and unified warehouse management. Each store maintains its own branding, checkout flow, and provider configuration while benefiting from the shared infrastructure underneath.

Transitioning from single store to multi-store requires no additional installation or migration. The platform is multi-tenant by design, so scaling from one store to many is purely a matter of configuration.

## Features

- **Inventory** — aggregate feeds from multiple suppliers, automatic product discovery and matching across distributors by EAN and manufacturer codes
- **Orders** — full order lifecycle, automated fulfilment with cost-optimized or speed-optimized allocation strategies
- **Warehouse** — goods in/out, stock management, reservations, document generation with sequential numbering
- **Pricing** — pricelist generation, daily snapshots, price aggregation across suppliers
- **Payments** — pluggable payment providers (Stripe, PayNow) with webhook handling
- **Invoicing** — proforma, advance, standard, final, and credit note invoices via pluggable providers
- **Shipping** — carrier discovery, shipment estimation, label generation, and tracking
- **Marketplaces** — order import and offer export to external marketplaces
- **PIM** — structured product data with health scoring and approval workflow
- **RMA** — returns management with configurable return centers
- **Notifications** — email notifications via AWS SES with customizable Mustache templates for each order lifecycle stage
- **Multi-tenancy** — per-store configuration for branding, checkout, providers, and RMA settings

## Architecture

CommerceLink is built around a **plugin-based architecture**. The core application handles business logic, while integrations are extracted into independent libraries discovered at runtime via Java's `ServiceLoader` (SPI).

```
provider-api            Base plugin system: ProviderDescriptor<T>
   |
   +-- invoicing-api    InvoicingProvider interface
   |      +-- invoicing-fakturownia
   |      +-- invoicing-saldeosmart
   |
   +-- payments-api     PaymentProvider interface
   |      +-- payments-stripe
   |      +-- payments-paynow
   |
   +-- shipping-api     ShippingProvider interface
   |      +-- shipping-furgonetka
   |
   +-- marketplace-api  MarketplaceProvider interface
   |      +-- marketplace-morele
   |      +-- marketplace-empik
   |
   +-- supplier-api     SupplierDescriptor interface
   |      +-- supplier-ingrammicro
   |      +-- supplier-also
   |      +-- ... (14 suppliers)
   |
   +-- pim-api          PimCatalog interface
          +-- pim-commercelink
```

## Repositories

### Core

| Repository | Description |
|------------|-------------|
| [app](https://github.com/commerce-link/app) | Main Spring Boot application (BSL 1.1) |
| [starter](https://github.com/commerce-link/starter) | Shared Spring Boot starter: AWS, DynamoDB, security, email, CSV, storage |
| [commons](https://github.com/commerce-link/commons) | Pure Java library with shared enums and types |
| [provider-api](https://github.com/commerce-link/provider-api) | Base plugin system defining `ProviderDescriptor<T>` |
| [rest-client](https://github.com/commerce-link/rest-client) | Lightweight OAuth2-enabled HTTP client |

### Provider APIs & Implementations

| API | Implementation | Purpose |
|-----|---------------|---------|
| [invoicing-api](https://github.com/commerce-link/invoicing-api) | [invoicing-fakturownia](https://github.com/commerce-link/invoicing-fakturownia), [invoicing-saldeosmart](https://github.com/commerce-link/invoicing-saldeosmart) | Invoice creation and management |
| [payments-api](https://github.com/commerce-link/payments-api) | [payments-stripe](https://github.com/commerce-link/payments-stripe), [payments-paynow](https://github.com/commerce-link/payments-paynow) | Payment processing |
| [shipping-api](https://github.com/commerce-link/shipping-api) | [shipping-furgonetka](https://github.com/commerce-link/shipping-furgonetka) | Shipping and tracking |
| [marketplace-api](https://github.com/commerce-link/marketplace-api) | [marketplace-morele](https://github.com/commerce-link/marketplace-morele), [marketplace-empik](https://github.com/commerce-link/marketplace-empik) | Marketplace integration |
| [supplier-api](https://github.com/commerce-link/supplier-api) | 14 supplier implementations | Inventory feed aggregation |
| [pim-api](https://github.com/commerce-link/pim-api) | [pim-commercelink](https://github.com/commerce-link/pim-commercelink) | Product information management |
| storefront-api | *Coming soon* | Publishing to e-commerce platforms (WooCommerce, Shoper, etc.) |

### Enterprise Services

| Repository | Description                                                                                   |
|------------|-----------------------------------------------------------------------------------------------|
| [pim](https://github.com/commerce-link/pim) | PIM microservice (structuralized product data, health scoring, approval workflow) |

## Tech Stack

- **Java 21** / **Spring Boot 3.5**
- **AWS DynamoDB** as primary database
- **AWS S3, SQS, SES, EventBridge, Secrets Manager** for infrastructure
- **LocalStack** for local development
- **Thymeleaf + Bulma CSS** for UI
- **Maven** for builds

## Getting Started

See individual repository READMEs for setup instructions. For local development you will need:

- Java 21+
- Maven
- [AWS NoSQL Workbench](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/workbench.html) (local DynamoDB)
- [LocalStack](https://localstack.cloud/) (local AWS services)

## Licensing

- **Core application** (`app`): [Business Source License 1.1](https://github.com/commerce-link/app/blob/main/LICENSE) — free for internal use, converts to MIT after 4 years per version
- **All other repositories**: [MIT License](LICENSE)

## Roadmap

See our [project board](https://github.com/orgs/commerce-link/projects) for planned features and current progress.

## Contributing

We welcome contributions! Please read our [Contributing Guidelines](https://github.com/commerce-link/.github/blob/main/CONTRIBUTING.md).

## Security

To report a vulnerability, please see our [Security Policy](https://github.com/commerce-link/.github/blob/main/SECURITY.md).

## Get in Touch

We are here to help you succeed with CommerceLink. Reach out to us at **hello@commercelink.pl** if you:

- Need help setting up or deploying the platform
- Want to discuss a custom integration or provider implementation
- Have a feature request or want to share feedback
- Are interested in commercial support or consulting

If you are looking for a fully managed, hosted solution and prefer not to self-host, visit **[commercelink.pl](https://commercelink.pl)**.

---

Built and maintained by [Pragmatic Coders](https://pragmaticcoders.com/) in Krakow, Poland.
