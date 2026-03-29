# Vital Ops

**AI-powered operations automation for solo and small-crew trades operators.**

Built and operated by [Forever Vitality Enterprises](mailto:angelmiralda@proton.me) — Blind Bay, BC.

---

## What This Is

Vital Ops automates the client communication and scheduling overhead that costs trades operators hours every week. The core product: when a job runs late, the system automatically notifies downstream clients by SMS — no phone tag, no dropped balls, no angry voicemails.

Positioning: *"We're That Friend. Except we show up on time and send an invoice."*

Target market: ~140,000 solo and small-crew operators across Canada. Current focus: BC. Expansion: Ontario.

---

## Product Stages

| Stage | Name | Description | Price |
|-------|------|-------------|-------|
| 1 | **Client Comms** | Delay cascade notifications, VIP pin-locking, availability checks via text, spouse/family calendar sync | $400/mo ($500 setup) |
| 2 | **Your Own AI** | Full Claude AI account setup, route planning, material sourcing | ~$800 one-time |
| 3 | **Full Stack** | Integrations: Jobber, QuickBooks, Trello, ServiceTitan | TBD |

Stage 1 is the confirmed universal entry point. Stages 2 and 3 are bolt-ons.

---

## Tech Stack

| Layer | Tool |
|-------|------|
| Orchestration | Make.com |
| AI | Claude API (`claude-sonnet-4-20250514`) |
| Inbound messaging | Twilio WhatsApp Sandbox |
| Outbound messaging | Twilio SMS |
| Scheduling | Google Calendar API |
| Client data | Google Sheets (Config + Contacts tabs, cloned per client) |

---

## Repository Structure

```
vital-ops/
├── landing/
│   └── vital_ops_v9.html        # Sales/pitch page (four-theme switcher)
├── prompts/
│   └── delay_parse.txt          # Claude prompt: extract delay minutes from operator message
│   └── sms_generation.txt       # Claude prompt: generate personalized client SMS
├── sheets/
│   └── config_template.md       # Documents the Google Sheet structure (Config + Contacts tabs)
└── README.md
```

---

## Make.com Scenario: VITALOPS_TEMPLATE_v1

The core automation scenario. This is cloned per client — the architecture is designed for repeatability. Target onboarding time: under 30 minutes per new client.

### Scenario Flow

```
Operator sends delay via WhatsApp
        ↓
Twilio WhatsApp Sandbox → Make.com Custom Webhook
        ↓
Claude API — extract delay duration (number only)
        ↓
Google Sheets — read Config tab (operator name, preferences)
        ↓
Google Sheets — read Contacts tab (client list, VIP flags)
        ↓
Google Calendar — fetch today's remaining jobs
        ↓
Set Variables (build notification list, skip VIP clients)
        ↓
Claude API — generate personalized SMS per client
        ↓
Twilio SMS — send to each client
        ↓
Text Aggregator — compile send summary
        ↓
Twilio SMS — confirmation summary back to operator
```

### Key References

- **Google Sheet ID:** `15xD2_2wTpHDyybLo3UMweF1KfuaXVa0S`
- **Calendar:** `vitalopsai@gmail.com`
- **Client phone format in calendar events:** `phone: +1XXXXXXXXXX` (stored in event description)
- **VIP logic:** VIP-flagged clients in the Contacts tab are excluded from delay cascade notifications

### Test Jobs (dev calendar)

| Client | Time | Phone |
|--------|------|-------|
| Thompson Site Prep | 7:00 AM | +16045550104 |
| Patterson Plumbing Quote | 9:00 AM | +16045550101 |
| Henderson Grading Work | 11:30 AM | +16045550102 |
| McAllister Final Grade | 2:00 PM | +16045550103 |

---

## Google Sheet Structure

### Config Tab

Stores per-client operator settings: operator name, phone number, SMS tone preference, and any global flags.

### Contacts Tab

Stores the operator's client list: client name, phone number, VIP flag. VIP clients are excluded from automated delay notifications.

---

## Pilot Client

- **Name:** David Alan Soucey — Soucey Farms
- **Phone:** +12502532448
- **SMS tone:** Casual
- Test data remains in place until go-live, at which point the real client list is populated.

---

## Economics (per client/month)

| Item | Cost |
|------|------|
| Make.com (pro-rated) | ~$0.80 |
| Twilio | ~$1.55 |
| Claude API | ~$0.15 |
| **Total COGS** | **~$2.50** |
| **Revenue** | **$400.00** |
| **Gross margin** | **~99%** |

---

## Status

- [x] Make.com Modules 1–10 built and green
- [x] Google Sheet template complete
- [x] Pilot client identified
- [ ] Twilio account restoration (compliance review in progress)
- [ ] End-to-end test
- [ ] VIP filter module
- [ ] Pilot client go-live
- [ ] Scenario clone workflow validation
- [ ] Ontario market expansion planning
