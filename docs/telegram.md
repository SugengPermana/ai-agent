# 📱 Telegram Integration

This guide explains how to connect **Hermes AI Assistant** to a Telegram Bot.

After completing this setup, the assistant can be accessed remotely through Telegram.

The integration flow is:

```text
┌──────────────┐
│    User      │
│   Telegram   │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Telegram Bot │
└──────┬───────┘
       │
       │ Telegram Bot API
       ▼
┌──────────────┐
│    Hermes    │
│   Gateway    │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│    Hermes    │
│    Agent     │
└──────────────┘
```

---

# 📋 Requirements

Before starting, make sure:

* Hermes Agent is installed
* Google Gemini is configured
* Telegram is installed
* You have access to a Telegram account
* The Hermes Gateway is available

If Gemini has not been configured yet, follow:

👉 [Configuration Guide](./configuration.md)

---

# 1. Create a Telegram Bot

Telegram provides an official bot management account called **BotFather**.

Open Telegram and search for:

```text
@BotFather
```

Open the verified BotFather account.

Start the conversation with:

```text
/start
```

---

# 2. Create a New Bot

Send:

```text
/newbot
```

BotFather will ask for a bot name.

For example:

```text
Hermes AI Assistant
```

Then choose a username.

Telegram bot usernames must end with:

```text
bot
```

For example:

```text
sugeng_hermes_ai_bot
```

After the bot is created, BotFather will provide a token similar to:

```text
1234567890:AAxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

This is your **Telegram Bot Token**.

---

# 🔐 Important: Protect Your Bot Token

The bot token provides access to your Telegram bot.

Never publish it on GitHub.

Do not put the real token inside:

* README
* Screenshots
* Git commits
* Public configuration files
* Source code

Store it securely in your environment configuration.

---

# 3. Configure the Telegram Bot Token

Open your Hermes environment file:

```bash
nano ~/.hermes/.env
```

Add:

```env
TELEGRAM_BOT_TOKEN=your_telegram_bot_token
```

For example:

```env
TELEGRAM_BOT_TOKEN=1234567890:AAxxxxxxxxxxxxxxxx
```

Replace the example token with your actual token.

---

# 4. Find Your Telegram User ID

For security, the assistant should not necessarily accept commands from every Telegram user.

Your Telegram account has a unique numeric user ID.

You can use a Telegram user-ID bot to find it.

The result will look similar to:

```text
Your Telegram ID: 123456789
```

Copy the numeric ID.

---

# 5. Configure Authorized Users

Add your Telegram user ID to the Hermes environment:

```env
TELEGRAM_ALLOWED_USERS=123456789
```

For example:

```env
TELEGRAM_ALLOWED_USERS=123456789
```

This allows the configured Telegram account to interact with the assistant.

If multiple users are supported by your Hermes configuration, follow the format required by your installed version.

---

# 6. Configure Telegram Through Hermes

Start Hermes:

```bash
hermes
```

Open the Telegram integration/setup options provided by your installed Hermes version.

The exact menu may differ between Hermes releases.

If Hermes provides an onboarding or pairing flow, follow the prompts to connect the Telegram bot.

The general configuration is:

```text
Telegram Bot Token
        │
        ▼
Hermes Telegram Integration
        │
        ▼
Hermes Gateway
        │
        ▼
Hermes Agent
```

---

# 7. Start the Hermes Gateway

Start the gateway:

```bash
hermes gateway start
```

Check its status:

```bash
hermes gateway status
```

A successful setup should indicate that the gateway is running.

---

# 8. Verify the Gateway

If Hermes provides gateway diagnostics, run:

```bash
hermes doctor
```

You can also inspect the gateway logs if necessary.

Depending on the Hermes version, logs may be available through the Hermes CLI or inside the Hermes runtime directory.

---

# 9. Open Your Telegram Bot

Open the bot that you created earlier.

Press:

```text
START
```

or send:

```text
/start
```

Then send a simple message:

```text
Hello
```

If everything is configured correctly, the message should travel through:

```text
Telegram
    ↓
Telegram Bot API
    ↓
Hermes Gateway
    ↓
Hermes Agent
    ↓
Gemini
    ↓
Hermes Agent
    ↓
Telegram
```

---

# 10. Test the Assistant

Start with a simple request:

```text
Hello, introduce yourself.
```

Then test a reasoning request:

```text
Explain what an AI agent is.
```

You can also test whether the agent can interact with its environment:

```text
What is the current working directory?
```

The exact capabilities available depend on the tools and permissions configured for your Hermes installation.

---

# 🧪 Example Conversation

Example:

```text
User:
Hello Hermes

