# 📧 AI-Powered Gmail Inbox Zero Automation

An intelligent n8n workflow that automatically categorizes, labels, and routes your emails using GPT-4o-mini — achieving Inbox Zero on autopilot.

![n8n](https://img.shields.io/badge/n8n-Workflow-FF6D5A?style=for-the-badge&logo=n8n&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4o--mini-412991?style=for-the-badge&logo=openai&logoColor=white)
![Gmail](https://img.shields.io/badge/Gmail-API-EA4335?style=for-the-badge&logo=gmail&logoColor=white)
![Telegram](https://img.shields.io/badge/Telegram-Alerts-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)

---

## 🎯 What It Does

This workflow transforms your chaotic inbox into an organized, automated system:

| Step | Action |
|------|--------|
| 1️⃣ | Fetches unprocessed emails from Gmail |


<img width="1615" height="1298" alt="image" src="https://github.com/user-attachments/assets/9a45f3a7-44ac-40bc-a844-45e60bac3c65" />

| 2️⃣ | Sends email metadata to OpenAI for intelligent classification |


<img width="2472" height="1310" alt="image" src="https://github.com/user-attachments/assets/4cb0e75e-7c57-47cd-9285-517dea63b3ad" />

| 3️⃣ | Applies category labels automatically |


<img width="2435" height="1287" alt="image" src="https://github.com/user-attachments/assets/49596aee-4d40-47de-887a-1bc27b002db6" />


| 4️⃣ | Archives low-priority emails (newsletters, promotions, social) |


| 5️⃣ | Sends instant Telegram alerts for urgent items |

---

## 🧠 AI Classification Categories

The workflow uses a carefully crafted prompt to classify emails into 8 categories:

| Category | Description | Auto-Archive |
|----------|-------------|:------------:|
| `URGENT` | Needs immediate action today | ❌ |
| `WORK` | Professional emails from humans | ❌ |
| `PERSONAL` | Friends & family | ❌ |
| `FINANCE` | Banks, invoices, receipts | ❌ |
| `SOCIAL` | LinkedIn, Twitter, Facebook notifications | ✅ |
| `SUBSCRIPTIONS` | Security alerts, login codes, ToS updates | ✅ |
| `NEWSLETTERS` | Blog digests, news roundups | ✅ |
| `PROMOTIONS` | Marketing, sales, discounts | ✅ |

> **Multi-label Support:** A single email can receive multiple categories (e.g., `WORK` + `URGENT`)

---
<img width="1953" height="807" alt="image" src="https://github.com/user-attachments/assets/83a9d734-f6eb-46b1-aafc-550b7ee7d9d4" />

## 🏗️ Workflow Architecture

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Trigger   │────▶│ Fetch Gmail │────▶│ Build Label │────▶│  Get Inbox  │
│  (Manual)   │     │   Labels    │     │     Map     │     │   Emails    │
└─────────────┘     └─────────────┘     └─────────────┘     └──────┬──────┘
                                                                   │
                    ┌──────────────────────────────────────────────┘
                    ▼
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Process    │────▶│  Get Full   │────▶│   Prepare   │────▶│  AI Classify │
│  One-by-One │     │   Message   │     │  Email Data │     │  (OpenAI)   │
└─────────────┘     └─────────────┘     └─────────────┘     └──────┬──────┘
       ▲                                                           │
       │            ┌──────────────────────────────────────────────┘
       │            ▼
       │      ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
       │      │   Parse AI  │────▶│  Add Gmail  │────▶│   Archive   │
       │      │   Response  │     │   Labels    │     │   Check     │
       │      └─────────────┘     └─────────────┘     └──────┬──────┘
       │                                                     │
       │                          ┌──────────────────────────┴──────────────┐
       │                          ▼                                         ▼
       │                    ┌───────────┐                            ┌───────────┐
       │                    │  Archive  │                            │   Keep    │
       │                    │   Email   │                            │ in Inbox  │
       │                    └─────┬─────┘                            └─────┬─────┘
       │                          │                                        │
       │                          └────────────────┬───────────────────────┘
       │                                           ▼
       │                                    ┌─────────────┐
       │                                    │   Notify    │
       │                                    │   Check     │
       │                                    └──────┬──────┘
       │                          ┌────────────────┴────────────────┐
       │                          ▼                                 ▼
       │                   ┌─────────────┐                   ┌─────────────┐
       │                   │  Telegram   │                   │     No      │
       │                   │    Alert    │                   │   Action    │
       │                   └──────┬──────┘                   └──────┬──────┘
       │                          │                                 │
       │                          └────────────────┬────────────────┘
       │                                           ▼
       └───────────────────────────────────[ Next Email ]
```

---

## ⚙️ Setup Instructions

### Prerequisites

- [n8n](https://n8n.io/) instance (self-hosted or cloud)
- Gmail account with API access enabled
- OpenAI API key
- Telegram Bot (optional, for urgent alerts)

### Step 1: Create Gmail Labels

Create these labels in your Gmail account:

```
AI-Urgent
AI-Work
AI-Personal
AI-Finance
AI-Subscriptions
AI-Newsletters
AI-Promotions
AI-Social
AI-Processed
```

### Step 2: Configure Credentials in n8n

| Credential | Type | Notes |
|------------|------|-------|
| Gmail | OAuth2 | Enable Gmail API in Google Cloud Console |
| OpenAI | API Key | Get from platform.openai.com |
| Telegram | Bot Token | Create bot via @BotFather |

### Step 3: Import the Workflow

1. Download `AI_Email_Organizer_Portfolio_Ready.json`
2. In n8n, go to **Workflows** → **Import from File**
3. Select the JSON file
4. Update the Telegram `chatId` in the "Telegram Alert" node

### Step 4: Test & Activate

1. Click **Execute Workflow** to test manually
2. Check that labels are being applied correctly
3. Once validated, set up a Schedule Trigger (e.g., every 15 minutes)

---

## 🔧 Configuration Options

### Customizing Categories

Edit the system prompt in the "AI Classify (Direct API)" node to modify:

- Category definitions
- Classification rules
- Priority scoring logic

### Adjusting Auto-Archive Rules

In the "Parse Result" node, modify the `archiveTags` array:

```javascript
const archiveTags = ['NEWSLETTERS', 'PROMOTIONS', 'SOCIAL', 'SUBSCRIPTIONS'];
```

### Changing Batch Size

The workflow processes emails one at a time for stability. To change this, modify the "Process One at a Time" node settings.

---

## 📊 Key Features

| Feature | Implementation |
|---------|----------------|
| **Structured Output** | Uses `response_format: json_object` for reliable parsing |
| **Low Temperature** | `0.1` for consistent, deterministic classifications |
| **Label ID Mapping** | Fetches Gmail labels dynamically to avoid hardcoding IDs |
| **Error Handling** | Graceful fallbacks with debug logging |
| **Idempotency** | `AI-Processed` label prevents re-processing |
| **Rate Limit Safe** | Sequential processing with SplitInBatches |

---

## 🛡️ Error Handling

The workflow includes several safeguards:

- **Missing Labels:** Debug log captures any Gmail labels not found
- **API Failures:** Default category (`WORK`) applied if AI fails
- **Malformed JSON:** Regex extraction with fallback parsing
- **Loop Protection:** `AI-Processed` label excludes previously handled emails

---

## 📁 File Structure

```
.
├── AI_Email_Organizer_Portfolio_Ready.json   # Main workflow file
├── README.md                                  # This file
└── screenshots/                               # (Optional) Workflow screenshots
    └── workflow-overview.png
```

---

## 🚀 Future Improvements

- [ ] Add scheduled trigger (cron) for automatic execution
- [ ] Implement email threading detection
- [ ] Add sentiment analysis for priority scoring
- [ ] Create dashboard for classification statistics
- [ ] Support for custom user-defined categories

---

## 📝 License

MIT License - Feel free to use and modify for your own projects.

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

---

## 👤 Author

**Allen**  
MSc Data Science & AI | Automation Enthusiast

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat&logo=linkedin)](https://www.linkedin.com/in/alireza-sharafzad/)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat&logo=github)](https://github.com/AllenSharafzad)

---

<p align="center">
  <i>Built with ☕ and n8n</i>
</p>
