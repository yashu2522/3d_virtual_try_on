# 3D Virtual Try-On Pipeline

This workspace contains a combined virtual try-on system:

1. `VITON-HD-main/` generates the 2D try-on preview.
2. `threedmodel/` converts that 2D result into a 3D Gaussian / `.ply` asset.

The flow is:

- upload a person image
- select a garment
- run the VITON-HD 2D try-on stage
- send the generated image to TRELLIS
- save the 3D result as a `.ply` file

## Project Structure

- `VITON-HD-main/` - 2D virtual try-on using VITON-HD
- `threedmodel/` - TRELLIS 3D Gaussian backend
- `u2net/` - model files used by the try-on pipeline

## Requirements

- Python environment with the packages required by both projects
- NVIDIA GPU recommended for the 3D backend
- The VITON-HD assets, checkpoints, and dataset folders expected by the existing scripts

## Run Locally

Open two terminals.

### 3D backend

From `threedmodel/`:

```powershell
$env:ATTN_BACKEND = "xformers"
python api_server.py
```

The TRELLIS API starts on port `7860`.

### 2D try-on server

From `VITON-HD-main/`:

```powershell
python web_tryon_server.py
```

The web app starts on port `5000`.

## Local 3D Endpoint

Configure the 2D server to call the local 3D backend at:

```text
http://127.0.0.1:7860/generate-gaussian
```

## Output Locations

- 2D try-on images: `VITON-HD-main/results/<run_name>/`
- 3D model files: `VITON-HD-main/results/3d_models/`
