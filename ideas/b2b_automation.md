Below is a **ready-to-import n8n workflow JSON template** for your system.
It includes the core automation structure:

• Gmail trigger
• Email parsing
• Gemini/OpenAI intent classification
• Routing (positive / not interested / neutral)
• Positive auto-reply
• Negative reply logging to Google Sheets
• Gmail labeling for manual review

You will only need to configure credentials for:

* Gmail
* Google Sheets
* Gemini/OpenAI API

---

# Ready-to-Import n8n Workflow JSON

Import this via:

**n8n → Workflows → Import from Clipboard**

{
"name": "B2B Email Reply Automation",
"nodes": [
{
"parameters": {
"resource": "message",
"operation": "getAll",
"returnAll": false,
"limit": 10,
"simple": false,
"filters": {
"labelIds": ["INBOX"]
}
},
"id": "gmail_trigger",
"name": "Gmail Trigger",
"type": "n8n-nodes-base.gmail",
"typeVersion": 1,
"position": [200, 300],
"credentials": {
"gmailOAuth2": {
"id": "YOUR_GMAIL_CREDENTIAL",
"name": "Gmail Account"
}
}
},
{
"parameters": {
"values": {
"string": [
{
"name": "email_body",
"value": "={{$json.snippet}}"
},
{
"name": "sender",
"value": "={{$json.from}}"
},
{
"name": "threadId",
"value": "={{$json.threadId}}"
}
]
}
},
"id": "extract_data",
"name": "Extract Email Data",
"type": "n8n-nodes-base.set",
"typeVersion": 2,
"position": [400, 300]
},
{
"parameters": {
"method": "POST",
"url": "[https://generativelanguage.googleapis.com/v1beta/models/gemini-pro](https://generativelanguage.googleapis.com/v1beta/models/gemini-pro):generateContent",
"authentication": "none",
"sendBody": true,
"bodyParameters": {
"parameters": [
{
"name": "contents",
"value": "={{[{"parts":[{"text":"Classify this email reply into positive_interest, not_interested, or neutral: " + $json.email_body}]}]}}"
}
]
},
"queryParameters": {
"parameters": [
{
"name": "key",
"value": "YOUR_GEMINI_API_KEY"
}
]
}
},
"id": "ai_classifier",
"name": "Gemini Intent Classifier",
"type": "n8n-nodes-base.httpRequest",
"typeVersion": 4,
"position": [600, 300]
},
{
"parameters": {
"conditions": {
"string": [
{
"value1": "={{$json.intent}}",
"operation": "equal",
"value2": "positive_interest"
}
]
}
},
"id": "if_positive",
"name": "IF Positive Interest",
"type": "n8n-nodes-base.if",
"typeVersion": 1,
"position": [800, 300]
},
{
"parameters": {
"operation": "send",
"to": "={{$json.sender}}",
"subject": "Thanks for your interest",
"message": "Hi,\n\nThanks for your reply. We'd love to schedule a quick call to understand your needs and show how we can help.\n\nWould you be available sometime this week?\n\nBest,\nSales Team"
},
"id": "send_reply",
"name": "Send Positive Reply",
"type": "n8n-nodes-base.gmail",
"typeVersion": 1,
"position": [1000, 200],
"credentials": {
"gmailOAuth2": {
"id": "YOUR_GMAIL_CREDENTIAL",
"name": "Gmail Account"
}
}
},
{
"parameters": {
"conditions": {
"string": [
{
"value1": "={{$json.intent}}",
"operation": "equal",
"value2": "not_interested"
}
]
}
},
"id": "if_negative",
"name": "IF Not Interested",
"type": "n8n-nodes-base.if",
"typeVersion": 1,
"position": [800, 450]
},
{
"parameters": {
"operation": "append",
"sheetId": "YOUR_SHEET_ID",
"range": "Sheet1!A:C",
"options": {}
},
"id": "add_to_sheet",
"name": "Add to Suppression List",
"type": "n8n-nodes-base.googleSheets",
"typeVersion": 4,
"position": [1000, 450],
"credentials": {
"googleSheetsOAuth2Api": {
"id": "YOUR_GOOGLE_SHEET_CREDENTIAL",
"name": "Google Sheets"
}
}
},
{
"parameters": {
"operation": "addLabel",
"messageId": "={{$json.id}}",
"labelIds": ["NeedsReview"]
},
"id": "label_review",
"name": "Label for Manual Review",
"type": "n8n-nodes-base.gmail",
"typeVersion": 1,
"position": [1000, 600],
"credentials": {
"gmailOAuth2": {
"id": "YOUR_GMAIL_CREDENTIAL",
"name": "Gmail Account"
}
}
}
],
"connections": {
"Gmail Trigger": {
"main": [
[
{
"node": "Extract Email Data",
"type": "main",
"index": 0
}
]
]
},
"Extract Email Data": {
"main": [
[
{
"node": "Gemini Intent Classifier",
"type": "main",
"index": 0
}
]
]
},
"Gemini Intent Classifier": {
"main": [
[
{
"node": "IF Positive Interest",
"type": "main",
"index": 0
}
]
]
},
"IF Positive Interest": {
"main": [
[
{
"node": "Send Positive Reply",
"type": "main",
"index": 0
}
],
[
{
"node": "IF Not Interested",
"type": "main",
"index": 0
}
]
]
},
"IF Not Interested": {
"main": [
[
{
"node": "Add to Suppression List",
"type": "main",
"index": 0
}
],
[
{
"node": "Label for Manual Review",
"type": "main",
"index": 0
}
]
]
}
}
}

---

# What You Must Configure

After importing:

### 1️⃣ Add Gmail Credentials

```
Gmail OAuth2
```

Used in:

* Gmail Trigger
* Send Email
* Add Label

---

### 2️⃣ Add Google Sheets Credentials

Create a sheet like:

| Email | Date | Reason |
| ----- | ---- | ------ |

Put the **Sheet ID** in:

```
Add to Suppression List node
```

---

### 3️⃣ Add Gemini API Key

Replace:

```
YOUR_GEMINI_API_KEY
```

---

# Improvements I Recommend (Production Level)

A senior architecture version would add:

### Conversation Thread Memory

Store email threads in:

```
PostgreSQL
```

---

### Rate Limiting

Cold email replies sometimes come in bursts.

Add:

```
Split In Batches
```

---

### AI Reply Generation

Instead of static replies:

```
Gemini → generate contextual reply
```

---

### CRM Sync

Push positive leads to:

* HubSpot
* Pipedrive
* Airtable

---

# If you want, I can also show you something **MUCH more powerful**:

**A full AI Sales Agent architecture used by modern outbound teams**

This system can automatically:

• detect objections
• book meetings
• follow up automatically
• update CRM
• manage entire cold outreach replies

(all inside **n8n + Gemini**).
