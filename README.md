# anaconda_comfy

Small helper repo for recovering a broken ComfyUI install by rebuilding a clean conda environment.

## What this repo contains
- `RECOVERY_GUIDE.md`: step-by-step tutorial (clone ComfyUI, create conda env, restore custom-node deps).
- `environment.yml`: a reproducible conda environment definition (Python + PyTorch + CUDA + pip deps).

## How to use
1. Clone ComfyUI:
   ```bash
   git clone https://github.com/comfyanonymous/ComfyUI
   cd ComfyUI
   ```
2. Copy the files from this repo into the ComfyUI folder:
   - Copy `environment.yml` to the ComfyUI root
   - (Optional) Copy `RECOVERY_GUIDE.md` for reference
3. Create + activate the environment:
   ```bash
   conda env create -f environment.yml
   conda activate comfy
   ```
4. Restore node dependencies:
   ```bash
   pip install comfy-cli
   comfy node restore-dependencies
   ```

## The “nuclear option”
If something breaks after installing/updating custom nodes:
```bash
conda deactivate
conda env remove -n comfy -y
conda env create -f environment.yml
conda activate comfy
comfy node restore-dependencies
```

## Notes
- For best compatibility/performance with Triton and attention kernels, WSL2 (Ubuntu) is often more stable than Windows-native.
- Conda isolation makes recovery fast, but `pip` can still override versions inside an environment—treat each env as disposable and rebuild when needed.
