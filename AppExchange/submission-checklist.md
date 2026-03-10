# xVio Scan — AppExchange Security Review Submission Checklist

**Product:** xVio Scan (Salesforce Connected App)
**Listing Type:** API / Connected App (Freemium)
**Review Scope:** Tenant-facing auth, API endpoints, data handling, infrastructure
**Last Updated:** 2026-03-02
**Change Ticket:** CHG-106

---

## Status Legend

| Status | Meaning |
|--------|---------|
| DONE | Implemented, verified, and deployed to dev |
| IN PROGRESS | Work started, not yet complete |
| TODO | Not yet started |
| N/A | Not applicable to xVio Scan |
| BLOCKED | Waiting on a dependency |

---

## 1. Application Security

| # | Requirement | Status | Evidence / Notes |
|---|-------------|--------|------------------|
| 1.1 | All data transmitted over HTTPS/TLS 1.2+ | DONE | CloudFront enforces HTTPS. API Gateway accessed only via CloudFront (HTTPS_ONLY origin policy). TLS 1.2 minimum on all endpoints. |
| 1.2 | SSL/TLS certificates valid and properly configured | DONE | ACM-managed certificates for `*.xvio.ai` domains. Auto-renewal via AWS Certificate Manager. |
| 1.3 | HSTS enabled (Strict-Transport-Security header) | DONE | CloudFront Function adds `strict-transport-security: max-age=63072000; includeSubDomains; preload` (CHG-106 Task 4, commit 9c835a4). |
| 1.4 | X-Content-Type-Options header | DONE | CloudFront Function adds `x-content-type-options: nosniff` (CHG-106 Task 4). |
| 1.5 | X-Frame-Options header | DONE | CloudFront Function adds `x-frame-options: DENY` (CHG-106 Task 4). |
| 1.6 | X-XSS-Protection header | DONE | CloudFront Function adds `x-xss-protection: 1; mode=block` (CHG-106 Task 4). |
| 1.7 | Referrer-Policy header | DONE | CloudFront Function adds `referrer-policy: strict-origin-when-cross-origin` (CHG-106 Task 4). |
| 1.8 | Permissions-Policy header | DONE | CloudFront Function adds `permissions-policy: camera=(), microphone=(), geolocation=(), payment=()` (CHG-106 Task 4). |
| 1.9 | No mixed content (HTTP resources on HTTPS pages) | DONE | All resources served over HTTPS. CloudFront viewer protocol policy redirects HTTP to HTTPS. |
| 1.10 | CSP (Content-Security-Policy) configured for frontend | TODO | CSP meta tag or header needed on xVio Scan App frontend. Must define allowed script-src, style-src, connect-src, img-src directives. |
| 1.11 | No sensitive data in error responses | DONE | All Lambda handlers use standardized `errorResponse()` utility. No stack traces or internal details exposed. Error format: `{ error, message, correlationId }`. |
| 1.12 | Custom error pages (no server defaults) | DONE | API Gateway custom 4xx/5xx gateway responses with JSON format. CloudFront custom error responses configured. |

---

## 2. Authentication & Authorization

