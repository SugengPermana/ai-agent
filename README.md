# 🤖 Hermes AI Assistant — Gemini + Telegram

A personal AI assistant built with **Hermes Agent**, **Google Gemini via AI Studio**, and **Telegram Bot**.

This guide explains how to build your own AI assistant from scratch, configure Gemini as the LLM provider, and control the assistant remotely through Telegram.

## Architecture

```text
┌─────────────────────┐
│      Telegram       │
│        User         │
└──────────┬──────────┘
           │
           │ Telegram Bot API
           ▼
┌─────────────────────┐
│   Hermes Gateway    │
│                     │
│   Hermes AI Agent   │
└──────────┬──────────┘
           │
           │ Gemini API
           ▼
┌─────────────────────┐
│   Google AI Studio  │
│       Gemini        │
└─────────────────────┘
```

The workflow is:

**Telegram → Hermes Gateway → Hermes Agent → Gemini API → Hermes Agent → Telegram**

---

# 📋 Requirements

Before starting, make sure you have:

* Windows 10/11
* WSL2
* Ubuntu
* Git
* Internet connection
* Google account
* Telegram account

Hermes Agent currently supports Linux and WSL2. Native Windows is not supported, so Windows users should run Hermes inside WSL2.

---

# 1. Install WSL2

If you already have WSL2 and Ubuntu installed, you can skip this step.

Open **PowerShell as Administrator**:

```powershell
wsl --install
```

Restart your computer after installation.

Then open Ubuntu:

```bash
wsl
```

Verify:

```bash
uname -a
```

You should now be inside your Ubuntu environment.

---

# 2. Install Git

Update your packages:

```bash
sudo apt update
```

Install Git:

```bash
sudo apt install git -y
```

Verify:

```bash
git --version
```

---

# 3. Install Hermes Agent

Install Hermes using the official installer:

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

Reload your shell:

```bash
source ~/.bashrc
```

Verify the installation:

```bash
hermes --version
```

You should see the Hermes Agent version.

Official Hermes documentation recommends this installation method for Linux/WSL2.

---

# 4. Initialize Hermes

Run:

```bash
hermes setup
```

Hermes will guide you through its initial configuration.

After setup, Hermes stores its configuration under:

```text
~/.hermes/
```

Important files include:

```text
~/.hermes/
├── config.yaml
├── .env
├── logs/
├── memory/
├── skills/
└── personas/
```

The `.env` file is used for sensitive credentials, while `config.yaml` contains the main Hermes configuration.

---

# 5. Create a Google AI Studio API Key

Open Google AI Studio:

https://aistudio.google.com/apikey

Create an API key for your Google project.

Keep this key private.

> ⚠️ Never upload your Gemini API key to GitHub.

Google's Gemini documentation recommends using API keys for authentication and provides key management through Google AI Studio.

---

# 6. Configure Gemini

Hermes supports Google Gemini as a native provider.

Open the Hermes environment file:

```bash
nano ~/.hermes/.env
```

Add:

```env
GOOGLE_API_KEY=YOUR_GOOGLE_AI_STUDIO_API_KEY
```

For example:

```env
GOOGLE_API_KEY=AIzaSyxxxxxxxxxxxxxxxxxxxxxxxx
```

Save the file.

You can also use:

```env
GEMINI_API_KEY=YOUR_GOOGLE_AI_STUDIO_API_KEY
```

Hermes checks both `GOOGLE_API_KEY` and `GEMINI_API_KEY` for the Gemini provider.

---

# 7. Select Gemini as the Model Provider

Run:

```bash
hermes model
```

Choose:

```text
Google AI Studio
```

Hermes should detect the API key and display the available Gemini models.

Select the model you want to use.

For example:

```text
gemini-3-flash-preview
```

The exact model list may change as Google releases or retires models.

---

# 8. Verify Gemini Configuration

Run:

```bash
hermes doctor
```

This checks whether Hermes can find the configured provider credentials.

