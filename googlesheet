Bullhorn Placement & Commission Automation

An n8n workflow that retrieves placement and assignment data from Bullhorn Staffing, processes commission information, and writes the resulting placement records into Google Sheets.

The workflow is designed to automate a recurring placement/commission reporting process and route records based on the number of commission participants.

Overview

This workflow:

Runs automatically on a weekday schedule.

Retrieves the existing Bullhorn OAuth refresh token from Google Sheets.

Refreshes the Bullhorn REST API access token.

Stores the new refresh token back in Google Sheets.

Authenticates with the Bullhorn BBO API.

Retrieves assignment records.

Processes each assignment individually.

Retrieves detailed placement information from Bullhorn.

Extracts placement, candidate, company, owner, employment, status, and commission data.

Routes placements according to the number of commission participants.

Writes the processed records into the Placements Google Sheet.

The workflow contains 17 n8n nodes and is configured with an execution order of v1.

Workflow Architecture

Schedule Trigger
      │
      ▼
Get Old Token
      │
      ▼
Refresh Bullhorn OAuth Token
      │
      ▼
Clear Old Token
      │
      ▼
Write New Token
      │
      ▼
Get Bullhorn REST Token
      │
      ├──────────────► Refresh Token for BBO
      │                       │
      │                       ▼
      │                  Clear Placements Sheet
      │                       │
      │                       ▼
      │                  Get Assignments
      │                       │
      │                       ▼
      │                    Split Out
      │                       │
      │                       ▼
      │                  Loop Over Items
      │                       │
      │                       ▼
      │                  Get Placements
      │                       │
      │                       ▼
      │                  Edit Fields
      │                       │
      │                       ▼
      │               Commission Routes
      │                 /       |       \
      │                /        |        \
      │               ▼         ▼         ▼
      │         3 Commissions  4 Commissions  Filter
      │                │         │             │
      │                ▼         ▼             ▼
      │            Placements  Placements   Loop
      │
      └── Bullhorn REST authentication

Integrations

Bullhorn Staffing

The workflow uses Bullhorn APIs for:

OAuth token refresh

REST API authentication

Assignment retrieval

Placement retrieval

Commission information

The workflow uses the Bullhorn REST endpoints for authentication and placement retrieval, plus the Bullhorn BBO API for assignment data.

Google Sheets

Google Sheets is used for:

Storing the Bullhorn refresh token

Replacing the previous refresh token

Clearing the previous placement dataset

Writing processed placement records

The source workflow references a workbook containing:

Token sheet

Placements sheet

Main Workflow Components

1. Schedule Trigger

The workflow is configured with a weekday cron schedule:

0 30 7 * * 1-5

This corresponds to 7:30 AM, Monday through Friday, according to the timezone configured for the n8n instance.

2. Token Management

The workflow reads the existing refresh token from Google Sheets and sends it to Bullhorn's OAuth token endpoint.

The refresh-token process:

Google Sheets
     ↓
Existing Refresh Token
     ↓
Bullhorn OAuth Token Endpoint
     ↓
New Access / Refresh Token
     ↓
Google Sheets

The old token is cleared before the new refresh token is written.

3. Bullhorn REST Authentication

After obtaining the OAuth access token, the workflow calls the Bullhorn REST login endpoint and obtains a BhRestToken and REST URL.

These values are then used for subsequent Bullhorn REST API requests.

4. BBO Authentication

The workflow also authenticates with Bullhorn's BBO API using the configured BBO login process.

The resulting token is used to retrieve assignment records.

5. Assignment Retrieval

The workflow requests assignment data from the BBO API and splits the returned Assignments array into individual n8n items.

Each assignment is then processed through the loop.

6. Placement Retrieval

For each assignment, the workflow retrieves the corresponding Bullhorn Placement record.

The placement request extracts information including:

Placement ID

Owner

Candidate

Placement dates

Employment type

Status

Pay rate

Client bill rate

Overtime rates

Job order

Client corporation

Commission information

7. Field Transformation

The Edit Fields node transforms the Bullhorn response into reporting-friendly fields.

The workflow prepares information such as:

Field

Description

Start Date

Placement start date

Employee Name

Candidate name

Owner

Placement owner

Employee Title

Job title

Company Name

Client/company

Placement ID

Bullhorn placement ID

Total Commissions

Number of commission records

Employment Type

Placement employment type

Rep1–Rep4

Commission representatives

Rep1–Rep4 Role

Commission roles

Rep1–Rep4 Commission

Commission percentages

End Date

Placement end/prospective end date

Status

Placement status

Date Added

Placement creation/addition date

8. Commission Routing

The Commission Routes switch routes records based on the Total Commissions value.

Current routes include:

3 Commissions

4 Commissions

Additional/fallback processing through the filter and loop

For records with three commission participants, the workflow writes the applicable three-representative structure.

For records with four commission participants, it writes all four representatives and their roles/commission percentages.

9. Google Sheets Output

The final placement data is written to the Placements sheet.

