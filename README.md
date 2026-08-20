# AICVC Hermes Plugins — Install Guide

> Built and taught by **Dr. Allen Harper, AI Cyber Value Creator** — join the
> community at [AI Cyber Value Creators on Skool](https://www.skool.com/ai-cyber-value-creators).

This is the front door to the AI Cyber Value Creator platform: a hosted
[Hermes Agent](https://github.com/NousResearch/hermes-agent) dashboard with a
suite of plugins that walk you through the whole value-creation process —
your builder level, the AICVC roadmap, competitive YouTube intelligence,
short-form content, offers, delivery, and daily executive intel.

**The plugins:**

| Plugin | What it does |
|---|---|
| [AI Cyber Value Creator](https://github.com/harperaa/hermes-plugin-ai-cyber-value-creator) | The AICVC roadmap: coached steps from ICP clarity to scale, run as agent tasks |
| [Your Level](https://github.com/harperaa/hermes-plugin-value-creator-level) | The Levels of AI Building ladder — placement exam, badges, level-up challenges |
| [Long Form (YouTube Insights)](https://github.com/harperaa/hermes-plugin-youtube-insights) | Competitor channel tracking, transcripts, trends, an insight base, long-form scripts |
| [Short Form (Shorts Lab)](https://github.com/harperaa/hermes-plugin-short-form) | Shorts + ads research, derivative scripts, image-ad cloning, site describer videos |
| [Value Dashboard](https://github.com/harperaa/hermes-plugin-value-dashboard) | Your metrics home — AI ROI computed from real provider usage |
| [Offer Doc](https://github.com/harperaa/hermes-plugin-offer-doc) | Coached offer design (promise, bonuses, guarantee, payments, timing) → signed 1-2 page doc |
| [Delivery Kit](https://github.com/harperaa/hermes-plugin-delivery-kit) | Client delivery workspace and artifacts |
| [Daily Brief](https://github.com/harperaa/hermes-plugin-daily-brief) | Scheduled executive intel brief, delivered every morning |

There are three ways to run it. **Railway (option 1) is the recommended
path** — one click, hosted, always on, scheduled jobs just run.

---

## Option 1 — Railway (recommended, ~10 minutes)

You get the full platform hosted on Railway: every plugin pre-installed,
cron jobs wired, HTTPS URL, data on a persistent volume.

**You need:** a [Railway](https://railway.app) account on a paid plan (the
container is not free-tier sized), and an xAI API key from
https://console.x.ai (you add it *after* boot, in the dashboard).

### Step by step

1. **Open the deploy link:**

   **https://railway.com/deploy/0wlZzT?referralCode=7XgUoY**

2. Click **Deploy Now** on the template page.
3. A configuration panel appears. **Leave the password variable empty**
   (recommended) — you'll create your login in the browser on first visit.
   Click **Deploy** again. (Yes — you click Deploy **twice**: once on the
   template page, once on the configuration panel.)
4. Railway builds the project. Wait for the deployment to go green —
   first boot takes a few minutes while it seeds the plugins and jobs.
   You'll see `[hermes-plugins-seed] seed complete` in the deploy logs.
5. **Open the service**: click the service card (the box named after the
   image) in your new project.
6. **Click the URL at the top** of the service panel — it looks like
   `https://<something>.up.railway.app`. That's your dashboard.
7. **Claim your instance** — the first visit shows **"Your New Login"**:
   enter your **email address** and choose a **password with 8+ characters
   including an uppercase letter, a lowercase letter, and a symbol**
   (e.g. `MyValue2026!`). This first sign-in *creates* your login and you
   own the instance from then on. Do this right after boot.
8. You'll land on the **Roadmap** — the Getting Started card walks you
   through adding your xAI key and everything else.

**Forgot your password?** There's no email reset. In Railway: your project
→ the service → **Variables** → set
`HERMES_DASHBOARD_BASIC_AUTH_PASSWORD` to a new value meeting the rules
above → **Redeploy**. The container adopts it as your new password.

**Updating:** Railway → your service → **Deployments** → **Redeploy**
pulls the current `stable` image. Your data lives on the `/opt/data`
volume and survives updates.

---

## Option 2 — Local Docker (free, your machine)

The same image, running locally. Your data lives in a Docker volume and
survives restarts and upgrades. You need
[Docker Desktop](https://docs.docker.com/get-docker/) (Mac/Windows) or
Docker Engine (Linux).

### Mac — Apple Silicon (M1/M2/M3/M4)

```bash
docker run -d --name hermes-plugins --platform linux/amd64 \
  -p 127.0.0.1:9119:9119 \
  -v hermes_data:/opt/data ghcr.io/harperaa/hermes-plugins:stable
```

(The image is linux/amd64; `--platform linux/amd64` runs it under Apple's
Rosetta emulation — slower, but fully functional.)

### Mac — Intel, and Linux

```bash
docker run -d --name hermes-plugins \
  -p 127.0.0.1:9119:9119 \
  -v hermes_data:/opt/data ghcr.io/harperaa/hermes-plugins:stable
```

### Windows (PowerShell, with Docker Desktop running)

```powershell
docker run -d --name hermes-plugins -p 127.0.0.1:9119:9119 -v hermes_data:/opt/data ghcr.io/harperaa/hermes-plugins:stable
```

### Then, on every platform

1. Open **http://localhost:9119** — the first visit shows **"Your New
   Login"**: enter your email and choose a password (8+ characters with an
   uppercase letter, a lowercase letter, and a symbol). That claims your
   instance — do it right after boot.
2. Follow the Getting Started card to add your **xAI API key**
   (https://console.x.ai).

**Update later:**

```bash
docker pull ghcr.io/harperaa/hermes-plugins:stable
docker rm -f hermes-plugins
# then re-run the same docker run command for your platform above
```

Your login, keys, and data persist in the `hermes_data` volume.

---

## Option 3 — Manual plugin installs (bring your own Hermes)

Already running your own [Hermes Agent](https://github.com/NousResearch/hermes-agent)?
Each plugin installs individually:

```bash
hermes plugins install harperaa/<repo-name>
```

Install what you want — each repo's README has its specific setup, keys,
and usage:

- https://github.com/harperaa/hermes-plugin-ai-cyber-value-creator
- https://github.com/harperaa/hermes-plugin-value-creator-level
- https://github.com/harperaa/hermes-plugin-youtube-insights
- https://github.com/harperaa/hermes-plugin-short-form
- https://github.com/harperaa/hermes-plugin-value-dashboard
- https://github.com/harperaa/hermes-plugin-offer-doc
- https://github.com/harperaa/hermes-plugin-delivery-kit
- https://github.com/harperaa/hermes-plugin-daily-brief

The Railway/Docker image ships all of them pre-wired (plus scheduled
jobs and the mentee onboarding flow) — manual install is for people who
want to pick pieces à la carte on their own agent.

---

## Questions

Ask in the [AI Cyber Value Creators Skool community](https://www.skool.com/ai-cyber-value-creators)
— that's where the workshops, the roadmap walkthroughs, and Dr. Harper's
mentoring live.