You can also start Hermes directly:

```bash
hermes
```

Then test it:

```text
Hello, introduce yourself.
```

If Hermes responds, your Gemini integration is working.

---

# 9. Check the Hermes Configuration

Your configuration should contain something similar to:

```yaml
model:
  default: gemini-3-flash-preview
  provider: gemini
  base_url: https://generativelanguage.googleapis.com/v1beta
```

The exact model name may be different depending on which Gemini model you selected.

The important parts are:

```yaml
provider: gemini
```

and:

```yaml
base_url: https://generativelanguage.googleapis.com/v1beta
```

Hermes uses this native Gemini API endpoint for the Google provider.

---

# 10. Create a Telegram Bot

Now we connect Hermes to Telegram.

Open Telegram and search for:

```text
@BotFather
```

Start a conversation with BotFather.

Run:

```text
/newbot
```

BotFather will ask for:

### Bot name

Example:

```text
Sugeng AI Assistant
```

### Bot username

The username must end with:

```text
bot
```

Example:

```text
sugeng_ai_assistant_bot
```

BotFather will then provide a bot token similar to:

```text
123456789:AAxxxxxxxxxxxxxxxxxxxxxxxx
```

Save this token.

> ⚠️ The Telegram bot token is a secret. Never commit it to GitHub.

---

# 11. Get Your Telegram User ID

Hermes can restrict the bot so only specific Telegram users can interact with it.

One way to find your numeric Telegram user ID is to message:

```text
@userinfobot
```

It will return your numeric user ID.

Example:

```text
123456789
```

Save this ID.

Your Telegram username is **not** the same thing as your numeric user ID.

---

# 12. Configure Telegram in Hermes

Open:

```bash
nano ~/.hermes/.env
```

Add:

```env
TELEGRAM_BOT_TOKEN=YOUR_TELEGRAM_BOT_TOKEN
TELEGRAM_ALLOWED_USERS=YOUR_TELEGRAM_USER_ID
```

Example:

```env
GOOGLE_API_KEY=AIzaSyxxxxxxxxxxxxxxxx

TELEGRAM_BOT_TOKEN=123456789:AAxxxxxxxxxxxxxxxx

TELEGRAM_ALLOWED_USERS=123456789
```

If you want to allow multiple Telegram users:

```env
TELEGRAM_ALLOWED_USERS=123456789,987654321
```

The allowlist is important because it prevents arbitrary Telegram users from accessing your assistant.

---

# 13. Configure the Telegram Gateway

Run:

```bash
hermes gateway setup
```

Select:

```text
Telegram
```

Follow the setup wizard.

Depending on your Hermes version, it may ask for:

```text
Bot Token
Allowed Telegram Users
```

Enter the credentials you created earlier.

---

# 14. Start the Telegram Gateway

Start Hermes Gateway:

```bash
hermes gateway start
```

Check the status:

```bash
hermes gateway status
```

You should see that the gateway is running.

---

# 15. Test Your AI Assistant

Open Telegram.

Find your bot:

```text
@sugeng_ai_assistant_bot
```

Press:

```text
Start
```

Then send:

```text
Hello
```

The message flow should now be:

```text
Telegram
   ↓
Telegram Bot API
   ↓
Hermes Gateway
   ↓
Hermes Agent
   ↓
Google Gemini API
   ↓
Gemini Response
   ↓
Hermes Gateway
   ↓
Telegram
```

Your AI assistant is now accessible through Telegram.

---

# 16. Test the Agent's Capabilities

Because Hermes is an AI agent rather than just a basic chatbot, you can test tasks such as:

```text
Explain what files are inside my current project.
```

or:

```text
Check the current directory and summarize the project structure.
```

or:

```text
Create a simple Python script that checks the system information.
```

Depending on the enabled Hermes tools and permissions, the agent can interact with tools and perform multi-step tasks.

---

# 17. Useful Hermes Commands

### Start Hermes

```bash
hermes
```

### Select a model

