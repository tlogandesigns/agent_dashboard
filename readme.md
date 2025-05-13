# Agent Dashboard

A custom ExpressionEngine module to schedule, approve, and publish social media posts to Meta, Twitter/X, and LinkedIn. Built with Bootstrap 5 and a streamlined approval workflow for marketing teams and real estate agents.

---

## Features

- Multi-platform post scheduling (Meta, X, LinkedIn)
- Role-based approval workflow (User → Reviewer → Admin)
- OAuth integration for secure API publishing
- CRON or CLI background script for post delivery
- Responsive Bootstrap UI with calendar and filter views
- Native ExpressionEngine channel integration

---

## Architecture


```
[ Agents / Marketing Admins (Web UI) ]
         |
[ EE Frontend Templates (Bootstrap + jQuery/AJAX) ]
         |
[ ExpressionEngine Backend (Channels + Add-Ons) ]
         ├─ EE Channel: Scheduled Posts
         ├─ EE Channel: Social Accounts
         ├─ EE Member Roles: User, Reviewer, Admin
         ├─ Custom EE Module: Post Scheduler Engine
         ├─ OAuth Integration: Meta / X / LinkedIn
         |
[ Cron or Worker Script (PHP CLI or queued) ]
         └─ Fetch Scheduled Posts → Publish via API → Log Result
         |
[ Third-Party APIs ]
    ├─ Meta Graph API (FB/IG)
    ├─ Twitter/X API
    ├─ LinkedIn Marketing API
         |
[ MySQL DB (via EE) ]
    ├─ exp_channel_titles
    ├─ exp_channel_data (Scheduled Posts, Accounts)
    ├─ exp_members (Auth)
    ├─ Custom tables (optional: post_logs, tokens, retries)
```

| Component                    | Description                                                               |
|------------------------------|---------------------------------------------------------------------------|
| EE Channel: Scheduled Posts  | Fields: Post Text, Image(s), Platform(s), Date/Time, Status, Account      |
| Custom EE Module: Scheduler  | Handles queue checking, API auth, post dispatch, error logging            |
| OAuth Integration            | Store and refresh tokens securely per account                             |
| Approval Workflow            | Role-based review before publish (draft → approved → queued)              |
| Frontend UI (Bootstrap)      | Post composer, calendar view, filter by platform/status/account           |
| Cron/Worker Script           | Executes publishing logic every X minutes (e.g. via CRON or queue system) |

This diagram shows how users interact with the EE frontend, backend logic, MySQL storage, and third-party social media APIs.

---

## Requirements

- PHP 8.x
- MySQL 5.7+
- ExpressionEngine 7.x
- Bootstrap 5 (via EE templates)
- CRON job or ability to run PHP scripts via CLI
- Developer accounts for Meta, Twitter, and LinkedIn

---

## Installation

1. Clone or download this repo into your ExpressionEngine `/system/user/addons/` directory.
2. Enable the add-on in **Add-On Manager**.
3. Create required EE Channels:
   - `scheduled_posts` with fields: post_body, post_image, platform(s), publish_datetime, status
   - `social_accounts` with fields: account_name, platform, access_token
4. Add EE Member roles:
   - `User` – draft posts
   - `Reviewer` – approve/reject
   - `Admin` – manage all
5. Set up a CRON job or CLI worker to trigger `publish_scheduled_posts.php`.

---

## API Setup

- Create apps with:
  - [Meta Graph API](https://developers.facebook.com/)
  - [Twitter/X API](https://developer.twitter.com/)
  - [LinkedIn Marketing API](https://learn.microsoft.com/linkedin/)
- Save API keys in EE `config.php` or secure site settings.
- OAuth tokens are stored per user/account and refreshed as needed.

---

## Roles & Permissions

| Role     | Capabilities                          |
|----------|---------------------------------------|
| User     | Create and edit post drafts           |
| Reviewer | Approve or reject drafts              |
| Admin    | Full access to posts and integrations |

---

## Usage

- Navigate to `/scheduler` to access the dashboard.
- Compose a post, choose your platform(s), and schedule date/time.
- If you’re a Reviewer or Admin, approve the post to queue it.
- The background script handles publishing and error logging.

---

