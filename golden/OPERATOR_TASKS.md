# Operator Tasks (Human-run setup only)

This file is the only place for human-operated setup: accounts, credentials, DNS, webhooks, billing, and production toggles.
Do not put app logic, product requirements, or development plans here.

## Accounts and service setup (placeholders)

- Cloud provider account(s) (prod + staging) with least-privilege access.
- Domain registrar + DNS hosting.
- TLS certificate management (managed service preferred).
- Email delivery provider account (transactional + notifications).
- Chat/notification provider account(s) (Slack/Teams).
- Object storage (S3-compatible) buckets and access policies.
- CDN + WAF configuration.
- Logging/monitoring provider.
- Queue/streaming provider (if used).
- CRM accounts for integration testing (Salesforce/HubSpot).
- Cloud storage apps for import integrations (Google Drive/Dropbox/Box/OneDrive).
- Automation platform account (Zapier/Make) if offered.

## Credentials and secrets rules

- Store secrets only in an approved secret manager (never in git, never in tickets, never in chat logs).
- Rotate credentials on schedule and on any suspected exposure.
- Use separate credentials per environment (staging vs prod).

## Environment variables (names only; define values in your secret manager)

Core:
- APP_ENV
- APP_BASE_URL
- LOG_LEVEL

Auth & access:
- AUTH_PROVIDER
- AUTH_JWKS_URL
- AUTH_CLIENT_ID
- AUTH_CLIENT_SECRET
- SESSION_SECRET
- ENCRYPTION_KEY_ID

Database:
- DATABASE_URL
- DATABASE_READ_REPLICA_URL (optional)

Object storage & files:
- STORAGE_PROVIDER
- STORAGE_BUCKET
- STORAGE_REGION
- STORAGE_ENDPOINT (optional, for S3-compatible)
- STORAGE_ACCESS_KEY_ID
- STORAGE_SECRET_ACCESS_KEY
- FILES_KMS_KEY_ARN (or equivalent)

Queues / events:
- QUEUE_PROVIDER
- QUEUE_URL
- EVENT_TOPIC_VIEWS
- EVENT_TOPIC_AUDIT

Email:
- EMAIL_PROVIDER
- EMAIL_FROM
- EMAIL_API_KEY

Notifications:
- SLACK_WEBHOOK_URL (optional)
- TEAMS_WEBHOOK_URL (optional)

Integrations (enable per tenant as needed):
- GOOGLE_OAUTH_CLIENT_ID
- GOOGLE_OAUTH_CLIENT_SECRET
- MICROSOFT_OAUTH_CLIENT_ID
- MICROSOFT_OAUTH_CLIENT_SECRET
- DROPBOX_OAUTH_CLIENT_ID
- DROPBOX_OAUTH_CLIENT_SECRET
- BOX_OAUTH_CLIENT_ID
- BOX_OAUTH_CLIENT_SECRET
- SALESFORCE_OAUTH_CLIENT_ID
- SALESFORCE_OAUTH_CLIENT_SECRET
- HUBSPOT_OAUTH_CLIENT_ID
- HUBSPOT_OAUTH_CLIENT_SECRET

Security/abuse controls:
- WAF_ALLOWLIST (optional)
- RATE_LIMIT_POLICY_ID (optional)

## DNS and TLS checklist

- Create DNS zone and records for:
  - App (e.g., `app.`)
  - API (e.g., `api.`)
  - Tracking/view domain if separated (optional)
- Validate TLS issuance and renewal.
- Confirm HSTS policy (if used) is correct before enabling preload.

## Webhooks (if applicable)

- Register webhook endpoints with each provider.
- Store webhook secrets in secret manager.
- Validate signature verification in staging before enabling in production.
- Define retry/backoff settings and dead-letter handling.

## Payments (only if applicable)

- Create payment provider account(s).
- Configure products/plans and tax settings.
- Configure webhook endpoints and verify signature handling.
- Run a full purchase → invoice → refund test in staging.

## Post-deploy verification (smoke-test placeholders)

- Can sign in/out in staging and prod.
- Can upload a test file and view it in the browser.
- Access controls behave as expected (turn link off, expire link, password gate, identity gate).
- Audit log entries appear for key actions (upload, share, view, revoke).
- Email/Slack notifications deliver successfully.
- Integration connections can be created and revoked without affecting core service.
- Backups run and a restore procedure is documented and tested.
