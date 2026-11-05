# USD Exchange Rate Alert with n8n

n8n automation that checks the USD/BRL exchange rate, analyzes the value and takes a different action depending on the rate. It uses the AwesomeAPI public endpoint, Google Sheets and Gmail.

## How it works

```text
Webhook
  -> HTTP Request (AwesomeAPI)
  -> Convert the bid value to a number
  -> Switch
     ├── rate > 5.30  -> "nao comprar" -> Google Sheets
     └── rate <= 5.22 -> "comprar muito" -> Gmail alert
```

1. A `POST` call triggers the webhook.
2. The workflow fetches the current USD/BRL rate.
3. The `bid` field is converted to a numeric value (`valor_convertido`).
4. The Switch node applies two rules:
   - **Above R$ 5.30:** record the rate in Google Sheets.
   - **At or below R$ 5.22:** send an email saying the dollar is low.

## Stack

n8n, AwesomeAPI, Webhook, Google Sheets, Gmail and JSON.

## Structure

```text
├── alerta-dolar.json   n8n workflow export
└── README.md
```

## How to run

1. Import `alerta-dolar.json` into n8n (local at `http://localhost:5678` or n8n Cloud).
2. Connect your own Google Sheets and Gmail credentials.
3. Select your spreadsheet in `salva a cotaçao na planilha` (columns: `date`, `exchange_rate`).
4. Configure the recipient in the `Send an Email` node.
5. Activate the workflow and copy the production webhook URL.
6. Send a `POST` request:

   ```powershell
   Invoke-WebRequest -Method POST -Uri "http://localhost:5678/webhook/nova_transaçao"
   ```

## Customizing

- Change the thresholds in the `cotaçao do dolar` node (for example, buy at `<= 5.00` and skip at `> 5.50`).
- Adapt the HTTP Request pair to other currencies, such as `EUR-BRL` or `BTC-BRL`.

## Security

Credentials are never stored in the repository. Each person who imports the workflow must connect their own Google and Gmail accounts.
