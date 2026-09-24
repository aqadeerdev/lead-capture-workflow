# Lead Capture Workflow (with CRM Integration + Automated Notification)

This is an n8n workflow that turns a form submission into a lead which is validated, logged to a CRM, forwarded to the team on Slack, and acknowledged with an automatic email. Bad form submissions are handled with a proper failure path instead of silently dropping them.

## The Problem
Small businesses and solo teams lose leads when there's a gap between form submission and filing the lead into the CRM or someone from the team actually reviewing it. This workflow closes that gap without required long-term maintenance from a developer.

## What It Does
1. A trigger listens for new Tally form submissions in real time via webhook.
2. Custom JavaScript code validates and shapes the submitted data and flags malformed submissions instead of letting it flow downstream silently.
3. Invalid submissions are routed to a separate Slack alert with the specific error for debugging.
4. A valid submission is written to Airtable as a new record and posted to a Slack channel with the lead's detail.
5. The submitter receives an automatic thank-you email.

## Tools Used
- **n8n** (self-hosted via **Docker**) for workflow orchestration
- **Tally** for form and webhook trigger
- **JavaScript** (n8n Code node) for validation and data shaping
- **Airtable** for lightweight CRM record storage
- **Slack API** for team notifications
- **SMTP** for sending automated confirmation email via **Gmail**

## Setup
1. Import `workflow/lead-capture-workflow.json` into your own n8n instance.
2. Create your own credentials. Personal access tokens for Airtable, bot OAuth token for Slack, and app password for SMTP.
3. Create a Tally form with name, email, and message fields and connect it to the workflow's Webhook node URL under Tally's Integrations -> Webhook Settings. 
3. Update the Airtable/Slack/email nodes to reference your own base, channel, and sender address. 

**Note:** The Code node extracts fields by matching on Tally's field labels (`Full Name`, `Email Address`, `Message`). Adjust these if your form uses different labels.


## Demo
[Video walkthrough link]

## Possible Improvements
- Swap Airtable for a full CRM (HubSpot, Pipedrive) for production use.
- Add deduplication logic to check for existing email before creating a new Airtable record.
- Add retry/backoff handling on the Slack/email nodes for transient API failures.