Hermes:
Hello! How can I help you?

User:
What is the current working directory?

Hermes:
The current working directory is:
/mnt/c/project/ai-agent
```

This demonstrates the difference between simply calling an LLM and using an agent capable of interacting with its runtime environment.

---

# 🔐 Telegram Security

Telegram access should be restricted whenever possible.

The basic security model is:

```text
Telegram User
      │
      ▼
Telegram Bot
      │
      ▼
Authorized User Check
      │
      ├── ❌ Unauthorized
      │
      └── ✅ Authorized
              │
              ▼
        Hermes Gateway
              │
              ▼
        Hermes Agent
```

The important credentials are:

```env
TELEGRAM_BOT_TOKEN=your_telegram_bot_token
TELEGRAM_ALLOWED_USERS=your_telegram_user_id
```

Never commit the real values to GitHub.

---

# ⚠️ Troubleshooting

## Bot Does Not Respond

Check whether the gateway is running:

```bash
hermes gateway status
```

If it is not running:

```bash
hermes gateway start
```

---

## Gateway Is Running but Telegram Does Not Respond

Check:

1. Telegram bot token
2. Telegram user ID
3. Gemini API configuration
4. Hermes gateway status
5. Hermes logs

Run:

```bash
hermes doctor
```

---

## Invalid Bot Token

If Telegram reports an authentication problem, verify the token provided by BotFather.

You can generate a new token through BotFather if the previous token was revoked or compromised.

---

## Unauthorized User

If the bot ignores or rejects your messages, verify that your Telegram user ID is correctly configured:

```env
TELEGRAM_ALLOWED_USERS=your_telegram_user_id
```

Make sure there are no accidental spaces or incorrect characters.

---

## Gemini Works but Telegram Does Not

If Hermes works correctly from the terminal but not through Telegram, the problem is likely within the Telegram integration or gateway rather than the Gemini provider.

Test Hermes directly first:

```bash
hermes
```

If Hermes responds normally, check:

```bash
hermes gateway status
```

Then verify the Telegram configuration.

---

# 🔄 Complete Integration

After the Telegram integration is complete, the architecture becomes:

```text
                        INTERNET
                           │
             ┌─────────────┴─────────────┐
             │                           │
             ▼                           ▼
      ┌─────────────┐             ┌─────────────┐
      │  Telegram   │             │   Gemini    │
      │  Bot API    │             │     API     │
      └──────┬──────┘             └──────▲──────┘
             │                           │
             │                           │
             ▼                           │
      ┌─────────────┐                    │
      │   Hermes    │                    │
      │   Gateway   │                    │
      └──────┬──────┘                    │
             │                           │
             ▼                           │
      ┌─────────────┐                    │
      │   Hermes    │────────────────────┘
      │    Agent    │
      └─────────────┘
             │
             ▼
      Local Environment
```

---

# ✅ Telegram Setup Checklist

Before moving on, verify:

* [ ] Telegram bot created
* [ ] Bot username configured
* [ ] Bot token obtained
* [ ] Bot token stored securely
* [ ] Telegram user ID obtained
* [ ] Authorized user configured
* [ ] Hermes Gateway started
* [ ] Gateway status checked
* [ ] Telegram bot responds
* [ ] Gemini generates responses
* [ ] Unauthorized users are restricted

---

# 🚀 Next Step

The Telegram integration is now complete.

You can continue with:

👉 [Troubleshooting Guide](./troubleshooting.md)

For a deeper understanding of the system:

👉 [Architecture Guide](./architecture.md)

---

## 🎉 Result

After completing this guide, you have a remotely accessible AI assistant:

```text
                    YOUR TELEGRAM
                           │
                           ▼
                    ┌───────────┐
                    │ Telegram  │
                    │    Bot    │
                    └─────┬─────┘
                          │
                          ▼
                  ┌───────────────┐
                  │ Hermes        │
                  │ Gateway       │
                  └───────┬───────┘
                          │
                          ▼
                  ┌───────────────┐
                  │ Hermes Agent  │
                  └───────┬───────┘
                          │
                          ▼
                  ┌───────────────┐
                  │ Google Gemini │
                  └───────────────┘
```

The assistant can now be controlled remotely through Telegram while the agent itself runs in your local WSL2 environment.
