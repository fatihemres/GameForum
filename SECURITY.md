# Security notes

GameForum is an **educational / historical project** that has not undergone a production security assessment. Please do not deploy it publicly as-is.

## Credential exposure

The API historically included fixed secrets in `Game.Forum.Api/appsettings.json`. A proposed cleanup removes those values from the tracked configuration and validates secrets provided through standard .NET configuration providers. This does not make historical Git commits private or erase previously disclosed values.

- Treat all previously committed signing and encryption keys as exposed.
- Rotate JWT signing keys and revoke affected sessions where applicable.
- The legacy account password scheme uses reversible AES with a fixed IV: changing its key invalidates existing password verification. Plan account reset or data migration before any rotation in an environment with users.
- Before exposing the service, replace reversible password encryption with salted, adaptive password hashing and migrate affected accounts.
- Reassess all dependencies and other authentication/authorization flows; the original project targets out-of-support .NET 6.
- Use ASP.NET Core User Secrets for local development and a proper secret manager/environment configuration in deployment.

## Reporting

Please do not open a public issue containing credentials, exploit instructions, or personal data. Contact the repository owner privately instead.

A passing build alone is not a security audit. This document does not claim the above architectural issues are resolved.
