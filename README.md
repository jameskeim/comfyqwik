# comfyqwik: Lazy Person's ComfyUI Setup Scripts

*Because time spent waiting for downloads is time you could be generating images of cats wearing space helmets.*

## What's This?

A collection of scripts to make setting up and managing ComfyUI on cloud GPU servers easier, faster, and less painful. Born from the desire to minimize the time spent paying for expensive GPU instances while staring at progress bars.

## Key Design Philosophy

These scripts follow a "separation of concerns" design to make debugging easier:

1. **Installation** (`comfyqwik`): Sets up ComfyUI and custom nodes
2. **Model Management** (`install-models` and `clean-models`): Handles the downloading and cleanup of models
3. **Model Registry** (`model_downloads.txt`): Central list of what models to download

This separation lets you run the sometimes lengthy model downloads in the background while you work, and makes it easier to debug any issues with specific model downloads.

## The Scripts

### 1. `comfyqwik`

The main setup script that does ComfyUI installation in parallel because waiting is for chumps:

- Clones ComfyUI
- Sets up Python venv with all requirements
- Installs PyTorch with CUDA (offers choice between 12.1 and 12.6)
- Installs essential custom nodes in parallel:
  - ComfyUI Manager
  - ControlNet Preprocessors
  - Impact Pack
  - Advanced CLIP Text Encode
  - ReActor (face swap and restoration)
  - IP-Adapter Plus
  - ComfyUI Essentials
  - InstantID
  - WAS Node Suite
- Creates the necessary model directories
- Does it all simultaneously to save your precious time and money
- **No longer downloads models** - this is now a separate step (see `install-models`)

```bash
# Just run it
./comfyqwik

# Or specify a custom install location
./comfyqwik /path/to/install/dir
```

### 2. `install-models`

The essential script for downloading all needed models (required after installation):

- Downloads all models listed in `model_downloads.txt`
- Uses parallel downloads optimized for your CPU count
- Special handling for IPAdapter and ReActor models
- Includes face restoration, swapping, and InstantID models
- Shows a clean progress indicator without terminal spam
- Works with existing ComfyUI installations

```bash
# Run in your ComfyUI directory
./install-models

# Or specify a custom ComfyUI location
./install-models /path/to/comfyui
```

### 3. `clean-models`

For when your cloud provider charges by the GB and you're not made of money:

- Removes all model files but keeps the directory structure
- Preserves your ComfyUI installation and custom nodes
- Reports space saved and number of files removed
- Creates placeholder files to maintain directory structure
- Makes your storage costs more manageable

```bash
# Run in your ComfyUI directory
./clean-models

# Or specify a custom ComfyUI location
./clean-models /path/to/comfyui
```

### 4. `model_downloads.txt`

The shopping list of models to download:

- Easy to edit format: `URL|destination_folder`
- Organized by model type (SDXL, ControlNet, VAE, etc.)
- Includes latest IP-Adapter models (standard, Plus, FaceID)
- Comprehensive set of ControlNet and T2I Adapter models
- Just add or comment out lines to customize your setup

## Typical Workflow

For the truly work-efficient (aka lazy) cloud user:

1. Start your expensive GPU instance
2. Run `comfyqwik` to set up ComfyUI and custom nodes lightning fast
3. Once you see the ComfyUI installation is complete, start a screen session for the model downloads:
   ```bash
   screen -S models
   ./install-models
   # Press Ctrl+A, D to detach from screen and let downloads continue in background
   ```
4. ????????????????????
5. Check on download progress when needed: `screen -r models`
6. Run `clean-models` before shutting down to save on storage costs
7. Next time, just run `install-models` to get your models back

> **Note:** Model downloads are intentionally separated from the main installation process to provide better visibility and easier debugging of any download issues.

## Requirements

- Bash shell
- Git
- Git LFS (for ReActor models)
- Python 3.8+
- wget
- A desire to save time and money

## Advanced Features

- **ReActor Integration**: Automatically downloads and installs specialized face swapping and restoration models
- **IP-Adapter Special Handling**: Renames models for unified loader compatibility
- **Optimized Parallel Processing**: Automatically scales to use 75% of available CPU cores for downloads
- **Extended Disk Space Management**: Placeholder files ensure proper directory structure preservation

## Why These Scripts Exist

Because manually typing the same commands over and over or watching progress bars crawl by on a system charging you by the minute is a special kind of torture. These scripts let you be productively lazy - the best kind of lazy.

Happy generating!
