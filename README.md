# comfyqwik: Lazy Person's ComfyUI Setup Scripts

*Because being a digital nomad doesn't mean doing it the hard way.*

## What's This?

A collection of scripts to make setting up and managing ComfyUI on cloud GPU servers easier, faster, and less painful. Born from the desire to minimize the time spent paying for expensive GPU instances while staring at progress bars.

## Key Design Philosophy

These scripts follow a "separation of concerns" design to make debugging easier:

1. **Installation** (`comfyqwik`): Sets up ComfyUI and custom nodes
2. **Model Management** (`install-models` and `clean-models`): Handles the downloading and cleanup of models
3. **Model Registry** (`model_downloads.txt`): Central list of what models to download
4. **ReActor Installation** (`reactorinstall`): Specifically manages ReActor face swap models

This separation lets you run the sometimes lengthy model downloads in the background while you work, and makes it easier to debug any issues with specific model downloads.

## Script Locations

It's important to place these scripts in the correct locations:

- `comfyqwik` - Run from the parent directory where you want ComfyUI installed (it will create the ComfyUI directory)
- All other scripts (`install-models`, `clean-models`, `reactorinstall`, `model_downloads.txt`) - These should be placed inside the ComfyUI directory after installation

Alternatively, you can keep all scripts in one location and specify the ComfyUI path as an argument when running the scripts.

## The Scripts

### 1. `comfyqwik`

The main setup script that does ComfyUI installation in parallel because waiting is for chumps:

- Clones ComfyUI into the current directory or a specified path
- Sets up Python venv with all requirements
- Installs PyTorch with CUDA (offers choice between 12.1 and 12.6)
- Installs essential custom nodes in parallel:
  - ComfyUI Manager (the app store for ComfyUI)
  - ControlNet Preprocessors (for image conditioning)
  - Impact Pack (workflow enhancement powerhouse)
  - Advanced CLIP Text Encode (better text prompting)
  - ReActor (face swap and restoration)
  - IP-Adapter Plus (enhanced image prompting)
  - ComfyUI Essentials (quality of life improvements)
  - InstantID (face preservation magic)
  - WAS Node Suite (utility Swiss Army knife)
- Sets up starting scripts and desktop shortcuts
- Creates the necessary model directories
- Does it all simultaneously to save your precious time and money
- **No longer downloads models** - this is now a separate step (see `install-models`)

```bash
# Run from where you want ComfyUI installed (creates a ComfyUI directory)
./comfyqwik

# Or specify a custom install location
./comfyqwik /path/to/install/dir
```

This script creates a self-contained ComfyUI base installation with some essential custom nodes. It means you don't have to restart the server as often using ComfyUI Manager. You have the option of keeping this installation installed. I've noticed that installation of each of the python libraries seems to take the longest. Model downloading has been split into two separate scripts. This provides a little more control over what you want to install and the option to delete all the models at the end of a session. 

### 2. `install-models`

The essential script for downloading all needed models (required after installation):

- Downloads all models listed in `model_downloads.txt`
- Uses parallel downloads optimized for your CPU count (75% of cores)
- Special handling for IPAdapter models with proper renaming for compatibility
- Shows clean success/failure reporting for each download
- Creates all required model directories if they don't exist
- Works with existing ComfyUI installations
- Has a test mode for checking configuration

```bash
# Run from inside your ComfyUI directory
./install-models

# Or specify a custom ComfyUI location
./install-models /path/to/comfyui

# Test configuration without installing
./install-models --test
```

### 3. `clean-models`

For when your cloud provider charges by the GB and you're not made of money:

- Removes all model files but keeps the directory structure
- Preserves your ComfyUI installation and custom nodes
- Reports space saved and number of files removed
- Creates placeholder files to maintain directory structure
- Makes your storage costs more manageable

```bash
# Run from inside your ComfyUI directory
./clean-models

# Or specify a custom ComfyUI location
./clean-models /path/to/comfyui
```

### 4. `reactorinstall`

Dedicated script for installing ReActor face swapping/restoration models:

- Downloads the specialized face swap models repository
- Copies models to correct locations with proper structure
- Handles inswapper, reswapper, face restoration models
- Sets up face detection and SAM models
- Extracts buffalo_l model for face recognition
- Option to keep repository for reference

```bash
# Run from inside your ComfyUI directory
./reactorinstall

# Or specify a custom location and keep repo for reference
./reactorinstall /path/to/comfyui --keep-repo
```

### 5. `model_downloads.txt`

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
3. Once you see the ComfyUI installation is complete, enter the ComfyUI directory and start a screen session for the model downloads:
   ```bash
   screen -S models
   ./install-models
   # Press Ctrl+A, D to detach from screen and let downloads continue in background
   ```
4. Start another screen session for ComfyUI itself:
   ```bash
   screen -S comfyui
   cd ComfyUI
   ./start_comfyui.sh
   # Press Ctrl+A, D to detach and keep running
   ```
5. Generate amazing images while models download in the background
6. If you need ReActor face swap capabilities:
   ```bash
   screen -S reactor
   ./reactorinstall
   # Press Ctrl+A, D to detach
   ```
7. Run `clean-models` before shutting down to save on storage costs
8. Next time, just run `install-models` to get your models back

> **Note:** Model downloads are intentionally separated from the main installation process to provide better visibility and easier debugging of any download issues.

## Requirements

- Bash shell (Linux or macOS, or WSL on Windows)
- Git
- Python 3.8+
- wget
- NVIDIA GPU with CUDA support (for optimal performance)
- A desire to save time and money

## Quality of Life Improvements

- **Smart Parallelization**: All scripts use parallel processing where possible, calibrated to your CPU
- **Clean Progress Reporting**: No more terminal spam, just the results you need
- **ReActor Integration**: Separate dedicated installation for specialized face swapping
- **IP-Adapter Special Handling**: Renames models for unified loader compatibility
- **Convenient Status Updates**: Clear section headers show exactly what's happening
- **Graceful Error Handling**: Scripts try to recover from errors where possible

## Why These Scripts Exist

Because manually typing the same commands over and over or watching progress bars crawl by on a system charging you by the minute is a special kind of torture. These scripts let you be productively lazy - the best kind of lazy.

Happy generating!

## Important Compatibility Notice

**These scripts have only been tested on Ubuntu systems running on Lambda Labs Cloud GPU servers.**

I make no promises that they will run on your system, other cloud GPU providers, or even all Lambda Labs servers. Use at your own risk.

If you successfully run these scripts anywhere, consider it a happy accident rather than an intended feature.

## Troubleshooting

### Common Issues

**Permission Denied Errors**
```
chmod +x comfyqwik install-models clean-models reactorinstall
```

**Model Download Failures**
If models fail to download, you can:
1. Run `install-models` again (it will skip already downloaded files)
2. Check your internet connection
3. Verify HuggingFace isn't down or rate-limiting you

**CUDA Not Found**
Ensure NVIDIA drivers are properly installed:
```
nvidia-smi
```

### Need Help?

If you encounter issues not covered here, please fork the repository and make your changes.

## License

These scripts are provided "as is" without any warranty. Use at your own risk.

## Acknowledgments

- The ComfyUI team for creating the amazing base software
- All custom node developers whose work these scripts help install
- The AI image generation community for inspiration
