# 🏗️ System Architecture

This document explains the architecture and communication flow of the Hermes AI Assistant.

The system combines **Telegram**, **Hermes Gateway**, **Hermes Agent**, and **Google Gemini** to create a remotely accessible AI assistant.

---

## Overview

The assistant is designed around four main components:

1. **Telegram** — User interface
2. **Hermes Gateway** — Communication bridge
3. **Hermes Agent** — AI agent runtime
4. **Google Gemini** — Large Language Model

The overall architecture looks like this:

```text
                    ┌──────────────────────┐
                    │       TELEGRAM       │
                    │        USER          │
                    └──────────┬───────────┘
                               │
                               │ Telegram Bot API
                               ▼
                    ┌──────────────────────┐
                    │    HERMES GATEWAY    │
                    │                      │
                    │  Message Transport   │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │     HERMES AGENT     │
                    │                      │
                    │  Agent Runtime       │
                    │  Tools               │
                    │  Skills              │
                    │  Memory              │
                    │  Context             │
                    └──────────┬───────────┘
                               │
                               │ Gemini API
                               ▼
                    ┌──────────────────────┐
                    │    GOOGLE GEMINI     │
                    │                      │
                    │   Large Language     │
                    │       Model          │
                    └──────────────────────┘
```

---

# 🔄 Message Flow

When a user sends a message through Telegram, the request travels through several components.

```text
Telegram User
     │
     │ 1. Send message
     ▼
Telegram Bot API
     │
     │ 2. Forward message
     ▼
Hermes Gateway
     │
     │ 3. Pass message to agent
     ▼
Hermes Agent
     │
     │ 4. Process instruction
     │
     │ 5. Request model response
     ▼
Google Gemini API
     │
     │ 6. Generate response
     ▼
Hermes Agent
     │
     │ 7. Return response
     ▼
Hermes Gateway
     │
     │ 8. Send response
     ▼
Telegram Bot API
     │
     ▼
Telegram User
```

---

# 🧩 Components

## 1. Telegram

Telegram acts as the **user-facing interface**.

Instead of building a dedicated web or mobile application, the assistant can be accessed through a Telegram bot.

The user can send natural-language instructions such as:

```text
What files are inside my current project?
```

or:

```text
Help me create a Python script.
```

Telegram communicates with the assistant through the Telegram Bot API.

### Responsibilities

* Receive user messages
* Send messages to the bot
* Display AI responses
* Provide remote access to the assistant

---

# 2. Hermes Gateway

The Hermes Gateway acts as the **communication layer** between Telegram and the Hermes Agent.

Its purpose is to handle communication between the external Telegram interface and the local agent.

```text
Telegram
    │
    ▼
Hermes Gateway
    │
    ▼
Hermes Agent
```

This separation allows the AI agent to run independently from the user interface.

### Responsibilities

* Receive Telegram messages
* Forward messages to Hermes Agent
* Return agent responses to Telegram
* Maintain the communication channel
* Handle gateway lifecycle

---

# 3. Hermes Agent

Hermes Agent is the core of the system.

Unlike a traditional chatbot that simply sends a prompt to an LLM and returns the response, an AI agent can work with tools and perform multi-step operations.

The agent is responsible for interpreting the user's request and determining what actions need to be performed.

```text
User Request
     │
     ▼
Hermes Agent
     │
     ├── Understand request
     │
     ├── Reason about task
     │
     ├── Use tools
     │
     ├── Execute actions
     │
     └── Generate response
```

Depending on the configured capabilities, the agent can interact with:

* Terminal
* Files
* Skills
* External services
* APIs
* Other available tools

---

# 4. Google Gemini

Google Gemini is used as the **Large Language Model (LLM)** powering the assistant.

The model is accessed through the Google AI Studio / Gemini API.

```text
Hermes Agent
     │
     │ Prompt / Context
     ▼
Google Gemini API
     │
     │ Model Response
     ▼
Hermes Agent
```

Gemini is responsible for language understanding, reasoning, and generating natural-language responses.

Hermes Agent remains responsible for the agent workflow and tool interaction.

---

# 🧠 Agent vs Traditional Chatbot

A traditional chatbot generally follows this pattern:

```text
User
 ↓
Prompt
 ↓
LLM
 ↓
Response
```

The Hermes AI Assistant follows a more agent-oriented architecture:

```text
User
 ↓
Agent
 ↓
Understand Task
 ↓
Reason
 ↓
Choose Tool
 ↓
Execute Tool
 ↓
Observe Result
 ↓
Reason Again
 ↓
Generate Response
 ↓
User
```

This allows the assistant to perform tasks rather than only generate text.

---

# 🔐 Authentication & Security

The system uses several credentials.

## Google Gemini API Key

Used to authenticate requests to Google Gemini.

```env
GOOGLE_API_KEY=your_google_ai_studio_api_key
```

## Telegram Bot Token

