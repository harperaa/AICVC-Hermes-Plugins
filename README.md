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
| [Long Form](https://github.com/harperaa/hermes-plugin-long-form) | Competitor channel tracking, transcripts, trends, an insight base, long-form scripts |
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
cron jobs wired, HTTPS URL, data on a persistent volume. **Two templates
to choose from:**

| Template | What you get | Extra requirement |
|---|---|---|
| **Standard** | The full platform (all plugins, scheduled jobs) | — |
| **Memory Edition** | Everything in Standard **plus a persistent AI memory system** ([Honcho](https://honcho.dev)): your agent remembers every conversation and meeting, builds profiles of the people you work with, and you can browse it all with the OpenConcho desktop app | An OpenAI API key at deploy time (powers the memory reasoning + embeddings) |

**You need:** a [Railway](https://railway.app) account on a paid plan (the
container is not free-tier sized), and an xAI API key from
https://console.x.ai (you add it *after* boot, in the dashboard).

### Deploy — Standard template

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

### Deploy — Memory Edition

1. **Open the deploy link:**

   **https://railway.com/deploy/8eU3xZ?referralCode=7XgUoY**

Same flow as Standard, with two differences:

1. The configuration panel asks for your **OpenAI API key** (required —
   get one at https://platform.openai.com/api-keys). This powers the
   memory system's reasoning and embeddings; your agent itself still runs
   on the xAI key you add after boot.
2. Your project deploys **four services** instead of one: the dashboard
   (`hermes`) plus the memory stack (`honcho-db`, `honcho-api`,
   `honcho-deriver`). First boot wires them together automatically —
   conversations and meetings start flowing into memory with zero setup.

**Browse your agent's memory with the OpenConcho desktop app:**

1. Download it from
   https://github.com/offendingcommit/openconcho/releases
   (Mac Apple Silicon: the `aarch64.dmg`; Intel Mac: `x64.dmg`;
   Windows: the `-setup.exe`).
2. **Your memory server URL:** in Railway, open your project and click
   the **honcho-api** service — the `https://…up.railway.app` domain
   shown at the top of the card is the URL.
3. **Your access token:** log into your dashboard and open the **Keys**
   page — the token is there as `HONCHO_ADMIN_JWT`. (For security the
   token is never printed in logs.) The deploy logs do carry a
   `MEMORY (Honcho)` banner with your URL and a **Memory check** line —
   if that says anything but WORKING, it tells you exactly what to fix.
4. In OpenConcho: add a connection, paste the URL and token, hit
   **Test connection**, then **Save**. You'll see your workspace, the
   people from your meetings as peers, and everything your agent has
   concluded so far.

**If the deploy sticks on "Applying changes"** for more than a couple of
minutes with no service activity: refresh the page first; if the services
still show no deployments, delete the project and hit the deploy link
again (a rare Railway provisioning hiccup — your configuration is not the
problem).

### Upgrading a Standard deployment to Memory Edition

Already running the Standard template? You don't migrate anything — the
memory stack installs **into your existing project** and your data stays
exactly where it is:

1. Open the **Memory Upgrade** deploy link (the three memory services
   only — no second dashboard):

   **https://railway.com/deploy/pNtlwv?referralCode=7XgUoY**

2. On the deploy page, click the **"Deploy to: New Project"** dropdown
   and select **your existing project** instead.
3. Paste your **OpenAI API key** (required) → Save Config → **Deploy**.
   The `honcho-db`, `honcho-api`, and `honcho-deriver` services appear
   next to your existing dashboard service.
4. Add three variables to your existing **hermes** service (Variables →
   New Variable, exact values including the `${{...}}` syntax):
   - `HONCHO_JWT_SECRET` = `${{honcho-api.AUTH_JWT_SECRET}}`
   - `HONCHO_BASE_URL` = `http://${{honcho-api.RAILWAY_PRIVATE_DOMAIN}}:8000`
   - `HONCHO_PUBLIC_URL` = `${{honcho-api.RAILWAY_PUBLIC_DOMAIN}}`
5. **Redeploy** the hermes service. First boot self-wires the memory
   system — and then something great happens: your **entire existing
   history** ships into memory automatically — every past conversation,
   meeting, brief, doc, and insight. Nothing to export, nothing to run.

   **What to expect:** the first sync run (within the hour, daytime)
   drains your whole history in one go — watch the peers and sessions
   fill in inside OpenConcho. Only a truly huge history spills into a
   second hourly run. After the raw history lands, the memory system
   keeps digesting it into conclusions in the background, so answers
   about your history get noticeably richer over the first day. New
   conversations are captured live from the moment you redeploy.
6. OpenConcho access works exactly as described above (URL from the
   honcho-api service card, token on your Keys page).

### After deploy (both templates)

**Forgot your password?** There's no email reset. In Railway: your project
→ the hermes service → **Variables** → set
`HERMES_DASHBOARD_BASIC_AUTH_PASSWORD` to a new value meeting the rules
above → **Redeploy**. The container adopts it as your new password.

**Updating:** Railway → the service → **Deployments** → **Redeploy**
pulls the current image. On the Memory Edition, redeploying `honcho-api`
and `honcho-deriver` the same way upgrades the memory system (migrations
run automatically at boot). Your data lives on persistent volumes and
survives updates.

---

## Option 2 — Local Docker (free, your machine)

The same image, running locally. Your data lives in a Docker volume and
survives restarts and upgrades. You need
[Docker Desktop](https://docs.docker.com/get-docker/) (Mac/Windows) or
Docker Engine (Linux).

### Mac (Apple Silicon or Intel) and Linux

```bash
docker run -d --name hermes-plugins \
  -p 127.0.0.1:9119:9119 \
  -v hermes_data:/opt/data ghcr.io/harperaa/hermes-plugins:stable
```

The image is multi-arch (amd64 + arm64), so Apple Silicon Macs
(M1/M2/M3/M4) run it natively. Only if you see
`no matching manifest for linux/arm64` (an older, pre-multi-arch image),
add `--platform linux/amd64` right after `run` and re-pull later.

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
- https://github.com/harperaa/hermes-plugin-long-form
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
