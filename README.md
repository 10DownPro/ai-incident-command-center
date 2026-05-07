# AI Incident Command Center

A custom ServiceNow scoped application that automates IT incident analysis using AI-powered recommendations and real-time Slack notifications. Built to simulate how modern enterprise operations teams handle critical incidents at scale.

This project brings together scoped app development, REST API integration, AI-assisted automation, and cross-platform alerting into a single working workflow.

---

## Why I Built This

Enterprise incident response is one of those areas where speed and clarity matter more than anything. Analysts waste time writing summaries, deciding severity, and chasing stakeholders. I wanted to build something that handles the repetitive parts automatically so teams can focus on actually fixing problems.

This project reflects real patterns I have seen and worked with in enterprise environments, specifically around incident management, cross-team communication, and operational workflows.

---

## What It Does

The app lets analysts create incident records and run AI-powered analysis with a single button click. Once triggered, the system generates a full incident breakdown and pushes a formatted alert directly to Slack.

Here is what happens when a user hits "Run AI Analysis":

1. The UI Action fires a Script Include on the server side
2. The Script Include pulls incident data using GlideRecord
3. A REST call is made to the OpenAI Chat Completions API via RESTMessageV2
4. The AI response is parsed and written back to the incident record
5. A Slack notification is sent with incident details and the AI-generated summary

Each analysis populates the following on the record:

- AI-generated incident summary
- Recommended severity level
- Likely root cause
- Suggested remediation steps
- Stakeholder communication update

![Scoped Application Overview](screenshots/scoped-app.png)

---

## Architecture

```
Incident Record
      |
      v
UI Action ("Run AI Analysis")
      |
      v
Script Include (Server-Side)
      |
      v
OpenAI REST API (Chat Completions)
      |
      v
AI Response Parsing + Record Update
      |
      v
Slack Webhook Notification
```

The entire flow runs server-side within the ServiceNow scoped application. No client-side scripts or external middleware needed.

---

## Custom Table: AI Incident Analysis

The app uses a dedicated custom table to track both incident details and AI outputs in one place.

| Field | Purpose |
|---|---|
| Incident Number | Auto-numbered unique identifier |
| Incident Description | Free-text description of the issue |
| Affected System | System or service impacted |
| Current Severity | Analyst-assigned severity level |
| AI Status | Tracks whether analysis has been run |
| AI Recommended Severity | AI-suggested severity based on analysis |
| AI Summary | AI-generated operational summary |
| Likely Root Cause | AI-identified probable cause |
| Remediation Steps | AI-recommended fix actions |
| Stakeholder Update | AI-drafted communication for leadership |
| Slack Sent | Boolean flag for notification status |
| Analysis Timestamp | When the analysis was executed |

![Custom Table Configuration](screenshots/custom-table.png)

![Choice Field Setup](screenshots/choice-fields.png)

---

## OpenAI Integration

The integration is built using ServiceNow REST Messages with the following configuration:

- **Endpoint:** OpenAI Chat Completions API
- **Method:** HTTP POST
- **Auth:** API key passed via request headers
- **Request Body:** JSON payload with dynamic incident variables injected at runtime
- **Response Handling:** Parsed JSON mapped back to incident record fields

The REST Message, headers, authentication, request body structure, and response parsing are all fully implemented. During development, actual API calls were mocked to avoid paid usage while still validating the complete integration architecture end to end.

![OpenAI REST Message Configuration](screenshots/openai-rest-message.png)

![Script Include Logic](screenshots/script-include.png)

---

## Slack Integration

After analysis completes, a formatted Slack alert fires automatically using an Incoming Webhook. The message includes:

- Incident number
- Analysis timestamp
- Severity level
- AI-generated summary
- Stakeholder update

This mirrors how real enterprise ops teams push critical incident updates to war rooms and leadership channels.

![Slack REST Message Setup](screenshots/slack-rest-message.png)

![Slack Notification Output](screenshots/slack-notification.png)

---

## UI Action

The "Run AI Analysis" button is configured as a server-side UI Action on the incident form. One click triggers the full pipeline: AI analysis, record update, and Slack notification.

![UI Action Configuration](screenshots/ui-action.png)

![AI Analysis Output on Record](screenshots/ai-analysis-output.png)

---

## Sample Records

![Sample Incident Records](screenshots/sample-records.png)

---

## Technologies

| Technology | How It Is Used |
|---|---|
| ServiceNow | Platform, scoped app, custom tables, UI Actions |
| JavaScript | All server-side scripting |
| GlideRecord | Database queries and record operations |
| RESTMessageV2 | Outbound REST calls to OpenAI and Slack |
| OpenAI API | AI-powered incident analysis |
| Slack Webhooks | Real-time notification delivery |
| JSON | Request/response formatting and parsing |

---

## Additional Configuration Details

Beyond the core features, the application also includes:

- REST Message setup with HTTP methods, custom headers, and structured request bodies
- Webhook integration configuration for Slack
- Auto-numbering on incident records
- Custom choice fields for severity, status, and AI analysis state
- Incident status workflow logic to track progression

---

## Future Improvements

- Microsoft Teams integration for organizations not on Slack
- Automated escalation based on AI-recommended severity
- CMDB relationship mapping to tie incidents to affected configuration items
- Historical trend analysis across past incidents
- AI confidence scoring to flag low-certainty recommendations
- Auto-generated knowledge articles from resolved incidents
- ServiceNow Virtual Agent integration for self-service triage

---

## What This Project Demonstrates

This is not a tutorial walkthrough or a copy-paste demo. It is a working scoped application that reflects real enterprise patterns:

- **ServiceNow Development:** Scoped apps, custom tables, Script Includes, UI Actions, GlideRecord
- **API Integration:** RESTMessageV2, HTTP methods, JSON handling, dynamic payloads
- **AI Automation:** Structured AI prompts, response parsing, field mapping
- **Operational Workflows:** Incident management, severity classification, stakeholder communication
- **Cross-Platform Alerting:** Slack webhooks, formatted notifications, real-time delivery

---

## Author

**T'Vedt Lazenby**
Systems Administrator | AI + Automation Engineer
Founder, Tech Teens Inc.

[LinkedIn](https://linkedin.com/in/YOUR-URL) | [GitHub](https://github.com/YOUR-URL)
