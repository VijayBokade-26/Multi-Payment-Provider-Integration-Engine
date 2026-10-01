# Replacing the Payment Orchestration Layer with Direct Provider Integrations

## Goal

The goal of this initiative is to reduce dependency on the existing payment orchestration layer and enable the platform to manage payment provider integrations directly.

Rather than replacing existing orchestration calls one-to-one with provider-specific APIs, I designed a common **payment provider layer** that keeps the application's business modules independent of individual payment providers.

## Architecture

The current integration flow follows this structure:

**Application → Common Provider Layer → Provider Middleware → Stripe / NMI / PayPal / Other Providers**

This separation allows business modules to interact with a common payment interface instead of containing provider-specific implementation details.

An important goal of the provider middleware is **extensibility**. New payment providers should be introduced primarily through their own provider implementation, without requiring significant changes to the application's core business modules.

## Current Implementation — Connections Module

I started the migration with the **Connections module**, which acts as one of the entry points for managing payment provider integrations.

The common provider layer now manages its own:

- Merchant records
- Provider connections
- Common connection operations

Provider-specific responsibilities remain inside their respective provider implementations.

For example, operations such as **Stripe credential validation** are handled by the Stripe provider implementation rather than being introduced into the common payment layer.

## Migration Approach

Instead of directly copying the behavior of the existing orchestration layer, I am reviewing the current payment flow method-by-method.

For each responsibility, I evaluate:

**Existing behavior → Actual responsibility → Correct abstraction layer → Provider-specific implementation (if required)**

This approach helps determine whether functionality belongs in the common provider layer, the provider middleware, or the individual payment provider implementation.

The objective is to build a maintainable and extensible multi-provider architecture where additional payment providers can be integrated with minimal changes to the core application.
