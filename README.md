# SeaTable Static Catalog Generator

Generate static HTML catalogs from SeaTable database views with images and metadata.

This also runs automatically every Monday at noon PST.

## Features

- Downloads images from SeaTable and stores them locally
- Generates responsive HTML catalog pages styled with the shared house
  stylesheet (colors, type, buttons) plus minimal page-specific CSS for
  the catalog grid
- Supports multiple catalogs from different views using config files
- Images shared across all catalogs (hash-based filenames prevent duplicates)
- Customizable headers and page titles per catalog

## Setup

1. Install dependencies:
```bash
pip install requests
```

2. Create a config file for your catalog (see examples below)

## Usage

**Structure to Generate a Catalog:**

```bash
python3 generate_catalog.py <config_file.json>
```

### These are the Catalogs json to run:
```bash
# Generate "Available Works" catalog
python3 generate_catalog.py config_produced_works.json

# Generate "Currently Showing" catalog
python3 generate_catalog.py config_currently_showing.json
```

### Update All Catalogs

```bash
python3 generate_catalog.py config_produced_works.json
python3 generate_catalog.py config_currently_showing.json
git add catalog.html showing.html images/
git commit -m "Update catalogs"
git push
```

## Configuration Files for Catalogs

Each catalog needs a JSON config file with these fields:

```json
{
  "view_name": "SeaTable view name",
  "output_file": "output.html",
  "header_logo": "page-header-assets/logo.png",
  "header_title": "page-header-assets/title-image.png",
  "page_title": "HTML page title",
  "include_purchase_button": TRUE or FALSE
}
```

### Example: Available Works W/O Purchase Info button

**config_produced_works.json:**
```json
{
  "view_name": "Available or in Process",
  "output_file": "catalog.html",
  "header_logo": "page-header-assets/logo.png",
  "header_title": "page-header-assets/available-works.png",
  "page_title": "Available Works - John Woodruff",
  "include_purchase_button": false
}
```

### Example: Currently Showing with Purchase Info Button

**config_currently_showing.json:**
```json
{
  "view_name": "Currently Showing",
  "output_file": "showing.html",
  "header_logo": "page-header-assets/logo.png",
  "header_title": "page-header-assets/currently-showing-title.png",
  "page_title": "Currently Showing - John Woodruff",
  "include_purchase_button": true
}
```

## Output Structure

Everything lives at the repository root (this repo is dedicated to the catalog):

```
catalog/                       # repo root
├── catalog.html                # Available Works catalog
├── showing.html                # Currently Showing catalog
├── page-header-assets/         # Header images
│   ├── logo.png
│   ├── available-works.png
│   └── currently-showing.png
└── images/                     # Shared images (all catalogs)
    ├── a1b2c3d4e5f6.jpg
    ├── b2c3d4e5f6a7.jpg
    └── ...
```

## Fonts & Shared Styling

Every generated page includes two stylesheet links in the `<head>`, in this
exact order (required — the shared stylesheet depends on the Adobe Fonts
embed loading first):

```html
<link rel="stylesheet" href="https://use.typekit.net/fpc1twk.css">
<link rel="stylesheet" href="https://jofowood.github.io/shared/jofowo-github.css">
```

This is hardcoded into `generate_catalog.py`'s HTML template — no per-config
setup needed.

**What the shared stylesheet provides:** base type (Orator via Typekit),
color variables, the `.wrap` page container, `.btn` / `.btn.primary` button
styles, and the shared `.site-footer` / `.back-to-top` / `.copyright-line`
footer pattern. The generator uses all of these instead of duplicating them.

**What stays local to `generate_catalog.py`'s own `<style>` block:**
catalog-specific layout only — the responsive grid (`.catalog-grid`), the
image lockup header (`.catalog-header`), and artwork card internals
(`.artwork-card`, `.artwork-image`, `.artwork-meta`, etc.), per the shared
stylesheet's own convention of keeping page-specific layout out of the
house CSS.

If the shared stylesheet (`jofowo-github.css`) changes — new button
variants, color updates, etc. — those propagate automatically to every
generated page without touching this repo.

## Image Management

- **Hash-based filenames**: Same image = same filename across all catalogs
- **Shared directory**: All catalogs use `images/` at the repo root
- **No duplicates**: Images downloaded once, reused everywhere
- **Clean unused images**: `rm -rf images/* && regenerate all catalogs`

## SeaTable Configuration

The script uses these constants (edit in `generate_catalog.py` if needed):
- **API Token**: Read-only token for SeaTable
- **Server**: https://cloud.seatable.io
- **Table**: "Works & Exhibits"

## Workflow

1. **Edit data** in SeaTable view
2. **Run generator** with appropriate config file
3. **Commit changes**: `git add catalog.html showing.html images/ && git commit -m "Update catalog" && git push`
4. **View live**: https://jofowood.github.io/catalog/[output-file].html

## Embedding in Website

```html
<iframe src="https://jofowood.github.io/catalog/catalog.html"
        width="100%" height="800" frameborder="0"
        style="border: none;">
</iframe>
```

## Adding New Catalogs

1. Create new config file (e.g., `config_new_series.json`)
2. Set the SeaTable view name
3. Choose output filename (root-level, e.g., `new-series.html`)
4. Specify header images
5. Run: `python3 generate_catalog.py config_new_series.json`

## Troubleshooting

**"Config file not found"**: Check the config file path
**"Missing required fields"**: Ensure all 5 required fields are in config
**Images not loading**: Run the generator to download images
**Authentication errors**: Verify API token in script
