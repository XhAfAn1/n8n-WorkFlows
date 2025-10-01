#  Self-Hosted n8n on Render

This guide explains how to deploy **n8n** (workflow automation) on **Render**, activate it with authentication, keep it alive using cron-job.org, and import workflows.

##  Table of Contents
- [Deploy n8n on Render](#1-deploy-n8n-on-render)
- [Activate n8n](#2-activate-n8n)
- [Keep n8n Alive](#3-keep-n8n-alive-prevent-render-sleep)
- [Import Workflows](#4-import-workflows-into-n8n)
- [Run Workflows on Schedule](#5-run-workflows-on-schedule-optional)


## 1. Deploy n8n on Render

1. Go to [Render Dashboard](https://dashboard.render.com/)
2. Create a **New Web Service**
3. Choose the **Docker** environment or use the official `n8nio/n8n` image
4. Set the service name, region, and free/paid plan

### Environment Variables

Add the following under **Environment Variables** in Render:

```env
N8N_HOST=xhafans-n8n.onrender.com   # Replace with your Render URL
N8N_PROTOCOL=https
WEBHOOK_URL=https://xhafans-n8n.onrender.com/
N8N_PORT=10000
N8N_USER_MANAGEMENT_DISABLED=false  # Enables login screen
N8N_ENCRYPTION_KEY=longrandomstring # Used to encrypt credentials
```

>  **Important**: Change `longrandomstring` to any secure random string (keep it safe).

## 2. Activate n8n

1. Visit your deployed Render URL (e.g. `https://xhafans-n8n.onrender.com`)
2. You should see the "Create Owner Account" screen
3. Enter your email and password to create the first Owner user
4. active account using activation key
5. Log in — n8n is now active and secure

## 3. Keep n8n Alive (Prevent Render Sleep)

Free Render instances can sleep after inactivity, which causes `503 Service Unavailable` errors.

### Use cron-job.org to Keep It Awake

1. Create an account at [cron-job.org](https://cron-job.org/)
2. Add a new job with:
   - **URL**: `https://xhafans-n8n.onrender.com/healthz`
   - **Schedule**: Every 5 minutes (or shorter if needed)

This pings n8n regularly, so Render keeps the service alive.

>  **Tip**: If you want to trigger workflows externally, use the workflow's webhook URL instead.

## 4. Import Workflows into n8n

### Exporting Locally

If you run n8n locally via Docker:

```bash
n8n export:workflow --all --output=workflows.json
```

### Importing on Render

1. Open the Render n8n editor
2. Click **Import** → **From File** and upload your `workflows.json`
3. Re-link credentials (Telegram, APIs, etc.) since they are not included in exports

## 5. Run Workflows on Schedule (Optional)

Instead of relying on cron-job.org for triggers, use the Cron node inside n8n:

1. Add a **Cron node** in your workflow
2. Configure it to run at your desired interval
3. Connect it to the workflow chain
4. Deploy the workflow — n8n will handle scheduling internally


**You now have a fully working n8n automation platform hosted on Render **