Used to authenticate communication with the Telegram Bot API.

```env
TELEGRAM_BOT_TOKEN=your_telegram_bot_token
```

## Telegram User Allowlist

The assistant can restrict access to specific Telegram user IDs.

```env
TELEGRAM_ALLOWED_USERS=your_telegram_user_id
```

This creates an additional access-control layer.

---

# 🔒 Credential Flow

Sensitive credentials are stored locally and are not committed to the repository.

```text
                    Local Environment
                           │
              ┌────────────┴────────────┐
              │                         │
       GOOGLE_API_KEY          TELEGRAM_BOT_TOKEN
              │                         │
              ▼                         ▼
        Gemini API               Telegram API
```

The credentials are stored in:

```text
.env
```

The `.env` file is excluded from Git using `.gitignore`.

Only the template is included in the repository:

```text
.env.example
```

---

# 🖥️ Runtime Environment

The assistant runs locally using **WSL2 and Ubuntu**.

```text
┌───────────────────────────────────────┐
│               Windows                 │
│                                       │
│  ┌─────────────────────────────────┐  │
│  │          WSL2 / Ubuntu          │  │
│  │                                 │  │
│  │       Hermes AI Assistant       │  │
│  │                                 │  │
│  │       ┌─────────────────┐       │  │
│  │       │ Hermes Gateway  │       │  │
│  │       └────────┬────────┘       │  │
│  │                │                │  │
│  │       ┌────────▼────────┐       │  │
│  │       │  Hermes Agent   │       │  │
│  │       └─────────────────┘       │  │
│  │                                 │  │
│  └─────────────────────────────────┘  │
│                                       │
└───────────────────────────────────────┘
```

Telegram and Google Gemini are external services accessed through the internet.

---

# 🌐 External Services

The project depends on two primary external services:

| Service           | Purpose            |
| ----------------- | ------------------ |
| Telegram Bot API  | User communication |
| Google Gemini API | Language model     |

The local environment contains:

| Component      | Purpose               |
| -------------- | --------------------- |
| Hermes Gateway | Message transport     |
| Hermes Agent   | Agent runtime         |
| WSL2 / Ubuntu  | Execution environment |

---

# 🔁 Complete System Flow

The complete interaction can be summarized as:

```text
┌──────────────┐
│    User      │
└──────┬───────┘
       │
       │ Message
       ▼
┌──────────────┐
│   Telegram   │
└──────┬───────┘
       │
       │ Bot API
       ▼
┌──────────────┐
│    Hermes    │
│   Gateway    │
└──────┬───────┘
       │
       │ Agent Request
       ▼
┌──────────────┐
│    Hermes    │
│    Agent     │
└──────┬───────┘
       │
       │ Prompt + Context
       ▼
┌──────────────┐
│    Gemini    │
│     API      │
└──────┬───────┘
       │
       │ Response
       ▼
┌──────────────┐
│    Hermes    │
│    Agent     │
└──────┬───────┘
       │
       │ Result
       ▼
┌──────────────┐
│    Hermes    │
│   Gateway    │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│   Telegram   │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│     User     │
└──────────────┘
```

---

# 🎯 Design Goals

The architecture was designed around several goals:

### Remote Access

Allow the assistant to be accessed remotely through Telegram.

### Local Execution

Keep the agent runtime under the user's own local environment.

### Modular Components

Separate the communication layer, agent runtime, and LLM provider.

### Extensibility

Allow additional tools, skills, and integrations to be added later.

### Security

Keep API credentials local and restrict Telegram access to authorized users.

---

# 🚀 Future Architecture

The architecture can be extended with additional components in the future:

```text
                         ┌──────────────┐
                         │   Telegram   │
                         └──────┬───────┘
                                │
                         ┌──────▼───────┐
                         │    Hermes    │
                         │   Gateway    │
                         └──────┬───────┘
                                │
                         ┌──────▼───────┐
                         │    Hermes    │
                         │    Agent     │
                         └──────┬───────┘
                                │
              ┌─────────────────┼─────────────────┐
              │                 │                 │
              ▼                 ▼                 ▼
        ┌───────────┐     ┌───────────┐     ┌───────────┐
        │  Gemini   │     │   Tools   │     │  Memory   │
        │    API    │     │ & Skills  │     │  Storage  │
        └───────────┘     └───────────┘     └───────────┘
```

Potential future integrations include:

* Additional LLM providers
* Custom agent skills
* Web search
* Voice interfaces
* Scheduled tasks
* Persistent memory
* External APIs
* Docker-based deployment

---

## 📚 Related Documentation

* [Installation](./installation.md)
* [Configuration](./configuration.md)
* [Telegram Setup](./telegram.md)
* [Troubleshooting](./troubleshooting.md)

---

## 👨‍💻 Author

**Sugeng Permana**

Frontend & Backend Developer exploring AI agents, cloud technologies, and modern software development.
