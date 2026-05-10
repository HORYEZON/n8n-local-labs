# 💫 N8N DEPLYMENT TO PRODUCTION

This is where n8n stops being “local experiment” and becomes actual infrastructure

Because locally, you’re doing:

      Your PC → Docker → localhost:5678

But in production?

  You need:

          • a server running 24/7
          • public domain
          • persistent storage
          • reverse proxy / HTTPS
          • backups
          • restart policies

  Basically:

        “turn my laptop workflow into a real online service humans can trust.”

## 🧠 Typical Production Architecture

      Internet
        ↓
      Domain (example.com)
        ↓
      Reverse Proxy (Nginx / Traefik / Caddy)
        ↓
      Docker Container (n8n)
        ↓
      Database / Volume

## 🚀 Common ways people host n8n in prod

      | Method             | Difficulty | Recommended?              |
      | ------------------ | ---------- | ------------------------- |
      | VPS + Docker       | Medium     | ✅ BEST                    |
      | Docker Compose     | Medium     | ✅ BEST                    |
      | Railway / Render   | Easy       | ⚠️ okay for hobby         |
      | Kubernetes         | Hard       | ☠️ overkill for beginners |
      | Bare metal install | Medium     | meh                       |
      | n8n Cloud          | Easy       | 💰 paid                   |

## ⚡ Most common real-world setup

🖥️ VPS (Ubuntu server)

    Examples:
          DigitalOcean
          Hetzner
          Linode
          Vultr

    Then:
        Ubuntu Server
          └── Docker
              └── n8n container

## 🧱 Production usually uses Docker Compose

Instead of giant commands:

      services:
        n8n:
          image: docker.n8n.io/n8nio/n8n
          ports:
            - "5678:5678"
          volumes:
            - n8n_data:/home/node/.n8n
          restart: always

  Why?
        • cleaner
        • restart automatically
        • easier updates
        • easier backups

### 🌐 How public access works

Locally:

      localhost:5678

Production:

      https://automation.yourdomain.com

  Usually via:

      → Nginx
      → Traefik
      → Caddy

  These handle:

      → HTTPS
      → SSL certs
      → routing
      → security

### 💾 Where workflows are stored in prod

Exactly same concept as local:

    Docker Volume / Database

Could be:
    • SQLite (small setups)
    • PostgreSQL (recommended prod)

### 🧠 Real production stack example | This is the “serious setup.”

      Ubuntu VPS
      ├── Docker
      ├── Docker Compose
      ├── n8n
      ├── PostgreSQL
      ├── Nginx
      ├── SSL (Let's Encrypt)
      └── Backups

# ⚠️ Important prod concerns

    🔒 Security

      • Never expose raw port 5678 publicly.
      • Use reverse proxy + HTTPS.

    💾 Backups

      • Your volume/database matters more than container.

    🔄 Auto restart | because servers crash. Humans also crash, but Docker can auto-recover at least.

      • Production containers usually:

            restart: always

### ⚡ Simple mental model

      • GitHub = storage/versioning
      • Docker = runtime
      • VPS = actual computer on the internet
      • n8n = automation engine

---

# 🚀 BEST FREE OPTIONS FOR N8N / BACKEND HOSTING

    | Platform                                                    | Free?      | Supports Docker? | Sleep?          | Good for n8n?   |
    | ----------------------------------------------------------- | ---------- | ---------------- | --------------- | --------------- |
    | [Railway](https://railway.app?utm_source=chatgpt.com)       | ✅         | ✅              | sometimes       | BEST beginner   |
    | [Render](https://render.com?utm_source=chatgpt.com)         | ✅         | ✅              | YES             | decent          |
    | [Koyeb](https://www.koyeb.com?utm_source=chatgpt.com)       | ✅         | ✅              | less aggressive | good            |
    | [Fly.io](https://fly.io?utm_source=chatgpt.com)             | ⚠️ limited | ✅              | no sleep        | harder          |
    | [Northflank](https://northflank.com?utm_source=chatgpt.com) | ✅         | ✅              | depends         | underrated      |
    | [Deta Space](https://deta.space?utm_source=chatgpt.com)     | ✅         | ⚠️ limited      | yes-ish         | not ideal       |
    | [Glitch](https://glitch.com?utm_source=chatgpt.com)         | ✅         | ❌              | sleeps hard     | meme-tier       |


## 🧠 My recommendation for you

  🥇 Railway | Very beginner-friendly.

      Closest to:

          “backend Vercel experience”

      You can:
        • connect GitHub
        • deploy Docker container
        • add PostgreSQL
        • use env vars
        • custom domain
        • easy logs

  🥈 Render | Not ideal for serious automation.

      Good alternative.

      But:
          free tier sleeps

      Meaning:
        • workflows may delay
        • webhooks may wake slowly
        • scheduled triggers become unreliable

  🥉 Koyeb | Feels cleaner than Render sometimes.

      Underrated.

      Actually decent for:
        • Docker
        • APIs
        • lightweight backend services

## ⚠️ But here’s the catch

    n8n is:
      • stateful
      • persistent
      • long-running

    Free platforms HATE this.
    Why?
      Because free tier companies fear:
          “someone mining crypto using free containers.”

    Reasonable fear, honestly.

    So free plans often:
      • sleep after inactivity
      • limit execution time
      • restrict storage
      • kill background jobs

    Which is bad for automation systems.

## 💀 The uncomfortable truth

    For real production reliability:

        VPS still wins.

    Because:
      • always online
      • no sleeping
      • full control
      • cheaper long-term

    Even a $4-$6 VPS destroys most free-tier backend platforms.










