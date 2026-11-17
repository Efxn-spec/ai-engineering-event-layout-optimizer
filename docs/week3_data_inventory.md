# Data Inventory — EventLayout-RL (Week 3)

> ⚠️ **Synthetic practice dataset.** This project's real floor-plan images do not exist yet, so Week 3 uses the image files from the Topic 3 Dummy Dataset Pack. Results from this data do **not** show how well EventLayout-RL performs.

## Requirements → Data Needs

| Requirement ID | Needed information / feature | Candidate source | Why usable / constraint |
|---|---|---|---|
| **FR1 / NFR2:** read the floor-plan image into a correct grid map | Images with exact, readable colours (walls, pillars, exits, power points), plus a correct answer for each image | **Now:** dummy pack `03_images/` (6 PNG + `labels.csv`)<br>**Later:** self-made colour-coded floor plans + hand-made answer maps | The dummy images use pure colours, like the colour code, and `labels.csv` gives an answer per image. Limitation: they are shapes, not rooms. |
| **RAI2:** the user checks the map; mistakes must be traceable | Exact identity of each source file and the output it produced | Same source; SHA-256 fingerprints + lineage manifest | Lets any wrong map be traced back to the exact input file. |

## Data Inventory

| Source ID | Filename / URL | Type and purpose | Version / date / owner | Access / rights | Inspection evidence |
|---|---|---|---|---|---|
| S01 | `topic3_dummy_datasets/03_images/shape_01.png` … `shape_06.png` | Image (PNG). Practice stand-in for colour-coded floor-plan images (FR1, NFR2). | Topic 3 Dummy Dataset Pack (files dated 2 Nov 2026), downloaded 17 Nov 2026; course lecturer | Synthetic teaching material from the course portal; no personal data. Prepared copies are in this repo. | 6 images; all open; RGB PNG. Sizes differ: 92×97 to 152×132 pixels. Each has exactly 2 pure colours: white plus red `(255,0,0)` or blue `(0,0,255)`. |
| S02 | `topic3_dummy_datasets/03_images/labels.csv` | Table (CSV). The "answer" for each image; stand-in for hand-made answer maps (NFR2). | Same pack | Same as S01 | 6 rows; columns `filename`, `shape_label`, `colour`. Every image has exactly one label; no label without an image. |

## Planned Project Data (replaces the practice data)

| Source | Type and purpose | Access / rights | Notes |
|---|---|---|---|
| Self-made colour-coded floor plans | Images (PNG) for training and testing (FR1, FR3, NFR2, NFR3) | Own work; can be shared | Black = walls, red = pillars, green = exits, blue = power points, white = floor. Each gets a hand-made answer map. |
| CubiCasa5K: <https://github.com/CubiCasa/CubiCasa5k> | Real floor-plan images, to test the reader on real drawings | CC BY-NC 4.0 (non-commercial); **not** committed to this repo | Large download, used only for testing. The images are not colour-coded, so they need converting first. |
| Fire Safety Risk Assessment: Small and Medium Places of Assembly (UK Government) | PDF; source of the exit and aisle rules (FR4, RAI1) | Open Government Licence | Rule values are copied by hand into a rules settings file; the PDF is not processed by the AI. |

## Raw vs Prepared Storage

| Data | Location | In GitHub? |
|---|---|---|
| Raw (S01, S02) | Colab: `/content/week3_project/data/raw/topic3_dummy_datasets/03_images/` | No; the original ZIP stays on the course portal |
| Prepared (Part B) | `data/processed/images/`, `data/processed/image_manifest.csv` | Yes |
| Lineage (Part B) | `data/week3_manifest.json` | Yes |
| Pipeline run (Part C) | `data/pipeline_v1/images/` with `pipeline_manifest.json` | Yes |
