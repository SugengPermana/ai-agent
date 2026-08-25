# 🛠️ Installation Guide

This guide explains how to install and prepare the environment for **Hermes AI Assistant**.

The project runs inside **WSL2 / Ubuntu** on Windows.

---

## 📋 Requirements

Before starting, make sure you have:

* Windows 10 or Windows 11
* WSL2
* Ubuntu
* Git
* Internet connection
* A Google account
* A Telegram account

The assistant itself runs inside the Linux environment provided by WSL2.

---

# 1. Install WSL2

If WSL2 is already installed, skip this step.

Open **PowerShell as Administrator** and run:

```powershell
wsl --install
```

Restart Windows after the installation finishes.

Then open Ubuntu from the Start Menu.

You can verify WSL with:

```powershell
wsl --status
```

You should see information about the installed WSL version.

---

# 2. Verify Ubuntu

Open your Ubuntu terminal and run:

```bash
uname -a
```

Then check the Linux distribution:

```bash
cat /etc/os-release
```

You should see Ubuntu information.

---

# 3. Update Ubuntu

Before installing additional tools, update the package index:

```bash
sudo apt update
```

Upgrade existing packages:

```bash
sudo apt upgrade -y
```

---

# 4. Install Required Packages

Install Git and other basic utilities:

```bash
sudo apt install -y git curl
```

Verify Git:

```bash
git --version
```

Verify curl:

```bash
curl --version
```

---

# 5. Install Hermes Agent

Hermes Agent can be installed using its official installation script.

Run:

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

After the installation finishes, reload your shell:

```bash
source ~/.bashrc
```

Verify Hermes:

```bash
hermes --version
```

If the command returns a version number, Hermes has been installed successfully.

---

# 6. Initialize Hermes

Run the Hermes setup command:

```bash
hermes setup
```

Follow the setup prompts provided by Hermes.

The setup process prepares the local Hermes environment and configuration.

---

# 7. Verify Hermes Installation

Run:

```bash
hermes doctor
```

This command can help identify configuration or environment problems.

You can also start Hermes directly:

```bash
hermes
```

If Hermes starts successfully, the basic installation is complete.

Exit the interactive session when finished.

---

# 8. Check the Hermes Directory

Hermes stores its local configuration and runtime data inside:

```bash
~/.hermes/
```

You can inspect the directory with:

```bash
ls -la ~/.hermes/
```

Depending on the Hermes version and configuration, you may see files and directories such as:

```text
~/.hermes/
├── config.yaml
├── .env
├── logs/
├── memory/
├── skills/
└── personas/
```

The exact structure may vary between Hermes versions.

---

# 9. Create the Environment File

Sensitive credentials should be stored in an environment file rather than directly inside the repository.

Create or edit:

```bash
nano ~/.hermes/.env
```

The Gemini and Telegram credentials will be configured in the next documentation guides.

For now, the file can remain empty.

---

# 10. Verify Your PATH

If the `hermes` command cannot be found after installation, check your PATH:

```bash
echo $PATH
```

Reload your shell:

```bash
source ~/.bashrc
```

Then try:

```bash
hermes --version
```

If Hermes was installed into a user-local binary directory, make sure that directory is included in your PATH.

---

# 11. Clone This Documentation Repository

If you want to keep a copy of this project documentation locally:

```bash
git clone https://github.com/YOUR_USERNAME/hermes-ai-assistant.git
```

Move into the project:

```bash
cd hermes-ai-assistant
```

Replace:

```text
YOUR_USERNAME
```

with your GitHub username.

---

# 12. Environment Check

Before continuing with Gemini and Telegram configuration, verify the basic environment.

Run:

```bash
git --version
```

```bash
hermes --version
```

```bash
hermes doctor
```

You should now have:

```text
Windows
   │
   ▼
WSL2
   │
   ▼
Ubuntu
   │
   ├── Git
   ├── curl
   └── Hermes Agent
```

---

# 13. Next Steps

The basic Hermes installation is now complete.

Continue with the following guides:

### Configure Google Gemini

Set up Google AI Studio and connect Gemini to Hermes.

👉 [Configuration Guide](./configuration.md)

### Connect Telegram

Create a Telegram Bot and connect it to Hermes Gateway.

👉 [Telegram Setup Guide](./telegram.md)

### Understand the Architecture

Learn how Telegram, Hermes Gateway, Hermes Agent, and Gemini communicate.

👉 [Architecture Guide](./architecture.md)

---

# 🔍 Troubleshooting

If Hermes cannot be started, check the installation:

```bash
hermes doctor
```

If the command itself is unavailable:

```bash
source ~/.bashrc
```

Then:

```bash
hermes --version
```

If the problem persists, see:

👉 [Troubleshooting Guide](./troubleshooting.md)

---

## ✅ Installation Checklist

Before moving to the next guide, make sure:

* [ ] WSL2 is installed
* [ ] Ubuntu is working
* [ ] Git is installed
* [ ] curl is installed
* [ ] Hermes Agent is installed
* [ ] `hermes --version` works
* [ ] `hermes doctor` runs successfully
* [ ] `~/.hermes/` exists

Once all requirements are complete, continue to the **Google Gemini configuration**.
