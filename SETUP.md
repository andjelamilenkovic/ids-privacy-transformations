# Setup Guide

This guide walks through setting up and running this project from scratch,
including fixes for common issues encountered when testing on a clean
Windows machine.

## Hardware Requirements

Your CPU **must support AVX/AVX2 instructions** — this is required by
TensorFlow and cannot be bypassed. Any standard laptop/desktop CPU from
2013 onward supports this.

**Not supported:** low-power CPUs such as Intel Atom, Celeron, or Pentium
"N"-series (e.g., N4000, N4020, N5000), commonly found in budget
laptops/Chromebooks. These will fail with a DLL/import error when loading
TensorFlow, with no available workaround.

**If your only available machine does not meet this requirement**, use
Google Colab instead — see "Alternative: Google Colab" at the end of this
guide.

## Step 1 — Download and unzip the project

1. On GitHub, click the green **`<> Code`** button → **Download ZIP**.
2. Extract the ZIP file (right-click → Extract All) to a folder of your choice.

## Step 2 — Install Python 3.11.x (exact version required)

This project was built and tested with **Python 3.11.9**. Newer versions
(3.12+) will cause installation errors, because some pinned packages in
the TensorFlow 2.15 dependency chain do not have prebuilt installers for
newer Python versions.

**Option A — via winget (Windows 10/11, recommended):**
1. Open Command Prompt (Start → type `cmd` → Enter).
2. Run: `winget install Python.Python.3.11`
3. Close and reopen Command Prompt after installation finishes.
4. Verify: `py -0` should list Python 3.11 among installed versions.

**Option B — manual download:**
1. Go to python.org/downloads, download Python 3.11.9.
2. Run the installer. **Check the box "Add python.exe to PATH"** on the
   first screen before clicking Install.

## Step 3 — Open Command Prompt in the project folder

1. Open the extracted project folder in Windows Explorer.
2. Click the address bar at the top of the Explorer window (where the
   path is shown) to select the full path text. Copy it (Ctrl+C).
3. Open Command Prompt (Start → type `cmd` → Enter).
4. Type `cd ` (with a trailing space), then paste the path (Ctrl+V or
   right-click), then press Enter.
5. Verify you're in the right place: type `dir` — you should see
   `requirements.txt` in the listed files.

If you have multiple Python versions installed, use `py -3.11` instead of
`python` in all commands below to explicitly target the correct version
(e.g., `py -3.11 -m pip install ...`).

## Step 4 — Install required packages

**First, upgrade pip** (older pip versions fail on some packages with very
long file paths, a common issue on Windows):
```
python -m pip install --upgrade pip
```

Then install the project dependencies (~130 packages, including
TensorFlow, pandas, and Jupyter — this can take 5–15 minutes):
```
python -m pip install -r requirements.txt
```

**If this fails with an error mentioning "Windows Long Path support"**,
running the pip upgrade above first (before installing requirements)
usually resolves it. If it still fails and you have administrator access,
open Command Prompt **as Administrator** and run:
```
reg add "HKLM\SYSTEM\CurrentControlSet\Control\FileSystem" /v LongPathsEnabled /t REG_DWORD /d 1 /f
```
then restart your computer and try again.

**If TensorFlow later fails to import with a "DLL load failed" error**,
install the Microsoft Visual C++ Redistributable (x64):
https://aka.ms/vs/17/release/vc_redist.x64.exe — then restart your
computer. If the error persists, your CPU likely lacks AVX support (see
Hardware Requirements above) — use Google Colab instead.

## Step 5 — Download the data

1. Open `data/README.md` in this repo for the Zenodo links.
2. Download `trainset.parquet` and `testset.parquet` (CICIDS2017), and
   `trainset_2018.parquet` and `testset_2018.parquet` (CIC-IDS-2018).
3. Place all four files in the `data/` folder of this project.

## Step 6 — Start Jupyter

```
python -m jupyter notebook
```

The command window will print a URL containing a `token=...` value and
will appear to "hang" — **this is normal**: it means the server is
running in the background. Do not close this window while working in the
notebook.

A browser tab should open automatically. **If it doesn't**, copy the full
URL printed in the command window (starting with `http://localhost:8888/...`)
and paste it manually into your browser.

## Step 7 — Fix file paths before running a notebook

Before running either notebook, two things need to match **your** folder
layout (the original paths are set for the authors' own machine):

1. **`PATH` and `MODEL_PATH` variables** — in the first code cell of each
   notebook, these must point to your local `data/` and `models/` folders
   respectively. Any line that loads a model (`load_model(...)`) should
   use `MODEL_PATH`, not `PATH`.
2. **Results file paths** — any line reading a file from the `results/`
   folder (e.g., `pd.read_csv('multiseed_summary.csv')`) needs the folder
   included in the path (e.g., `pd.read_csv('results/multiseed_summary.csv')`),
   unless you are running the notebook from inside the `results/` folder
   itself.

This applies to **both** `ids_complete_experiments.ipynb` and
`ids_retraining_privacy_metrics.ipynb`.

## Step 8 — Run the notebook

1. In the Jupyter file browser, open the desired notebook from `notebooks/`.
2. Menu: **Kernel → Restart & Run All**.
3. Full execution takes approximately 20–30 minutes per notebook.

## Alternative: Google Colab (for computers without AVX support)

1. Go to colab.research.google.com (requires a free Google account).
2. File → Upload notebook → select the desired `.ipynb` file from `notebooks/`.
3. TensorFlow, pandas, and scikit-learn are pre-installed in Colab — no
   `pip install` step is needed.
4. In the left sidebar, click the folder icon, then the upload icon, and
   upload the parquet data files and the contents of `models/` directly
   into the Colab session (they will appear under `/content/`). Note:
   these files are deleted when the session ends and must be re-uploaded
   each time you reopen the notebook.
5. In the first code cell, set `PATH` and `MODEL_PATH` to `/content/`.
6. Menu: **Runtime → Run all**.