| # | Requirement | Status | Evidence / Notes |
|---|-------------|--------|------------------|
| 2.1 | OAuth 2.0 with PKCE for Salesforce integration | DONE | Authorization Code flow with S256 PKCE. Code verifier stored in DynamoDB with 10-min TTL. Code challenge sent in authorization URL. |
| 2.2 | No client secrets exposed to browser/mobile | DONE | Consumer Key and Consumer Secret stored in AWS Secrets Manager (`xvio/{stage}/tenant/{tenantId}/salesforce`). Token exchange happens server-side in Lambda. Frontend never sees client secret. |
| 2.3 | Tokens stored securely (httpOnly cookies or encrypted storage) | DONE | Salesforce OAuth tokens encrypted with AES-256-GCM before DynamoDB storage. xVio JWT returned via redirect (mobile deep link or web). Mobile stores in Capacitor Preferences (OS-level encryption). |
| 2.4 | JWT access token expiry enforced | DONE | Default 15 minutes, per-tenant configurable (5-1440 min) via admin console. RS256 signed. (CHG-106 Task 1, commit 12f1186). |
| 2.5 | Refresh token expiry enforced | DONE | Tied to `sessionDurationDays` (default 180 days, per-tenant configurable). (CHG-106 Task 1). |
| 2.6 | Session management with proper logout/invalidation | DONE | Logout clears all auth tokens, IndexedDB data, and localStorage (`clearAllData()`). Refresh token invalidated on logout. |
| 2.7 | Rate limiting on authentication endpoints | DONE | DynamoDB-based rate limiter: Login=10/min, Callback=20/min, Refresh=30/min per IP. Returns 429 with CORS headers. (CHG-106 Task 3, commit 9c835a4). |
| 2.8 | PKCE code verifier securely stored and single-use | DONE | Stored in DynamoDB with 10-min TTL. Deleted after successful exchange. Cannot be replayed. |
| 2.9 | Salesforce Org ID validation on login | DONE | After OAuth callback, `organization_id` from userinfo compared to tenant config `authConfig.salesforce.orgId`. Mismatch results in ACCESS DENIED + audit log. |
| 2.10 | CSRF protection on OAuth flow | DONE | `state` parameter generated via `crypto.getRandomValues()` (32 bytes), stored in sessionStorage (single-use), verified on callback. Mismatch throws CSRF error. |
| 2.11 | API key tokens scoped (not full access by default) | DONE | API keys issue scoped JWTs with explicit scopes (`jobs:read`, `jobs:write`, `*`). Scope enforcement on every handler via `authorizeTenantRequest()`. |

---

## 3. Data Security

| # | Requirement | Status | Evidence / Notes |
|---|-------------|--------|------------------|
| 3.1 | Data encrypted at rest (DynamoDB) | DONE | AWS-managed encryption enabled on all DynamoDB tables. |
| 3.2 | Sensitive fields encrypted at application level | DONE | Salesforce OAuth tokens (accessToken, refreshToken, instanceUrl) encrypted with AES-256-GCM before DynamoDB storage. Encryption version field (`v1`) supports future key rotation. |
| 3.3 | Data encrypted in transit | DONE | HTTPS everywhere: CloudFront (TLS 1.2+), API Gateway, S3 presigned URLs, AppSync WSS, Salesforce API calls. |
| 3.4 | Multi-tenant data isolation | DONE | DynamoDB partition key always includes `tenantId` (`TENANT#{tenantId}#APP#{appId}#...`). Cross-tenant queries structurally impossible. All queries validate tenant membership. |
| 3.5 | S3 file isolation | DONE | Files stored under tenant-scoped S3 key prefixes. SSE-S3 (AES-256) encryption. `blockPublicAccess: BLOCK_ALL` on all buckets. |
| 3.6 | No PII in application logs | DONE | Structured JSON logging with `tenantId`, `userId`, `action`, `correlationId`. No email addresses, names, or document content logged. IP addresses truncated in rate limiter logs. |
| 3.7 | Data retention policies documented | DONE | `deleteSourceAfterProcessing` flag per service. Diagnostic log retention configurable (default 30 days). DynamoDB TTL for automatic record expiration. Log retention: 7 days (dev), 365 days (prod). |
| 3.8 | Secrets stored in Secrets Manager (not env vars) | DONE | JWT signing key, Salesforce credentials, and integration secrets all in AWS Secrets Manager. Lambda-level 5-min memory cache to reduce API calls. |
| 3.9 | API responses mask sensitive values | DONE | Salesforce credentials masked in API responses (e.g., `3MVG9...***`). Tokens never returned in list/get endpoints. |
| 3.10 | Data cleanup on user logout | DONE | `clearAllData()` removes auth tokens, IndexedDB data, localStorage, and sync cache. |

---

## 4. Input Validation

