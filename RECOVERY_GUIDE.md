# How to solve EVERYTHING FOREVER! (Anaconda Edition)

### Tutorial: Fixing broken installations and custom node chaos

**tl;dr**
- Use **Anaconda/Miniconda** to create isolated sandboxes for your environment.
- Use the `environment.yml` file provided below to recreate your setup in seconds.
- Use **comfy-cli** to restore node dependencies automatically.
- Run on **Linux (or WSL2)** for maximum performance.

---

## Why?
Most ComfyUI "bricks" happen because a custom node updates a library (like `numpy` or `Pillow`) to a version that another node hates. By using Anaconda, you stop fixing broken installs and start replacing them. If it breaks, delete the environment and run one command to be back in business.

## The Tools
1. **Anaconda/Miniconda:** Manages Python versions and (on Linux/WSL) CUDA libraries.
2. **comfy-cli:** The official manager for ComfyUI.
3. **Linux/WSL2:** Better support for Triton and overall stability.

---

## Setup & Installation

### 1) Clone ComfyUI

```bash
git clone https://github.com/comfyanonymous/ComfyUI
cd ComfyUI
```

### 2) Create the environment
Instead of manual `pip install` commands, use the provided `environment.yml`. This ensures your Python version and PyTorch/CUDA versions are locked to a known-good combination.

```bash
# Create the environment from the file
conda env create -f environment.yml

# Activate it
conda activate comfy
```

### 3) Initialize comfy-cli
Once inside your environment, install and run the manager:

```bash
pip install comfy-cli

# From the ComfyUI root folder
comfy node restore-dependencies
```

---

## The "Nuclear Option" (If something breaks)
If a custom node bricks your installation, do not waste hours debugging. Purge and rebuild:

```bash
# 1. Leave the environment (if active)
conda deactivate

# 2. Kill the broken environment
conda env remove -n comfy -y

# 3. Recreate it instantly
conda env create -f environment.yml
conda activate comfy

# 4. Restore all custom node dependencies
comfy node restore-dependencies
```

---

## Performance tip: Linux & WSL2
Windows users often struggle with Triton (speed boosts) and some attention kernels. Recommendation: Install WSL2 (Ubuntu) from the Microsoft Store. Inside WSL2, follow the Linux setup. In practice, this is often faster and more stable for deep-learning libraries.

---

## Q&A
**Q: Why Anaconda over "Portable"?**
The Portable version is a black box. Anaconda is transparent—you can clone environments (`conda create --name backup --clone comfy`) before testing risky nodes.

**Q: Why does it break in the first place?**
Custom nodes sometimes force-install incompatible versions of core libraries. Conda can catch conflicts for conda-installed packages, but `pip` can still override things—so keeping installs isolated per-environment is the big win.

---

## Part 2: The environment file
Save the following as `environment.yml` in your **ComfyUI root folder**.

```yaml
name: comfy
channels:
  - pytorch
  - nvidia
  - conda-forge
  - defaults
dependencies:
  - python=3.11
  - pip
  - pytorch
  - torchvision
  - torchaudio
  - pytorch-cuda=12.1
  - numpy
  - git
  - pip:
      - -r requirements.txt
      - comfy-cli
```

### How to use these files together
1. Save this tutorial as `RECOVERY_GUIDE.md`.
2. Save the YAML block as `environment.yml` inside your ComfyUI folder.
3. Run `conda env create -f environment.yml`.

If you want, I can generate specialized YAML variants for **AMD (ROCm)** or **Mac (MPS)**.
