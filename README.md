# Buffer Logger

Desktop GUI for managing lab buffer recipes. Designed for researchers who work with multi-component buffers, Buffer Logger organizes your entire buffer library in a three-tier hierarchy — stock solutions, working buffers, and compound mixes — and handles all the concentration math for you.

## Key Features

- **Three-tier buffer system** — Stocks, simple (working) buffers, and compound buffers that reference other buffers as source components
- **Automatic final concentration calculation** — When building compound buffers from source buffers, final concentrations of every ingredient are computed in real time as you adjust volumes
- **Unit-aware math** — Handles molar (M → pM), percent (%), and mass-based (mg/mL, µg/mL) concentration units, plus volume units from L to µL
- **Auto-fill mode** — Select one source buffer to act as the fill-up component; its volume adjusts automatically so all sources sum to the total volume
- **Volume calculator** — Scale any compound buffer recipe to a different target volume with one click
- **Image export** — Export selected buffer recipes as publication-ready PNG or TIFF images (300 DPI) for lab notebooks or presentations
- **Category management** — Organize buffers within each tier using custom categories for quick navigation
- **Dependency-safe deletion** — Prevents deleting a buffer that is referenced by a compound buffer, showing you exactly which buffers depend on it
- **Persistent storage** — All buffers are saved to a local `buffers.json` file automatically
- **Built-in defaults** — Ships with a set of common lab buffers (BRB, antifading, tubulin mixes, MAP buffers, and more) so you can start working right away

## Requirements

- **Python 3.8+**
- **tkinter** (included with standard Python installations)
- **Pillow** (optional) — required only for the image export feature. Install with `pip install Pillow`

## Installation & Running

1. Download or clone this repository
2. Run the application:
   ```
   python buffer_logger.py
   ```
3. (Optional) Install Pillow for image export:
   ```
   pip install Pillow
   ```

No other dependencies are needed. All buffer data is stored in `buffers.json` in the same folder as the script.

---

## User Manual

### Overview

The main window is divided into four panels:

| Panel | Description |
|-------|-------------|
| **Stock Solutions** (blue) | Pure reagents and concentrated buffer stocks (e.g., BSA 10%, 20× BRB) |
| **Simple Buffers** (green) | Working-concentration buffers defined by their component list (e.g., 1× BRB, Antifading) |
| **Compound Buffers** (orange) | Mixes assembled from other buffers by volume — the app calculates all final concentrations automatically |
| **Detail Panel** (right) | Shows and edits the selected buffer's full recipe |

The toolbar at the top provides access to the **Volume Calculator** and **Export Image** features.

---

### Buffer Types Explained

#### Stock Solutions
Stocks are the simplest tier. Each stock is defined by one or more ingredients, each with a name, concentration, and unit. Use these for pure reagents (e.g., "GTP 0.1 M") or concentrated multi-component buffers (e.g., "20× BRB" with PIPES, EGTA, and MgCl₂).

#### Simple Buffers
Simple buffers have the same structure as stocks — a list of ingredients at their working concentrations. The distinction is organizational: simple buffers represent the actual working solutions you use at the bench (e.g., "1× BRB" at 80 mM PIPES).

#### Compound Buffers
Compound buffers are where Buffer Logger shines. Instead of listing ingredients directly, you specify **which buffers** you're pipetting and **how much** of each. The app then:
1. Traces through all source buffers (even nested compound buffers)
2. Calculates the dilution factor for each source
3. Computes the final concentration of every ingredient in the mix
4. Displays results in real time as you edit volumes

This mirrors how buffer preparation actually works in the lab — you don't weigh out each chemical; you pipette from existing stocks and buffers.

---

### Creating a New Buffer

1. Click the **+** button at the bottom of the appropriate tier column (Stock, Simple, or Compound)
2. A new buffer is created with a default name
3. Edit the name in the title bar of the detail panel
4. Optionally set a **pH** value and assign a **category**
5. Add components or source buffers (see below)
6. Click **Save**

---

### Editing Stock and Simple Buffers

When you select a stock or simple buffer, the detail panel shows:

- **Name** — editable in the colored title bar
- **pH** — optional pH value
- **Category** — optional category for organization
- **Components table** — each row has:
  - **Ingredient** name (free text)
  - **Concentration** value
  - **Unit** dropdown (M, mM, µM, nM, pM, %, mg/mL, µg/mL)
  - **✕** button to remove the row

Click **+ Add Component** to add more ingredients. Click **Save** when done.

---

### Editing Compound Buffers

Compound buffers have a split-view detail panel:

#### Left side — Source Buffers
- **Total Volume** — the final volume of the mix (with unit selector)
- **Source buffer rows** — each row has:
  - **Fill radio button** — select one buffer to be the auto-fill component
  - **Buffer** dropdown — choose any existing buffer (stock, simple, or compound)
  - **Volume** — how much of that buffer to add
  - **Unit** dropdown (L, mL, µL)
  - **✕** button to remove the row
