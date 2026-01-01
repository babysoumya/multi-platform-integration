# HubSpot Integration

This document provides an overview of the HubSpot integration implemented in the system. The integration follows the same architectural patterns and conventions used for existing third‑party integrations to ensure consistency, maintainability, and ease of extension.

---

## Overview

The system includes multiple third-party integrations that are implemented using a shared authentication and data-ingestion framework. AirTable, Notion, and HubSpot are implemented following the same architectural patterns so that each integration behaves consistently across backend services and the frontend UI.

All integrations are treated uniformly by the system, with no special-case logic. This makes the integration layer predictable, maintainable, and easy to extend.

---

## OAuth Authentication

### Backend

Backend logic is implemented in:

```
/backend/integrations/hubspot.py
```

The following responsibilities are handled:

* **Authorization redirect**: Constructs the HubSpot OAuth authorization URL with the required scopes and redirects the user to HubSpot’s consent screen.
* **OAuth callback handling**: Processes the authorization code returned by HubSpot and exchanges it for access and refresh tokens.
* **Credential management**: Stores and retrieves OAuth credentials, automatically refreshing access tokens when required.

The implementation follows the shared integration structure used across the system, making the behavior predictable and easy to reason about.

A dedicated HubSpot developer application is used to obtain the required OAuth credentials:

* Client ID
* Client Secret

These credentials are configured via environment variables to keep sensitive information out of the codebase.

---

### Frontend

Frontend integration logic is implemented in:

```
/frontend/src/integrations/hubspot.js
```

The frontend is responsible for:

* Initiating the HubSpot OAuth flow via backend endpoints
* Handling loading and error states during authentication
* Updating integration status once authorization is complete

HubSpot is registered alongside other integrations in all relevant UI locations, including:

* Integration imports
* Integration listings
* Selection menus

As a result, HubSpot appears and behaves identically to other supported integrations from a user perspective.

---

## Data Loading

Once authentication is complete, HubSpot data is loaded through:

```
get_items_hubspot
```

### Data Fetching & Mapping

This logic performs the following steps:

1. Retrieves valid HubSpot credentials from the credential store.
2. Calls HubSpot APIs using the authenticated access token.
3. Fetches supported HubSpot entities (such as contacts, companies, or deals).
4. Normalizes each entity into the shared `IntegrationItem` model.

The mapping strategy aligns with the shared integration model, ensuring a consistent structure across integrations regardless of the data source.

Each `IntegrationItem` includes:

* A stable external identifier
* A human‑readable name or label
* An integration‑specific type
* Raw metadata for downstream use

For observability during development, the resulting list of items is logged, which allows easy verification of the ingestion pipeline.

---

## Summary

The HubSpot integration is implemented as a first‑class integration within the system:

* OAuth authentication is fully supported and secure
* HubSpot is seamlessly integrated into the existing UI and backend flow
* HubSpot data is normalized into a common integration model

This approach keeps all integrations consistent across the platform and allows additional providers to be added with minimal effort in the future.
