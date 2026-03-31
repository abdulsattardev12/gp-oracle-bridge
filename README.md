# GP Oracle Bridge

Layered .NET Framework integration service that exposes Web API endpoints for Microsoft Dynamics GP transaction processing and authentication workflows.

## Overview

This repository is organized into four projects:

- `GPAdapter`: API layer (ASP.NET Web API)
- `GPBusinessLayer`: business logic and orchestration
- `GPDataLayer`: DTOs, models, enums, and integration data contracts
- `GPUtils`: shared logging and utility components

The API receives authenticated requests, validates payloads, and forwards transaction data through business and data layers for GP processing.

## Tech Stack

- C# / .NET Framework 4.8
- ASP.NET Web API + OWIN
- JWT-based token generation
- Microsoft Dynamics GP eConnect integration
- log4net + custom logging wrappers

## Project Structure

```text
gp-oracle-bridge/
  GPAdapter/
    Controllers/
    Filter/
    App_Start/
    Web.config
  GPBusinessLayer/
    Business/
  GPDataLayer/
    Data/
  GPUtils/
```

## Main API Endpoints

The adapter exposes controller actions including:

- `POST /api/Auth/GetToken` - returns JWT token after validating credentials
- `POST /api/Gp/Transaction` - accepts GP transaction payload and processes it

Note: route templates come from ASP.NET Web API configuration in `GPAdapter/App_Start/WebApiConfig.cs`.

## Configuration

Primary application settings are defined in `GPAdapter/Web.config` under `appSettings`.

Typical keys include:

- JWT/auth settings (`secretKey`, `issuer`, auth user/key)
- logging settings (`loggerGSIUrl`, `loggerGSIEnable`, `serviceName`, `xmlDirectory`)
- database/eConnect connection settings for GP and middleware

Before running locally:

1. Update all credentials, secrets, and connection strings in `Web.config`
2. Ensure required SQL Server/GP resources are reachable
3. Ensure required assemblies/packages are restored

## Build and Run

1. Open the solution/projects in Visual Studio (2019+ recommended)
2. Restore NuGet packages
3. Build in `Debug` or `Release`
4. Run `GPAdapter` via IIS Express or local IIS

The API host/port is controlled by Visual Studio/IIS settings and project configuration.

## Notes

- Keep sensitive values out of source control for production setups
- Validate environment-specific connection strings before deployment
- Logging behavior is controlled by utility layer + config flags

