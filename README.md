# EEG Reward Learning Replication — Analysis Notebook
**Team PANDAAS**
**Abriti Chakraborty, Adarsh Panda, Anushree Rege**

---

## Paper
Hassall, C.D., et al. (2022). Probabilistic reward learning task — EEG dataset.
https://www.sciencedirect.com/science/article/pii/S1053811922005729?via%3Dihub

---

## Dataset
Oxford site · 12 subjects · BrainVision · 31 EEG channels (no mastoid, no dedicated EOG)

---

## Goal
Reproduce the Reward Positivity (RewP) — the differential ERP for win vs. loss feedback in the 240–340 ms post-feedback window — using a pipeline that deliberately differs from the original in filter settings, artifact threshold, and ROI definition, to test robustness.

---

## Setup

```bash
# 1. Clone the repo
git clone https://github.com/AnushreeRege/EEG_PANDAAS.git
cd EEG_PANDAAS

# 2. Install dependencies
pip install mne numpy pandas scipy matplotlib

# 3. Set your local data path
# Open the notebook and set BIDS_ROOT to where your sub-XX folders live

# 4. Run the notebook
jupyter lab
# Open EEG_PANDAAS.ipynb and run cells in order
```

---

## Notebook Structure

The notebook runs in five sequential stages:

| Stage | Description |
|-------|-------------|
| 1 | Library imports & configuration |
| 2 | Data loading & inspection |
| 3 | Data cleaning — filtering, ICA, referencing, epoching |
| 4 | Sanity checks on a representative subject |
| 5 | Group-level analysis & visualisation |

---

## Data Structure

```
./
└── sub-XX/
    └── eeg/
        ├── sub-XX_task-casinos_eeg.vhdr
        └── sub-XX_task-casinos_events.tsv
```

Set `BIDS_ROOT` at the top of Stage 1 to point to the folder containing your `sub-XX` directories. It defaults to the current working directory.

---

## Key Pipeline Decisions

- **Two-track filtering** — 0.1–30 Hz for ERP computation; 1–30 Hz separately for ICA to produce a flatter baseline and more stable component separation. Cleaned ICA weights are applied back onto the 0.1 Hz data.
- **Surrogate EOG channels** — No dedicated ocular electrodes were present in the dataset. Fp1, F7, and F8 are used as proxies for detecting blink and saccade components during ICA.
- **Average referencing** — No mastoid channels were available, making average referencing across all 31 scalp channels the only principled option. This is the most likely explanation for amplitude attenuation relative to the original paper.
- **Strict artifact rejection** — ±100 µV static threshold. More conservative than the original; trades trial count for signal quality.
- **Two-channel ROI** — Mean of FCz and Cz rather than FCz alone, to reduce the influence of single-electrode noise on the observed effect.

---

## Outputs

Running Stage 5 saves the following files to your working directory:

| File | Contents |
|------|----------|
| `fig3a_fcz_values.csv` | Win/Loss waveforms by condition at FCz |
| `fig3c_fcz_values.csv` | Win–Loss difference waves at FCz |
| `fig3d_rewp_values.csv` | Subject-level RewP scores by task × cue context |

---

## Software

Run `mne.sys_info()` inside the notebook to confirm your active environment. Core dependencies:

- `mne`
- `numpy`
- `pandas`
- `scipy`
- `matplotlib`
