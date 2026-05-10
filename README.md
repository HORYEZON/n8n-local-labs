# FULL CLI COMMANDS (LOCAL N8N SETUP + BACKUP + UPDATE)

🚀 1. Create volume + run n8n

      docker volume create n8n_data

      docker run -it --name n8n ^
        -p 5678:5678 ^
        -v n8n_data:/home/node/.n8n ^
        docker.n8n.io/n8nio/n8n

      ONE LINE:
          docker run -it --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n

      👉 Open: http://localhost:5678

👤 2. Reset / create account (if locked)

      docker exec -it n8n n8n user-management:reset

      docker restart n8n

💾 3. Export workflows

      docker exec -it n8n n8n export:workflow --all --output=/home/node/.n8n/workflows

📥 4. Copy workflows to PC (GitHub-ready files)

      docker cp n8n:/home/node/.n8n/workflows ./workflows

📤 5. Import workflows back (optional restore)

      docker exec -it n8n n8n import:workflow --input=/home/node/.n8n/workflows

🔄 6. Update n8n version safely

      docker pull docker.n8n.io/n8nio/n8n

      docker stop n8n
      docker rm n8n

      docker run -it --name n8n ^
        -p 5678:5678 ^
        -v n8n_data:/home/node/.n8n ^
        docker.n8n.io/n8nio/n8n

📦 7. Backup volume (IMPORTANT)

Backup:

      docker run --rm -v n8n_data:/data -v %cd%:/backup alpine ^
        tar czf /backup/n8n_backup.tar.gz -C /data .

Restore:

      docker run --rm -v n8n_data:/data -v %cd%:/backup alpine ^
        sh -c "tar xzf /backup/n8n_backup.tar.gz -C /data"

## ⚡ QUICK CHEAT SHEET

      docker volume create n8n_data
      docker run -it --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n

      docker exec -it n8n n8n user-management:reset
      docker exec -it n8n n8n export:workflow --all --output=/home/node/.n8n/workflows
      docker cp n8n:/home/node/.n8n/workflows ./workflows

      docker pull docker.n8n.io/n8nio/n8n

##  ⚠️ REMINDER

🧠 Important reminder (because future-you will forget)
      • Your workflows live in n8n_data
      • Container = replaceable
      • Volume = your actual brain
      • GitHub = backup, not runtime

If you want next level setup, I can convert this into:
      • docker-compose.yml (clean DevOps version)
      • auto backup script
      • one-command start/stop system

## 🐋 DOCKER CORE CONCEPTS

      | Concept   | Simple Description                            | Analogy                               | What happens if you delete it                 |
      | --------- | --------------------------------------------- | ------------------------------------- | --------------------------------------------- |
      | Image     | The blueprint of the app (read-only template) | A recipe book 🍳                      | Nothing happens to your data                  |
      | Container | Running instance of the image                 | A cooking session using the recipe 👨‍🍳 | App stops, but data may still exist elsewhere |
      | Volume    | Persistent storage for data                   | A hard drive / notebook 💾            | ⚠️ Data is lost if deleted                   |

##  🧠 FINAL MENTAL STACK

      IMAGE      → blueprint
      CONTAINER  → running app
      VOLUME     → saved data
      NETWORK    → communication

### 💀 One-liner truth

      Image builds it, container runs it, volume remembers it, network connects it.

### ⚠️ Why people get confused

      Because in Docker:

      deleting container ❌ does NOT delete data (if volume exists)
      deleting volume ❌ wipes everything
      image can be redownloaded anytime ❌ no data loss

# SUMMARY COMMANDS

• RUN N8N DOCKER LOCALLY

      docker volume create n8n_data

      docker run -it --rm --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n

• CREATE NEW ACCOUNT

      docker exec -it n8n n8n user-management:reset

      docker restart n8n

• EXPORT WORKFLOW FILE

      docker exec -it n8n n8n export:workflow --all --output=/home/node/.n8n/workflows

• PULL NEWEST N8N IMAGE VERSION

      docker pull docker.n8n.io/n8nio/n8n

      docker cp n8n:/home/node/.n8n/workflows ./workflows
