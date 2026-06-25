# ShutterTag

A Lightroom Classic plugin that uses AI vision models to generate metadata for your photos, and syncs that metadata across folder stacks or arbitrary photo selections.

---

## Features

### Generate Tags (AI Tagging)

Select one or more photos in the Library and run **Library → Plug-in Extras → Generate Tags…**

- Renders a full-quality JPEG export of each photo (800px, full Develop processing) and sends it to an AI vision model via [fal.ai](https://fal.ai) + OpenRouter
- Returns a **keyword list**, **title**, **caption**, and **alt text** for each photo
- Review all results before anything is written to your catalog
- Estimated cost per photo is shown in the review dialog

**Output options (set in Plug-in Manager):**

| Mode | Description |
|------|-------------|
| Lightroom catalog | Writes keywords, title, caption, and alt text directly to each photo |
| CSV file | Appends one row per photo to a file you specify (filepath, filename, title, caption, alt_text, keywords) |

**Per-run settings:**

| Setting | Options |
|---------|---------|
| Vision model | Any enabled model from your Plug-in Manager list |
| Style | Artistic, Commercial, Descriptive, Documentary, Minimal, Nature/Wildlife, Simple, Social Media, Technical, Travel |
| Title | Enable/disable; max length (characters or words); overwrite or append |
| Caption | Enable/disable; max length (characters or words); overwrite or append |
| Alt Text | Enable/disable; max length (characters); overwrite or append; written to LR's IPTC Accessibility field |
| Keywords | Enable/disable; max count; append or overwrite |
| Keyword hierarchy | Reuse from catalog (match against existing tree) or always create new |
| New tag placement | Create at root level, or nest under a named parent keyword |

Each field has a checkbox to include or exclude it from the run. When included, radio buttons select Append or Overwrite. Each field section also exposes a **Prompt** text box showing the exact instruction sent to the AI — generated automatically from the selected Style, but editable for full control. Changing Style or any limit regenerates the prompt for built-in styles; for custom styles your text is preserved.

**Save Style…** and **Delete Style** (in the Style group) let you store the current prompt texts and limits as a named style that appears alongside the built-ins. A **Restore Defaults** button resets all field settings to their plugin defaults.

**Style themes** shape how the AI describes your photos. Each theme injects style-specific language into the prompt while keeping your field settings (max length, field selection) intact:

| Theme | Best for |
|-------|----------|
| Artistic | Gallery, fine-art, mood-driven work |
| Commercial | Stock licensing — agency-ready, search-optimized keywords |
| Descriptive | General use; detailed scene and subject coverage |
| Documentary | Events, journalism — who/what/where/when captions |
| Minimal | Clean, high-level tags with minimal noise |
| Nature / Wildlife | Species names, habitat, behavior, scientific terms |
| Simple | Plain-language keywords for broad audiences |
| Social Media | Short punchy captions, hashtag-friendly keywords |
| Technical | Precise, factual — equipment, technique, specifications |
| Travel | Location-first, geographic and cultural keywords |

### Sync Stack Metadata

Select a **Folder** in the Library panel and run **Library → Plug-in Extras → Sync Stack Metadata…**

Copies metadata from the **top photo in each folder stack** to all child photos in that stack.

**Fields that can be synced:**

| Field | Notes |
|-------|-------|
| Keywords | Merge (add missing) or Overwrite; skip-list validated against your catalog |
| Title | Overwrites child title with stack top's title |
| Caption | Overwrites child caption |
| Alt Text | Copies IPTC Accessibility alt text field |
| Rating | Copies star rating |
| Capture Time | Copies capture date/time (catalog only; EXIF on disk is unchanged) |
| Color Label | Copies red/yellow/green/blue/purple label |
| GPS | Copy if child has no GPS, or always overwrite |
| Face Regions | Copies face bounding boxes via native XMP I/O (requires identical framing — see note below) |

**Skip keywords** — enter a comma-separated list of keyword names to exclude from sync. Click **Save** to normalize the list and validate each name against your catalog; unrecognized names are flagged inline so you can catch typos before the sync runs.

**Face region sync** copies XMP face region data (MWG-RS standard) from the stack top to each child photo using native Lua XMP I/O — no external tools required. Coordinates are automatically transformed between EXIF orientations so the bounding box lands on the correct physical position on each photo. This is only correct when child photos share the same composition and aspect ratio as the stack top (e.g. bracketed exposures or multi-exposure sets). A confirmation dialog explains this before the sync proceeds.

**Workflow:**

1. Navigate to a Folder in the Library panel
2. Run **Sync Stack Metadata…** and choose which fields to sync
3. Click **Preview Changes** to see a diff of every pending write before committing
4. Click **Confirm and Sync** to apply

The preview step is on by default and can be skipped with the "Sync immediately" checkbox for bulk operations.

### Sync Selected Photos Metadata

Select two or more photos in the Library filmstrip and run **Library → Plug-in Extras → Sync Selected Photos Metadata…**

Copies metadata from the **primary (highlighted) photo** to all other selected photos. If any selected photo is the top of a collapsed folder stack, all members of that stack are included automatically.

The dialog shows the source photo thumbnail, filename, and a preview of each field's current value so you can confirm what will be copied before choosing which fields to sync.

**Fields that can be synced:**

| Field | Notes |
|-------|-------|
| Keywords | Merge (add missing) or Overwrite |
| Title | Copies title from source |
| Caption | Copies caption from source |
| Alt Text | Copies IPTC Accessibility alt text field |
| Rating | Copies star rating |
| Color Label | Copies red/yellow/green/blue/purple label |
| GPS | Copy if target has no GPS, or always overwrite |
| Face Regions | Copies face bounding boxes via native XMP I/O (requires identical framing) |

**Workflow:**

1. Select the source photo and all target photos in the filmstrip (the primary/highlighted photo is the source)
2. Run **Sync Selected Photos Metadata…** and choose which fields to sync
3. Click **Preview Changes** to review every pending write, then **Confirm and Sync** to apply

Collapsed stacks in the selection are expanded automatically — selecting a stack top syncs all photos in that stack.

---

## Installation

1. Download or clone this repository.
2. In Lightroom Classic: **File → Plug-in Manager → Add** → select the `shuttertag.lrplugin` folder.
3. Open **Plug-in Manager → ShutterTag → fal.ai Settings**, paste your [fal.ai API key](https://fal.ai/dashboard), and click **Verify** to confirm it is accepted before running a job.
4. Under **Available Models**, check the models you want to use.

---

## Requirements

- **Lightroom Classic** (SDK 13.0 or later)
- A **fal.ai API key** — sign up at [fal.ai](https://fal.ai). AI tagging routes through OpenRouter so any vision-capable model listed there is available.
- Internet access for AI tagging (both sync features work entirely offline)

---

## AI Models and Cost

Models are listed in `models.csv` and shown with estimated cost-per-image in the Plug-in Manager. Enable or disable individual models; all enabled models appear in the Generate Tags dialog's model picker.

The review dialog shows the estimated cost for each photo and a total for the batch. Costs are reported by OpenRouter and are estimates — actual billing occurs through your fal.ai account.

For large batches (> 20 photos), a confirmation dialog appears before any processing begins.

---

## Logs

Logging is **off by default**. To enable it, open **Plug-in Manager → ShutterTag → Logging** and set the level to **Standard** (key events) or **Verbose** (full detail including API request/response).

Log file locations:
- Windows: `%APPDATA%\Adobe\Lightroom\Logs\ShutterTag.log`
- macOS: `~/Library/Application Support/Adobe/Lightroom/Logs/ShutterTag.log`

The log rotates automatically at 2 MB (one backup kept as `.log.1`). It covers AI tagging runs, sync operations, write errors, and wedge detection events. API keys and base64 image data are automatically redacted from every line.

---

## Known Limitations

- **Folder stacks only.** Sync Stack Metadata works with folder stacks (`stackPositionInFolder`). Collection stacks are not currently supported.
- **Lightroom keyword ID drift.** A Lightroom bug can cause keyword writes to fail after many `addKeyword` calls in one session. ShutterTag detects this ("catalog wedged") and tells you to restart Lightroom. If it happens repeatedly, use **Hold Option/Alt at launch → Test Integrity + Optimize Catalog**.
- **AI results vary.** The quality of generated titles, captions, and keywords depends on the model and the photo. Review results before saving.
- **Video files are skipped.** Generate Tags skips video files automatically and reports them in the log.
