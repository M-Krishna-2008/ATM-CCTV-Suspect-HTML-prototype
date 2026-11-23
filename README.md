# ATM-FaceTrace | Forensic Video Analytics Prototype

**ATM-FaceTrace** is an offline, air-gapped forensic video analytics prototype designed for law enforcement and bank fraud investigators. It addresses the critical challenge of identifying the clearest facial frames of an individual linked to a specific ATM transaction (e.g., fraudulent cash-out or card skimming) and matching those faces across surveillance archives with strict legal chain-of-custody integrity.

---

## 1. How to Open & Run

- **Zero Installation / No Build Step:** Fully self-contained in a single file: `atm-facetrace-prototype.html`.
- **Offline Capable:** Double-click `atm-facetrace-prototype.html` in any modern desktop browser (Chrome, Edge, Firefox, Safari).
- **No Remote Dependencies:** Uses system font stacks, inline SVGs, Canvas procedural graphics, and the standard Web Crypto API (`crypto.subtle`). No external CDNs, fonts, or images are required.

---

## 2. 6-Step Demo Script (~3 Minutes)

Follow this end-to-end walkthrough for an interactive presentation or hackathon jury evaluation:

### Step 1: Ingest & Integrity Verification (`#/upload`)
1. Notice the pre-filled case reference (`FIR 214/2026 - ATM cash-out`) and the legal advisory banner: *"Candidate matches require human verification. This tool does not identify persons."*
2. Click **"Load Demo Clip (.dav Unsupported)"** to demonstrate format validation handling for proprietary raw surveillance codecs (with step-by-step conversion guidance).
3. Click **"Load Demo Clip (Valid MP4/MKV)"** to inspect the automatic SHA-256 custody hash generation (`9f2c...a41e`) and CFR normalization report.
4. Click **"Continue to Transaction Setup ›"**.

### Step 2: Spatial & Temporal Calibration (`#/setup`)
1. On the ATM lobby scene canvas, observe the default Keypad and Cash Dispense polygons.
2. Click **"Draw Keypad Zone"** or click and drag any vertex point to customize the interactive boundary.
3. Click **"Auto-suggest Zones"** to see intelligent cluster snapping based on person dwell heatmaps.
4. Scrub the time slider under the canvas to inspect synchronized OSD timestamps. Notice the live-computed **Analysis Window** (`14:30:08 to 14:33:08`) responding to pre/post window inputs.
5. Click **"Start 7-Stage Forensic Analysis ›"**.

### Step 3: Multi-Stage Tracking & Failure Recovery (`#/processing`)
1. Watch the 7-stage pipeline animate with live stage percentages and telemetry tracking boxes for bodies **T1**, **T2**, and **T3**.
2. Toggle **"Simulate Failure Mode"** to show real-world resilient exception handling (OCR timestamp glare occlusion) and click **"Retry Stage With Manual Offset"** to resume without losing state.
3. Or click **"Fast-Forward to Results ›"**.

### Step 4: Track Correlation, Q-Scores & Enhancement (`#/results`)
1. Examine the temporal timeline strip and the top candidate person cards (**T1: 0.86 Link**, **T2: 0.31**, **T3: 0.12**). Click **"View Link Score Breakdown"** to view weighted factors (Dwell 0.40, Proximity 0.20, Time Fit 0.20, Keypad hands 0.10, Face visibility 0.10).
2. Browse the 5 best face frames. Notice the dynamically computed **Reliability Badges** (`HIGH`, `MEDIUM`, `LOW`) and flag chips (`CAP`, `LOW_LIGHT`, `PROFILE_POSE`, `MASK`). Hover over badges to see plain-language forensic reasons.
3. Click face tiles to step through sub-scores in the drawer ($Q = 100 \times [0.25R + 0.25S + 0.20P + 0.15V + 0.10L + 0.05C]$).
4. Click **"Toggle Side-by-Side"** or press <kbd>E</kbd> to inspect the enhanced visual with its mandatory diagonal watermark: `ENHANCED - NOT FOR MATCHING`.
5. Click **"Search Across Clips with This Face ›"**.

