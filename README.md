# USD Exchange Rate Alert with n8n

Automation built with **n8n** that checks the current US dollar exchange rate against the Brazilian real, analyzes the value, and performs different actions based on the detected rate.

The project uses a public exchange rate API, Google Sheets, and Gmail.

---

## What this automation does

The workflow works as follows:

1. Receives a request through a Webhook.
2. Retrieves the current USD exchange rate.
3. Converts the value returned by the API.
4. Analyzes the exchange rate using a Switch node.
5. Depending on the value:
   - records the exchange rate in a spreadsheet; or
   - sends an email alert.

Workflow overview:

```text
Webhook
   ↓
HTTP Request
   ↓
USD/BRL Exchange Rate
   ↓
Value Conversion
   ↓
Switch
├── High exchange rate → Google Sheets
└── Low exchange rate  → Gmail
```

---

## Technologies used

- n8n
- REST API / HTTP Request
- Webhook
- Google Sheets
- Gmail
- JSON
- AwesomeAPI
- Git
- GitHub

---

## API used

The automation uses **AwesomeAPI** to retrieve the USD/BRL exchange rate.

Endpoint:

```text
https://economia.awesomeapi.com.br/json/last/USD-BRL
```

The workflow uses the `bid` field returned by the API and converts it into a numeric value that can be analyzed by n8n.

---

## Automation rules

After retrieving the data from the API, the workflow creates the field `valor_convertido`, which is sent to a **Switch** node. Two rules are configured.

### Exchange rate above R$ 5.30

Condition:

```text
valor_convertido > 5.30
```

The value is sent to the workflow path identified as `nao comprar`. In this path, the value is recorded in Google Sheets.

### Exchange rate lower than or equal to R$ 5.22

Condition:

```text
valor_convertido <= 5.22
```

The value is sent to the path identified as `comprar muito`. In this case, the user receives an email alert informing them that the US dollar exchange rate is low.

---

## Project structure

```text
n8n-alerta-dolar/
├── alerta-dolar.json   # n8n workflow export
└── README.md
```

The main project file is `alerta-dolar.json`. It contains the entire n8n workflow structure.

---

## How to install

### 1. Download the project

Download the project from GitHub using **Code → Download ZIP**, or clone it with Git:

```bash
git clone https://github.com/evfernandes-dev/n8n-alerta-dolar.git
```

Then enter the project folder:

```bash
cd n8n-alerta-dolar
```

### 2. Open n8n

You need an n8n installation. For a local installation, n8n can usually be accessed at:

```text
http://localhost:5678
```

You can also use n8n Cloud.

### 3. Import the workflow

Inside n8n:

1. Open the workflows section.
2. Click the workflow menu.
3. Select the option to import a file.
4. Choose `alerta-dolar.json`.

The workflow will be loaded into your n8n instance.

---

## Configure your credentials

Personal credentials should never be shared through GitHub. Anyone who imports this project must configure their own accounts.

### Google Sheets

Open the node `salva a cotaçao na planilha`, connect your Google account, and select your own spreadsheet.

The spreadsheet can contain two columns:

| date | exchange_rate |
|---|---|
| 2026/10/04 18:30 | 5.21 |

Then select the desired spreadsheet and sheet inside the Google Sheets node.

### Gmail

Open the node `Send an Email`, connect your own Gmail account, and configure the address that will receive the alerts. You can also customize the recipient, subject, content, and alert message.

Example message:

```text
Subject: USD Exchange Rate

The US dollar exchange rate is low.
Current exchange rate: R$ 5.21
```

---

## Webhook

The workflow starts through a Webhook.

- Method: `POST`
- Configured path: `nova_transaçao`

After activating the workflow, n8n provides a production URL. In a local installation, it may look similar to:

```text
http://localhost:5678/webhook/nova_transaçao
```

You can send a POST request to start the workflow. PowerShell example:

```powershell
Invoke-WebRequest -Method POST -Uri "http://localhost:5678/webhook/nova_transaçao"
```

You can also use Postman, Insomnia, curl, another system, or another automation.

---

## How to run

After configuring Gmail and Google Sheets:

1. Import the workflow.
2. Configure your credentials.
3. Select your own spreadsheet.
4. Configure the destination email address.
5. Activate the workflow.
6. Copy the production Webhook URL.
7. Send a POST request.
8. n8n retrieves the current exchange rate.
9. The value is analyzed.
10. The workflow executes the corresponding action.

Expected workflow:

```text
Webhook
   ↓
HTTP Request
   ↓
USD to BRL Conversion
   ↓
Switch
   ↓
Decision
├── Google Sheets
└── Gmail
```

---

## Customizing the values

You can change the thresholds that decide which action is executed. Open the node `cotaçao do dolar` and change the conditions.

For example:

- Buy: `<= 5.00`
- Do not buy: `> 5.50`

This allows each user to adapt the automation to their own goals.

---

## Other currencies

The project can also be adapted to retrieve other currency pairs, for example:

- EUR-BRL
- BTC-BRL

The change is made by adjusting the pair used in the HTTP Request to the AwesomeAPI.
