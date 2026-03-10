# xVio Scan -- Third-Party Dependency Inventory

**Prepared for:** Salesforce AppExchange Security Review
**Application:** xVio Scan v1.1.0
**Prepared by:** ExonPro Innovations LLP
**Date:** 2026-02-24
**Last Updated:** 2026-02-24

---

## Table of Contents

1. [Overview](#overview)
2. [Backend Runtime Dependencies](#1-backend-runtime-dependencies)
   - [AWS SDK Packages](#11-aws-sdk-packages)
   - [Authentication / Security](#12-authentication--security)
   - [Email Processing](#13-email-processing)
   - [Validation / Utility](#14-validation--utility)
3. [Infrastructure Dependencies (CDK)](#2-infrastructure-dependencies-cdk)
4. [Frontend Dependencies (xVio Scan App)](#3-frontend-dependencies-xvio-scan-app)
   - [Runtime Dependencies](#31-runtime-dependencies)
   - [Dev Dependencies](#32-frontend-dev-dependencies)
5. [Backend Dev Dependencies](#4-backend-dev-dependencies)
6. [Infrastructure Dev Dependencies](#5-infrastructure-dev-dependencies)
7. [License Summary](#license-summary)
8. [Risk Assessment](#risk-assessment)
9. [Methodology](#methodology)

---

## Overview

This document provides a complete inventory of all third-party dependencies used by the xVio Scan application across three components:

- **Backend** -- AWS Lambda handlers and business logic (TypeScript)
- **Infrastructure** -- AWS CDK stacks for cloud resource provisioning
- **Frontend** -- xVio Scan mobile/web app (Svelte + Capacitor; separate repository)

All dependency license data was verified using `license-checker --production` against installed `node_modules` as of 2026-02-24.

---

## 1. Backend Runtime Dependencies

### 1.1 AWS SDK Packages

All AWS SDK v3 packages are published by Amazon Web Services under the **Apache-2.0** license.

| # | Package | Installed Version | License | Purpose |
|---|---------|-------------------|---------|---------|
| 1 | `@aws-sdk/client-apigatewaymanagementapi` | 3.962.0 | Apache-2.0 | WebSocket API message delivery to connected clients |
| 2 | `@aws-sdk/client-bedrock-agent-runtime` | 3.988.0 | Apache-2.0 | Invoke Bedrock agents for AI-powered document processing |
| 3 | `@aws-sdk/client-bedrock-agentcore` | 3.988.0 | Apache-2.0 | Bedrock AgentCore gateway for memory and tool orchestration |
| 4 | `@aws-sdk/client-bedrock-runtime` | 3.988.0 | Apache-2.0 | Direct Bedrock model invocation for AI inference |
| 5 | `@aws-sdk/client-cloudwatch` | 3.962.0 | Apache-2.0 | Publish custom CloudWatch metrics for monitoring |
| 6 | `@aws-sdk/client-cloudwatch-logs` | 3.962.0 | Apache-2.0 | Query and manage CloudWatch log groups |
| 7 | `@aws-sdk/client-cognito-identity-provider` | 3.962.0 | Apache-2.0 | Admin user pool management (Cognito) |
| 8 | `@aws-sdk/client-dynamodb` | 3.962.0 | Apache-2.0 | Low-level DynamoDB operations |
| 9 | `@aws-sdk/client-kms` | 3.962.0 | Apache-2.0 | Encryption key management via AWS KMS |
| 10 | `@aws-sdk/client-route-53` | 3.962.0 | Apache-2.0 | DNS record management for custom domains |
| 11 | `@aws-sdk/client-s3` | 3.962.0 | Apache-2.0 | S3 object storage for document uploads and scans |
| 12 | `@aws-sdk/client-secrets-manager` | 3.962.0 | Apache-2.0 | Retrieve tenant secrets (API keys, credentials) |
| 13 | `@aws-sdk/client-ses` | 3.967.0 | Apache-2.0 | Send transactional emails via SES |
| 14 | `@aws-sdk/client-sesv2` | 3.980.0 | Apache-2.0 | SES v2 API for email sending and configuration |
| 15 | `@aws-sdk/client-sns` | 3.980.0 | Apache-2.0 | Publish notifications via SNS topics |
| 16 | `@aws-sdk/client-sqs` | 3.962.0 | Apache-2.0 | Queue message processing via SQS |
| 17 | `@aws-sdk/client-ssm` | 3.962.0 | Apache-2.0 | Read configuration from SSM Parameter Store |
| 18 | `@aws-sdk/client-sts` | 3.975.0 | Apache-2.0 | Assume IAM roles and get temporary credentials |
| 19 | `@aws-sdk/client-textract` | 3.962.0 | Apache-2.0 | Document text extraction via AWS Textract |
| 20 | `@aws-sdk/lib-dynamodb` | 3.962.0 | Apache-2.0 | High-level DynamoDB document client |
| 21 | `@aws-sdk/protocol-http` | 3.370.0 | Apache-2.0 | HTTP request/response protocol utilities |
| 22 | `@aws-sdk/s3-request-presigner` | 3.962.0 | Apache-2.0 | Generate pre-signed S3 URLs for secure uploads/downloads |
| 23 | `@aws-sdk/signature-v4` | 3.370.0 | Apache-2.0 | AWS Signature V4 request signing |
| 24 | `@aws-sdk/util-dynamodb` | 3.962.0 | Apache-2.0 | DynamoDB attribute marshalling/unmarshalling |
| 25 | `@aws-sdk/util-utf8-node` | 3.259.0 | Apache-2.0 | UTF-8 encoding utilities for Node.js |

### 1.2 Authentication / Security

| # | Package | Installed Version | License | Purpose |
|---|---------|-------------------|---------|---------|
| 26 | `aws-jwt-verify` | 4.0.1 | Apache-2.0 | Verify Cognito JWT tokens for admin authentication |
| 27 | `jose` | 6.1.3 | MIT | JWT signing, verification, and encryption for tenant auth (SFDC Connected App) |

### 1.3 Email Processing

| # | Package | Installed Version | License | Purpose |
|---|---------|-------------------|---------|---------|
| 28 | `mailparser` | 3.9.3 | MIT | Parse inbound email messages (MIME parsing) for email-to-scan feature |

### 1.4 Validation / Utility

| # | Package | Installed Version | License | Purpose |
|---|---------|-------------------|---------|---------|
| 29 | `zod` | 3.25.76 | MIT | Runtime schema validation for API request/response payloads |
| 30 | `aws-lambda` | 1.0.7 | MIT | AWS Lambda runtime utilities |

---

## 2. Infrastructure Dependencies (CDK)

These packages are used exclusively at deploy-time for infrastructure provisioning. They are **not** included in the runtime application bundle.

| # | Package | Installed Version | License | Category | Purpose |
|---|---------|-------------------|---------|----------|---------|
| 1 | `aws-cdk-lib` | 2.221.0 | Apache-2.0 | Runtime | Core AWS CDK library for all infrastructure constructs |
| 2 | `constructs` | 10.4.4 | Apache-2.0 | Runtime | Base construct library required by CDK |
| 3 | `@aws-cdk/aws-bedrock-agentcore-alpha` | 2.221.0-alpha.0 | Apache-2.0 | Runtime | CDK L2 constructs for Bedrock AgentCore resources |

---

## 3. Frontend Dependencies (xVio Scan App)

> **Note:** The frontend (xvio-scan-app) is maintained in a separate Git repository. This inventory is included for completeness based on the package.json present at the gitignored path `frontends/xvio-scan-app/`.

### 3.1 Runtime Dependencies

| # | Package | Version (package.json) | License | Purpose |
|---|---------|------------------------|---------|---------|
| 1 | `@aspect-ops/exon-ui` | ^0.4.3 | Proprietary (ExonPro) | ExonPro's internal UI component library |
| 2 | `@capacitor/android` | ^6.0.0 | MIT | Capacitor Android runtime bridge |
| 3 | `@capacitor/app` | ^6.0.3 | MIT | App state lifecycle management (foreground/background) |
| 4 | `@capacitor/browser` | ^6.0.6 | MIT | In-app browser for OAuth flows and external links |
| 5 | `@capacitor/camera` | ^6.1.3 | MIT | Native camera access for document scanning |
| 6 | `@capacitor/clipboard` | ^6.0.3 | MIT | System clipboard read/write |
| 7 | `@capacitor/core` | ^6.0.0 | MIT | Capacitor core runtime for native bridge communication |
| 8 | `@capacitor/filesystem` | ^6.0.4 | MIT | Read/write files on device storage |
| 9 | `@capacitor/haptics` | ^6.0.3 | MIT | Haptic feedback for touch interactions |
| 10 | `@capacitor/ios` | ^6.2.1 | MIT | Capacitor iOS runtime bridge |
| 11 | `@capacitor/keyboard` | ^6.0.4 | MIT | Keyboard show/hide events and control |
| 12 | `@capacitor/preferences` | ^6.0.4 | MIT | Key-value persistent storage (device-local) |
| 13 | `@capacitor/push-notifications` | ^6.0.5 | MIT | Push notification registration and handling |
| 14 | `@capacitor/share` | ^6.0.4 | MIT | Native share sheet for document sharing |
| 15 | `@capacitor/status-bar` | ^6.0.3 | MIT | Status bar styling and visibility control |
| 16 | `idb` | ^8.0.3 | ISC | Lightweight IndexedDB wrapper for local data caching |
| 17 | `lucide-svelte` | ^0.562.0 | ISC | Icon library (Svelte bindings for Lucide icons) |
| 18 | `mermaid` | ^11.12.2 | MIT | Diagram/chart rendering for AI-generated visualizations |
| 19 | `zod` | ^4.3.5 | MIT | Runtime schema validation for API payloads |

### 3.2 Frontend Dev Dependencies

| # | Package | Version (package.json) | License | Purpose |
|---|---------|------------------------|---------|---------|
| 1 | `@capacitor/assets` | ^3.0.5 | MIT | Generate app icons and splash screens |
| 2 | `@capacitor/cli` | ^6.0.0 | MIT | Capacitor CLI for build and sync commands |
| 3 | `@sveltejs/adapter-static` | ^3.0.0 | MIT | SvelteKit static site adapter for Capacitor |
| 4 | `@sveltejs/kit` | ^2.0.0 | MIT | SvelteKit application framework |
| 5 | `@sveltejs/vite-plugin-svelte` | ^4.0.0 | MIT | Vite plugin for Svelte compilation |
| 6 | `svelte` | ^5.0.0 | MIT | Svelte reactive UI framework |
| 7 | `svelte-check` | ^4.0.0 | MIT | Svelte type-checking and diagnostics |
| 8 | `typescript` | ^5.0.0 | Apache-2.0 | TypeScript compiler |
| 9 | `vite` | ^5.0.0 | MIT | Build tool and dev server |

---

## 4. Backend Dev Dependencies

These packages are used only during development, testing, and build. They are **not** included in the deployed Lambda bundles.

| # | Package | Version (package.json) | License | Purpose |
|---|---------|------------------------|---------|---------|
| 1 | `@aws-sdk/client-lambda` | ^3.995.0 | Apache-2.0 | Invoke Lambda functions in API integration tests |
| 2 | `@aws-sdk/client-sfn` | ^3.995.0 | Apache-2.0 | Step Functions client for integration tests |
| 3 | `@aws-sdk/credential-providers` | ^3.971.0 | Apache-2.0 | AWS credential resolution for test environments |
| 4 | `@jest/globals` | ^29.7.0 | MIT | Jest global test utilities (expect, describe, it) |
| 5 | `@types/aws-lambda` | ^8.10.145 | MIT | TypeScript type definitions for AWS Lambda events |
| 6 | `@types/jest` | ^29.5.13 | MIT | TypeScript type definitions for Jest |
| 7 | `@types/jsonwebtoken` | ^9.0.10 | MIT | TypeScript type definitions for jsonwebtoken |
| 8 | `@types/mailparser` | ^3.4.6 | MIT | TypeScript type definitions for mailparser |
| 9 | `@types/node` | ^20.14.15 | MIT | TypeScript type definitions for Node.js |
| 10 | `@types/uuid` | ^10.0.0 | MIT | TypeScript type definitions for uuid |
| 11 | `@typescript-eslint/eslint-plugin` | ^7.18.0 | MIT | ESLint rules for TypeScript |
| 12 | `@typescript-eslint/parser` | ^7.18.0 | BSD-2-Clause | ESLint parser for TypeScript syntax |
| 13 | `@vitest/coverage-v8` | ^1.6.0 | MIT | Code coverage via V8 engine for Vitest |
| 14 | `aws-sdk-client-mock` | ^4.0.1 | MIT | Mock AWS SDK clients in unit tests |
| 15 | `esbuild` | ^0.27.1 | MIT | Ultra-fast JS/TS bundler for Lambda packaging |
| 16 | `eslint` | ^8.57.0 | MIT | JavaScript/TypeScript linting |
| 17 | `jest` | ^29.7.0 | MIT | Test runner for unit and integration tests |
| 18 | `jsonwebtoken` | ^9.0.2 | MIT | Generate JWTs for test authentication fixtures |
| 19 | `prettier` | ^3.3.3 | MIT | Code formatting |
| 20 | `ts-jest` | ^29.4.6 | MIT | Jest transformer for TypeScript files |
| 21 | `ts-node` | ^10.9.2 | MIT | TypeScript execution engine for scripts |
| 22 | `typescript` | ^5.5.4 | Apache-2.0 | TypeScript compiler |
| 23 | `vitest` | ^1.6.0 | MIT | Alternative test runner (Vite-native) |

---

## 5. Infrastructure Dev Dependencies

| # | Package | Version (package.json) | License | Purpose |
|---|---------|------------------------|---------|---------|
| 1 | `@types/jest` | ^29.5.14 | MIT | TypeScript type definitions for Jest |
| 2 | `@types/node` | 22.7.9 | MIT | TypeScript type definitions for Node.js |
| 3 | `aws-cdk` | 2.1030.0 | Apache-2.0 | AWS CDK CLI toolkit |
| 4 | `esbuild` | ^0.27.2 | MIT | JS/TS bundler for Lambda asset compilation |
| 5 | `jest` | ^29.7.0 | MIT | Test runner for infrastructure tests |
| 6 | `ts-jest` | ^29.2.5 | MIT | Jest transformer for TypeScript |
| 7 | `ts-node` | ^10.9.2 | MIT | TypeScript execution engine |
| 8 | `typescript` | ~5.6.3 | Apache-2.0 | TypeScript compiler |

---

## License Summary

### Backend (Production -- 325 total transitive packages)

| License | Count | Percentage |
|---------|------:|------------|
| Apache-2.0 | 233 | 71.7% |
| MIT | 76 | 23.4% |
| BSD-2-Clause | 6 | 1.8% |
| ISC | 3 | 0.9% |
| BSD-3-Clause | 2 | 0.6% |
| 0BSD | 2 | 0.6% |
| MIT-0 | 1 | 0.3% |
| (MIT OR EUPL-1.1+) | 1 | 0.3% |
| UNLICENSED | 1 | 0.3% |

> The single UNLICENSED entry is `exon-ai-hub-backend@0.1.0` -- this is the application itself (not a third-party dependency).

### Infrastructure (Production -- 42 total transitive packages)

| License | Count | Percentage |
|---------|------:|------------|
| MIT | 27 | 64.3% |
| Apache-2.0 | 7 | 16.7% |
| ISC | 4 | 9.5% |
| BSD-3-Clause | 2 | 4.8% |
| (MIT OR GPL-3.0-or-later) | 1 | 2.4% |
| UNKNOWN | 1 | 2.4% |

> The UNKNOWN entry is `infrastructure@0.1.0` -- this is the infrastructure project itself (not a third-party dependency).
> The `(MIT OR GPL-3.0-or-later)` entry is the `case@1.6.3` package (internal transitive dependency of `aws-cdk-lib`). Since it is dual-licensed under MIT, the MIT license applies.

### Direct Dependency Count (declared in package.json)

| Component | Runtime | Dev | Total |
|-----------|--------:|----:|------:|
| Backend | 30 | 23 | 53 |
| Infrastructure | 3 | 8 | 11 |
| Frontend | 19 | 9 | 28 |
| **Total** | **52** | **40** | **92** |

---

## Risk Assessment

### Non-Permissive License Flags

| Package | License | Location | Risk | Mitigation |
|---------|---------|----------|------|------------|
| `case@1.6.3` | (MIT OR GPL-3.0-or-later) | Infrastructure (transitive, inside aws-cdk-lib) | **None** | Dual-licensed; MIT applies. Deploy-time only, not in runtime bundle. |
| `@zone-eu/mailsplit@5.4.8` | (MIT OR EUPL-1.1+) | Backend (transitive, via mailparser) | **None** | Dual-licensed; MIT applies. |

### GPL / AGPL / SSPL Dependencies

**None found.** No dependencies use GPL-only, AGPL, SSPL, or any other copyleft-only license.

### Compliance Statement

All third-party dependencies used by xVio Scan -- across backend, infrastructure, and frontend components -- are distributed under **permissive open-source licenses** (Apache-2.0, MIT, ISC, BSD-2-Clause, BSD-3-Clause, 0BSD, MIT-0). No copyleft-only licensed packages are included.

The two dual-licensed packages (`case` and `@zone-eu/mailsplit`) both offer MIT as an option, which is the license under which they are used.

All AWS SDK packages (constituting 71.7% of backend transitive dependencies) are first-party Amazon Web Services libraries licensed under Apache-2.0.

The `@aspect-ops/exon-ui` package listed in the frontend is a proprietary internal library owned by ExonPro Innovations LLP and does not introduce any third-party license obligations.

---

## Methodology

1. **Source files reviewed:**
   - `backend/package.json` -- Backend direct dependencies
   - `infrastructure/package.json` -- Infrastructure direct dependencies
   - `frontends/xvio-scan-app/package.json` -- Frontend direct dependencies (separate repository; included for completeness)

2. **License verification tool:**
   - `npx license-checker --json --production` executed against installed `node_modules` for both backend and infrastructure components
   - Results verified against npm registry metadata for all direct dependencies

3. **Scope:**
   - Production (runtime) dependencies: fully audited with transitive dependency tree
   - Dev dependencies: catalogued from package.json; not deployed to production

4. **Date of audit:** 2026-02-24

---

*Prepared by ExonPro Innovations LLP for Salesforce AppExchange Security Review submission.*