| # | Requirement | Status | Evidence / Notes |
|---|-------------|--------|------------------|
| 4.1 | All inputs validated with schemas | DONE | Zod schema validation on all Lambda handler inputs before processing. Type-safe parsing with explicit error messages. |
| 4.2 | XSS prevention | DONE | `sanitizeText()` utility strips HTML tags, javascript/data/vbscript URLs, and on* event handlers. Svelte auto-escaping on frontend. No jsdom dependency. |
| 4.3 | SQL injection prevention | DONE | DynamoDB (NoSQL) with parameterized expressions. No raw string interpolation in query expressions. ExpressionAttributeNames and ExpressionAttributeValues used throughout. |
| 4.4 | Open redirect prevention (redirectUri allowlist) | DONE | Zod `refine()` on `redirectUri`: only `*.xvio.ai`, `*.exonpro.com` (HTTPS), `localhost` (dev), and `exon-ai-hub://` (mobile deep link) allowed. All other URLs rejected. (CHG-106 Task 2, commit 9c835a4). |
| 4.5 | File upload validation (size limits) | DONE | Base64 upload: 20MB limit. Presigned upload: 50MB limit. Enforced at Lambda handler level. |
| 4.6 | File upload validation (type checks) | DONE | MIME type detection before upload. Pipeline config validates allowed file types per service. |
| 4.7 | Request body size limits | DONE | API Gateway default 10MB payload limit. Lambda-level validation for specific endpoints. |

---

## 5. API Security

| # | Requirement | Status | Evidence / Notes |
|---|-------------|--------|------------------|
| 5.1 | WAF rate limiting (per-IP) | DONE | AWS WAF v2: 2000 req/5min (dev), 3000 req/5min (prod) per IP. OWASP Common Rules, Known Bad Inputs, SQLi Rules, IP Reputation. WAF enabled on preprod and production. |
| 5.2 | Application-level rate limiting (per-endpoint) | DONE | DynamoDB-based rate limiter on auth endpoints. Per-IP, per-operation windowed counters with TTL cleanup. (CHG-106 Task 3). |
| 5.3 | CORS properly configured (origin allowlist) | DONE | Explicit origin allowlist per environment (no wildcard with credentials). CloudFront Function validates origin on 2xx. Lambda adds CORS on 4xx/5xx. API Gateway preflight configured per resource. |
| 5.4 | API Gateway throttling | DONE | Stage throttling: 1000 req/min, 500 burst. |
| 5.5 | Request size limits enforced | DONE | API Gateway 10MB payload limit. Per-endpoint limits in Lambda handlers. |
| 5.6 | Cognito authorization on admin routes | DONE | All `/admin/*` routes use Cognito User Pool authorizer. |
| 5.7 | JWT authorization on tenant routes | DONE | All `/tenant/*` routes validated via JWT RS256 verification with `authorizeTenantRequest()`. Issuer: `api.xvio.ai`, Audience: `xvio-api`. |
| 5.8 | No wildcard CORS with credentials | DONE | CORS origins are explicit domain lists. CloudFront Function matches against allowlist patterns. |
| 5.9 | Custom WAF block response (JSON format) | DONE | WAF custom response returns JSON error format, not HTML default. |

---

## 6. Infrastructure Security

| # | Requirement | Status | Evidence / Notes |
|---|-------------|--------|------------------|
| 6.1 | IAM least-privilege policies | DONE | Lambda functions have scoped IAM roles. DynamoDB access limited to specific table/index ARNs. S3 access limited to specific bucket ARNs. Secrets Manager access scoped to `xvio/{stage}/*` prefix. |
| 6.2 | VPC configuration | N/A | Serverless architecture (Lambda, DynamoDB, S3, API Gateway). No VPC required. All AWS services accessed via AWS SDK with IAM auth. CloudFront + WAF provide edge protection. |
| 6.3 | CloudWatch monitoring and alerting | DONE | Alarms: Lambda errors (>0 in 5min), API 5xx (>5 in 5min), DynamoDB throttles (>0 in 5min), SQS DLQ messages (>0). Structured JSON logging with correlation IDs. |
| 6.4 | DynamoDB PITR enabled | DONE | `pointInTimeRecoveryEnabled: true` on all DynamoDB tables across all environments (dev, preprod, prod). 1-hour RPO target. |
| 6.5 | S3 bucket policies (no public access) | DONE | `blockPublicAccess: BLOCK_ALL` on all S3 buckets. CDK context: `@aws-cdk/aws-s3:publicAccessBlockedByDefault: true`. |
| 6.6 | Secrets in AWS Secrets Manager | DONE | JWT signing key, Salesforce Consumer Key/Secret, integration credentials all stored in Secrets Manager (not environment variables). |
| 6.7 | CloudTrail enabled | DONE | AWS CloudTrail auditing for all API calls. |
| 6.8 | CloudFront access logs | DONE | Stored in dedicated S3 bucket for access auditing. |
| 6.9 | DynamoDB Streams for event-driven processing | DONE | `NEW_AND_OLD_IMAGES` stream for audit trail and event-driven workflows. |
| 6.10 | SQS Dead Letter Queue | DONE | Failed messages routed to DLQ after 3 retries. Alarm on DLQ messages > 0. |

