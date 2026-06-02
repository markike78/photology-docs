# AI Conduit — Lightroom Classic Plugin

A Lightroom Classic plugin that sends selected photos to AI image editing models via [fal.ai](https://fal.ai) and imports the results directly back into your catalog.

## Features

- Edit one or more photos with a text prompt using state-of-the-art AI models
- **Compare Models** — run multiple models side-by-side on a single photo and choose which results to import
- **Model Options** — per-model sub-dialog for aspect ratio, output resolution, and advanced parameters (safety tolerance, seed, thinking level, web search, and more depending on the model)
- **Enhance Prompt** — one-click AI expansion of a short prompt into a detailed, technically precise instruction (uses GPT-4o mini via fal.ai; costs less than $0.0001 per use)
- Async queue-based processing with live status updates (queue position, processing, downloading)
- Batch processing with parallel API workers
- Pre-flight confirmation dialog showing estimated cost and account balance
- Results dialog with thumbnails, output filename, resolution, and processing time per photo
- **Prompt Catalog** — save, load, and remove named prompts; ships with 20 built-in starter prompts organised by category; manage the full catalog from **Manage Presets…**
- Dynamic model list driven by `models.csv` — enable or disable models in Plug-in Manager
- **Copy Metadata from source** — optionally copy metadata from the source photo to each output (both Edit with AI and Compare Models): Keywords, Title, Caption, Alt Text, Capture Time, GPS, Rating, Color Label, and Face Tags (face regions + person keywords)
- AI generation metadata written to each output photo (prompt, model, service, date) — visible in the Lightroom metadata panel under "All Plug-in Metadata"
- Output files saved alongside originals with `-Edit` suffix and imported above the source photo

## Supported Models

Models are managed in **Plug-in Manager → Available Models**. Default models are pre-checked; others can be enabled at any time.

| Model | Provider | Approx. Cost |
|---|---|---|
| FLUX.2 Dev Edit | Black Forest Labs | $0.012 |
| FLUX.2 Dev Turbo Edit | Black Forest Labs | $0.020 |
| FLUX.2 Pro Edit | Black Forest Labs | $0.030 |
| FLUX.1 Kontext Pro | Black Forest Labs | $0.040 |
| FLUX.1 Pro Fill | Black Forest Labs | $0.050 |
| Seedream 5.0 Lite Edit | ByteDance | $0.035 |
| Seedream 4.5 Edit | ByteDance | $0.040 |
| Gemini 3 Pro Image Preview | Google | $0.150 |
| Nano Banana 2 Edit | Google | $0.080 |
| Nano Banana Pro Edit | Google | $0.150 |
| GPT Image 1.5 Edit | OpenAI | $0.034 |
| GPT Image 2 Edit | OpenAI | $0.040 |
| Grok Imagine Edit | xAI | $0.022 |

Costs are approximate base prices per image at default settings. Resolution and quality options affect the final cost. See `ai-edit.lrplugin/models.csv` for the full evaluated list including models that did not pass testing.

## Requirements

- Adobe Lightroom Classic (SDK 15.3 / current release)
- A [fal.ai](https://fal.ai) account with an API key
- An Admin API key (optional — required to show account balance in the confirmation dialog)

## Installation

1. Download or clone this repository
2. In Lightroom Classic, open **Plug-in Manager** (File → Plug-in Manager)
3. Click **Add** and navigate to the `ai-edit.lrplugin` folder
4. Click **Done**

## Configuration

Open **Plug-in Manager → AI Conduit — fal.ai Settings** and enter:

- **API Key** — your fal.ai key from [fal.ai/dashboard](https://fal.ai/dashboard) → API Keys. Used for all model inference calls. Click **How to generate API keys ↗** in the panel for step-by-step instructions.
- **Admin API Key** — a separate fal.ai key with account/billing scope. Optional — only needed to display your current credit balance in the confirmation dialog.

Under **Available Models**, check the models you want available in the edit dialog. Click **Select All** to enable every model, or **Reset to Defaults** to restore the original selection.

## Usage

1. Select one or more photos in the Library module
2. Go to **Library → Plug-in Extras → Edit with AI…**
3. Choose a model from the dropdown
4. Optionally select a saved prompt from the **Preset** dropdown, or type a new prompt
5. Optionally click **Enhance Prompt** to have AI expand your prompt into a more detailed instruction
6. Set **LR Export Size** (resolution of the image sent to the model) and **Output Format**
7. Click **Model Options…** to configure aspect ratio, output resolution, and model-specific advanced settings (available options vary by model)
8. Under **Copy metadata from source**, check which fields to copy from the source photo to each output. Use **All** or **None** to toggle at once. **Face Tags** will ask for confirmation before writing face region XMP to disk.
9. Check **Skip Confirmations** in the dialog footer to bypass the cost/balance and face-tag confirmation dialogs for this run
10. Click **Run AI Edit**
11. Review the estimated cost and account balance in the confirmation dialog, then click **Continue**
12. Progress is shown while photos are processed. Click cancel in the progress bar to stop.
13. A results dialog shows each output photo with its filename, resolution, and processing time. Photos are imported into your catalog above their source photo.

## Compare Models

**Library → Plug-in Extras → Compare Models…** runs multiple models on a single photo in parallel so you can evaluate results side-by-side before committing.

### How to use

1. Select exactly one photo in the Library module
2. Go to **Library → Plug-in Extras → Compare Models…**
3. Select the models to run — selections are remembered between sessions
4. Optionally select a saved prompt from the **Preset** dropdown, or type a new prompt; click **Enhance Prompt** to expand it
5. Set **LR Export Size** and **Output Format**
6. Under **Copy metadata from source**, check which fields to copy to each imported result
7. Click **Run Comparison**
8. Confirm the total estimated cost in the dialog that follows
9. Progress is shown while all models run in parallel. Click cancel to stop.
10. A tiled results grid shows a thumbnail, model name, cost, and processing time for each result
11. Check **Import** under each thumbnail you want to keep (all successful results are pre-checked); use **Select all** / **Select none** to toggle
12. Click **Preview** under any thumbnail to view it full-size; navigate between results with Prev / Next
13. Click **Import Selected** to import the checked images above the source photo, or **Discard All** to delete everything

### Output files

Compare output files are named `{original}-{ModelName}.{ext}` (e.g. `IMG_0001-FLUX2-Dev-Edit.jpg`). Each imported file gets the same AI metadata fields as a standard Edit with AI result.

## Prompt Catalog

Both **Edit with AI** and **Compare Models** share a Prompt Catalog accessible from the **Preset** dropdown.

- **Load** — select a preset from the dropdown to fill the prompt box; if the preset has a preferred model and that model is enabled, the model selector updates automatically
- **Save as…** — click **Save as…**, enter a name in the dialog that appears, and click Save. If the name already exists you will be asked to confirm the overwrite. The current model is saved with the preset and will be auto-selected when the preset is loaded.
- **Delete** — select a preset and click **Delete** to remove it from the catalog
- **Manage Presets…** — opens a dedicated CRUD dialog (**Library → Plug-in Extras → Manage Presets…**) to rename presets, change their preferred model, or edit their text without running an edit

The plugin ships with the following built-in presets:

| Name | Category |
|---|---|
| Restore: Clean and enhance scanned photos | Restore |
| Restore: Cleanup scanned photo | Restore |
| Restore: Old photo damage | Restore |
| Restore: Colorize Black and White | Restore |
| Restore: Black and White portrait | Restore |
| Portrait: Smooth skin | Portrait |
| Portrait: Full retouch | Portrait |
| Portrait: Enhance eyes | Portrait |
| Portrait: Add background blur | Portrait |
| Portrait: Tame wild hair | Portrait |
| Landscape: Dramatic sky | Landscape |
| Sky: Replace with dramatic sunset | Sky |
| Sky: Replace with clear blue | Sky |
| Background: Replace with studio neutral | Background |
| Background: Replace with outdoor environment | Background |
| Product: Clean studio background | Product |
| Remove: Power lines & wires | Remove |
| Remove: Lens Flare | Remove |
| Style: Cinematic color grade | Style |
| Style: Film emulation — Kodak Portra | Style |

New presets added to the bundled list are injected automatically on next Lightroom startup without overwriting any user-created or user-modified entries.

## Output Files

Output files are saved in the same folder as the source photo with `-Edit` appended to the filename (e.g. `IMG_0001.jpg` → `IMG_0001-Edit.jpg`). If a file with that name already exists, a counter is appended (`-Edit-1`, `-Edit-2`, etc.).

## AI Metadata

Each output photo has the following fields written to it, visible in Lightroom's metadata panel when "All Plug-in Metadata" is selected:

| Field | Example |
|---|---|
| AI Prompt | `make the sky more dramatic` |
| AI Model | `FLUX.2 Pro Edit` |
| AI Service | `fal.ai` |
| AI Model ID | `fal-ai/flux-2-pro/edit` |
| AI Edit Date/Time | `2026-05-26T14:30:00Z` |

The date/time is stored in UTC ISO 8601 format.

## Data & Privacy

Input photos are sent to fal.ai as JPEG data. fal.ai stores request payloads for 30 days under their standard data retention policy. Output images on the fal.ai CDN are set to expire 5 minutes after generation. See [fal.ai's privacy policy](https://fal.ai/privacy) for full details.
