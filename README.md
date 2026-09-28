# Oman Desert PV Aerosol Benchmark

Open day-ahead photovoltaic forecasting benchmark for Ibri, Oman (23.05° N, 56.42° E), 2022 to 2024,
built entirely from free global data. Companion code and data for:

> S. Sarangi and J. Pandey. *Aerosol Features Add Day-Ahead Predictive Information Beyond All-Sky
> Irradiance: A Modeled Desert Photovoltaic Forecasting Benchmark for Oman.* GlobalSouthAI Workshop,
> NeurIPS 2026.

## Contents

| File | Description |
|---|---|
| `FeedIn.ipynb` | Full pipeline: data assembly, features, five model variants, evaluation, paired bootstrap, SHAP, figures |
| `data/oman_desert_pv_dataset.csv` | Merged hourly dataset (26,302 rows): NASA POWER irradiance and weather, CAMS EAC4 aerosols (linearly interpolated from 3-hourly), pvlib-modeled PV output |
| `results/test_predictions.csv` | Hourly test-period (Jul to Dec 2024) predictions of all five variants, with daytime and high-dust masks |
| `results/results_main.csv`, `results_dust.csv`, `results_paired_bootstrap.csv` | Tables reported in the paper |
| `requirements.txt` | Library versions used |

## Important: the target is modeled, not metered

`pv_power_kw` is the AC output of a 1 MW fixed-tilt block computed with pvlib (PVWatts chain) from
NASA POWER weather. It is not measured plant output. Soiling, curtailment, clipping at a real plant,
and inverter behaviour are not represented.

## Day-ahead framing

A forecast issued at 00:00 UTC covers the next 24 hours. Every input is a lag of 24 h or more
(generation 24/48/168 h; weather 24 h; aerosols 24 h, plus AOD550 48 h) or a calendar encoding.
Training: 8 Jan 2022 to 30 Jun 2024. Test: 1 Jul to 31 Dec 2024.

## Reproducing

1. Register free at the [Copernicus ADS](https://ads.atmosphere.copernicus.eu), accept the EAC4 licence,
   and set your token as the `ADS_API_KEY` environment variable (or Kaggle secret).
2. `pip install -r requirements.txt`, then run `FeedIn.ipynb` top to bottom. NASA POWER needs no key.
   If `data/oman_desert_pv_dataset.csv` is present you can skip the download cells.

Seeds are fixed at 42. CatBoost is deterministic; GRU results can vary slightly across GPUs.

## Data sources and attribution

- **NASA POWER:** These data were obtained from the NASA Langley Research Center (LaRC) POWER Project
  funded through the NASA Earth Science/Applied Science Program. https://power.larc.nasa.gov
- **CAMS EAC4:** Generated using Copernicus Atmosphere Monitoring Service information 2026. Neither the
  European Commission nor ECMWF is responsible for any use that may be made of the Copernicus
  information or data it contains. Inness et al. (2019), *Atmos. Chem. Phys.* 19, 3515 to 3556.
- **pvlib:** Holmgren, Hansen and Mikofski (2018), *JOSS* 3(29), 884.

## Licence

Code: MIT (see `LICENSE`). Derived dataset: CC BY 4.0, subject to the NASA POWER and Copernicus
terms above.

## Citation

```bibtex
@inproceedings{sarangi2026aerosol,
  title     = {Aerosol Features Add Day-Ahead Predictive Information Beyond All-Sky Irradiance:
               A Modeled Desert Photovoltaic Forecasting Benchmark for Oman},
  author    = {Sarangi, Shamik and Pandey, Jitendra},
  booktitle = {GlobalSouthAI Workshop at the 40th Conference on Neural Information Processing Systems (NeurIPS)},
  year      = {2026}
}
```
