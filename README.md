# GameForum

An experimental gaming forum built with ASP.NET Core, a separate MVC frontend, SQL Server, Entity Framework Core, Redis-backed caching, authentication, and a layered solution structure.

> **Status:** Historical learning project (2023), under review before further development. This is not currently presented as a secure or production-ready deployment. The original API targets .NET 6 and uses older dependencies.

## Solution overview

- `Game.Forum.Api` — ASP.NET Core API, JWT authentication and Swagger in development.
- `Game.Forum.UI` — MVC web frontend.
- `Game.Forum.Application`, `Game.Forum.Domain` — application logic and domain abstractions.
- `Game.Forum.Infrastructure`, `Game.Forum.Persistence` — external services, SQL Server and persistence.
- `Game.Forum.Utils` — shared utilities.

## Development

1. Install the SDK appropriate for the current projects and make a local SQL Server / SQL Express database available.
2. Review the SQL Server connection string in `Game.Forum.Api/appsettings.json` for your machine. Do **not** commit passwords or environment-specific secrets.
3. Set the following secrets locally. In development you can use ASP.NET Core User Secrets:

   ```powershell
   dotnet user-secrets init --project Game.Forum.Api
   dotnet user-secrets set "Jwt:SigningKey" "<a-new-random-secret-with-32-or-more-utf8-bytes>" --project Game.Forum.Api
   dotnet user-secrets set "AppSettings:SecretKey" "<a-new-secret-of-exactly-32-utf8-bytes>" --project Game.Forum.Api
   ```

   In other environments, set `Jwt__SigningKey` and `AppSettings__SecretKey` via a secure secret manager or environment variables. The API deliberately refuses to start when either secret is missing or of an invalid length.

4. Restore/build the solution with `dotnet restore GameForum.sln` and `dotnet build GameForum.sln`. This project has **not** been rebuilt as part of the current documentation/security update.

## Important: migration and security

**Do not rotate `AppSettings:SecretKey` against a live existing user database without a user migration/reset plan.** The legacy account implementation uses deterministic reversible AES encryption with a fixed IV instead of a password hash. Changing the key makes legacy passwords fail verification. Do not reuse the previously committed credentials. For a real deployment, migrate to a supported .NET version and modern password hashing (for example ASP.NET Core `PasswordHasher`), then handle existing user accounts explicitly. Rotating the JWT signing key invalidates existing signed tokens.

The previous secrets were committed to public Git history: removing their current-file copies does **not** erase exposure in history. Treat them as compromised and rotate/retire them as applicable. Also review `SECURITY.md`.

## Related project

- [SteamAPI](https://github.com/fatihemres/SteamAPI) — modernized standalone .NET API for Steam store search.

## License

No license has been selected for this repository. Please obtain the owner's permission before reuse.