- **+ Add Source** to add another buffer to the mix
- **H₂O to fill** indicator — shows how much water is needed, or warns if source volumes exceed the total

#### Right side — Final Concentrations
A live-updating table showing every ingredient and its calculated final concentration in the mix. This updates automatically whenever you change any volume, add or remove a source, or change the total volume.

---

### Auto-Fill Mode

Auto-fill is one of Buffer Logger's most useful features for compound buffers:

1. Select a compound buffer
2. Click the **radio button** next to one of the source buffer rows
3. That buffer is now the "fill" component — its volume automatically adjusts so that all source volumes add up to the total volume
4. As you change other volumes, the fill buffer volume recalculates instantly
5. Click the radio button again (or select index -1) to disable auto-fill

This is ideal when one component (like a base buffer) acts as the diluent — you set the volumes of your specific reagents, and the fill buffer makes up the difference.

---

### Volume Calculator

The Volume Calculator lets you scale any compound buffer recipe to a different target volume:

1. Open it from the **Volume Calculator** button in the toolbar, or from the **Vol. Calc** button in a compound buffer's detail panel
2. Select a compound buffer from the dropdown
3. Enter the desired **target volume** and unit
4. Click **Calculate**

The calculator shows:
- **Recipe** — each source buffer with its scaled volume
- **Total** — including water if needed
- **Final Concentrations** — all ingredient concentrations at the new volume

This is useful when you need to prepare a larger or smaller batch than the recipe's default volume.

---

### Category Management

Categories help you organize buffers within each tier:

1. Click the **☰** button at the bottom-right of any tier column
2. In the category dialog, you can:
   - **+ Add** a new category
   - **Rename** an existing category
   - **Delete** a category (buffers become uncategorized, not deleted)
3. Assign a buffer to a category using the **Category** dropdown in the detail panel
4. Categorized buffers appear grouped under their category header in the list

---

### Renaming a Buffer

1. Select the buffer
2. Edit the name directly in the colored title bar of the detail panel
3. Click **Save**

If the buffer is referenced by any compound buffers, all references are updated automatically.

---

### Deleting a Buffer

1. Select the buffer in its tier column
2. Click the **✕** button at the bottom of that column
3. Confirm the deletion

If the buffer is used as a source in any compound buffer, deletion is blocked. A message will tell you which compound buffers depend on it. Remove those references first, then delete.

---

### Image Export

Export your buffer recipes as high-resolution images for lab notebooks, presentations, or documentation:

1. Click **Export Image** in the toolbar
2. Check the buffers you want to include (use **Select All** / **Select None** for convenience)
3. Choose what to include:
   - **Volumes / Recipe** — source buffers and their volumes (compound buffers only)
   - **Final Concentrations** — calculated ingredient concentrations
4. Click **Export as Image**
5. Choose a save location and format (PNG or TIFF)

The exported image uses a clean two-column layout at 300 DPI, color-coded by buffer tier. Stock/simple buffers show their component list; compound buffers show their recipe and/or final concentrations depending on your selection.

> **Note:** Image export requires the Pillow library. Install it with `pip install Pillow`.

---

### Data Storage

All buffer data is stored in `buffers.json` in the same directory as the script. The file is updated automatically every time you save, add, rename, or delete a buffer.

- **Backup:** Copy `buffers.json` to keep a backup of your buffer library
- **Transfer:** Copy the file to another computer to transfer your buffers
- **Reset:** Delete `buffers.json` and restart the app to restore the built-in default buffers

---

### Supported Units

| Type | Units |
|------|-------|
| Concentration (molar) | M, mM, µM, nM, pM |
| Concentration (percent) | % |
| Concentration (mass) | mg/mL, µg/mL |
| Volume | L, mL, µL |

The app automatically converts between units of the same type when calculating final concentrations and chooses the most readable unit for display (e.g., 0.001 M is displayed as 1 mM).

---

## Default Buffers

Buffer Logger ships with a curated set of buffers commonly used in cytoskeleton and in vitro reconstitution research:

- **Stocks:** BSA 10%, Methylcellulose 10%, GTP 0.1 M, DTT 0.9 M, GMPCPP 0.5 mM, Neutravidin 1 mg/mL, Glucose 150 mg/mL, Glucose Oxidase, Catalase, Tubulin stocks, MAP stock, 20× BRB, 10× MAP Buffer, 15× HKEM
- **Simple Buffers:** 1× BRB, BRB/BSA, BRB/MAP, BRB/MC, Neutravidin 50 µg/mL, Antifading, GTP 10 mM
- **Compound Buffers:** Polymerization mix, Capping mix, Incorporation mix, Wash Buffer

These can be modified or deleted. Delete `buffers.json` to restore them at any time.

## License

GPL-3.0
