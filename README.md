# Ollama Local Agent UI

A local AI-powered coding assistant for Windows, powered by [Ollama](https://ollama.com/) and local language models.

Local Agent allows you to work with your code projects using an AI assistant running locally on your computer. It can inspect project files, help you understand code, and perform coding-related tasks using its available project tools.

The application is distributed as a standalone Windows executable. **You do not need Python or the source code to use it.**

---

## Features

- **Local AI:** Run language models locally through Ollama.
- **Code assistance:** Ask questions about your projects and get coding help.
- **Project awareness:** Work with files inside your selected project.
- **File operations:** Use the available project tools to inspect and modify files.
- **Chat history:** Keep conversations organized.
- **Projects and groups:** Organize your chats.
- **Ignore rules:** Control which files and folders the agent should ignore.
- **Windows application:** No Python installation required.

## Requirements

Before using Local Agent, you need:

- Windows 10 or Windows 11 (64-bit).
- An internet connection for downloading Ollama and AI models.
- [Ollama](https://ollama.com/download/windows).
- Enough VRAM (GPU memory) and disk space for your chosen AI model.

**Important:** The executable does not include Ollama or any AI models. These must be installed separately.

---

# Installation

## 1. Download Local Agent

1. Open the [Releases](https://github.com/Mommy-Of-Light/Local-ia/releases/latest) page.
2. Download the latest Windows executable.
3. Create a folder for the application, for example:

   `C:\LocalAgent`

4. Place the downloaded executable inside that folder.
5. Double-click the executable to launch it.

You do not need to install Python, PyInstaller, or any development tools.

> **Tip:** Keep the application in a folder where your Windows account has permission to write files. Avoid placing it inside `C:\Program Files`.

## 2. Install Ollama

Local Agent uses Ollama to communicate with locally running AI models.

1. Visit the official Ollama website:

   https://ollama.com/download/windows

2. Download the Windows installer.
3. Run the installer.
4. Complete the installation.
5. Make sure Ollama is running.

Ollama runs locally on your computer and provides the API that Local Agent uses.

## 3. Download an AI model

After installing Ollama, open PowerShell or Windows Terminal.

To download the default model, run:

```powershell
ollama pull qwen3.5:4b
```

This downloads the Qwen 3.5 4B model to your computer.

The download may take some time depending on your internet connection.

### Verify the installation

Run:

```powershell
ollama list
```

You should see your downloaded model in the list.

You can also test the model directly:

```powershell
ollama run qwen3.5:4b
```

If the model responds to your message, Ollama is working correctly.

Type `/bye` to exit the model conversation.

---

# Choosing other AI models

You are not limited to one model. Ollama supports many locally running models.

Browse available models here:

https://ollama.com/library

To download another model, use:

```powershell
ollama pull MODEL_NAME
```

For example:

```powershell
ollama pull qwen3.5:4b
```

To see all models installed on your computer:

```powershell
ollama list
```

To remove a model you no longer need:

```powershell
ollama rm MODEL_NAME
```

### Which model should I use?

| Model size | General recommendation |
|---|---|
| 1B–3B | Lightweight, suitable for less powerful computers |
| 4B–8B | Good balance between performance and resource usage |
| 14B and above | More demanding; generally benefits from more RAM or VRAM |

Actual performance depends on your hardware, model architecture, and available memory.

**Recommended starting point:** `qwen3.5:4b`

---

# Using Local Agent

## 1. Start the application

Launch the downloaded executable.

Make sure Ollama is installed and running before using the assistant.

## 2. Select a project

Choose the folder containing the source code you want the assistant to work with.

The assistant uses the selected project as its working directory.

## 3. Start a conversation

Create a new chat and ask the assistant to help with your project.

Examples:

```text
Explain the structure of this project.
```

```text
Find the function responsible for user authentication.
```

```text
Find and explain potential bugs in this project.
```

```text
Help me refactor the database connection code.
```

```text
Add error handling to the configuration loader.
```

## 4. Review tool activity

When the assistant uses project tools, you can inspect the tool activity and its results in the Tool Details panel.

Review the proposed operations and their results when working with important files.

## 5. Organize conversations

Use chats, groups, and projects to keep your conversations organized.

---

# Application data

Local Agent stores its persistent application data in the folder containing the executable.

Depending on your usage, this includes:

| File | Purpose |
|---|---|
| `chat_data.json` | Stores chat and conversation data |
| `ignore_rules.txt` | Stores file and folder ignore rules |

**Important:** Back up these files if you want to preserve your conversations and settings when moving or replacing the executable.

Do not delete these files unless you intend to remove the associated application data.

---

# Troubleshooting

## Local Agent cannot connect to Ollama

Make sure Ollama is installed and running.

Open PowerShell and execute:

```powershell
ollama list
```

If the command works, Ollama is installed.

You can also check whether its local API is responding:

```powershell
Invoke-RestMethod http://localhost:11434/api/tags
```

If Ollama is not running, start it from the Windows Start menu.

## Model not found

If the application reports that a model is unavailable, download it:

```powershell
ollama pull qwen3.5:4b
```

Then check:

```powershell
ollama list
```

Make sure the model name matches the model configured in Local Agent.

## The application closes immediately

Try launching the executable from PowerShell instead of double-clicking it.

Navigate to the application folder:

```powershell
cd C:\LocalAgent
```

Then run:

```powershell
.\local-code.exe
```

Replace the executable name if yours is different.

This can help reveal error messages.

## The model is slow

Local AI performance depends heavily on your computer's hardware.

Try:

- Closing other memory-intensive applications.
- Using a smaller model.
- Making sure your GPU is supported and available to Ollama.
- Checking that sufficient RAM and disk space are available.

## Windows blocks the executable

Windows may display a security warning for downloaded executables.

Only run the application if you downloaded it from this project's official GitHub Releases page and trust its origin.

---

# Privacy

Local Agent is designed to work with locally hosted AI models through Ollama.

- Model inference is performed locally by Ollama.
- Your model files are stored on your computer.
- The application communicates with Ollama through its local API.

An internet connection is required to download Ollama and models. Other network activity may depend on the software and services you use.

---

# Frequently Asked Questions

### Do I need Python installed?

No. The distributed Windows executable is intended to run without a Python installation.

### Do I need to install Ollama?

Yes. Ollama is required to run the local AI model.

### Are AI models included in the executable?

No. Download models separately using Ollama.

### Does the application require an internet connection?

An internet connection is needed to download Ollama and AI models. Once installed, local model inference can work without an internet connection.

### Can I use models other than Qwen?

Yes, provided the model is supported by Ollama and configured correctly in the application.

### Where are my conversations saved?

In `chat_data.json`, located beside the executable.

---

# Links

- [Ollama Website](https://ollama.com/)
- [Download Ollama for Windows](https://ollama.com/download/windows)
- [Ollama Model Library](https://ollama.com/library)
- [Local Agent Releases](https://github.com/Mommy-Of-Light/Local-ia/releases/latest)

---

<p align="center">
  Built for local AI-powered coding assistance.
</p>
