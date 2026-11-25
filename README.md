# Local Agent

A local AI-powered coding and image-generation assistant for Windows, powered by [Ollama](https://ollama.com/) and [stable-diffusion.cpp](https://github.com/leejet/stable-diffusion.cpp).

Local Agent lets you work with code projects using AI models running locally on your computer. It can inspect project files, help explain and modify code, and optionally generate images using locally hosted image-generation models.

The application is distributed as a standalone Windows executable.
**You do not need Python or the source code to use the released application.**

---

## Features

- **Local AI chat and coding:** Run language models locally through Ollama.
- **Project awareness:** Work with files inside a selected project.
- **File operations:** Inspect and modify project files using the available project tools.
- **Multiple image models:** Discover image models from a model directory and their `manifest.json` files.
- **Image generation:** Generate images locally through stable-diffusion.cpp.
- **Model switching:** Use Stable Diffusion 1.5 for lightweight generations or Qwen Image 2.1 Turbo for more complex prompts.
- **Image display:** View generated images directly in the chat.
- **Image storage:** Save generated images in the active project when a project is selected.
- **Chat history:** Keep conversations organized.
- **Projects and groups:** Organize chats.
- **Ignore rules:** Control which files and folders the agent should ignore.
- **Standalone Windows application:** No Python installation required for end users.

---

## Requirements

### Required for chat and coding

- Windows 10 or Windows 11, 64-bit.
- [Ollama for Windows](https://ollama.com/download/windows).
- At least one model downloaded through Ollama.
- Enough RAM, VRAM, and disk space for the selected language model.

### Optional: image generation

Image generation is optional. To use it, you need:

- The Local Agent image-generation runtime archive (`sd-master-...zip`).
- The CUDA 12 runtime archive (`cudart-sd-...zip`) for the supplied CUDA build.
- One or more compatible image models and their required components.
- A compatible NVIDIA GPU and sufficient GPU/system memory for the selected model.

**The Local Agent executable does not include Ollama, image-generation runtime files, CUDA runtime files, or model weights.** These are downloaded and installed separately.

| Component             | Required? | Purpose                                    |
| --------------------- | --------- | ------------------------------------------ |
| Local Agent           | Yes       | Main application                           |
| Ollama                | Yes       | Chat and coding inference                  |
| Ollama language model | Yes       | Powers the assistant                       |
| stable-diffusion.cpp  | Optional  | Image-generation runtime engine            |
| CUDA runtime files    | Optional  | CUDA support for the supplied NVIDIA build |
| Image model files     | Optional  | Generate images                            |

---

## Installation

### 1. Download Local Agent

1. Open the [Local Agent Releases](https://github.com/Mommy-Of-Light/Local-ia/releases/latest) page.

2. Download the latest Windows executable.

3. Create a folder for the application, for example:

    ``` text
    C:\LocalAgent\
    ```

4. Place the executable in that folder and launch it.

You do not need Python, PyInstaller, or development tools to run the released executable.

**Tip:** Keep the application and its writable data in a folder where your Windows account has permission to write.
Avoid placing it in `C:\Program Files` unless the application is configured to store all writable data elsewhere.

### 2. Install Ollama

1. Visit [Ollama for Windows](https://ollama.com/download/windows).
2. Download and run the installer.
3. Complete the installation and make sure Ollama is running.

Local Agent communicates with Ollama through its local API.

### 3. Download a language model

Open PowerShell or Windows Terminal and run:

``` powershell
ollama pull qwen3.5:4b
```

This downloads the example Qwen 3.5 4B model.
You can choose another model supported by your Ollama installation.

Verify the model is installed:

``` powershell
ollama list
```

Test it directly:

``` powershell
ollama run qwen3.5:4b
```

If it responds, Ollama and the model are working. Type `/bye` to exit.

---

## Optional: Install Image Generation

Skip this entire section if you only want to use Local Agent for chat and coding.

Local Agent uses **stable-diffusion.cpp** as its image-generation engine.
It starts the local server automatically when you request an image.
You do not need to start the server manually.

### 1. Download the two runtime archives

Each Local Agent GitHub Release that provides image-generation support should include these two runtime ZIP files as separate release assets:

- `sd-master-532d2c1-bin-win-cuda12-x64.zip`
- `cudart-sd-bin-win-cu12-x64.zip`

Download both archives from the **Assets** section of the release.
Extract them into permanent folders. For example:

``` text
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

The actual contents depend on the supplied archive.
Keep the two extracted folders separate.

You can use another drive or folder.
The paths above are examples only.

**Important:**

- Extract both archives before configuring Local Agent.
- Do not delete or move the folders after configuring the application unless you update the paths in Image Settings.
- Keep the CUDA DLL files together in their extracted folder.
- You do not need to add the CUDA folder to the Windows `PATH`. Local Agent prepends the configured CUDA folder to the environment of the image-generation server process.
- Do not start a second server manually when Local Agent is managing the server.

### 2. Create a model directory

Choose a permanent directory for image models. For example:

``` text
C:\AI\Models\
```

This directory is the **Models Root** that you will select in Local Agent.

Models are not included in the application executable or the runtime ZIP archives.
Download the model files separately from their model publishers and place each file in the appropriate category folder.

### 3. Supported example models

Local Agent discovers image models by scanning for model manifests under:

``` text
<Models Root>\LocalAgent\<model-id>\manifest.json
```

The manifest tells Local Agent which stable-diffusion.cpp recipe to load and which model component files belong to each recipe slot.

#### Stable Diffusion 1.5

Stable Diffusion 1.5 is the lighter model in the supplied example setup and is useful for faster, simpler generations.

Example checkpoint:

- [Stable Diffusion 1.5 FP16 checkpoint](https://huggingface.co/Comfy-Org/stable-diffusion-1.5/resolve/main/v1-5-pruned-emaonly-fp16.safetensors)

Place it here:

``` text
C:\AI\Models\checkpoints\v1-5-pruned-emaonly-fp16.safetensors
```

Create this manifest:

`C:\AI\Models\LocalAgent\sd15\manifest.json`

``` json
{
  "id": "sd15",
  "name": "Stable Diffusion 1.5",
  "description": "Stable Diffusion 1.5 FP16 checkpoint",
  "recipe": "sd1-checkpoint",
  "order": 1,
  "components": {
    "model": "checkpoints/v1-5-pruned-emaonly-fp16.safetensors"
  },
  "defaults": {
    "width": 512,
    "height": 512,
    "steps": 20,
    "cfg_scale": 7,
    "sampling_method": "euler_a"
  }
}
```

The manifest is a small JSON file.
The checkpoint itself is a separate model download.

#### Qwen Image 2.1 Turbo

Qwen Image 2.1 Turbo is the more demanding model in this example setup.
It is intended for more complex prompts, detailed scenes, and stronger prompt comprehension.
It uses a diffusion model, a Qwen text encoder, and a VAE.

The tested component filenames in this setup are:

- Diffusion model: `Qwen-Image-2.1-Turbo-AD-Q6_K.gguf`
- Text encoder: `Qwen-Image-2.1-Turbo-Abliterated-Uncensored-AD-Q6_K.gguf`
- VAE: `qwen_image_2.1_vae_bf16.safetensors`

Download these files from their respective model publishers.
Do not rename the files unless you also update the manifest.

Place the files in these folders:

``` text
C:\AI\Models\
    diffusion_models\
        Qwen-Image-2.1-Turbo-AD-Q6_K.gguf

    text_encoders\
        Qwen-Image-2.1-Turbo-Abliterated-Uncensored-AD-Q6_K.gguf

    vae\
        qwen_image_2.1_vae_bf16.safetensors
```

Create this manifest:

`C:\AI\Models\LocalAgent\qwen-image-21-turbo\manifest.json`

``` json
{
  "id": "qwen-image-21-turbo",
  "name": "Qwen Image 2.1 Turbo",
  "description": "Qwen Image 2.1 Turbo GGUF diffusion model",
  "recipe": "qwen-image-21-turbo",
  "order": 2,
  "components": {
    "diffusion_model": "diffusion_models/Qwen-Image-2.1-Turbo-AD-Q6_K.gguf",
    "llm": "text_encoders/Qwen-Image-2.1-Turbo-Abliterated-Uncensored-AD-Q6_K.gguf",
    "vae": "vae/qwen_image_2.1_vae_bf16.safetensors"
  },
  "required_any": [
    ["vae", "tae"]
  ],
  "defaults": {
    "width": 1024,
    "height": 1024,
    "steps": 8,
    "cfg_scale": 1,
    "sampling_method": "euler"
  }
}
```

For ordinary text-to-image generation with this tested setup, the manifest does not include an `llm_vision` component.
Image-editing workflows that use reference images may have additional component requirements.

### 4. Complete model directory structure

After adding both example models, your Models Root should look like this:

``` text
C:\AI\Models\
│
├── checkpoints\
│   └── v1-5-pruned-emaonly-fp16.safetensors
│
├── diffusion_models\
│   └── Qwen-Image-2.1-Turbo-AD-Q6_K.gguf
│
├── text_encoders\
│   └── Qwen-Image-2.1-Turbo-Abliterated-Uncensored-AD-Q6_K.gguf
│
├── vae\
│   └── qwen_image_2.1_vae_bf16.safetensors
│
└── LocalAgent\
    ├── sd15\
    │   └── manifest.json
    │
    └── qwen-image-21-turbo\
        └── manifest.json
```

The `LocalAgent` folder contains the model manifests.
The model component folders contain the actual downloaded weights.

You can add more compatible models by creating their component folders and a corresponding `manifest.json` with the correct recipe ID, slot names, and relative file paths.
The recipe and component slots must be supported by the installed stable-diffusion.cpp build.

### 5. Configure Image Settings

1. Start Local Agent.
2. Open **Image Settings**.
3. Set the following paths:

| Setting             | Example                                                                     |
| ------------------- | --------------------------------------------------------------------------- |
| Server executable   | `C:\AI\stable-diffusion\sd-master-532d2c1-bin-win-cuda12-x64\sd-server.exe` |
| CUDA runtime folder | `C:\AI\stable-diffusion\cudart-sd-bin-win-cu12-x64`                         |
| Models Root         | `C:\AI\Models`                                                              |

1. Save the settings.
2. Refresh the model list.

Local Agent scans the Models Root for manifests and checks that the referenced component files exist.
The discovered image models appear in the model selector.

Settings are stored separately from the executable:

``` text
%APPDATA%\LocalAgent\settings.json
```

The configured paths must continue to point to the extracted runtime and model directories.
Moving the EXE does not require reconfiguring these paths, provided the runtime and model directories remain where they are.

### 6. Generate an image

Select an image model in the model selector, enter a prompt, and use
**Generate Image**.

For example:

``` text
A tiny astronaut exploring an enormous greenhouse on Mars, cinematic lighting, intricate details, wide composition.
```

When requested, Local Agent:

1. Starts its local stable-diffusion.cpp server if needed.
2. Loads the selected model recipe and its components.
3. Sends the prompt to the local image-generation API.
4. Receives the generated image.
5. Displays it in the conversation.
6. Saves the generated image in the active project's `generated_images` folder when a project is selected.

The local image-generation server uses:

``` text
http://127.0.0.1:1234
```

No cloud image-generation service is required.

**Performance note:** Generation time and maximum practical resolution depend on the GPU, VRAM, system RAM, model, and settings.
Qwen Image 2.1 Turbo is more demanding than Stable Diffusion 1.5.
A model that works on one GPU is not guaranteed to fit or run well on another.

---

## Choosing an Image Model

The example setup provides two different options:

| Model                | Best suited for                                       |
| -------------------- | ----------------------------------------------------- |
| Stable Diffusion 1.5 | Lightweight use, quick drafts, simpler prompts        |
| Qwen Image 2.1 Turbo | Complex prompts, detailed scenes, richer compositions |

The selected image model is loaded by the same stable-diffusion.cpp
server. Switching models changes the active recipe and component set.

---

## Choosing Other AI Language Models

Local Agent is not limited to Qwen. Ollama supports many local language models.

Browse the [Ollama model library](https://ollama.com/library).

Download a model:

``` powershell
ollama pull MODEL_NAME
```

List installed models:

``` powershell
ollama list
```

Remove a model:

``` powershell
ollama rm MODEL_NAME
```

### General model-size guidance

| Model size    | General guidance                                         |
| ------------- | -------------------------------------------------------- |
| 1B--3B        | Lightweight; useful on less powerful computers           |
| 4B--8B        | Often a good balance of capability and resource use      |
| 14B and above | More demanding; generally benefits from more RAM or VRAM |

Actual performance depends on model architecture, quantization, context length, and available memory.

**Recommended starting point:** `qwen3.5:4b`

---

## Using Local Agent

### 1. Start the application

Launch the Local Agent executable. Make sure Ollama is installed and running before using chat or coding features.

If image generation is configured, Local Agent starts its image-generation server automatically when needed.

### 2. Select a project

Choose the folder containing the source code you want the assistant to work with.

### 3. Start a conversation

Create a new chat and ask the assistant to help with your project.
Examples:

``` text
Explain the structure of this project.
```

``` text
Find the function responsible for user authentication.
```

``` text
Find and explain potential bugs in this project.
```

``` text
Help me refactor the database connection code.
```

``` text
Add error handling to the configuration loader.
```

### 4. Generate images (optional)

Select an image model, enter a text prompt, and generate an image. The result appears in the conversation.

### 5. Review tool activity

When the assistant uses project tools, inspect their activity and results in the Tool Details panel.
Review proposed operations and results when working with important files.

### 6. Organize conversations

Use chats, projects, and groups to keep conversations organized.

---

## Application Data

Local Agent stores application data separately from downloaded model weights and runtime files.

| File or folder                                                               | Purpose                                       |
| ---------------------------------------------------------------------------- | --------------------------------------------- |
| `chat_data.json`                                                             | Chat and conversation data                    |
| `ignore_rules.txt`                                                           | File and folder ignore rules                  |
| `%APPDATA%\LocalAgent\settings.json`                                         | Image-generation paths and settings           |
| `%APPDATA%\LocalAgent\image_server.log`                                      | Image-generation server log                   |
| `<project>\generated_images\`                                                | Generated images saved for a selected project |
| `<Models Root>\LocalAgent\`                                                  | Image model manifests                         |
| `<Models Root>\checkpoints\`, `diffusion_models\`, `text_encoders\`,  `vae\` | Image model component files                   |

Back up your chat data, ignore rules, settings, generated images, manifests, and model files if you want to preserve your setup.

The runtime and model files are stored separately wherever you choose to extract or download them.

---

## Troubleshooting

### Local Agent cannot connect to Ollama

Make sure Ollama is installed and running.

``` powershell
ollama list
```

Check whether the local API responds:

``` powershell
Invoke-RestMethod http://localhost:11434/api/tags
```

### Ollama model not found

Download the model:

``` powershell
ollama pull qwen3.5:4b
```

Then check:

``` powershell
ollama list
```

Make sure the model name selected in Local Agent matches an installed Ollama model.

### No image models appear

Check that:

- Image Settings points to the **Models Root**, not an individual model file.
- Each model has a manifest at `LocalAgent\<model-id>\manifest.json`.
- Each manifest is valid JSON.
- Every component path in the manifest is relative to the Models Root.
- Every referenced model file exists.
- The recipe ID and component slot names are supported by the installed stable-diffusion.cpp build.

After correcting a manifest or adding files, refresh the model list.

### Image generation does not start

Check that:

- The runtime ZIP archives have been extracted.
- The configured `sd-server.exe` path is correct.
- The CUDA runtime folder contains the required DLL files.
- The Models Root is correct.
- The selected model manifest and all component files are present.
- The GPU and system have enough available resources.

Check the log:

``` text
%APPDATA%\LocalAgent\image_server.log
```

### Image generation is slow or runs out of memory

Image-generation performance depends on the GPU, VRAM, system RAM, resolution, and model.

Try:

- Closing other GPU- or memory-intensive applications.
- Starting with a smaller or lighter model, such as Stable Diffusion 1.5.
- Using a lower resolution where supported.
- Checking that the NVIDIA GPU is available to the runtime.
- Ensuring enough system RAM and disk space are available.

Qwen Image 2.1 Turbo is more demanding than Stable Diffusion 1.5 and may not be suitable for GPUs with limited memory.

### The application closes immediately

Launch the executable from PowerShell to see whether an error is printed:

``` powershell
cd C:\LocalAgent
.\LocalAgent.exe
```

Replace `LocalAgent.exe` with the actual executable filename if different.

### Windows blocks the executable

Only run the application if you downloaded it from the project's official GitHub Releases page and trust its origin.

---

## Privacy and Offline Use

Local Agent is designed to use locally hosted AI models:

- Chat and coding inference run locally through Ollama.
- Image generation runs locally through stable-diffusion.cpp.
- Model weights are stored on your computer.
- Generated images are saved locally.

An internet connection is needed to download the application, Ollama, runtime archives, and model weights.
Once those components are downloaded, local inference and image generation can operate without an internet connection.

Other network activity may depend on the software and services you use.

---

## Frequently Asked Questions

### Do I need Python installed?

No. The released Windows executable is intended to run without a Python installation.

### Do I need Ollama?

Yes. Ollama is required for local chat and coding models.

### Do I need stable-diffusion.cpp?

No. Image generation is optional.

### Are the image-generation runtime and models included in the EXE?

No. The runtime ZIP archives and model weights are separate downloads.
This keeps the main application download smaller and lets users skip image generation entirely.

### Can I use Local Agent without image generation?

Yes. Install Local Agent, Ollama, and at least one compatible Ollama model.

### Can I use other image models?

Potentially, yes.
A model must be supported by the installed stable-diffusion.cpp build and have a manifest that correctly describes its recipe, component slots, and file paths.

### Can I use other language models?

Yes. Use any model supported by Ollama and available in your local Ollama installation.

### Where are conversations saved?

In `chat_data.json`, beside the application data location used by your build.

### Where are image-generation settings saved?

In `%APPDATA%\LocalAgent\settings.json`.

### Where are generated images saved?

In the active project's `generated_images` folder when a project is selected.

### Does image generation use a cloud service?

No. Image generation runs locally through stable-diffusion.cpp.

---

## Links

- [Local Agent Releases](https://github.com/Mommy-Of-Light/Local-ia/releases/latest)
- [Ollama](https://ollama.com/)
- [Download Ollama for Windows](https://ollama.com/download/windows)
- [Ollama Model Library](https://ollama.com/library)
- [stable-diffusion.cpp](https://github.com/leejet/stable-diffusion.cpp)
- [Stable Diffusion 1.5 FP16 checkpoint](https://huggingface.co/Comfy-Org/stable-diffusion-1.5/resolve/main/v1-5-pruned-emaonly-fp16.safetensors)

---

*Built for local AI-powered coding and image generation.*