```bash
hermes model
```

### Run diagnostics

```bash
hermes doctor
```

### Configure the gateway

```bash
hermes gateway setup
```

### Start the gateway

```bash
hermes gateway start
```

### Check gateway status

```bash
hermes gateway status
```

### Stop the gateway

```bash
hermes gateway stop
```

---

# 18. Run Hermes in the Background

If you want the Telegram bot to continue running without keeping the interactive terminal open:

```bash
hermes gateway start --daemon
```

Then check:

```bash
hermes gateway status
```

This allows the gateway to continue running in the background.

---

# 19. Security

There are several credentials in this project that must remain private.

Never commit:

```text
~/.hermes/.env
```

to GitHub.

Your `.env` contains secrets such as:

```env
GOOGLE_API_KEY=...
TELEGRAM_BOT_TOKEN=...
```

Add the following to `.gitignore` if you create a project repository around your configuration:

```gitignore
.env
.hermes/
*.key
*.pem
```

If you accidentally expose an API key or Telegram bot token, revoke and regenerate it immediately.

---

# 20. Recommended Project Structure

A clean documentation repository can look like this:

```text
hermes-ai-assistant/
│
├── README.md
├── .gitignore
│
├── docs/
│   ├── architecture.md
│   ├── telegram.md
│   └── gemini.md
│
└── screenshots/
    ├── telegram-chat.png
    ├── hermes-terminal.png
    ├── gemini-config.png
    └── gateway.png
```

Never put the actual `.env` file inside the repository.

---

# 21. Troubleshooting

## Hermes command not found

Try:

```bash
source ~/.bashrc
```

Then:

```bash
hermes --version
```

If it still doesn't work, check:

```bash
echo $PATH
```

---

## Gemini API key error

Run:

```bash
hermes doctor
```

Then check:

```bash
cat ~/.hermes/.env
```

Make sure the variable exists:

```env
GOOGLE_API_KEY=YOUR_KEY
```

Do not share the actual key publicly.

---

## Telegram bot doesn't respond

Check the gateway:

```bash
hermes gateway status
```

Then run:

```bash
hermes doctor
```

Verify:

```env
TELEGRAM_BOT_TOKEN=...
TELEGRAM_ALLOWED_USERS=...
```

Make sure your Telegram numeric user ID is included in the allowlist.

---

## Telegram says the bot doesn't exist

Make sure you are opening the exact username generated by BotFather.

For example:

```text
@sugeng_ai_assistant_bot
```

---

## Gemini model is unavailable

Don't hardcode an old model ID.

Run:

```bash
hermes model
```

and select one of the currently available Gemini models.

Google's model availability can change over time.

---

# 22. Final Result

After completing the setup, you will have your own self-hosted AI assistant:

```text
                    ┌─────────────────┐
                    │    TELEGRAM     │
                    │                 │
                    │  "Hello Hermes" │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ Telegram Bot API │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ Hermes Gateway  │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │   Hermes Agent  │
                    │                 │
                    │  Tools / Skills │
                    │  Memory / Agent │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │  Google Gemini  │
                    │   AI Studio     │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │  AI Response    │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │    TELEGRAM     │
                    └─────────────────┘
```

## 🎯 What You Built

This project combines:

* **Hermes Agent** — AI agent runtime
* **Google AI Studio / Gemini** — LLM provider
* **Telegram Bot** — remote conversational interface
* **Hermes Gateway** — communication layer between Telegram and the agent
* **Hermes Tools & Skills** — agent capabilities
* **Memory** — persistent agent context

The result is a personal AI assistant that can be accessed remotely through Telegram while the actual agent runs on your own Linux/WSL2 environment.

---

## 📚 References

* Hermes Agent: https://github.com/NousResearch/hermes-agent
* Google AI Studio: https://aistudio.google.com/
* Gemini API Documentation: https://ai.google.dev/gemini-api/docs
* Telegram Bot API: https://core.telegram.org/bots/api
