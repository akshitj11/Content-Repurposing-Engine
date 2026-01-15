# 🚀 AI-Powered Content Repurposing Engine

[![n8n](https://img.shields.io/badge/n8n-Workflow-EA4B71)](https://n8n.io)
[![Claude AI](https://img.shields.io/badge/Claude-Sonnet%204-7C3AED)](https://anthropic.com)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

> **Transform one long-form article into platform-optimized content for Twitter, LinkedIn, Instagram, and Email in seconds using Claude AI and n8n automation.**

<img width="1701" height="627" alt="Screenshot 2026-01-15 194855" src="https://github.com/user-attachments/assets/79f2b0b0-40ec-4aa0-8afa-e24854092a3e" />

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Architecture](#-architecture)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Usage](#-usage)
- [Cost Analysis](#-cost-analysis)
- [Roadmap](#-roadmap)



---

## 🎯 Overview

Content creators spend hours adapting their articles for different social platforms. This automated workflow solves that problem by:

1. **Taking** one long-form article as input
2. **Generating** platform-specific content using Claude AI
3. **Posting** automatically to Twitter, LinkedIn, Gmail, and archiving to Google Sheets
4. **Notifying** you of success/failure via Telegram

**Time saved:** 2-3 hours per article → **15 seconds**

---

## ✨ Features

### 🤖 AI-Powered Content Generation
- **Single API call** generates all formats simultaneously
- **Platform-optimized** content (character limits, tone, formatting)
- **Consistent quality** across all outputs

### 📱 Multi-Platform Publishing
- **Twitter/X**: 10-12 tweet threads with hooks and CTAs
- **LinkedIn**: Professional posts with strategic hashtags (under 1,300 chars)
- **Email**: Compelling subject lines + HTML-formatted body
- **Instagram**: Engaging captions with strategic emoji placement

### 🛡️ Built-in Safeguards
- **Manual review mode**: Preview before posting
- **Error handling**: Continues if one platform fails
- **Rate limiting**: Respects API constraints
- **Validation**: Ensures content meets platform requirements

### 📊 Content Archiving
- **Google Sheets integration**: Maintains posting history
- **Analytics tracking**: Monitor performance over time
- **Backup system**: Never lose generated content

### 🔔 Smart Notifications
- **Real-time alerts** via Telegram
- **Success/failure reporting**
- **Performance summaries**

---

## 🏗️ Architecture

```
┌─────────────────┐
│  Content Input  │
│  (Article/Blog) │
└────────┬────────┘
         │
         ▼
┌─────────────────────────────┐
│   Claude API (Single Call)  │
│   Generates ALL Formats     │
└────────┬────────────────────┘
         │
         ▼
┌─────────────────┐
│  Parse Response │
│  (JSON Cleanup) │
└────────┬────────┘
         │
         ▼
┌──────────────────────────────┐
│  Manual Review Gate (IF)     │
│  ├─ TRUE  → Stop & Preview   │
│  └─ FALSE → Auto-Post        │
└────────┬─────────────────────┘
         │ (FALSE branch)
         ├─────────┬─────────┬─────────┐
         ▼         ▼         ▼         ▼
    ┌────────┐ ┌──────┐ ┌──────┐ ┌────────┐
    │Twitter │ │LinkedIn│ │Gmail │ │Sheets │
    └───┬────┘ └───┬──┘ └───┬──┘ └───┬────┘
        └──────────┴────────┴─────────┘
                   │
                   ▼
            ┌─────────────┐
            │Merge Results│
            └──────┬──────┘
                   │
                   ▼
            ┌─────────────┐
            │   Summary   │
            └──────┬──────┘
                   │
                   ▼
            ┌─────────────┐
            │  Telegram   │
            │Notification │
            └─────────────┘
```

---

## 📦 Prerequisites

### Required
- **n8n** (v1.0+) - [Install Guide](https://docs.n8n.io/hosting/)
- **Claude API Key** - [Get Free Key](https://console.anthropic.com) ($5 free credit)
- **Node.js** 18+ (if self-hosting n8n)

### Optional (for auto-posting)
- **Twitter Developer Account** - [Apply Here](https://developer.twitter.com)
- **LinkedIn Developer App** - [Create App](https://www.linkedin.com/developers/)
- **Google Cloud Project** (for Gmail & Sheets) - [Setup Guide](https://console.cloud.google.com)
- **Telegram Bot Token** - [Create Bot](https://t.me/botfather)

---

## 🚀 Installation

### Option 1: Import Workflow JSON (Recommended)

1. **Clone this repository**
   ```bash
   git clone https://github.com/yourusername/content-repurposing-engine.git
   cd content-repurposing-engine
   ```

2. **Open n8n**
   - n8n Cloud: https://app.n8n.cloud
   - Self-hosted: `http://localhost:5678`

3. **Import workflow**
   - Click `+` → `Import from File`
   - Select `workflow.json` from this repo
   - Click `Import`

4. **Done!** Workflow is now in your n8n instance


---

## ⚙️ Configuration

### 1. Claude API Credential

1. Get API key from [console.anthropic.com](https://console.anthropic.com)
2. In n8n, click the "HTTP Request - Claude API" node
3. Add credential:
   - Type: **Header Auth**
   - Name: `x-api-key`
   - Value: `YOUR_CLAUDE_API_KEY`

### 2. Social Media Credentials (Optional)

#### Twitter OAuth2
```
1. Create app at developer.twitter.com
2. Enable OAuth 2.0 with Read+Write permissions
3. Add callback URL: https://your-n8n-instance.com/rest/oauth2-credential/callback
4. Copy Client ID and Client Secret to n8n
```

#### LinkedIn OAuth2
```
1. Create app at linkedin.com/developers
2. Request r_liteprofile, r_emailaddress, w_member_social scopes
3. Add redirect URI: https://your-n8n-instance.com/rest/oauth2-credential/callback
4. Configure OAuth2 in n8n
```

#### Gmail OAuth2
```
1. Create Google Cloud project
2. Enable Gmail API
3. Create OAuth 2.0 credentials
4. Add authorized redirect URI
5. Configure in n8n
```

#### Google Sheets OAuth2
```
Same as Gmail - use the same Google Cloud project
Enable Google Sheets API
```

#### Telegram Bot
```
1. Message @BotFather on Telegram
2. Create new bot with /newbot
3. Copy bot token
4. Add as credential in n8n
```

### 3. Workflow Parameters

Edit the Manual Trigger node fields:

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `content_text` | String | Yes | Your article content |
| `content_title` | String | Yes | Article title |
| `target_audience` | String | No | Target audience (default: "general audience") |
| `manual_review` | Boolean | No | Enable to review before posting |

---

## 💡 Usage

### Basic Usage

1. **Execute workflow** (click play button)
2. **Fill in the form:**
   ```
   Content Title: "5 Productivity Hacks That Actually Work"
   Content Text: [Paste your article]
   Target Audience: "professionals and entrepreneurs"
   Manual Review: false
   ```
3. **Click "Execute Workflow"**
4. **Wait 10-15 seconds** for completion
5. **Check results** in execution log or Telegram

### With Manual Review

Set `manual_review: true` to preview content before posting:

1. Workflow generates content
2. Stops at IF node with preview
3. Review the generated posts
4. Manually trigger posting if satisfied

### Scheduled Execution

Convert to scheduled workflow:

1. Replace **Manual Trigger** with **Schedule Trigger**
2. Set cron expression (e.g., daily at 9 AM)
3. Pull content from Google Sheets or CMS
4. Fully automated posting!



---

## 💰 Cost Analysis

### Per Workflow Execution

| Service | Cost | Notes |
|---------|------|-------|
| Claude API | $0.01 | 4,000 tokens input + output |
| Twitter API | Free | Within rate limits |
| LinkedIn API | Free | 100 posts/day limit |
| Gmail API | Free | 100 emails/day (personal) |
| Google Sheets API | Free | Within quotas |
| Telegram Bot | Free | Unlimited messages |

**Total per run: ~$0.01** (just Claude API)

**Monthly cost (100 articles):** $1.00

### Comparison

| Method | Time | Cost (monthly) |
|--------|------|----------------|
| Manual | 200 hours | $0 (your time) |
| Virtual Assistant | 200 hours | $2,000 |
| This Automation | 25 minutes | $1 |

**Time saved:** 199.5 hours/month
**Money saved:** $1,999/month

---






## 🗺️ Roadmap

### Phase 1: Core Features ✅
- [x] AI content generation
- [x] Multi-platform posting
- [x] Manual review mode
- [x] Error handling

### Phase 2: Enhancements 🚧
- [ ] Image generation (DALL-E integration)
- [ ] A/B testing (multiple variations)
- [ ] Analytics dashboard
- [ ] Content queue system
- [ ] Webhook trigger for CMS integration

### Phase 3: Advanced Features 📅
- [ ] Performance tracking
- [ ] Optimal posting time calculation
- [ ] Sentiment analysis
- [ ] Multi-language support
- [ ] Video content support (YouTube, TikTok)

### Phase 4: Enterprise Features 🔮
- [ ] Team collaboration
- [ ] Approval workflows
- [ ] Brand voice customization
- [ ] Compliance checks
- [ ] ROI reporting




---

## 🙏 Acknowledgments

- **[n8n](https://n8n.io)** - Amazing workflow automation platform
- **[Anthropic](https://anthropic.com)** - Claude AI API
- **Community contributors** - Thank you!


---

<p align="center">
  Made with ❤️ by <a href="https://github.com/akshitj11">Akshit Joshi</a>
</p>

<p align="center">
  <sub>Built with n8n • Powered by Claude AI • Open Source</sub>
</p>
