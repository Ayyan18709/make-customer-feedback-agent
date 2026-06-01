# Customer Feedback Agent — Make.com Blueprint

An automated Make.com workflow that watches a Google Form for new feedback submissions, analyzes the sentiment using an AI agent, routes negative feedback as a Slack alert, and logs every response to Google Sheets.

---

## Workflow Overview

```
Google Forms (Watch Responses)
        ↓
AI Local Agent (Sentiment Analysis)
        ↓
    Router
   ↙      ↘
Slack     Google Sheets
(negative  (all responses)
 only)
```

**Modules used:**
- **Google Forms** — `watchResponses`: triggers on every new form submission
- **AI Local Agent** — `RunLocalAIAgent`: classifies feedback as `positive` or `negative`
- **Router** — splits the flow based on sentiment
- **Slack** — `CreateMessage`: sends an alert for negative feedback to a private channel
- **Google Sheets** — `addRow`: logs sentiment, feedback text, and respondent email for every response

---

## Prerequisites

- A [Make.com](https://make.com) account
- Google account connected to Make (for Forms + Sheets)
- Slack workspace connected to Make
- An AI provider connection in Make (OpenAI, Anthropic, Gemini, etc.)

---

## Setup Instructions

### 1. Import the Blueprint

1. In Make.com, go to **Scenarios → Create a new scenario**
2. Click the three-dot menu → **Import Blueprint**
3. Upload `Customer_Feedback_Agent_blueprint.json`

### 2. Replace the Placeholder Values

Search for these strings in the blueprint and replace them with your own:

| Placeholder | What to put there |
|---|---|
| `YOUR_GOOGLE_FORM_ID` | The ID from your Google Form URL: `https://docs.google.com/forms/d/<ID>/edit` |
| `YOUR_QUESTION_ID` | The internal question ID from the form (visible in Make after connecting your form) |
| `YOUR_SLACK_CHANNEL_ID` | The Slack channel ID (right-click a channel → Copy link, the ID is at the end) |
| `YOUR_SPREADSHEET_ID` | The path or ID of your Google Sheet (e.g. `/Feedback` if searching by path) |

### 3. Connect Your Accounts

Inside each module, click **Add** next to the Connection field and authenticate:
- Module 2 (Google Forms) → Google account
- Module 3 (AI Agent) → Your AI provider (OpenAI, Anthropic, etc.)
- Module 5 (Slack) → Slack workspace
- Module 6 (Google Sheets) → Google account

### 4. Configure the Google Sheet

Create a sheet named `Sheet1` with these headers in row 1:

| A | B | C |
|---|---|---|
| Sentiment | Feedback | Email |

### 5. Set the Starting Point

On the Google Forms trigger module, click **Choose where to start** and select your preferred start point (e.g. "From now on").

### 6. Activate the Scenario

Toggle the scenario **ON**. It will now run automatically on every new form response.

---

## AI Prompt Logic

The AI agent uses a carefully tuned system prompt that:
- Outputs only `positive` or `negative` (no extra text)
- Weights dominant sentiment over minor side-notes
- Handles sarcasm, mixed feedback, and vague/empty responses (defaults to `negative`)
- Ignores personal data, order numbers, and greetings

You can customize the prompt in Module 3 → **Instructions** field.

---

## Customization Ideas

- Add a third router branch for **positive** feedback (e.g. post to a wins channel)
- Add a **timestamp** column to the Google Sheet using `{{2.lastSubmittedTime}}`
- Swap Slack for **email** (Gmail module) or **Teams** (Microsoft Teams module)
- Extend the AI output schema to include a `summary` or `category` field alongside `sentiment`
- Change the AI model in Module 3 to any supported provider

---

## License

MIT — free to use, fork, and adapt.
