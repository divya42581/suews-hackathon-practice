# Post-processing Q&A transcript

Date: 2026-06-09

This transcript records follow-up questions asked after the initial SUEWS smoke-test setup. It focuses on the example SUEWS output in `analysis/example-run/Output/KCL1_2012_SUEWS_60.txt` and the meteorological forcing in `analysis/example-run/Kc_2012_data_60.txt`.

## What is a smoke test?

A smoke test is a quick check that the full toolchain works end to end. In this setup it meant:

- SUEWS tools installed correctly.
- A bundled example configuration could be created and validated.
- The simulation ran.
- Output files were produced.
- Diagnostics found no hard failures.

It is not a scientific validation of the model result.

## What variables are simulated?

The SUEWS output includes:

- Time: `Year`, `DOY`, `Hour`, `Min`, `Dectime`
- Radiation and energy balance: `Kdown`, `Kup`, `Ldown`, `Lup`, `QN`, `QF`, `QS`, `QH`, `QE`, `QHlumps`, `QElumps`, `QHinit`, `QHresis`
- Temperatures and near-surface climate: `Tsurf`, `Ts`, `T2`, `Q2`, `U10`, `RH2`
- Water balance: `Rain`, `Irr`, `Evap`, `RO`, `TotCh`, `SurfCh`, `State`, `NWtrState`, `Drainage`, `SMD`, `FlowCh`, `AddWater`, `ROSoil`, `ROPipe`, `ROImp`, `ROVeg`, `ROWater`
- Water use and soil moisture by surface: `WUInt`, `WUEveTr`, `WUDecTr`, `WUGrass`, `SMDPaved`, `SMDBldgs`, `SMDEveTr`, `SMDDecTr`, `SMDGrass`, `SMDBSoil`
- Surface stores: `StPaved`, `StBldgs`, `StEveTr`, `StDecTr`, `StGrass`, `StBSoil`, `StWater`
- Solar/urban parameters: `Zenith`, `Azimuth`, `AlbBulk`, `Fcld`, `LAI`, `z0m`, `zdm`, `zL`, `UStar`, `TStar`, `Lob`, `RA`, `RS`
- CO2 fluxes: `Fc`, `FcPhoto`, `FcRespi`, `FcMetab`, `FcTraff`, `FcBuild`, `FcPoint`
- Snow variables: `QNSnowFr`, `QNSnow`, `AlbSnow`, `QM`, `QMFreeze`, `QMRain`, `SWE`, `MeltWater`, `MeltWStore`, `SnowCh`, `SnowRPaved`, `SnowRBldgs`
- Surface-specific temperatures: `Ts_Paved`, `Ts_Bldgs`, `Ts_EveTr`, `Ts_DecTr`, `Ts_Grass`, `Ts_BSoil`, `Ts_Water`
- Surface-specific radiation/storage: `QN_*` and `QS_*` for the same surface classes.

For heat-risk work, the most immediately useful variables are usually `T2`, `RH2`, `QN`, `QH`, `QE`, `QS`, `Ts`, and the surface-specific `Ts_*` variables.

## What is the simulation period?

The smoke-test simulation covered:

```text
2012-01-01 00:05:00 to 2013-01-01 00:00:00
```

The output analysed here has 8784 hourly records, from `2012-01-01 01:00:00` to `2013-01-01 00:00:00`.

## What is the spatial resolution and number of grids?

This smoke test uses one aggregated SUEWS site/grid:

- Number of grids/sites: `1`
- Site name: `KCL`
- Grid ID: `gridiv: 1`
- Latitude: `51.51`
- Longitude: `-0.12`
- Altitude: `10.7 m`
- Modelled surface area: `785000 m2`, about `0.785 km2`

This is not a raster grid such as 100 m or 1 km pixels. It is a single neighbourhood/site-scale SUEWS grid cell representing one aggregated urban area.

## T2 versus meteorological Tair

Compared SUEWS simulated `T2` with meteorological forcing `Tair`.

Saved artifacts:

- `analysis/example-run/postprocessing/suews_t2_vs_tair_profile.png`
- `analysis/example-run/postprocessing/suews_t2_vs_tair_profile.csv`

Summary:

- Matched hourly records: `8784`
- Mean simulated `T2`: `11.91 degC`
- Mean meteorological `Tair`: `11.11 degC`
- Mean `T2 - Tair`: `0.81 degC`

## Difference between Ts and Tsurf

In the SUEWS output definitions:

- `Tsurf` is bulk surface temperature.
- `Ts` is skin temperature.
- `Ts_Paved`, `Ts_Bldgs`, `Ts_Grass`, etc. are surface-type-specific surface or skin temperatures.

