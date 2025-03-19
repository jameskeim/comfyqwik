# quickncomfy: Lazy Person's ComfyUI Setup Scripts

*Because time spent waiting for downloads is time you could be generating images of cats wearing space helmets.*

## What's This?

A collection of scripts to make setting up and managing ComfyUI on cloud GPU servers easier, faster, and less painful. Born from the desire to minimize the time spent paying for expensive GPU instances while staring at progress bars.

## The Scripts

### 1. `para-comfy-install`

The main setup script that does ComfyUI installation in parallel because waiting is for chumps:

- Clones ComfyUI
- Sets up Python venv with all requirements
- Installs PyTorch with CUDA
- Installs tons of useful custom nodes
- Creates the necessary model directories
- Does it all simultaneously to save your precious time and money

```bash
# Just run it
./para-comfy-install

# Or specify a custom install location
./para-comfy-install /path/to/install/dir
```

### 2. `install-models`

When you need to download just the models, perhaps after a storage-saving cleanup:

- Downloads all models listed in `model_downloads.txt`
- Uses parallel downloads optimized for your CPU count
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
- Reports space saved
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
- Just add or comment out lines to customize your setup

## Typical Workflow

For the truly work-efficient (aka lazy) cloud user:

1. Start your expensive GPU instance
2. Run `para-comfy-install` to set up ComfyUI and custom nodes lightning fast
3. Run `install-models` to download all models (can run while you work on something else)
4. Create amazing AI art
5. Run `clean-models` before shutting down to save on storage costs
6. Next time, just run `install-models` to get your models back

## Requirements

- Bash shell
- Git
- Python 3.8+
- wget
- A desire to save time and money

## Why These Scripts Exist

Because manually typing the same commands over and over or watching progress bars crawl by on a system charging you by the minute is a special kind of torture. These scripts let you be productively lazy - the best kind of lazy.

Happy generating!
