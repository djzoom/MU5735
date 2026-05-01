# MU5735 3D Flight Reconstruction

3D visualization of China Eastern Airlines Flight MU5735's final minutes, built with Three.js.

**[Live Demo](https://djzoom.github.io/MU5735/)**

![Three.js](https://img.shields.io/badge/Three.js-r164-049ef4)
![License](https://img.shields.io/badge/license-MIT-green)
![Data](https://img.shields.io/badge/data-open%20source-orange)

## Overview

This project reconstructs the flight trajectory of MU5735 (B-1791, Boeing 737-89P) on March 21, 2022, route KMG→CAN, using publicly available ADS-B and FDR data. The visualization covers the final 2 minutes and 13 seconds — from cruise at FL291 through the two-phase descent to impact at Teng County, Guangxi.

All data comes from publicly disclosed open sources. No classified or restricted information is used.

本重建所使用的全部数据均来自网上公开披露的开源信息，未使用任何保密或受限数据。

## Data Sources & References

### 1. ADS-B Trajectory Data 航迹实测数据

| Parameter | Data Type |
|-----------|-----------|
| Position (lat/lon) | Measured |
| Altitude (ft) | Measured |
| Ground Speed (kts) | Measured |
| Heading (°) | Measured |
| Vertical Speed (ft/min) | Measured |

- **27 data points** selected from 397 granular ADS-B records
- Source: **[cathaypacific8747/mu5735](https://github.com/cathaypacific8747/mu5735)** — Flightradar24 ADS-B granular data, publicly archived
- Flight path and timing are real recorded data / 航迹与时间为真实记录数据
- Original data provider: [Flightradar24](https://www.flightradar24.com/)

### 2. FDR Disclosed Data 飞行数据记录器已披露数据

Data from the last ~90 seconds of FDR recording before power loss at ~26,000 ft.

| Time (CST) | Event | Source |
|-----------|-------|--------|
| 14:20:38 | Engine fuel switches RUN→CUTOFF (both engines) | FDR |
| 14:20:41 | AP-1/AP-2 disconnect; control column pushed forward | FDR |
| 14:20:44 | Last stable flight parameters at FL291 | FDR |
| 14:20:48 | Extreme attitude: pitch −30°~−40°, roll >90°~180° | FDR |
| 14:20:55 | Elevator/aileron at deflection limits; IAS >400 kts | FDR |
| 14:21:01 | FDR power loss — engine N2 below generator threshold | FDR |

**FDR phase details (NTSB FOIA):**
- **Phase 1** (T−90s ~ T−30s): Normal cruise — FL291, ~270 kts IAS, AP-1/AP-2 engaged, pitch/roll ≈ 0°
- **Phase 2** (T−30s ~ T−15s): Anomaly onset — fuel switches CUTOFF, AP disconnect, control column pushed forward, pitch goes rapidly negative, Ctrl Col Pos shows large-amplitude inputs
- **Phase 3** (T−15s ~ T−5s): Violent maneuvering — pitch −30°~−40°, roll exceeding 90°~180°, vertical/longitudinal acceleration violent oscillation, dual control input (two pilots fighting for control), elevator/aileron at deflection limits, IAS >350→400+ kts
- **Phase 4** (T−5s ~ T): Final FDR data — all parameters reach extreme values simultaneously, altitude extremely low, data ends

Where T = FDR stop time (~26,000 ft, 14:21:01 CST).

**References:**
- **[NTSB FOIA DCA22WA102](https://data.ntsb.gov/Docket?ProjectID=105683)** — U.S. National Transportation Safety Board, Freedom of Information Act release. Contains FDR parameter plots for the final ~90 seconds of recording.
- **[CAAC Investigation Progress Reports](http://www.caac.gov.cn/)** — Civil Aviation Administration of China, publicly released investigation updates (中国民航局事故调查进展情况通报)
- **[Wikimedia Commons: China Eastern Airlines Flight 5735](https://commons.wikimedia.org/wiki/Category:China_Eastern_Airlines_Flight_5735)** — NTSB FDR documents archived

### 3. Estimated Values 估算值

Attitude is split into three phases. Cruise is held at a stable nominal attitude; the FDR window uses disclosed envelope values; the post-FDR dive is derived from trajectory.

| Phase | Window (CST) | Pitch (俯仰角) | Bank (倾斜角) | Notes |
|-------|--------------|----------------|---------------|-------|
| Cruise 巡航 | up to 14:20:39 | Held at +2° AOA | 0° (wings level) | Visual altitude locked to ADS-B; no spline-derived attitude jitter |
| FDR disclosed 已披露段 | 14:20:41 – 14:21:01 | −29°~−41° (−35° + 6° osc, period ~8s) | ±88° (slow oscillation, period ~10s) | Matches NTSB-disclosed envelope (pitch −30°~−40°, roll >90°), modelled at aerodynamically plausible 737-LOC rates |
| Post-FDR dive 断电后俯冲 | 14:21:01 – impact | FPA (spline tangent) + AOA from V/S magnitude | Heading rate × 4, clamped to ±55° | Coordinated-turn proxy |

**Aerodynamic constraints applied across all phases:**

| Constraint | Cruise | FDR window | Post-FDR dive |
|------------|--------|------------|---------------|
| Pitch rate limit | ≤18 °/s | ≤30 °/s | ≤18 °/s |
| Roll rate limit | ≤50 °/s | ≤75 °/s | ≤50 °/s |
| Heading rate limit | ≤4 °/s | ≤22 °/s | ≤22 °/s |
| Smoothing τ (low-pass) | 2.5 s | 0.7 s | 1.1 s |

| Other | Method | Applied Period |
|-------|--------|---------------|
| G-load (G载荷) | Approximation from vertical speed; FDR window adds oscillation envelope | Entire flight |

### 4. Unavailable Data 未公开数据

The following are **not publicly available** and cannot be used:

- Complete FDR readout (full parameter set)
- CVR (Cockpit Voice Recorder) transcript
- Actual continuous pitch / roll / AOA time series
- Control surface deflection time series
- Engine parameters (N1, N2, EGT, fuel flow)
- Final CAAC investigation report (not yet published as of 2025)

### 5. Other Assets 其他资源

| Asset | Source | License |
|-------|--------|---------|
| Aircraft 3D model | Boeing 737-800 GLTF | [Flightradar24](https://www.flightradar24.com/) B738 model |
| Satellite imagery | Google Maps satellite tiles (zoom 13) | Google Terms of Service |
| Impact coordinates | 23°19'25.5"N 111°06'44.3"E | CAAC official announcement |
| Three.js | [r164.1](https://github.com/mrdoob/three.js/) | MIT License |

## Timeline of Events

| Time (CST) | Altitude | Event | Data Source |
|-----------|---------|-------|-------------|
| 14:20:38 | FL291 | Engine fuel switches RUN→CUTOFF | FDR (NTSB) |
| 14:20:41 | FL291 | AP-1/AP-2 OFF + control column pushed fwd | FDR (NTSB) |
| 14:20:44 | FL291 | Last stable flight parameters | FDR (NTSB) |
| 14:20:48 | FL291 | Extreme attitude — pitch −30°~−40°, roll >90° | FDR (NTSB) |
| 14:20:55 | ~FL291 | Control surface deflection limits reached | FDR (NTSB) |
| 14:21:00 | 27,025 ft | Descent detected — V/S −21,696 ft/min | ADS-B |
| 14:21:01 | ~26,000 ft | FDR stops — generator power loss | FDR (NTSB) |
| 14:21:15 | 17,325 ft | Dive steepens — heading 91°→123° | ADS-B |
| 14:21:46 | 7,850 ft | First dive bottom — GS 590 kts | ADS-B |
| 14:21:56 | ~7,850 ft | Recovery — V/S +3,520 ft/min | ADS-B |
| 14:22:05 | 8,600 ft | Peak recovery altitude | ADS-B |
| 14:22:22 | 8,175 ft | Second dive begins | ADS-B |
| 14:22:36 | 3,225 ft | Last ADS-B signal — 376 kts | ADS-B |
| 14:22:38 | 0 ft | Impact — Teng County, Guangxi (witnesses: spiral/tumble) | CAAC / witnesses |

## Technical Details

- **Rendering**: Three.js r164 with ES modules, logarithmic depth buffer, ACES filmic tone mapping
- **Trajectory**: CatmullRom spline (centripetal, open) through 27 ADS-B waypoints
- **Attitude**: Euler angles (YZX intrinsic order) — heading from ADS-B, pitch/roll from FDR (when available) or estimated from trajectory
- **Cruise attitude model**: Held at +2° AOA, wings level; visual altitude locked to ADS-B data to suppress spline Y-jitter (no shake along the green cruise line)
- **FDR attitude model**: During FDR window (t=271–291s), pitch eases into −35° with ±6° oscillation (period ~8s); bank oscillates ±88° (period ~10s) — matches NTSB-disclosed envelope at aerodynamically plausible 737 loss-of-control rates
- **Post-FDR attitude model**: Pitch = Flight Path Angle (spline tangent) + AOA estimate (V/S-based, capped at −18°); Bank = heading rate × 4, clamped to ±55°
- **Smoothing**: Frame-rate-independent exponential low-pass with phase-tuned time constants (τ = 2.5s cruise, 0.7s FDR, 1.1s post-FDR)
- **Rate limits**: Per-axis aerodynamic clamps on smoothed output — pitch ≤18°/s (30°/s during FDR), roll ≤50°/s (75°/s during FDR), heading ≤4°/s cruise / ≤22°/s dive
- **Terrain**: Google satellite tiles at zoom 13, procedural elevation displacement
- **UI**: Bilingual (English / 中文), responsive layout for mobile and desktop

## Usage

Open `index.html` in any modern browser. No build step, no server — all dependencies load from CDN (jsdelivr).

Controls:
- **Drag** — orbit camera
- **Scroll / pinch** — zoom
- **Timeline bar** — click to seek
- **Camera modes** — Tail / Free / Left / Right / Top / FPV (first-person view)

## References

1. **ADS-B data**: cathaypacific8747. *MU5735 ADS-B granular data*. GitHub. https://github.com/cathaypacific8747/mu5735
2. **FDR plots**: U.S. National Transportation Safety Board. *FOIA DCA22WA102 — FDR parameter plots*. https://data.ntsb.gov/Docket?ProjectID=105683
3. **Investigation reports**: Civil Aviation Administration of China. *MU5735 investigation progress reports*. http://www.caac.gov.cn/
4. **NTSB documents archive**: Wikimedia Commons. *Category: China Eastern Airlines Flight 5735*. https://commons.wikimedia.org/wiki/Category:China_Eastern_Airlines_Flight_5735
5. **Impact site**: CAAC official announcement — 23°19'25.5"N 111°06'44.3"E, Molang village, Teng County, Wuzhou, Guangxi
6. **Three.js**: mrdoob. *Three.js r164*. https://github.com/mrdoob/three.js/

## License

MIT — see [LICENSE](LICENSE).

## Disclaimer

This visualization is created for educational and research purposes only. All data used comes from publicly disclosed open sources. No classified or restricted information is used. Attitude values during the post-FDR phase are estimates and may not reflect actual aircraft state.

This project is not affiliated with China Eastern Airlines, CAAC, NTSB, or any investigation authority.

本可视化仅用于教育和研究目的。所使用的全部数据均来自公开披露的开源信息，未使用任何保密或受限数据。FDR断电后的姿态数据为估算值，可能不反映飞机实际状态。本项目与中国东方航空、中国民航局、美国国家运输安全委员会及任何调查机构无关。