---

## 7. Third-Party Dependencies

| # | Requirement | Status | Evidence / Notes |
|---|-------------|--------|------------------|
| 7.1 | All dependencies use permissive licenses (MIT, Apache-2.0, ISC, BSD) | TODO | Run `npx license-checker --production --summary` on backend/ and each frontend. Document all licenses. Verify no GPL/AGPL/SSPL in production deps. |
| 7.2 | No known critical vulnerabilities | TODO | Run `npm audit --production` on backend/ and frontends. Fix or document all critical/high findings. |
| 7.3 | Dependency inventory documented | DONE | `docs/xVio Scan/AppExchange/dependency-inventory.md` — 92 deps listed, all permissive licenses. (CHG-106 Task 10, commit 3e8a76d). |
| 7.4 | No dependency on jsdom or similar heavy DOM libraries | DONE | Removed jsdom dependency (CHG-041). XSS sanitization uses lightweight regex-based `sanitizeText()`. |

---

## 8. Documentation for Submission

| # | Requirement | Status | Evidence / Notes |
|---|-------------|--------|------------------|
| 8.1 | Architecture documentation | DONE | `docs/xVio Scan/AppExchange/architecture.md` — 19 sections, diagrams included. (CHG-106 Task 8, commit 3e8a76d). |
| 8.2 | OAuth flow documentation | DONE | `docs/xVio Scan/AppExchange/oauth-flow.md` — 4 mermaid sequence diagrams, all endpoints documented. (CHG-106 Task 9, commit 3e8a76d). |
| 8.3 | Data flow documentation | TODO | Document how data moves: user upload -> S3 -> SQS -> Textract -> Bedrock -> DynamoDB -> Salesforce export. Include encryption at each stage. |
| 8.4 | Dependency inventory | DONE | `docs/xVio Scan/AppExchange/dependency-inventory.md` — 92 deps, all permissive. (CHG-106 Task 10, commit 3e8a76d). |
| 8.5 | Privacy policy | TODO | Public-facing privacy policy for `xvio.ai`. Required for AppExchange listing. Must cover: data collected, data usage, data retention, third-party sharing, user rights. |
| 8.6 | Terms of service | TODO | Public-facing ToS for `xvio.ai`. Required for AppExchange listing. |
| 8.7 | Security review questionnaire responses | TODO | Salesforce provides a security questionnaire during review. Pre-draft answers based on this checklist. |

---

## 9. Security Testing

