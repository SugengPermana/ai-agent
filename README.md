# 🤖 Hermes AI Assistant

> A personal AI assistant powered by Google Gemini, built with Hermes Agent, and remotely controlled through Telegram.

Hermes AI Assistant is a personal AI agent that combines **Hermes Agent** with **Google Gemini via AI Studio** and **Telegram Bot**.

The project was built to explore how modern AI agents can interact with LLMs, use tools, maintain context, and be accessed remotely through a conversational interface.

---

## ✨ Features

* 🤖 AI assistant powered by Google Gemini
* 🧠 AI agent capabilities through Hermes Agent
* 💬 Telegram-based conversational interface
* 🔌 Telegram Bot integration
* 🛠️ Agent tool and skill integration
* 💾 Agent memory and context
* 🖥️ Runs locally through WSL2 / Linux
* 🔐 User access control through Telegram user IDs
* ⚙️ Configurable Gemini model provider
* 🚀 Gateway-based communication between Telegram and the agent

---

## 🏗️ Architecture

```text
┌──────────────────────┐
│       Telegram       │
│         User         │
└──────────┬───────────┘
           │
           │ Telegram Bot API
           ▼
┌──────────────────────┐
│    Hermes Gateway    │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│     Hermes Agent     │
│                      │
│  Tools / Skills      │
│  Memory / Context    │
│  Agent Runtime       │
└──────────┬───────────┘
           │
           │ Gemini API
           ▼
┌──────────────────────┐
│   Google AI Studio   │
│       Gemini         │
└──────────────────────┘
```

### Message Flow

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
Hermes Agent
   ↓
Telegram
```

The Telegram bot acts as the remote interface, while Hermes Agent handles the agent runtime and communicates with Gemini as the underlying language model.

---

## 🧰 Tech Stack

| Technology           | Purpose                         |
| -------------------- | ------------------------------- |
| **Hermes Agent**     | AI agent runtime                |
| **Google Gemini**    | Large Language Model            |
| **Google AI Studio** | Gemini API provider             |
| **Telegram Bot API** | Remote conversational interface |
| **Python**           | Agent environment               |
| **WSL2 / Ubuntu**    | Local runtime environment       |
| **Git**              | Version control                 |

---

## 📸 Screenshots

### Telegram Interface

![Telegram Interface](./screenshots/telegram.png)

The assistant can be accessed remotely through a Telegram bot.

### Hermes Agent

![Hermes Agent](./screenshots/hermes-terminal.png)

Hermes Agent running locally through WSL2.

### Gateway

![Hermes Gateway](./screenshots/gateway.png)

Hermes Gateway connecting the Telegram interface with the AI agent.

### Architecture

![Architecture](./screenshots/architecture.png)

---

## 🚀 Getting Started

### Requirements

Before installing the project, make sure you have:

* Windows 10/11
* WSL2
* Ubuntu
* Git
* A Google account
* Google AI Studio API key
* Telegram account

> Hermes Agent runs inside Linux/WSL2 rather than directly on native Windows.

---

## 📦 Installation

Clone the repository:

```bash
git clone https://github.com/SugengPermana/hermes-ai-assistant.git

cd hermes-ai-assistant
```

Follow the complete installation guide:

👉 [Installation Guide](./docs/installation.md)

---

## 🔑 Configuration

The assistant requires credentials for:

* Google Gemini
* Telegram Bot

Create your environment file:

```bash
cp .env.example .env
```

Then configure:

```env
GOOGLE_API_KEY=your_google_ai_studio_api_key

TELEGRAM_BOT_TOKEN=your_telegram_bot_token

TELEGRAM_ALLOWED_USERS=your_telegram_user_id
```

For detailed configuration:

👉 [Configuration Guide](./docs/configuration.md)

---

## 📱 Telegram Setup

Create a Telegram bot using **BotFather**, configure the bot token, and allow your Telegram user ID to access the assistant.

Detailed instructions:

👉 [Telegram Setup Guide](./docs/telegram.md)

---

## ▶️ Running the Assistant

Start Hermes:

```bash
hermes
```

To start the Telegram gateway:

```bash
hermes gateway start
```

Check the gateway status:

```bash
hermes gateway status
```

Once the gateway is running, open your Telegram bot and send a message.

Example:

```text
You:
Hello, what can you help me with?

AI Assistant:
Hello! I can help you with...
```

---

## 🧠 Why I Built This

This project started as an experiment to understand how **AI agents differ from traditional chatbots**.

Instead of only sending messages to an LLM and receiving responses, I wanted to explore how an agent can:

* Understand user instructions
* Interact with external tools
* Execute multi-step tasks
* Maintain context
* Use custom skills
* Connect to external interfaces
* Operate as a personal assistant

Telegram was chosen as the interface because it allows the assistant to be accessed remotely without building a dedicated frontend.

---

## 🔐 Security

This project uses sensitive credentials such as:

```text
GOOGLE_API_KEY
TELEGRAM_BOT_TOKEN
```

These credentials should **never be committed to GitHub**.

The actual `.env` file is excluded through `.gitignore`.

Only the example configuration should be committed:

```text
.env.example
```

Telegram access is also restricted using:

```env
TELEGRAM_ALLOWED_USERS=your_telegram_user_id
```

This prevents unauthorized Telegram users from interacting with the assistant.

---

## 📚 Documentation

Detailed documentation is available in the `docs` directory.

| Documentation                                | Description                              |
| -------------------------------------------- | ---------------------------------------- |
| [Architecture](./docs/architecture.md)       | How the system components communicate    |
| [Installation](./docs/installation.md)       | Complete installation guide              |
| [Configuration](./docs/configuration.md)     | Gemini and Hermes configuration          |
| [Telegram Setup](./docs/telegram.md)         | Creating and connecting the Telegram bot |
| [Troubleshooting](./docs/troubleshooting.md) | Common issues and solutions              |

---

## 📁 Project Structure

```text
hermes-ai-assistant/
│
├── README.md
├── LICENSE
├── .gitignore
├── .env.example
│
├── docs/
│   ├── architecture.md
│   ├── installation.md
│   ├── configuration.md
│   ├── telegram.md
│   └── troubleshooting.md
│
├── config/
│   └── config.example.yaml
│
└── screenshots/
    ├── telegram.png
    ├── hermes-terminal.png
    ├── gateway.png
    └── architecture.png
```

---

## 🚧 Future Improvements

* [ ] Add custom AI agent skills
* [ ] Improve long-term memory
* [ ] Add voice interaction
* [ ] Add web search capabilities
* [ ] Add scheduled tasks
* [ ] Add more Telegram commands
* [ ] Add Docker deployment
* [ ] Add monitoring dashboard
* [ ] Improve agent security and permissions

---

## 🎯 Learning Goals

Through this project, I explored:

* AI agent architecture
* LLM integration
* Google Gemini API
* Telegram Bot API
* Agent tools and skills
* Local AI development
* API authentication
* Gateway architecture
* Environment and secret management
* Remote AI assistant interaction

---

## 👨‍💻 Author

**Sugeng Permana**

Frontend & Backend Developer interested in AI agents, cloud technologies, and modern software development.

---

## 📄 License

This project is licensed under the [MIT License](./LICENSE).
