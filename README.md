# MU5735 3D Flight Reconstruction

3D visualization of China Eastern Airlines Flight MU5735's final minutes, built with Three.js.

**[Live Demo](https://djzoom.github.io/MU5735/)**

![Three.js](https://img.shields.io/badge/Three.js-r164-049ef4)
![License](https://img.shields.io/badge/license-MIT-green)
![Data](https://img.shields.io/badge/data-open%20source-orange)

## Overview

This project reconstructs the flight trajectory of MU5735 (B-1791, Boeing 737-89P) on March 21, 2022, route KMG-CAN, using publicly available ADS-B and FDR data. The visualization covers the final 2 minutes and 13 seconds — from cruise at FL291 through the two-phase descent to impact at Teng County, Guangxi.

All data comes from publicly disclosed open sources. No classified or restricted information is used.

本重建所使用的全部数据均来自网上公开披露的开源信息，未使用任何保密或受限数据。

## Data Sources & Fidelity

### Measured Data (ADS-B) 实测数据

| Parameter | Source |
|-----------|--------|
| Position (lat/lon) | ADS-B via Flightradar24 |
| Altitude | ADS-B |
| Ground Speed | ADS-B |
| Heading | ADS-B |
| Vertical Speed | ADS-B |

- **27 data points** selected from 397 granular ADS-B records
- Source: [cathaypacific8747/mu5735](https://github.com/cathaypacific8747/mu5735) — FR24 ADS-B granular data
- Flight path and timing are real recorded data / 航迹与时间为真实记录数据

### Disclosed Data (FDR / CAAC) 已披露数据

| Event | Time (CST) | Source |
|-------|-----------|--------|
| Engine fuel switches CUTOFF | 14:20:38 | FDR — CAAC/NTSB |
| Control column pushed forward | 14:20:41 | FDR — CAAC/NTSB |
| FDR power loss (~26,000 ft) | 14:21:01 | FDR — CAAC/NTSB |

- Source: [NTSB FOIA DCA22WA102](https://www.ntsb.gov)
- CAAC investigation progress reports (中国民航局调查进展通报)

### Estimated Values 估算值

| Parameter | Method |
|-----------|--------|
| Pitch (俯仰角) | Flight Path Angle from spline tangent + Angle of Attack estimate from V/S magnitude |
| Bank (倾斜角) | Heading rate × 6 (assumes coordinated flight) |
| G-load | Rough approximation from vertical speed |

These values are **not measured data**. The attitude estimation accounts for the control column being pushed forward (nose steeper than flight path).

### Unavailable Data 未公开数据

The following are **not publicly available** and are not used in this reconstruction:

- Actual pitch / roll / angle of attack
- Control surface deflections
- Engine parameters (N1, N2, EGT, fuel flow)
- Full FDR readout

### Other Sources

| Asset | Source |
|-------|--------|
| Aircraft 3D model | Boeing 737-800 GLTF (Flightradar24 B738) |
| Satellite imagery | Google Maps satellite tiles |
| Impact coordinates | 23°19'25.5"N 111°06'44.3"E — CAAC |

## Timeline of Events

| Time (CST) | Altitude | Event |
|-----------|---------|-------|
| 14:20:38 | FL291 | Engine fuel switches RUN→CUTOFF |
| 14:20:41 | FL291 | Abnormal control column input |
| 14:20:44 | FL291 | Last stable flight parameters |
| 14:21:00 | 27,025 ft | Descent begins, V/S −21,696 ft/min |
| 14:21:01 | ~26,000 ft | FDR stops (generator power loss) |
| 14:21:15 | 17,325 ft | Dive steepens, heading 91°→123° |
| 14:21:46 | 7,850 ft | First dive bottom, GS 590 kts |
| 14:21:56 | ~7,850 ft | Recovery, V/S +3,520 ft/min |
| 14:22:05 | 8,600 ft | Peak recovery altitude |
| 14:22:22 | 8,175 ft | Second dive begins |
| 14:22:36 | 3,225 ft | Last ADS-B signal, 376 kts |
| 14:22:38 | 0 ft | Impact — Teng County, Guangxi |

## Technical Details

- **Rendering**: Three.js r164 with ES modules, logarithmic depth buffer
- **Trajectory interpolation**: CatmullRom spline (centripetal parameterization) through 27 ADS-B waypoints
- **Attitude model**: Euler angles (YZX order) — heading from ADS-B, pitch from FPA + AOA estimate, bank from heading rate
- **Terrain**: Google satellite tiles at zoom 13 with procedural elevation
- **UI**: Bilingual (English / 中文), responsive for mobile and desktop

## Usage

Open `index.html` in a browser. No build step or server required — all dependencies load from CDN.

Controls:
- **Drag** — orbit camera
- **Scroll** — zoom
- **Timeline** — click to seek
- **Camera modes** — Tail / Free / Left / Right / Top / FPV

## License

MIT

## Disclaimer

This visualization is created for educational and research purposes. All data used comes from publicly disclosed open sources. This project is not affiliated with China Eastern Airlines, CAAC, or NTSB.

本可视化仅用于教育和研究目的。所使用的全部数据均来自公开披露的开源信息。本项目与中国东方航空、中国民航局及美国国家运输安全委员会无关。