The output includes:

Company

Owner

Job Title

Candidate

Start Date

End Date

Date Added

Placement ID

Placement Type

Commission representatives

Commission roles

Commission percentages

Status

Check

The workflow currently writes a 100.00% value into the Check field for the routed placement records.

Google Sheets Structure

Token Sheet

The workflow expects a refresh-token field:

Refresh token

The token is read before authentication and the refreshed token is written back after the OAuth refresh process.

Placements Sheet

The output sheet contains placement and commission reporting fields such as:

Placement
Company
Job Title
Owner
Placement Type
Status
Candidate
Date Added
Start Date
End Date
Rep1
Comm Role1
Rep1 %
Rep2
Comm Role2
Rep2 %
Rep3
Comm Role3
Rep3 %
Rep4
Comm Role4
Rep4 %
Check

Requirements

Before importing and running this workflow, you need:

n8n

Bullhorn Staffing API access

Bullhorn OAuth credentials

Bullhorn BBO API credentials

Google account access

Google Sheets API/OAuth credentials

A Google Spreadsheet containing the required Token and Placements sheets

Configuration

Bullhorn

Configure your Bullhorn credentials using n8n credentials rather than hard-coding secrets inside workflow nodes.

You will need the appropriate:

Client ID

Client Secret

Refresh Token

Bullhorn REST authentication

BBO authentication details

Google Sheets

Create or select a Google Spreadsheet and configure the required sheets:

Token
Placements

Make sure the n8n Google Sheets credential has permission to access the workbook.

Important: Do Not Commit Secrets

Never commit API keys, client secrets, passwords, OAuth refresh tokens, or private credentials to GitHub.

The original workflow JSON used during development contains authentication values. Before uploading the JSON to a public or shared GitHub repository:

Remove all hard-coded credentials.

Replace secrets with n8n credentials or secure environment variables.

Remove private Google Sheet IDs where appropriate.

Review every HTTP Request node for embedded authentication values.

If any real credentials were exposed, rotate/revoke them before publishing the repository.

For example, use placeholders such as:

YOUR_BULLHORN_CLIENT_ID
YOUR_BULLHORN_CLIENT_SECRET
YOUR_REFRESH_TOKEN
YOUR_BBO_USERNAME
YOUR_BBO_PASSWORD
YOUR_GOOGLE_SHEET_ID

Importing the Workflow into n8n

Open your n8n instance.

Create a new workflow.

Select Import from File.

Select the sanitized workflow JSON.

Reconnect the required credentials.

Configure the Google Spreadsheet and sheet names.

Verify the Bullhorn API configuration.

Test the workflow manually.

Confirm the placement records are written correctly.

Activate the workflow after successful testing.

Data Flow

Bullhorn OAuth
      ↓
REST Authentication
      ↓
BBO Authentication
      ↓
Assignments
      ↓
Individual Assignment
      ↓
Placement Details
      ↓
Field Transformation
      ↓
Commission Count
      ↓
Commission Routing
      ↓
Google Sheets

Error Handling & Validation

Before using this workflow in production, consider adding:

API error branches

Retry handling for Bullhorn API failures

Google Sheets error handling

Empty assignment validation

Missing commission validation

Missing representative validation

Rate-limit handling

Execution notifications

Logging/monitoring

Duplicate placement protection

In particular, commission fields reference multiple commission indexes. If a placement contains fewer commission records than expected, those fields should be validated before accessing them.

Security Notes

This workflow handles authentication tokens and business data. Recommended security practices:

Store credentials in n8n Credentials.

Do not hard-code passwords in workflow JSON.

Do not commit production tokens to Git.

Use private GitHub repositories for sensitive automation projects.

Rotate credentials if they are accidentally exposed.

Restrict Google Sheets permissions to the required account.

Review workflow exports before sharing them publicly.

Project Structure

A recommended repository structure:

bullhorn-placement-automation/
│
├── README.md
├── workflows/
│   └── bullhorn-placement-flow.json
│
├── docs/
│   └── workflow-architecture.md
│
└── .gitignore

Example .gitignore:

.env
*.secret
credentials.json
credentials.local.json
node_modules/
.DS_Store

Use Case

This automation is useful for staffing/recruiting operations that need to regularly synchronize Bullhorn placement information into a Google Sheets-based reporting or gross-margin tracking process.

It reduces manual work involved in:

Pulling placement records

Collecting assignment data

Extracting commission participants

Formatting placement information

Separating records by commission structure

Updating a reporting spreadsheet

Technologies

n8n

Bullhorn Staffing API

Bullhorn BBO API

OAuth 2.0

Google Sheets

REST APIs

JavaScript expressions in n8n

Notes

This README documents the workflow structure and behavior based on the supplied n8n workflow export. Endpoint configuration, credentials, spreadsheet IDs, and other environment-specific values should be configured separately for each deployment.

License

Add the license that matches how you intend to distribute this workflow.

For a private/client project, you can use:

Copyright © 2026. All rights reserved.
