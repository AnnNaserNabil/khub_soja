# Facebook Automation with n8n: Complete Knowledge Base for Shop Pages

## Table of Contents
1. [Overview](#overview)
2. [Prerequisites & Setup](#prerequisites--setup)
3. [Cloudflare Tunnel Configuration](#cloudflare-tunnel-configuration)
4. [Facebook/Meta API Configuration](#facebookmeta-api-configuration)
5. [n8n Facebook Nodes](#n8n-facebook-nodes)
6. [Core Automation Workflows](#core-automation-workflows)
7. [Message Handling & Auto-Responses](#message-handling--auto-responses)
8. [Comment Management](#comment-management)
9. [Product Catalog Integration](#product-catalog-integration)
10. [Advanced Chatbot Logic](#advanced-chatbot-logic)
11. [Best Practices for High-Volume Shops](#best-practices-for-high-volume-shops)
12. [Error Handling & Rate Limits](#error-handling--rate-limits)
13. [Testing & Debugging](#testing--debugging)
14. [Complete Workflow Examples](#complete-workflow-examples)
15. [Troubleshooting](#troubleshooting)

---

## Overview

This knowledge base covers everything needed to build a robust Facebook automation system using n8n for shop pages that handle high volumes of messages and comments for product sales.

### Key Capabilities
- **Auto-respond to Messenger messages** with product information
- **Reply to comments** on posts automatically
- **Send product catalogs** based on user queries
- **Handle order inquiries** and FAQs
- **Route complex queries** to human agents
- **Track conversations** in external databases (Google Sheets, Airtable, etc.)
- **Send order confirmations** and shipping updates

---

## Prerequisites & Setup

### Required Accounts & Tools
1. **n8n instance** (self-hosted or cloud)
2. **Cloudflare account** (for tunneling)
3. **Facebook Business Page** (with shopping enabled)
4. **Meta Developer Account**
5. **Meta App** with proper permissions
6. **Database/CRM** (optional but recommended): Google Sheets, Airtable, MySQL, etc.

### Recommended n8n Version
- n8n v1.0+ (latest stable version)
- Ensure Facebook nodes are updated

---

## Cloudflare Tunnel Configuration

### Why Cloudflare Tunnel?

Cloudflare Tunnel provides:
- **Secure webhook exposure** without opening firewall ports
- **HTTPS automatically** with valid SSL certificates
- **DDoS protection** from Cloudflare's network
- **No public IP required** for your n8n server
- **Reliable uptime** with Cloudflare's infrastructure

### Step 1: Install cloudflared

#### Linux (Debian/Ubuntu)
```bash
# Add Cloudflare repository
curl -fsSL https://pkg.cloudflare.com/cloudflare-main.gpg | sudo tee /usr/share/keyrings/cloudflare-main.gpg >/dev/null
echo "deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] https://pkg.cloudflare.com/cloudflared $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/cloudflared.list
sudo apt-get update && sudo apt-get install cloudflared
```

#### Linux (RPM-based)
```bash
sudo curl -fsSL https://pkg.cloudflare.com/cloudflared.gpg | sudo gpg --dearmor -o /usr/share/keyrings/cloudflared.gpg
echo "deb [signed-by=/usr/share/keyrings/cloudflared.gpg] https://pkg.cloudflare.com/cloudflared $(rpm -E %rhel) main" | sudo tee /etc/yum.repos.d/cloudflared.repo
sudo dnf install cloudflared
```

#### Docker
```yaml
# docker-compose.yml
version: '3.8'
services:
  cloudflared:
    image: cloudflare/cloudflared:latest
    command: tunnel run
    environment:
      - TUNNEL_TOKEN=YOUR_TUNNEL_TOKEN
    restart: unless-stopped
```

### Step 2: Create Cloudflare Tunnel

#### Via Dashboard (Recommended)
1. Go to **Cloudflare Dashboard** → **Zero Trust**
2. Navigate to **Networks** → **Tunnels**
3. Click **Create a Tunnel**
4. Name: `n8n-webhook`
5. Choose your domain
6. Copy the **tunnel token**

#### Via CLI
```bash
# Authenticate
cloudflared tunnel login

# Create tunnel
cloudflared tunnel create n8n-webhook

# Save the tunnel ID from output
```

### Step 3: Configure Tunnel Routes

Edit tunnel config:
```bash
cloudflared tunnel route dns n8n-webhook n8n.yourdomain.com
```

Or create `~/.cloudflared/config.yml`:
```yaml
tunnel: YOUR_TUNNEL_ID
credentials-file: /home/user/.cloudflared/YOUR_TUNNEL_ID.json

ingress:
  - hostname: n8n.yourdomain.com
    service: http://localhost:5678
  - service: http_status:404
```

### Step 4: Run the Tunnel

```bash
# Run tunnel
cloudflared tunnel run n8n-webhook
```

### Step 5: Run as Systemd Service (Production)

Create `/etc/systemd/system/cloudflared.service`:
```ini
[Unit]
Description=Cloudflare Tunnel
After=network.target

[Service]
Type=simple
User=www-data
ExecStart=/usr/bin/cloudflared tunnel run n8n-webhook
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Enable and start:
```bash
sudo systemctl daemon-reload
sudo systemctl enable cloudflared
sudo systemctl start cloudflared
sudo systemctl status cloudflared
```

### Step 6: Verify Tunnel

```bash
# Check tunnel status
cloudflared tunnel list

# Test connectivity
curl -I https://n8n.yourdomain.com
```

### Step 7: Configure n8n for Webhooks

In your n8n instance settings (`.env` or environment variables):
```bash
# Webhook URL
WEBHOOK_URL=https://n8n.yourdomain.com/

# Editor URL
N8N_EDITOR_BASE_URL=https://n8n.yourdomain.com/

# Host
N8N_HOST=n8n.yourdomain.com

# HTTPS enabled
N8N_SECURE_COOKIE=true
```

### Tunnel Architecture

```
┌─────────────────┐
│  Facebook       │
│  Webhooks       │
└────────┬────────┘
         │ HTTPS
         ▼
┌─────────────────┐
│  Cloudflare     │
│  Edge Network   │
└────────┬────────┘
         │ Encrypted Tunnel
         ▼
┌─────────────────┐
│  cloudflared    │
│  Daemon         │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  n8n Server     │
│  (Local/Private)│
└─────────────────┘
```

### Security Best Practices

1. **Restrict Tunnel Access**
```yaml
# In config.yml
ingress:
  - hostname: n8n.yourdomain.com
    service: http://localhost:5678
    originRequest:
      ipRules:
        - ip: FACEBOOK_IP_RANGE  # If needed
          action: allow
```

2. **Enable Access Policies** (Cloudflare Zero Trust)
   - Go to **Access** → **Applications**
   - Add your tunnel domain
   - Require authentication for editor access

3. **Use Cloudflare WAF**
   - Enable Web Application Firewall
   - Block suspicious patterns
   - Rate limit webhook endpoints

---

## Facebook/Meta API Configuration

### Step 1: Create Meta Developer App

1. Go to [Meta for Developers](https://developers.facebook.com/)
2. Click **My Apps** → **Create App**
3. Select **Business** as app type
4. Fill in app details:
   - App Name: `YourShop Automation`
   - Business Account: Select your business
5. Click **Create App**

### Step 2: Add Required Products

Add these products to your app:
- **Messenger**
- **Pages**
- **Instagram** (if cross-platform)

### Step 3: Configure Permissions

Required permissions for shop automation:

| Permission | Purpose |
|------------|---------|
| `pages_manage_posts` | Read/write page posts |
| `pages_manage_metadata` | Access page settings |
| `pages_read_engagement` | Read comments, reactions |
| `pages_read_user_content` | Read messages |
| `pages_messaging` | Send/receive messages |
| `pages_show_list` | Access page list |

### Step 4: Generate Access Token

1. Go to **Tools & Support** → **Graph API Explorer**
2. Select your app
3. Select your Page
4. Add required permissions
5. Click **Generate Access Token**
6. **IMPORTANT**: For production, use **Long-Lived Tokens**:
   - Exchange short-lived token for long-lived (60 days)
   - Or use **System User Access Token** (never expires)

### Step 5: Get Page ID

```
https://graph.facebook.com/me/accounts
```

Or find it in your Page's **About** section.

### Step 6: Set Up Webhooks with Cloudflare Tunnel

1. In Meta App Dashboard → **Messenger** → **Settings**
2. Under **Webhooks**, click **Add URL**
3. **Webhook URL**: `https://n8n.yourdomain.com/webhook/facebook`
   - This uses your Cloudflare Tunnel domain
4. **Verify Token**: Create a custom secret (e.g., `your_secret_verify_token`)
5. **Subscribe to fields**:
   - `messages`
   - `messaging_postbacks`
   - `messaging_optins`
   - `messaging_referrals`
   - `feed` (for comments)

### Webhook Verification in n8n

Create a webhook endpoint that handles Facebook's verification:

```javascript
// Code node for webhook verification
const mode = $request.query['hub.mode'];
const token = $request.query['hub.verify_token'];
const challenge = $request.query['hub.challenge'];

if (mode === 'subscribe' && token === 'your_secret_verify_token') {
  return {
    statusCode: 200,
    body: challenge
  };
} else {
  return {
    statusCode: 403,
    body: 'Forbidden'
  };
}
```

---

## n8n Facebook Nodes

### Available Facebook Nodes

#### 1. **Facebook Trigger**
- **Purpose**: Listen for real-time events (messages, comments)
- **Events**: Message received, Comment added, Post created
- **Setup**: Requires webhook configuration

#### 2. **Facebook Graph API**
- **Purpose**: Make custom API calls
- **Use Cases**: Fetch posts, update page info, custom queries
- **Method**: GET, POST, DELETE, PUT

#### 3. **Facebook Pages**
- **Purpose**: Manage page content
- **Operations**: Create post, Get posts, Delete post

#### 4. **Facebook Messenger**
- **Purpose**: Send/receive messages
- **Operations**: Send message, Get sender profile, Mark as read

### Credential Setup in n8n

1. Go to **Credentials** → **Add Credential**
2. Select **Facebook Graph API**
3. Enter:
   - **Access Token**: Your long-lived token
   - **Page ID**: Your shop page ID
4. Test connection
5. Save

---

## Core Automation Workflows

### Workflow Architecture

```
┌─────────────────┐
│  Facebook       │
│  Trigger        │
│  (Webhook)      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Verify Token   │─── Validate webhook
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Switch/Router  │─── Route by event type
└────────┬────────┘
         │
    ┌────┴────┬────────────┬──────────┐
    ▼         ▼            ▼          ▼
┌───────┐ ┌───────┐ ┌──────────┐ ┌─────────┐
│Message│ │Comment│ │Post Event│ │Reaction │
│Handler│ │Handler│ │ Handler  │ │ Handler │
└───────┘ └───────┘ └──────────┘ └─────────┘
     │          │
     ▼          ▼
┌─────────┐ ┌─────────┐
│  Log to │ │  Log to │
│   CRM   │ │   CRM   │
└─────────┘ └─────────┘
```

### Basic Workflow Structure

```json
{
  "nodes": [
    {
      "name": "Webhook",
      "type": "n8n-nodes-base.webhook",
      "parameters": {
        "httpMethod": "GET",
        "path": "facebook",
        "responseMode": "onReceived"
      }
    },
    {
      "name": "Facebook Trigger",
      "type": "n8n-nodes-base.facebookTrigger",
      "parameters": {
        "events": ["messages", "feed"]
      }
    },
    {
      "name": "Router",
      "type": "n8n-nodes-base.switch",
      "parameters": {
        "dataType": "string",
        "value1": "={{ $json.entry[0].messaging[0].message ? 'message' : 'comment' }}"
      }
    }
  ]
}
```

---

## Message Handling & Auto-Responses

### Auto-Response Workflow

```
Facebook Trigger → Extract Message → Intent Detection → Response Logic → Send Message
```

### Step-by-Step Implementation

#### 1. Trigger Setup
```json
{
  "node": "Facebook Trigger",
  "parameters": {
    "events": ["messages"],
    "pageId": "YOUR_PAGE_ID"
  }
}
```

#### 2. Extract Message Data
Use **Code Node** or **Set Node**:
```javascript
// Extract key fields
const message = $input.first().json;
return {
  senderId: message.entry[0].messaging[0].sender.id,
  recipientId: message.entry[0].messaging[0].recipient.id,
  messageText: message.entry[0].messaging[0].message?.text || '',
  timestamp: message.entry[0].messaging[0].timestamp,
  messageId: message.entry[0].messaging[0].message?.mid
};
```

#### 3. Intent Detection (Keywords)
Use **Switch Node** for keyword matching:
```
Conditions:
- "price", "cost", "how much" → Price Inquiry
- "available", "stock", "in stock" → Availability Check
- "order", "buy", "purchase" → Order Process
- "shipping", "delivery" → Shipping Info
- "return", "refund" → Return Policy
- Default → Human Agent / FAQ
```

#### 4. Response Templates
Store templates in **Google Sheets** or **Code Node**:

```javascript
const responses = {
  greeting: "Hi! 👋 Welcome to [Shop Name]. How can I help you today?",
  price_inquiry: "Our products range from $X to $Y. Which product are you interested in?",
  availability: "Let me check our stock... ✅ Yes, this item is available!",
  shipping: "We ship nationwide! 📦 Delivery takes 3-5 business days.",
  catalog: "Here's our latest catalog: [LINK]",
  human_handoff: "Let me connect you with a team member who can help better!"
};
```

#### 5. Send Response
Use **Facebook Messenger Node**:
```json
{
  "node": "Facebook Messenger",
  "parameters": {
    "operation": "send",
    "recipientId": "={{ $json.senderId }}",
    "message": "={{ $json.response }}",
    "messageType": "RESPONSE"
  }
}
```

### Quick Replies & Buttons

Enhance responses with interactive elements:

```javascript
{
  "attachment": {
    "type": "template",
    "payload": {
      "template_type": "button",
      "text": "What would you like to know?",
      "buttons": [
        {
          "type": "postback",
          "title": "📦 View Catalog",
          "payload": "VIEW_CATALOG"
        },
        {
          "type": "postback", 
          "title": "💰 Check Prices",
          "payload": "CHECK_PRICES"
        },
        {
          "type": "postback",
          "title": "👤 Talk to Human",
          "payload": "HUMAN_AGENT"
        }
      ]
    }
  }
}
```

---

## Comment Management

### Auto-Reply to Comments

#### Trigger Setup
```json
{
  "node": "Facebook Trigger",
  "parameters": {
    "events": ["feed"],
    "fields": ["comments"]
  }
}
```

#### Comment Processing Logic

```javascript
// Extract comment data
const comment = $input.first().json;
return {
  commentId: comment.entry[0].changes[0].value.comment_id,
  postId: comment.entry[0].changes[0].value.post_id,
  fromId: comment.entry[0].changes[0].value.from_id,
  fromName: comment.entry[0].changes[0].value.from_name,
  message: comment.entry[0].changes[0].value.message,
  createdTime: comment.entry[0].changes[0].value.created_time
};
```

#### Auto-Response Strategies

| Comment Type | Auto-Response |
|--------------|---------------|
| Price inquiry | "Hi [Name]! Sent you a DM with pricing details! 💌" |
| "Is this available?" | "Yes! Still available. Check your inbox for details!" |
| "How to order?" | "Great choice! I've sent ordering instructions to your Messenger!" |
| Negative feedback | "We're sorry to hear this. Our team will contact you shortly." |
| Spam | Flag for review, no auto-response |

#### Send Comment Reply via Graph API

```json
{
  "node": "Facebook Graph API",
  "parameters": {
    "operation": "create",
    "path": "/{{ $json.commentId }}/comments",
    "httpMethod": "POST",
    "body": {
      "message": "={{ $json.autoResponse }}"
    }
  }
}
```

---

## Product Catalog Integration

### Option 1: Google Sheets Catalog

#### Sheet Structure
```
| Product ID | Name | Price | Description | Image URL | Stock | Category |
|------------|------|-------|-------------|-----------|-------|----------|
| P001 | Widget A | $29.99 | Description... | https://... | 50 | Electronics |
```

#### Lookup Workflow
```
User Query → Extract Product Name → Google Sheets Lookup → Format Response → Send
```

#### Code for Product Search
```javascript
const products = $input.all();
const query = $input.first().json.messageText.toLowerCase();

const matches = products.filter(p => 
  p.json.Name.toLowerCase().includes(query) ||
  p.json.Category.toLowerCase().includes(query)
);

if (matches.length === 0) {
  return { response: "Sorry, I couldn't find that product. Try browsing our catalog: [LINK]" };
}

const product = matches[0].json;
return {
  response: `✅ Found: ${product.Name}\n💰 Price: ${product.Price}\n📦 Stock: ${product.Stock} available\n\n${product.Description}`
};
```

### Option 2: Airtable Integration

Use **Airtable Node** for more robust catalog management:
- Better media handling
- Relational data (categories, variants)
- Built-in image galleries

### Option 3: E-commerce Platform Integration

Connect to:
- **Shopify** (via Shopify nodes)
- **WooCommerce** (via HTTP Request)
- **Custom API** (via HTTP Request node)

---

## Advanced Chatbot Logic

### Conversation State Management

Track conversation context using **Database** or **n8n Memory**:

#### Database Schema (Google Sheets/Airtable)
```
| Sender ID | Last Message | Conversation State | Last Updated | Context Data |
|-----------|--------------|-------------------|--------------|--------------|
| 123456 | "I want to buy" | ORDER_FLOW | 2024-01-15 | {"product": "P001"} |
```

#### State Machine Example

```javascript
// State router
const state = $json.conversationState;
const message = $json.messageText;

switch(state) {
  case 'INITIAL':
    if (message.includes('buy') || message.includes('order')) {
      return { nextState: 'PRODUCT_SELECTION', response: 'Which product would you like?' };
    }
    break;
  case 'PRODUCT_SELECTION':
    // Store selected product
    return { nextState: 'QUANTITY', response: 'How many would you like?' };
  case 'QUANTITY':
    // Store quantity, move to shipping
    return { nextState: 'SHIPPING_INFO', response: 'Please share your delivery address' };
  case 'SHIPPING_INFO':
    // Complete order
    return { nextState: 'CONFIRMATION', response: 'Order confirmed! Order #12345' };
}
```

### Multi-Step Order Flow

```
1. Greeting → 2. Product Selection → 3. Quantity → 4. Customer Info → 5. Payment → 6. Confirmation
```

### Natural Language Processing (Optional)

Integrate with:
- **Dialogflow** (Google)
- **Rasa** (Open source)
- **OpenAI** (via HTTP Request)

Example with OpenAI:
```javascript
// Send to OpenAI for intent classification
const prompt = `Classify this message: "${message}". Categories: price, availability, order, shipping, complaint, other`;
// Use HTTP Request node to call OpenAI API
```

---

## Best Practices for High-Volume Shops

### Performance Optimization

#### 1. Rate Limiting
```javascript
// Add delay between messages
{
  "node": "Wait",
  "parameters": {
    "waitBetween": "1000ms"
  }
}
```

#### 2. Queue Management
- Use **n8n Queue Mode** for high volume
- Set up **Redis** for job queuing
- Configure worker nodes

#### 3. Database Caching
- Cache product data in memory
- Use **Redis** for session storage
- Implement TTL for temporary data

#### 4. Batch Processing
- Process comments in batches
- Use **Split In Batches** node
- Avoid API rate limits

### Response Time Targets

| Metric | Target |
|--------|--------|
| First response | < 30 seconds |
| Resolution time | < 5 minutes |
| Human handoff | < 2 minutes |

### Message Templates Compliance

Facebook requires:
- **24-hour window**: Only respond within 24h of user message
- **Message tags**: Use proper tags for non-promotional messages
- **Opt-out**: Allow users to stop messages

### Tag Types
```
- CONFIRMED_EVENT_UPDATE
- ISSUE_RESOLUTION
- NON_PROMOTIONAL_SUBSCRIPTION
- PAGE_EVENT_UPDATE
```

---

## Error Handling & Rate Limits

### Facebook API Rate Limits

| Endpoint | Rate Limit |
|----------|------------|
| Messenger Send | 5,000 messages/sec |
| Graph API Read | 200 calls/user/hour |
| Graph API Write | Varies by endpoint |

### Error Handling Strategy

#### 1. Try-Catch Pattern
```
[Main Flow] → [Error Trigger] → [Log Error] → [Send Fallback Response] → [Notify Admin]
```

#### 2. Retry Logic
```json
{
  "node": "Wait",
  "parameters": {
    "resume": "afterTimeInterval",
    "amount": 5,
    "unit": "seconds"
  }
}
// Connect to retry the failed operation
```

#### 3. Error Logging
```javascript
// Log errors to Google Sheets
{
  "timestamp": new Date().toISOString(),
  "workflow": $workflow.name,
  "error": $json.error.message,
  "userId": $json.senderId
}
```

### Fallback Responses

```javascript
const fallbacks = [
  "Sorry, I'm experiencing technical difficulties. A team member will help shortly!",
  "I'm having trouble processing that. Let me get a human to assist!",
  "Technical issue detected. Please try again in a few moments!"
];
```

### Cloudflare Tunnel Specific Error Handling

#### Monitor Tunnel Health
```bash
# Check tunnel status
cloudflared tunnel info n8n-webhook

# View logs
journalctl -u cloudflared -f
```

#### Automatic Restart on Failure
```ini
# In systemd service
Restart=always
RestartSec=5
StartLimitBurst=3
StartLimitInterval=60
```

---

## Testing & Debugging

### Testing Checklist

- [ ] Cloudflare tunnel is running and accessible
- [ ] Webhook receives events correctly
- [ ] Message extraction works
- [ ] Intent detection accurate
- [ ] Responses send successfully
- [ ] Error handling triggers properly
- [ ] Rate limiting works
- [ ] Database operations succeed

### Debug Mode Workflow

Create a parallel debug workflow:
```
[Main Flow] → [Debug Output] → [Send to Admin Messenger]
```

### Use n8n Execution Data

1. Enable **Save Execution Data** in workflow settings
2. Review **Executions** tab for failures
3. Use **Pin Data** for testing specific scenarios

### Facebook Debugger Tools

- **Graph API Explorer**: Test API calls
- **Webhook Test**: Verify webhook delivery
- **Page Inbox**: Monitor sent messages

### Test Webhook with curl

```bash
# Test your Cloudflare tunnel is accessible
curl -I https://n8n.yourdomain.com/webhook/facebook

# Simulate Facebook webhook (for testing)
curl -X POST https://n8n.yourdomain.com/webhook/facebook \
  -H "Content-Type: application/json" \
  -d '{
    "object": "page",
    "entry": [{
      "id": "PAGE_ID",
      "messaging": [{
        "sender": {"id": "SENDER_ID"},
        "recipient": {"id": "PAGE_ID"},
        "timestamp": 1234567890,
        "message": {"text": "test"}
      }]
    }]
  }'
```

---

## Complete Workflow Examples

### Example 1: Complete Auto-Response Workflow

**Workflow: Message → Intent → Response → Log**

```json
{
  "name": "Facebook Auto-Responder",
  "nodes": [
    {
      "name": "Facebook Trigger",
      "type": "n8n-nodes-base.facebookTrigger",
      "parameters": {
        "events": ["messages"],
        "pageId": "YOUR_PAGE_ID"
      }
    },
    {
      "name": "Extract Message",
      "type": "n8n-nodes-base.code",
      "parameters": {
        "jsCode": "const msg = $input.first().json;\nreturn {\n  senderId: msg.entry[0].messaging[0].sender.id,\n  text: msg.entry[0].messaging[0].message?.text || '',\n  timestamp: msg.entry[0].messaging[0].timestamp\n};"
      }
    },
    {
      "name": "Intent Router",
      "type": "n8n-nodes-base.switch",
      "parameters": {
        "dataType": "string",
        "value1": "={{ $json.text.toLowerCase() }}",
        "rules": [
          {"value2": "price", "operation": "contains"},
          {"value2": "order", "operation": "contains"},
          {"value2": "shipping", "operation": "contains"}
        ]
      }
    },
    {
      "name": "Send Price Response",
      "type": "n8n-nodes-base.facebookMessenger",
      "parameters": {
        "operation": "send",
        "recipientId": "={{ $json.senderId }}",
        "message": "Our prices range from $10-$100. Check our catalog: [LINK]"
      }
    },
    {
      "name": "Log to Google Sheets",
      "type": "n8n-nodes-base.googleSheets",
      "parameters": {
        "operation": "append",
        "sheetId": "YOUR_SHEET_ID",
        "range": "Messages!A:C",
        "columns": ["senderId", "text", "timestamp"]
      }
    }
  ]
}
```

### Example 2: Comment-to-DM Converter

When someone comments "interested" or "price":
1. Auto-reply to comment
2. Send detailed info via DM
3. Log lead to CRM

### Example 3: Order Status Checker

User sends order number → Lookup in database → Send status update

### Example 4: Abandoned Cart Recovery

1. Track users who asked about products
2. Wait 24 hours (using Wait node)
3. Send follow-up: "Still interested in [Product]?"

### Example 5: Review Request Automation

After order confirmation → Wait 7 days → Request review

### Example 6: Complete Product Inquiry Flow

```
Facebook Trigger
    │
    ▼
Extract Message & Sender
    │
    ▼
Check Conversation State (Database Lookup)
    │
    ├──► New User ──► Send Welcome + Menu
    │
    └──► Existing User ──► Check State
              │
              ├──► BROWSING ──► Send Product Info
              │
              ├──► ORDERING ──► Process Order
              │
              └──► SUPPORT ──► Route to Human
```

---

## Troubleshooting

### Common Issues

#### Webhook Not Receiving Events

**Check:**
- Cloudflare tunnel is running: `cloudflared tunnel list`
- Webhook URL is accessible: `curl -I https://n8n.yourdomain.com/webhook/facebook`
- Verify token matches in Facebook settings
- Page subscription is active
- Permissions are granted

**Test Tunnel:**
```bash
# Verify tunnel status
cloudflared tunnel info n8n-webhook

# Check if n8n is reachable locally
curl -I http://localhost:5678/webhook/facebook
```

#### Messages Not Sending

**Check:**
- Access token is valid (not expired)
- 24-hour window rule
- Message doesn't violate policies
- Page ID is correct

#### Rate Limit Errors

**Solutions:**
- Add Wait nodes between requests
- Implement exponential backoff
- Use batch processing
- Upgrade to Business API

#### Token Expiration

**Fix:**
```bash
# Exchange short-lived token for long-lived
curl -G "https://graph.facebook.com/v18.0/oauth/access_token" \
  -d "grant_type=fb_exchange_token" \
  -d "client_id=YOUR_APP_ID" \
  -d "client_secret=YOUR_APP_SECRET" \
  -d "fb_exchange_token=SHORT_LIVED_TOKEN"
```

#### Cloudflare Tunnel Issues

**Tunnel won't start:**
```bash
# Check configuration
cloudflared tunnel check-config

# View detailed logs
cloudflared tunnel run n8n-webhook --verbose
```

**Tunnel disconnects frequently:**
- Check network stability
- Increase systemd restart settings
- Consider running multiple tunnel instances

### Debug Commands

```javascript
// Log full event data in n8n
console.log(JSON.stringify($input.first().json, null, 2));

// Test API connection
// Use Facebook Graph API node with: /me/accounts

// Verify webhook
// Check Facebook App Dashboard → Webhooks
```

### Cloudflare Dashboard Diagnostics

1. Go to **Zero Trust Dashboard** → **Networks** → **Tunnels**
2. Check tunnel status (should show "Healthy")
3. View **Requests** tab for traffic analytics
4. Check **Events** for connection issues

---

## Quick Reference

### Essential Expressions

```javascript
// Extract sender ID
{{ $json.entry[0].messaging[0].sender.id }}

// Extract message text
{{ $json.entry[0].messaging[0].message.text }}

// Get timestamp
{{ new Date($json.entry[0].messaging[0].timestamp).toISOString() }}

// Comment data
{{ $json.entry[0].changes[0].value.message }}
```

### Common Graph API Endpoints

```
GET  /{page-id}/conversations          - List conversations
POST /{recipient-id}/messages          - Send message
GET  /{comment-id}/comments            - Get comment replies
POST /{post-id}/comments               - Reply to post
GET  /{page-id}/posts                  - Get page posts
```

### Recommended Node Stack

| Purpose | Node |
|---------|------|
| Trigger | Facebook Trigger / Webhook |
| Send Message | Facebook Messenger |
| Custom API | Facebook Graph API |
| Logic | Switch, IF, Code |
| Data | Google Sheets, Airtable |
| Delay | Wait |
| HTTP | HTTP Request |

### Cloudflare Commands Quick Reference

```bash
# Check tunnel status
cloudflared tunnel list

# Run tunnel
cloudflared tunnel run n8n-webhook

# View tunnel info
cloudflared tunnel info n8n-webhook

# Check config
cloudflared tunnel check-config

# Restart service
sudo systemctl restart cloudflared

# View logs
journalctl -u cloudflared -f
```

---

## Resources

### Official Documentation
- [n8n Facebook Nodes](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.facebook/)
- [Meta Graph API](https://developers.facebook.com/docs/graph-api)
- [Messenger Platform](https://developers.facebook.com/docs/messenger-platform)
- [Cloudflare Tunnel Docs](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/)

### Community
- [n8n Forum](https://community.n8n.io/)
- [n8n Facebook Automation Templates](https://n8n.io/workflows?categories=facebook)
- [Cloudflare Community](https://community.cloudflare.com/)

### Tools
- [Graph API Explorer](https://developers.facebook.com/tools/explorer/)
- [Webhook Tester](https://webhook.site/)
- [Cloudflare Zero Trust Dashboard](https://one.dash.cloudflare.com/)

---

*Last Updated: March 2026*
*Version: 1.0*
