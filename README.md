# PeaPod Script Approval System

A custom approval interface for the Baringa PeaPod podcast automation system.

## Overview

This form allows team leads to review, edit, and approve AI-generated podcast scripts before audio production begins. It integrates with Zapier workflows to create a human-in-the-loop approval process.

## Features

- 📝 **Script Review**: View complete podcast scripts with episode details
- ✏️ **Inline Editing**: Edit scripts directly in the browser before approval
- 🔄 **Real-time Sync**: Fetches scripts from Google Sheets via public API
- 🎨 **Branded Interface**: Custom Baringa-themed design
- ⚡ **Webhook Integration**: Sends approval data directly to Zapier

## How It Works

1. **Zap 1** generates a podcast script and stores it in Google Sheets
2. Approver receives email with unique approval link
3. Form loads script from Google Sheets using `episode_date` parameter
4. Approver reviews/edits script and clicks "Approve" or "Reject"
5. Form sends decision via webhook to **Zap 1B** for processing
6. If approved, audio generation begins automatically

## Configuration

Three key constants in `index.html` (lines 3-5):

```javascript
const SPREADSHEET_ID = '1wKThYAdRTzoPfb5rMih8vMlU1LW6EfS_l7l0uR9TcoY';  // Podcast_Approvals spreadsheet
const SHEET_NAME = 'Pending_Scripts';                                      // Worksheet name
const WEBHOOK_URL = 'https://hooks.zapier.com/hooks/catch/24513013/u80josc/';  // Zap 1B webhook
```

Update these if:
- Moving to a different Google Sheets spreadsheet
- Renaming the worksheet
- Creating a new Zap 1B (new webhook URL)

## URL Parameters

The form expects these query parameters:

- `episode_date` (required): Format `YYYY-MM-DD` - matches script in Google Sheets
- `team_name` (optional): Pre-filled team name
- `episode_title` (optional): Pre-filled episode title

**Example URL:**
```
https://peteshannon.github.io/peapod-approval/?episode_date=2025-11-18&team_name=Monetising%20Digital%20Media&episode_title=Tech%20News%20Digest
```

## Google Sheets Requirements

The `Pending_Scripts` worksheet must be **publicly readable** with these columns:

| Column | Description |
|--------|-------------|
| Episode_Date | Date in `YYYY-MM-DD` format (lookup key) |
| Team_Name | Team identifier |
| Episode_Title | Episode title |
| Full_Script | Complete podcast script (editable) |
| Created_Timestamp | When script was generated |
| Status | `pending` or `approved` |

## Webhook Payload

When approved/rejected, the form sends this JSON via GET query parameters:

```json
{
  "team_name": "Monetising Digital Media",
  "episode_date": "2025-11-18",
  "episode_title": "Tech News Digest",
  "script": "[Full script text, possibly edited]",
  "decision": "Approve",
  "comments": "[Optional feedback]",
  "timestamp": "2025-11-18T10:30:00.000Z"
}
```

## Development

**Local Testing:**
1. Update `WEBHOOK_URL` to a test endpoint (e.g., webhook.site)
2. Ensure Google Sheets is publicly accessible
3. Open `index.html` with valid `episode_date` parameter

**Deployment:**
- Hosted via GitHub Pages
- Live URL: https://peteshannon.github.io/peapod-approval/
- Updates automatically on push to `main` branch

## Security Notes

- ⚠️ Google Sheets must be publicly readable (contains no sensitive data)
- ✅ Webhook uses HTTPS
- ✅ Form validates data before submission
- ⚠️ No authentication on form (security via URL obscurity)

## Support

For issues or questions:
- **Project Owner**: Pete Shannon (pete.shannon@baringa.com)
- **Documentation**: See project knowledge base in main Baringa automation project

## License

Internal use only - Baringa Partners Ltd.

---

**Version**: 2.0  
**Last Updated**: November 2025  
**Part of**: PeaPod Podcast Automation System
