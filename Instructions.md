# 🌤️ AI Summit Workshop

> **Windows setup guide** · Prepare your tools and get ready to build.

[Development tools](#1-install-git) · [Workshop repository](#4-open-the-workshop-repository) · [Spec Kit](#5-set-up-spec-kit) · [AI access](#6-connect-to-an-ai-model)

---

## 🧰 Your setup at a glance

| Tool | What you'll use it for |
| --- | --- |
| **Git** | Work with the workshop repository |
| **Node.js & npm** | Install and run development tools |
| **OpenCode** | Work with an AI coding agent |
| **Python & uv** | Set up the Spec Kit tooling |
| **AI model access** | Connect through local vLLM or OpenCode |

Run the commands below in **PowerShell**.

## 1. Install Git

```powershell
winget install --id Git.Git -e --source winget
```

## 2. Set up Node.js

With **fnm** installed, initialize it for PowerShell and install Node.js 26:

```powershell
fnm env --use-on-cd --shell powershell | Out-String | Invoke-Expression
fnm install --use 26
```

**Check your installation:** both commands should print a version number.

```powershell
node -v
npm -v
```

## 3. Install OpenCode

```powershell
npm install -g @opencode/cli
```

<details>
<summary><strong>Alternative installation command</strong></summary>

```powershell
npm install -g --allow-scripts=@opencode/cli @opencode/cli
```

</details>

## 4. Open the workshop repository

📂 **[AI Summit Workshop on GitHub](https://github.com/kolithawarnakulasooriya/AI-Summit-Workshop)**

Use this repository for the workshop project and its instructions.

## 5. Set up Spec Kit

### Install uv

```powershell
winget install --id=astral-sh.uv -e
```

### Install Python

Enter the following command. If Windows opens the Python installer, follow its prompts to finish installation.

```powershell
python
```

### Install the Specify CLI

```powershell
uv tool install specify-cli
```

> **Build-tool error?** Install the build tools requested by the error message, then retry the installation.

## 6. Connect to an AI model

### 🖥️ Local vLLM

Use the API key provided by the workshop organizer:

```text
<YOUR_VLLM_API_KEY>
```

The project’s OpenCode settings are in [opencode.jsonc](opencode.jsonc). Keep the actual API key outside this shared guide.

### ☁️ OpenCode free models

Open the **[OpenCode sign-in page](https://opencode.ai/console/login/)** to access the available free models.

---

## ✅ Ready for the workshop

- [ ] Git is installed.
- [ ] `node -v` and `npm -v` print version numbers.
- [ ] OpenCode is installed.
- [ ] The workshop repository is available.
- [ ] Python, uv, and the Specify CLI are installed.
- [ ] AI model access is configured.
