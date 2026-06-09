# Data folder

The notebooks expect the **von Lühmann et al. (2020)** resting-state fNIRS
dataset to live here. The data is **not** included in this repository (it is
~0.6 GB + ~2.6 GB) — download it from NITRC and extract it into this folder.

## Download

NITRC project: *Open access multimodal fNIRS resting state dataset with and
without synthetic hemodynamic responses* (group_id 1071).

- Dataset I  (occipital, used by these notebooks): https://www.nitrc.org/frs/downloadlink.php/11885
- Dataset II (fronto-parietal, optional):           https://www.nitrc.org/frs/downloadlink.php/11884

## Expected layout

After extracting, this folder should look like:

```
data/
├── resting_state_1/        <- Dataset I  (required for all notebooks)
│   ├── Subj33/
│   │   ├── resting.snirf
│   │   ├── resting_hrf_20.snirf
│   │   ├── resting_hrf_50.snirf
│   │   └── resting_hrf_100.snirf
│   ├── Subj34/
│   └── ...
└── resting_state_2/        <- Dataset II (optional)
    └── ...
```

Only `resting_state_1/` is required. If your unzipped folders have different
names, rename them to `resting_state_1` / `resting_state_2`.
