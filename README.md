# fNIRS analysis notebooks

A modular series of Jupyter notebooks that demonstrate, with runnable code and
plots, how functional near-infrared spectroscopy (fNIRS) data is analysed. They
accompany an introductory fNIRS lecture and use the
[**cedalion**](https://doc.ibs.tu-berlin.de/cedalion/) toolbox together with the
open-access resting-state dataset of
[von Lühmann et al. (2020), *Frontiers in Neuroscience*](https://www.frontiersin.org/journals/neuroscience/articles/10.3389/fnins.2020.579353/full).

The dataset includes files where a **synthetic haemodynamic response** was added
to a known subset of channels — a *ground truth* that lets the notebooks show
whether the analysis actually recovers the response.

---

## Quick start

```bash
# 1. Get the code
git clone https://github.com/js-6743/fnirs_notebooks.git
cd fnirs_notebooks

# 2. Get the data (see "Data" below) -> extract into ./data/

# 3. Create the environment (installs cedalion + Jupyter; takes a while)
conda env create -f environment.yml
conda activate cedalion

# 4. Launch Jupyter and run the notebooks in order
jupyter lab
```

---

## 1. Data

The data is **not** included in this repo (~0.6 GB). Download Dataset I from
NITRC and extract it into the [`data/`](data/) folder:

| Dataset | Coverage | Link | Needed? |
|---|---|---|---|
| **Dataset I**  | occipital | https://www.nitrc.org/frs/downloadlink.php/11885 | **required** |

After extracting you should have `data/resting_state_1/Subj33/…` etc. See
[`data/README.md`](data/README.md) for the exact expected layout. If your
unzipped folder is named differently, rename it to `resting_state_1`.

## 2. Install cedalion (the environment)

The [`environment.yml`](environment.yml) in this folder builds a conda
environment called **`cedalion`** that contains cedalion, all its dependencies
and Jupyter:

```bash
conda env create -f environment.yml
conda activate cedalion
```

cedalion is a large package — expect the solve/download to take **10–20+
minutes**. If you prefer a guided walk-through (recommended the first time),
follow the official install video and docs:

- 📺 **Install video:** https://www.youtube.com/watch?v=G5zQawG6GDI
- 📖 Official install docs: https://doc.ibs.tu-berlin.de/cedalion/doc/dev/getting_started/installation.html

In Jupyter, select the **`Python (cedalion)`** kernel before running a notebook.

## 3. Run the notebooks

Open them in Jupyter and run **in order** — each is self-contained but the
concepts build up.

| # | Notebook | Topic |
|---|----------|-------|
| 1 | [`01_fnirs_signal_and_physiology.ipynb`](01_fnirs_signal_and_physiology.ipynb) | The raw fNIRS signal: montage, long vs short channels, frequency content (heartbeat / respiration / Mayer waves), simultaneously-recorded systemic signals (PPG/RESP/BP) |
| 2 | [`02_from_light_to_hemoglobin.ipynb`](02_from_light_to_hemoglobin.ipynb) | Modified Beer–Lambert law: intensity → optical density → HbO/HbR, with signal-quality pruning, TDDR motion correction and band-pass filtering |
| 3 | [`03_hemodynamic_response_glm.ipynb`](03_hemodynamic_response_glm.ipynb) | **Centerpiece.** Recovering a *known* synthetic response by block averaging and a GLM; nuisance-regressor comparison; spatial maps; consistency across subjects |
| 4 | [`04_synthetic_data_augmentation.ipynb`](04_synthetic_data_augmentation.ipynb) | *(bonus)* How the ground-truth files were made: generating and adding a synthetic HRF with `cedalion.sim.synthetic_hrf`, then recovering it |

Headless (no GUI) execution, e.g.:

```bash
jupyter nbconvert --to notebook --execute --inplace 01_fnirs_signal_and_physiology.ipynb
```

---

## Repository layout

```
fnirs_notebooks/
├── 01_fnirs_signal_and_physiology.ipynb
├── 02_from_light_to_hemoglobin.ipynb
├── 03_hemodynamic_response_glm.ipynb
├── 04_synthetic_data_augmentation.ipynb
├── environment.yml          <- conda env (cedalion + Jupyter)
├── README.md
├── .gitignore               <- keeps the large data out of git
└── data/                    <- download the dataset here (git-ignored)
    └── README.md
```

The notebooks locate the data automatically: they look for
`data/resting_state_1/` and a couple of fallback locations, so as long as the
data sits in `data/` you don't need to edit any paths.

## Key cedalion API used

- **I/O:** `cedalion.io.read_snirf`
- **Channels:** `cedalion.nirs.channel_distances`, `split_long_short_channels`
- **Beer–Lambert:** `cedalion.nirs.cw.int2od`, `od2conc`; `get_extinction_coefficients`
- **Quality / motion:** `cedalion.sigproc.quality.{sci,snr,prune_ch}`, `cedalion.sigproc.motion.tddr`
- **Filtering:** `cedalion.sigproc.frequency.freq_filter`
- **Epochs:** `cedalion.sigproc.epochs.to_epochs`
- **GLM:** `cedalion.models.glm.{Gamma,fit}`, `design_matrix.{hrf_regressors,drift_regressors,average_short_channel_regressor}`
- **Simulation:** `cedalion.sim.synthetic_hrf.{build_stim_df,build_synthetic_hrf_timeseries}`
