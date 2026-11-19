# Local Agent

A local AI-powered coding and image-generation assistant for Windows, powered by [Ollama](https://ollama.com/) and [stable-diffusion.cpp](https://github.com/leejet/stable-diffusion.cpp).

Local Agent allows you to work with your code projects using an AI assistant running locally on your computer. It can inspect project files, help you understand code, perform coding-related tasks, and optionally generate images using locally hosted image-generation models.

The application is distributed as a standalone Windows executable. **You do not need Python or the source code to use it.**

---

## Features

- **Local AI:** Run language models locally through Ollama.
- **Code assistance:** Ask questions about your projects and get coding help.
- **Project awareness:** Work with files inside your selected project.
- **File operations:** Use the available project tools to inspect and modify files.
- **AI image generation:** Generate images locally using stable-diffusion.cpp.
- **Image display:** View generated images directly inside the chat.
- **Image storage:** Save generated images inside your project folder.
- **Chat history:** Keep conversations organized.
- **Projects and groups:** Organize your chats.
- **Ignore rules:** Control which files and folders the agent should ignore.
- **Windows application:** No Python installation required.

---

## Requirements

### Required for coding and chat

- Windows 10 or Windows 11 (64-bit).
- [Ollama](https://ollama.com/download/windows).
- Enough RAM, VRAM, and disk space for your chosen language model.

### Optional: Image generation

If you want to use image generation, you will also need:

- The stable-diffusion.cpp Windows runtime.
- The CUDA 12 runtime files included in the provided configuration archives.
- A compatible Stable Diffusion model.
- A compatible NVIDIA GPU and sufficient system resources.

**Important:** Image generation is entirely optional. You do not need the image-generation runtime or an image model to use Local Agent for coding and chat.

The Local Agent executable does not include Ollama, image-generation runtime files, or AI model weights. These are installed separately.

| Software               | Required? | Purpose                    |
| ---------------------- | --------- | -------------------------- |
| Local Agent            | Yes       | Main application           |
| Ollama                 | Yes       | Chat and coding assistance |
| Language model         | Yes       | Powers the AI assistant    |
| stable-diffusion.cpp   | Optional  | Image-generation engine    |
| CUDA runtime files     | Optional  | NVIDIA GPU support         |
| Stable Diffusion model | Optional  | Generates images           |

---

## Installation

### 1. Download Local Agent

1. Open the [Releases](https://github.com/Mommy-Of-Light/Local-ia/releases/latest) page.
2. Download the latest Windows executable.
3. Create a folder for the application, for example:

   `C:\LocalAgent\`

4. Place the downloaded executable inside that folder.
5. Double-click the executable to launch it.

You do not need to install Python, PyInstaller, or any development tools.

> **Tip:** Keep the application in a folder where your Windows account has permission to write files. Avoid placing it inside `C:\Program Files\`.

### 2. Install Ollama

Local Agent uses Ollama to communicate with locally running language models.

1. Visit the official Ollama website:

   [https://ollama.com/download/windows](https://ollama.com/download/windows)

2. Download the Windows installer.
3. Run the installer.
4. Complete the installation.
5. Make sure Ollama is running.

Ollama runs locally on your computer and provides the API that Local Agent uses.

### 3. Download an AI model

After installing Ollama, open PowerShell or Windows Terminal.

To download the default model, run:

```powershell
ollama pull qwen3.5:4b
```

This downloads the Qwen 3.5 4B model to your computer.

The download may take some time depending on your internet connection.

#### Verify the installation

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

## Optional: Install Image Generation

You can skip this entire section if you only want to use Local Agent for coding and chat.

Local Agent uses **stable-diffusion.cpp** to generate images locally. The image-generation engine runs separately from Ollama.

The application starts the image-generation server automatically when you request an image. You do not need to start the server manually.

### 1. Download the image-generation runtime

The project includes an `image-generator-config` folder containing two ZIP archives:

- `sd-master-532d2c1-bin-win-cuda12-x64.zip`
- `cudart-sd-bin-win-cu12-x64.zip`

These archives contain the Windows image-generation engine and the required CUDA 12 runtime DLL files.

Extract both archives to permanent folders on your computer.

For example:

```text
C:\AI\stable-diffusion\
    sd-master-532d2c1-bin-win-cuda12-x64\
        sd-server.exe
        sd-cli.exe
        ...

    cudart-sd-bin-win-cu12-x64\
        cublas64_12.dll
        cublasLt64_12.dll
        cudart64_12.dll
        ...
```

You can use different folders or drive locations if you prefer.

**Important:**

- Extract both archives before configuring Local Agent.
- Do not delete or move the extracted runtime folders after configuration.
- Keep the CUDA DLL files together in their extracted folder.
- You do not need to add the CUDA folder to your Windows PATH. Local Agent configures the runtime environment when starting the image-generation server.

### 2. Download a Stable Diffusion model

The image-generation engine needs a compatible model file.

For the tested Stable Diffusion 1.5 model, download:

[Stable Diffusion 1.5 - FP16 model](https://huggingface.co/Comfy-Org/stable-diffusion-1.5/resolve/main/v1-5-pruned-emaonly-fp16.safetensors)

The model file is approximately 2.1 GB.

Create a folder to store your models, for example:

```text
C:\AI\Models\
```

Place the downloaded model inside it:

```text
C:\AI\Models\
    v1-5-pruned-emaonly-fp16.safetensors
```

You can store models on another drive if you prefer. The model does not need to be inside the Local Agent application folder.

### 3. Configure image generation in Local Agent

Start Local Agent and open **Image Settings**.

Configure the following three paths:

| Setting             | Example                                                                     |
| ------------------- | --------------------------------------------------------------------------- |
| Server executable   | `C:\AI\stable-diffusion\sd-master-532d2c1-bin-win-cuda12-x64\sd-server.exe` |
| CUDA runtime folder | `C:\AI\stable-diffusion\cudart-sd-bin-win-cu12-x64`                         |
| Model file          | `C:\AI\Models\v1-5-pruned-emaonly-fp16.safetensors`                         |

Select the actual locations where you extracted or saved these files.

Save the settings.

Local Agent stores these image-generation settings in:

```text
%APPDATA%\LocalAgent\settings.json
```

The settings are stored separately from the application executable, so you can move the executable without having to configure image generation again, provided the runtime and model paths remain unchanged.

### 4. Generate an image

Once configured, use the image-generation button inside Local Agent.

For example:

```text
Create a futuristic city at night with neon lights,
cinematic lighting, and highly detailed architecture.
```

The application will:

1. Start the local stable-diffusion.cpp server if it is not already running.
2. Send your prompt to the local image-generation API.
3. Receive the generated image.
4. Display it directly in the conversation.
5. Save it inside the selected project's `generated_images` folder.

The image-generation server listens locally on:

```text
http://127.0.0.1:1234
```

You do not need to configure a cloud service or manually start the server.

**Note:** Image generation can take several minutes, particularly on GPUs with limited VRAM. The tested configuration uses automatic memory fitting to make use of both GPU memory and system RAM.

---

## Choosing Other AI Models

You are not limited to one model. Ollama supports many locally running language models.

Browse available models here:

[https://ollama.com/library](https://ollama.com/library)

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

## Which model should I use?

| Model size    | General recommendation                                   |
| ------------- | -------------------------------------------------------- |
| 1B–3B         | Lightweight, suitable for less powerful computers        |
| 4B–8B         | Good balance between performance and resource usage      |
| 14B and above | More demanding; generally benefits from more RAM or VRAM |

Actual performance depends on your hardware, model architecture, and available memory.

**Recommended starting point:** `qwen3.5:4b`

---

## Using Local Agent

### 1. Start the application

Launch the downloaded executable.

Make sure Ollama is installed and running before using the coding assistant.

If you have configured image generation, Local Agent will start its image-generation server automatically when needed.

### 2. Select a project

Choose the folder containing the source code you want the assistant to work with.

The assistant uses the selected project as its working directory.

### 3. Start a conversation

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

### 4. Generate images (Optional)

If you have configured stable-diffusion.cpp and downloaded a compatible model, you can use the image-generation button to create images from text descriptions.

For example:

```text
Create a futuristic city at night with neon lights,
cinematic lighting, and highly detailed architecture.
```

The generated image appears directly in the conversation and is saved inside the selected project's `generated_images` folder.

If you have not configured image generation, you can simply ignore this feature and continue using Local Agent for coding and chat.

### 5. Review tool activity

When the assistant uses project tools, you can inspect the tool activity and its results in the Tool Details panel.

Review the proposed operations and their results when working with important files.

## 6. Organize conversations

Use chats, groups, and projects to keep your conversations organized.

---

## Application Data

Local Agent stores its persistent application data in different locations.

| File or folder                          | Purpose                                                   |
| --------------------------------------- | --------------------------------------------------------- |
| `chat_data.json`                        | Stores chat and conversation data                         |
| `ignore_rules.txt`                      | Stores file and folder ignore rules                       |
| `%APPDATA%\LocalAgent\settings.json`    | Stores image-generation configuration                     |
| `%APPDATA%\LocalAgent\image_server.log` | Image-generation server log                               |
| `generated_images/`                     | Stores generated images inside individual project folders |

**Important:** Back up these files and folders if you want to preserve your conversations, settings, and generated images when moving or replacing the executable.

The image-generation runtime and model files are stored separately wherever you choose to extract or download them.

Do not delete these files unless you intend to remove the associated application data.

---

## Troubleshooting

### Local Agent cannot connect to Ollama

Make sure Ollama is installed and running.

Open PowerShell and execute:

```powershell
ollama list
```

You can also check whether its local API is responding:

```powershell
Invoke-RestMethod http://localhost:11434/api/tags
```

If Ollama is not running, start it from the Windows Start menu.

### Model not found

If the application reports that a model is unavailable, download it:

```powershell
ollama pull qwen3.5:4b
```

Then check:

```powershell
ollama list
```

Make sure the model name matches the model configured in Local Agent.

### Image generation is not working

Make sure:

- The stable-diffusion.cpp runtime archive has been extracted.
- The CUDA runtime archive has been extracted.
- The configured `sd-server.exe` path is correct.
- The configured CUDA runtime folder contains the required DLL files.
- The configured model file exists.
- Your GPU and system have sufficient available resources.

Check the image-generation log:

```text
%APPDATA%\LocalAgent\image_server.log
```

This log can help identify missing DLL files, invalid model paths, or server startup problems.

### Image generation is slow

Image-generation performance depends heavily on your hardware.

Try:

- Closing other memory-intensive applications.
- Closing applications using your GPU.
- Making sure sufficient system RAM and VRAM are available.
- Checking that your NVIDIA GPU is being used.
- Using a smaller image resolution or fewer sampling steps if supported by the application.

Image generation may take several minutes on less powerful hardware.

### The application closes immediately

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

### The model is slow

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

## Privacy

Local Agent is designed to work with locally hosted AI models.

- Language-model inference is performed locally through Ollama.
- Optional image generation is performed locally through stable-diffusion.cpp.
- Your model files are stored on your computer.
- Generated images are stored in your project folders.
- The application communicates with the local runtimes through their local APIs.

An internet connection is required to download Ollama, runtime files, and AI models.

Once everything has been downloaded, local model inference and image generation can work without an internet connection.

Other network activity may depend on the software and services you use.

---

## Frequently Asked Questions

### Do I need Python installed?

No. The distributed Windows executable is intended to run without a Python installation.

### Do I need to install Ollama?

Yes. Ollama is required to run the local language models used for chat and coding.

### Do I need stable-diffusion.cpp?

No. Image generation is completely optional.

You only need it if you want to use the image-generation functionality.

### Can I use Local Agent without image generation?

Yes. You only need Local Agent, Ollama, and a compatible language model for coding and chat.

### Are AI models included in the executable?

No. Download models separately using the appropriate runtime.

### Does the application require an internet connection?

An internet connection is needed to download Ollama, runtime files, and AI models.

Once installed, local model inference and image generation can work without an internet connection.

### Can I use models other than Qwen?

Yes, provided the model is supported by Ollama and configured correctly in the application.

### Where are my conversations saved?

In `chat_data.json`, located beside the executable.

### Where are image-generation settings saved?

In `%APPDATA%\LocalAgent\settings.json`.

### Where are generated images saved?

Generated images are saved in the `generated_images` folder inside the selected project.

### Does image generation use a cloud service?

No. Image generation runs locally through stable-diffusion.cpp on your computer.

---

## Links

- [Ollama Website](https://ollama.com/)
- [Download Ollama for Windows](https://ollama.com/download/windows)
- [Ollama Model Library](https://ollama.com/library)
- [stable-diffusion.cpp GitHub](https://github.com/leejet/stable-diffusion.cpp)
- [Stable Diffusion 1.5 Model](https://huggingface.co/Comfy-Org/stable-diffusion-1.5/resolve/main/v1-5-pruned-emaonly-fp16.safetensors)
- [Local Agent Releases](https://github.com/Mommy-Of-Light/Local-ia/releases/latest)

---

<p align="center">
  Built for local AI-powered coding and image generation.
</p>