In this run:

- Mean `Tsurf`: `12.60 degC`
- Mean `Ts`: `15.25 degC`
- Mean `Ts - Tsurf`: `2.65 degC`

For heat-exposure analysis, `T2` is usually the first near-surface air-temperature variable to inspect. `Ts`, `Tsurf`, and `Ts_*` are useful for surface heating and material/land-cover interpretation.

Saved artifacts:

- `analysis/example-run/postprocessing/suews_ts_vs_tsurf_hourly_profile.png`
- `analysis/example-run/postprocessing/suews_ts_vs_tsurf_hourly_profile.csv`

## Difference between Ts - T2 and Tsurf - T2

Saved artifacts:

- `analysis/example-run/postprocessing/suews_surface_minus_t2_hourly_profile.png`
- `analysis/example-run/postprocessing/suews_surface_minus_t2_hourly_profile.csv`

Summary:

- Mean `Ts - T2`: `3.34 degC`
- Mean `Tsurf - T2`: `0.68 degC`
- `Ts - T2` shows stronger daytime spikes, especially in summer.

## Is a large Ts - T2 physically possible?

Yes, a large `Ts - T2` can be physically possible because `Ts` is a surface skin temperature and `T2` is air temperature at 2 m. Under strong sun, urban surfaces can be much hotter than the air above them.

Largest value checked in the run:

- Time: `2012-04-13 12:00`
- `T2`: `10.27 degC`
- `Ts`: `33.71 degC`
- `Ts - T2`: `23.44 degC`
- `Kdown`: `695 W m-2`
- `Ts_Paved`: `37.52 degC`
- `Ts_Bldgs`: `36.72 degC`

This pattern is physically plausible for sunlit urban materials. However, this is still a packaged demo/smoke-test case, and diagnostics flagged an energy-balance closure warning. The exact magnitudes should not be treated as validated urban-climate evidence without reviewing the configuration.

## Physics for RH2, T2, Tsurf, and Ts

These are linked outputs from SUEWS, not independent prescribed values.

`Tsurf`: bulk surface temperature. It is tied to the surface radiation and energy-balance solution:

```text
QN + QF = QS + QE + QH
```

Surface temperature is involved in longwave radiation and heat exchange, so it depends on radiation, albedo, emissivity, storage heat, evaporation, and sensible heat.

`Ts`: skin temperature. For this OHM smoke-test run, SUEWS diagnoses surface/skin temperature from sensible heat flux and aerodynamic resistance:

```text
Ts = Tair + QH * RA / (rho_air * cp_air)
```

where `QH` is sensible heat flux, `RA` is aerodynamic resistance for heat, and `rho_air * cp_air` is the volumetric heat capacity term for air.

`T2`: diagnostic 2 m air temperature. Conceptually it follows a flux-gradient profile:

```text
T2 ~= T_ref + aerodynamic_resistance * QH / (rho_air * cp_air)
```

It depends on forcing air temperature, sensible heat flux, roughness, displacement height, atmospheric stability, and roughness-sublayer treatment.

`RH2`: diagnostic 2 m relative humidity. It is derived from 2 m moisture and temperature:

```text
RH2 = actual water vapour at 2 m / saturation water vapour at T2
```

It depends on `Q2`, `T2`, pressure, evaporation/latent heat flux `QE`, aerodynamic resistance, and stability.

## How is Ts calculated?

The SUEWS source function used for the surface/skin temperature diagnostic is:

```fortran
tsfc_C = qh/(dens_air*vcp_air)*RA + temp_C
```

So:

```text
Ts = Tair + QH * RA / (rho_air * cp_air)
```

For surface-specific values:

```text
Ts_Paved = Tair + QH_Paved * RA / (rho_air * cp_air)
Ts_Bldgs = Tair + QH_Bldgs * RA / (rho_air * cp_air)
...
```

Surface sensible heat flux is calculated as a residual:

```text
QH_surf = QN_surf + QF - QS_surf - QE_surf
```

The chain for this run is:

```text
radiation + albedo/emissivity -> QN
OHM -> QS
evaporation/water balance -> QE
energy balance residual -> QH
QH + aerodynamic resistance -> Ts
```

This explains why `Ts` can spike during sunny daytime hours.

## New-chat continuity note

The questions and answers in this transcript are now saved in the repository. A new Codex chat will not automatically remember the conversation, but it can read this file and the setup transcript:

- `transcripts/session-setup-2026-06-09.md`
- `transcripts/post-processing-qa-2026-06-09.md`

The local workaround SUEWS runtime used in this chat is in the Codex workspace at:

```text
work/.venv/Scripts/suews.exe
```

That local runtime is not part of the GitHub repository.
