# 🚨 GitHub Actions Telegram Failure Notification Setup

This guide explains how to integrate **Telegram notifications** with your GitHub Actions CI/CD workflow to alert your DevOps team in case of a pipeline failure.

---

## 📌 Step 1: Create a Telegram Bot

1. Open Telegram and search for `@BotFather`.
2. Type `/start` and then `/newbot`.
3. Provide a name and a unique username for your bot.
4. You’ll receive a **bot token** like: `7275426022:AAEPbI0Zq`

---

## 👥 Step 2: Get Your Telegram Chat ID

1. Open [Web Telegram](https://web.telegram.org).
2. Go to the group where the bot and DevOps team are added.
3. In the URL, find the **group ID** at the end of the link. 
  For example: `https://web.telegram.org/k/#-123456789`

4. Use this as the **Chat ID**. Keep the minus `-` sign: Chat ID: `-123456789`


> ✅ Make sure your bot is added to the group **and has permission to send messages**.

---

## 🔐 Step 3: Add Secrets to GitHub

Go to your GitHub repository → **Settings** → **Secrets and Variables** → **Actions** and add the following:

| Name                  | Value                       |
|-----------------------|-----------------------------|
| `TELEGRAM_BOT_TOKEN`  | `77275426022:AAEPbI0Zq`     |
| `TELEGRAM_CHAT_ID`    | `-123456789`                |

---

## ⚙️ Step 4: Update GitHub Actions Workflow

Inside `.github/workflows/cicd.yml`, add the following job:

```yaml
notify-telegram-failure:
runs-on: ubuntu-latest
needs: [build-and-push, update-deployment-file]
if: failure()
steps:
 - name: Send Telegram Notification on Failure
   env:
     REPO: ${{ github.repository }}
     BRANCH: ${{ github.ref_name }}
     COMMIT: ${{ github.sha }}
     ACTOR: ${{ github.actor }}
     BOT_TOKEN: ${{ secrets.TELEGRAM_BOT_TOKEN }}
     CHAT_ID: ${{ secrets.TELEGRAM_CHAT_ID }}
   run: |
     MESSAGE=$(cat <<EOF
🚨 *GitHub Action FAILED*
📦 Repo: ${REPO}
🔁 Branch: ${BRANCH}
🔧 Commit: [${COMMIT}](https://github.com/${REPO}/commit/${COMMIT})
👤 By: ${ACTOR}
EOF
)
     curl -s -X POST "https://api.telegram.org/bot${BOT_TOKEN}/sendMessage" \
       -d chat_id="${CHAT_ID}" \
       -d parse_mode=Markdown \
       --data-urlencode text="${MESSAGE}"
```

## ✅ Result

Now, whenever your pipeline fails during build-and-push or update-deployment-file jobs, your Telegram group will receive a notification like:

```less
🚨 GitHub Action FAILED
📦 Repo: your-org/your-repo
🔁 Branch: branch-name
🔧 Commit: [abcdef1](https://github.com/your-org/your-repo/commit/abcdef1)
👤 By: developer-username

```