| # | Requirement | Status | Evidence / Notes |
|---|-------------|--------|------------------|
| 9.1 | SSL/TLS scan script created | DONE | `scripts/security/ssl-scan.sh` — testssl.sh wrapper for all endpoints. (CHG-106 Task 5, commit 1de87eb). |
| 9.2 | DAST scan script created | DONE | `scripts/security/dast-scan.sh` — OWASP ZAP wrapper for DAST scanning. (CHG-106 Task 6, commit 1de87eb). |
| 9.3 | SSL/TLS scan passed on staging | DONE | testssl.sh: 431 findings (0C/0H/0M/15L/186OK/229INFO). TLS 1.2+ only. (CHG-106 Task 7, commit fec0714). |
| 9.4 | DAST scan passed on staging | DONE | ZAP baseline: 65 PASS, 2 WARN (HSTS + non-storable), 0 FAIL. (CHG-106 Task 7, commit fec0714). |
| 9.5 | SSL/TLS scan passed on production | DONE | testssl.sh: 430 findings (0C/0H/0M/16L/184OK/229INFO) across 4 CloudFront IPs. All 15+ CVEs: NOT VULNERABLE. (CHG-106 Task 13, commit 26dfe60). |
| 9.6 | DAST scan passed on production | DONE | ZAP baseline: 118 PASS, 1 WARN, 0 FAIL. ZAP API: 118 PASS, 2 WARN, 0 FAIL. (CHG-106 Task 13, commit 26dfe60). |
| 9.7 | No critical/high findings in scan results | DONE | 0 CRITICAL, 0 HIGH across all SSL/TLS and DAST scans (staging + production). Only LOW: HSTS on error responses (known F-002). |
| 9.8 | Penetration test (if required by Salesforce) | TODO | Salesforce may require third-party pen test for API-type listings. Budget and schedule TBD. |
| 9.9 | Security scan reports archived | DONE | All reports in `scripts/security/reports/`: ssl-report-*, dast-*-report-*, stage-security-scan-*.html, production-security-scan-2026-03-02.md. |

---

## 10. Salesforce-Specific Requirements

| # | Requirement | Status | Evidence / Notes |
|---|-------------|--------|------------------|
| 10.1 | Connected App configured correctly | DONE | OAuth 2.0 Authorization Code with PKCE. Scopes: `api`, `refresh_token`. Callback URL: `https://api-{stage}.xvio.ai/v1/tenant/auth/callback`. |
| 10.2 | OAuth scopes minimal | DONE | Only `api` (REST API access) and `refresh_token` (offline access). No `full`, `web`, or `chatter_api` scopes. |
| 10.3 | Callback URL restricted | DONE | Single callback URL per environment. URL validated in both Connected App config and backend `redirectUri` allowlist. |
| 10.4 | No hardcoded Salesforce credentials | DONE | Consumer Key and Consumer Secret stored in AWS Secrets Manager. Per-tenant credential isolation. No credentials in source code, environment variables, or config files. |
| 10.5 | Handles Salesforce API limits gracefully | DONE | Auto-refresh on 401. Retry logic with exponential backoff. Rate limit errors surfaced to user with clear messaging. |
| 10.6 | Supports sandbox and production environments | DONE | `test.salesforce.com` for sandbox, `login.salesforce.com` for production. Audience detection based on instance URL (CHG-105, commit 118018a). |
| 10.7 | Consumer Secret rotation supported | DONE | Admin Console endpoint: `PUT /admin/tenants/{tenantId}/salesforce-credentials`. Lambda cache auto-clears within 5 minutes. Documented rotation procedure. |
| 10.8 | Salesforce token auto-refresh | DONE | On 401 from Salesforce API: refresh token exchange -> re-encrypt -> store -> retry original request. If refresh fails, user re-authenticates via OAuth. |
| 10.9 | Salesforce userinfo validated | DONE | After token exchange, `/userinfo` called to verify `organization_id`, `email`, and user identity before issuing xVio JWT. |
| 10.10 | Audit logging for Salesforce auth events | DONE | `AUTH_LOGIN_SUCCESS`, `AUTH_LOGIN_FAILURE`, `CREDENTIAL_UPDATE`, `CREDENTIAL_DELETE` events logged with tenant/user/IP context. |
| 10.11 | No Salesforce data stored unnecessarily | DONE | Only OAuth tokens (encrypted), instance URL (encrypted), and basic user profile (email, name) stored. No Salesforce record data persisted beyond export operation. |

---

## 11. Mobile App Requirements (iOS + Android)

