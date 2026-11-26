---
starter_id: custom
custom_starter:
  name: ASP.NET Core Web API + Blazor WebAssembly (standalone) + Shared
  docs_url: https://learn.microsoft.com/aspnet/core/blazor/security/webassembly/standalone-with-identity
package_manager: dotnet
project_name: zgloszenia-wspolnoty
hints:
  language_family: dotnet
  team_size: solo
  solution_layout: "Api (ASP.NET Core Web API) + Client (Blazor WASM standalone) + Shared (DTOs, validation)"
  frontend: blazor-wasm-standalone
  auth_strategy: "ASP.NET Core Identity user store + self-issued JWT access token (~15 min, role claims) + rotating refresh token persisted in DB; registration disabled; no external IdP"
  database: "Azure SQL Database (EF Core + Microsoft.EntityFrameworkCore.SqlServer); prod: Azure SQL Basic (DTU, ~5 USD/mo, no auto-pause); local: SQL Server 2022 container via docker compose (Rosetta on Apple Silicon)"
  deployment_target: azure-container-apps
  containerization: "single image (Api serves Blazor WASM static files); Dockerfile in repo; image pushed to GHCR from GitHub Actions; DB never containerized in prod"
  local_dev: "docker compose for SQL Server; app run via dotnet watch"
  deployment_priority: "early-skeleton: public deploy is MVP scope; first Azure deploy right after the first working feature (login + ticket list), then auto-deploy on every merge"
  ci_provider: github-actions
  ci_default_flow: auto-deploy-on-merge
  bootstrapper_confidence: best-effort
  path_taken: custom
  quality_override: false
  self_check_answers:
    typed: true
    from_official_starter: true
    conventions: true
    docs_current: true
    can_judge_agent: true
  has_auth: true
  has_payments: false
  has_realtime: false
  has_ai: false
  has_background_jobs: false
---

## Why this stack

A solo developer with years of .NET Framework 4.8 and older (MVC 5 era) experience, no React knowledge and a hard deadline of 2027-01-10 (3-week after-hours MVP), wants a separate API and frontend with self-implemented JWT authentication and authorization. Delivering on time matters most. The pick is an ASP.NET Core Web API backend plus a standalone Blazor WebAssembly client, with a Shared project for DTOs and DataAnnotations validation. Everything is C#, so the user can judge agent output, and the API contract cannot drift between client and server. This beats React+Vite, which would mean learning a new language and framework under deadline, and the Blazor Web App render-mode model, which is newer and easily mixed up by agents. Identity stores users and the three roles, with self-registration disabled because administration creates accounts. The API issues short-lived JWT access tokens carrying role claims, plus rotating refresh tokens stored in Azure SQL via EF Core (SQL Server provider, familiar to the user from .NET Framework work and the default, best-supported EF Core/Identity provider). Endpoints are protected with `AddJwtBearer` and role/policy `[Authorize]`, and no external IdP is used, to protect the timeline. The agent implements auth (TokenService, login/refresh/logout endpoints, role policies, client AuthenticationStateProvider + token DelegatingHandler) with integration tests, and the user reviews it; budget roughly 1–2 evenings against the PRD's FR scope. The API also serves the WASM static files, so there is a single container image, one origin and no CORS. GitHub Actions builds the image, pushes it to GHCR and deploys to Azure Container Apps on merge (scale-to-zero keeps cost low at this traffic). Production data lives in Azure SQL Database Basic (~5 USD/month, no serverless auto-pause, managed backups), never in a container. Locally, SQL Server 2022 runs via docker compose and the app via `dotnet watch`. The app must be publicly reachable by the deadline, so deployment is MVP scope: the first deploy happens right after the first working feature (walking skeleton), not at the end. The generated CLAUDE.md should carry .NET Framework→Core migration notes (Program.cs/DI/middleware/appsettings, no System.Web), Blazor WASM component conventions and the JWT/refresh-token flow.