### Step 5: Candidate Match Verification & Evidence Dossier (`#/search`)
1. Review the ranked candidate matches retrieved from archive NVR clips. Observe confidence bands and the automatic band-drop rule when low-clarity faces are compared.
2. Click **"Confirm"** (or press <kbd>C</kbd>) on Candidate 1 and **"Reject"** (or press <kbd>R</kbd>) on Candidate 3. Notice the immediate status update and audit log recording.
3. Click **"Generate Evidence Dossier"** to open the court-ready printable summary (equipped with Section 65B Indian Evidence Act certification, side-by-side crops, verification QR pattern, and evidence SHA-256 hash). Click **"Print / Save as PDF"** to test print formatting.

### Step 6: Immutable Hash Chain Verification & Tamper Detection (`#/audit`)
1. Switch to the **Auditor** role using the top right role dropdown. Notice that the sidebar automatically restricts access strictly to the Audit Log.
2. Click **"🛡 Verify Hash Chain"**. The Web Crypto API computes all SHA-256 blocks from the genesis seed and validates the chain as **INTACT & VALIDATED**.
3. Switch to **Admin** role and click **"⚠️ Tamper Demo"** to simulate an unauthorized database record mutation.
4. Click **"🛡 Verify Hash Chain"** again. The ledger immediately flags: **"TAMPER DETECTED: BROKEN AT ENTRY #3"** in red.
5. Click **"Repair Demo"** and **"Export CSV"** to demonstrate downloadable compliance records.

---

## 3. Mocked Prototype vs. Production Implementation

| Feature Area | Front-End Prototype (Current) | Production Architecture (Target Stack) |
|---|---|---|
| **Video Decoding & Probing** | Simulated metadata cards & Canvas rendering | `FFmpeg` C-bindings (`libavcodec`/`libavformat`) with hardware acceleration (`NVDEC`/`VAAPI`) |
| **Clock Synchronization** | Interactive scrub slider + simulated OCR reading | `Tesseract OCR` / custom CRNN reading digital CCTV OSD timestamp overlay |
| **Person Tracking** | Canvas vector animation with 3 tracks (T1, T2, T3) | `YOLO11m` detector + `BoT-SORT` tracker with camera-motion compensation |
| **Transaction Person Linkage** | Multi-factor weights formula applied to mock zone bounds | 3D ray-casting of person interaction box with ATM keypad and cash shutter zones |
| **Face Detection & Alignment** | Stylized SVG avatar silhouettes with accessories | `SCRFD-10G` sub-pixel face detector + 5-point facial landmark affine alignment |
| **Quality Scoring (Q-Score)** | Dynamic JS formula calculating 6 sub-metrics (0-100) | `BiSeNet` face parsing (occlusion) + `SigLIP`/`SER-FIQ` biometric quality assessment |
| **Embedding & Matching** | Simulated cosine similarity scores (0.52 - 0.71) | `AdaFace` / `ArcFace` 512-dimensional normalized embeddings indexed in `FAISS` HNSW flat index |
| **Visual Enhancement** | Stylized sharpened SVG avatar with watermarking | `GFPGAN` / `CodeFormer` face restoration module (strictly decoupled from matching) |
| **Audit Ledger** | Real `crypto.subtle` SHA-256 hash chaining in browser memory | Append-only `SQLite` database with cryptographically sealed blocks & hardware HSM signing |

---

## 4. Keyboard Shortcuts

- <kbd>←</kbd> / <kbd>→</kbd> : Step between ranked candidate face frames
- <kbd>E</kbd> : Toggle side-by-side original vs. enhanced comparison
- <kbd>C</kbd> : Confirm candidate match in search view
- <kbd>R</kbd> : Reject candidate match in search view
