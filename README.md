# USD Exchange Rate Alert (n8n)

n8n workflow that checks the USD/BRL exchange rate, records the value in Google Sheets, and sends an email when the dollar is cheap.

## How it works

Webhook → AwesomeAPI → Switch:

- Rate above R$ 5.30: save the rate to Google Sheets.
- Rate at or below R$ 5.22: send a Gmail alert.

## Setup

1. Import `alerta-dolar.json` into n8n.
2. Connect your Google Sheets and Gmail credentials.
3. Select your spreadsheet and email recipient.
4. Activate the workflow and call the production webhook URL shown by n8n.

Change the thresholds in the rate node if needed.
