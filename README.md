# PHC AI Agent — Stock, Footfall, Availability & Municipal Alerts

An AI agent for Primary Health Centres (PHCs) that answers natural-language queries about medicine stock, predicts patient footfall, checks staff/doctor availability, and automatically raises alerts to municipal authorities when critical thresholds are breached.

## Problem statement

Rural and semi-urban PHCs in India often lack real-time visibility into medicine stock levels, patient load patterns, and staff availability — leading to stockouts, overcrowding, and delayed escalation to municipal health authorities. This project builds an LLM-powered agent that unifies these signals and automates the alerting process.

## Features

- **Medicine stock tracking** — query current stock, get automatic low-stock flags
- **Patient footfall prediction** — trend-based forecasting using historical visit data, including seasonal (monsoon) spike patterns
- **Staff/doctor availability** — daily roster lookup for medical officers, nurses, pharmacists
- **Automatic municipal alerts** — email notifications triggered when stock or footfall crosses critical thresholds, independent of user queries

## Tech stack

| Component | Tool |
|---|---|
| LLM orchestration | Groq API (Llama 3.3 70B, function calling) |
| Database | SQLite |
| Alert delivery | SMTP (email) |
| UI (planned) | Streamlit |
| Synthetic data | Faker |

## Architecture

1. User submits a query (e.g. "What medicines are running low?")
2. Groq LLM orchestrator interprets the query and selects the relevant tool
3. Tool functions (`check_stock`, `predict_footfall`, `check_availability`, `raise_municipal_alert`) query the SQLite database
4. Groq composes a natural-language response from the tool output
5. If a critical condition is detected, an email alert is sent to the municipal authority in parallel — independent of the chat response

## Database schema

- `medicine_stock` — medicine name, category, quantity, reorder threshold, expiry
- `patients` — patient demographics
- `visits` — visit history with symptom/diagnosis records (used for footfall prediction)
- `staff_availability` — daily roster with presence tracking
- `alerts` — logged alerts with severity and municipal notification status

## Setup

1. Clone the repo
2. Install dependencies:
   ```
   pip install groq faker requests
   ```
3. Set the following as environment variables / Colab Secrets (never hardcode or commit these):
   - `GROQ_API_KEY`
   - `SMTP_EMAIL`
   - `SMTP_APP_PASSWORD`
   - `MUNICIPAL_EMAIL`
4. Run the notebook cells in order to seed the database and start the agent

## Example queries

```
"What medicines are running low?"
"What's the predicted footfall for tomorrow?"
"Who's available today?"
```

## Roadmap

- [ ] Streamlit UI for non-technical PHC staff
- [ ] Scheduled background checks (not just on-demand queries)
- [ ] Migrate from SQLite to PostgreSQL for production
- [ ] SMS alerts (Twilio/Fast2SMS) as an alternative to email
- [ ] Integration with ABDM (Ayushman Bharat Digital Mission) data standards

## Disclaimer

This is a portfolio/prototype project using synthetic data. Not yet validated for production use in an actual healthcare setting — patient data handling, security, and compliance review would be required before real deployment.

## Author

Saurabh Mungale — Data Science & Generative AI