| # | Requirement | Status | Evidence / Notes |
|---|-------------|--------|------------------|
| 11.1 | Secure token storage on device | DONE | Capacitor Preferences with OS-level encryption (iOS Keychain / Android EncryptedSharedPreferences). |
| 11.2 | Platform-appropriate OAuth browser | DONE | iOS: ASWebAuthenticationSession. Android: Chrome Custom Tabs. Prevents redirect hijacking. |
| 11.3 | Deep link validation | DONE | `xvio://` scheme validated. One-time 60-second exchange codes for mobile deep links. |
| 11.4 | No sensitive data in app logs | TODO | Verify no tokens, PII, or credentials logged in mobile app debug output. Test with release build. |
| 11.5 | App Transport Security (iOS) | TODO | Verify ATS enabled and no exceptions in `Info.plist`. All connections must be HTTPS. |
| 11.6 | Network Security Config (Android) | TODO | Verify `network_security_config.xml` does not allow cleartext traffic. No user-installed CA trust. |
| 11.7 | Mobile app installable and testable by reviewer | TODO | Provide TestFlight (iOS) and Google Play Internal Testing (Android) links to Salesforce reviewer. |

---

## 12. AppExchange Listing Requirements

| # | Requirement | Status | Evidence / Notes |
|---|-------------|--------|------------------|
| 12.1 | AppExchange listing created | IN PROGRESS | Listing research documented in `docs/xVio Scan/appexchange-listing-research.html`. |
| 12.2 | App logo and screenshots | TODO | Prepare app icon, feature screenshots, and listing graphics per AppExchange asset guidelines. |
| 12.3 | Listing description | TODO | Short description, long description, key features, and supported editions. |
| 12.4 | Support contact and documentation URL | TODO | Provide support email, documentation URL, and getting started guide URL. |
| 12.5 | Data processing agreement (if applicable) | TODO | May be required if processing EU customer data. Review GDPR implications. |
| 12.6 | Security review fee paid (if applicable) | N/A | Free for freemium listings. |

---

## Summary

| Category | DONE | IN PROGRESS | TODO | BLOCKED | N/A | Total |
|----------|------|-------------|------|---------|-----|-------|
| 1. Application Security | 11 | 0 | 1 | 0 | 0 | 12 |
| 2. Authentication & Authorization | 11 | 0 | 0 | 0 | 0 | 11 |
| 3. Data Security | 10 | 0 | 0 | 0 | 0 | 10 |
| 4. Input Validation | 7 | 0 | 0 | 0 | 0 | 7 |
| 5. API Security | 9 | 0 | 0 | 0 | 0 | 9 |
| 6. Infrastructure Security | 9 | 0 | 0 | 0 | 1 | 10 |
| 7. Third-Party Dependencies | 2 | 0 | 2 | 0 | 0 | 4 |
| 8. Documentation for Submission | 3 | 0 | 4 | 0 | 0 | 7 |
| 9. Security Testing | 8 | 0 | 1 | 0 | 0 | 9 |
| 10. Salesforce-Specific | 11 | 0 | 0 | 0 | 0 | 11 |
| 11. Mobile App | 3 | 0 | 4 | 0 | 0 | 7 |
| 12. AppExchange Listing | 0 | 1 | 4 | 0 | 1 | 6 |
| **TOTAL** | **84** | **1** | **16** | **0** | **2** | **103** |

**Overall Readiness: 84/103 items complete (82%)**

---

## Remaining Items (16 TODO + 1 IN PROGRESS)

### Must-have for submission
1. **1.10** CSP header on xVio Scan App frontend (separate repo)
2. **7.1** License audit (`npx license-checker --production`)
3. **7.2** Vulnerability audit (`npm audit --production`)
4. **8.3** Data flow documentation
5. **8.5** Privacy policy for xvio.ai
6. **8.6** Terms of service for xvio.ai
7. **8.7** Security review questionnaire pre-draft

### Mobile verification
8. **11.4** No sensitive data in mobile app logs (release build)
9. **11.5** iOS App Transport Security check
10. **11.6** Android Network Security Config check
11. **11.7** TestFlight + Play Internal Testing links

### AppExchange listing
12. **12.1** Listing creation (IN PROGRESS)
13. **12.2** App logo and screenshots
14. **12.3** Listing description
15. **12.4** Support contact and docs URL
16. **12.5** Data processing agreement (GDPR)

### Optional
17. **9.8** Third-party penetration test (if required by Salesforce)

---

*Copyright 2026 ExonPro Innovations LLP. All Rights Reserved.*
