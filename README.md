HubSpot Integration Task

This project has two main parts:

setting up the HubSpot OAuth flow (backend + frontend), and

loading data from HubSpot after authentication.

Below is everything I need to complete for both sides.

Part 1: HubSpot OAuth Integration
Backend

Inside /backend/integrations, there are three files:

airtable.py → already completed

notion.py → already completed

hubspot.py → this one is empty and I need to finish it

Using the structure from AirTable and Notion, I need to finish these functions:

authorize_hubspot

oauth2callback_hubspot

get_hubspot_credentials

I should follow the same patterns as the other two integrations and refer to HubSpot’s OAuth documentation for the exact endpoints.

Since the client credentials for AirTable and Notion are redacted, those integrations will not run.
So I need to create my own HubSpot app and generate:

Client ID

Client Secret

(Optional) I can also make my own Notion and AirTable credentials if I want to fully test everything.

Frontend

In /frontend/src/integrations, there are:

airtable.js

notion.js

hubspot.js → empty file that I need to fill in

I need to:

Write the full HubSpot integration logic in hubspot.js

Add HubSpot to the correct places in the UI so it appears like the other integrations
(imports, integration list, selection menu, etc.)

Basically, wherever AirTable and Notion appear, HubSpot should appear too.

Part 2: Loading HubSpot Items

After I get valid HubSpot credentials from the OAuth flow, I need to complete:

get_items_hubspot in /backend/integrations/hubspot.py

This function should:

Use the OAuth tokens I retrieved

Call HubSpot’s API endpoints

Return a list of IntegrationItem objects

The IntegrationItem model already has multiple fields, and depending on what I choose from HubSpot (e.g., contacts, deals, companies), I should map the returned fields appropriately. I can use the Notion and AirTable integrations as a reference for how the items are formatted.

For testing, printing the final list of items in the console is good enough.
