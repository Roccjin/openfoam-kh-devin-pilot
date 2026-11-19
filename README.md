# OpenFOAM Kelvin-Helmholtz Instability — Devin Autonomous Pipeline

Automated 2D multiphase shear instability simulation (`interIsoFoam` + `isoAdvector`, $Re \in [500, 5000]$) with real-time WebGL telemetry.

## Planned Architecture (To be implemented by Devin)

1. **Solver & Mesh Setup (`case_template/`)**
   - Geometric VOF (`isoAdvector`) to preserve sharp shear interfaces without `MULES` numerical diffusion.
   - Automated mesh refinement & boundary condition initialization.

2. **Courant Explosion SRE Guardian (`guardian/auto_tuner.py`)**
   - Streams `interIsoFoam` stdout in real time.
   - On `maxAlphaCo > 0.8` or PIMPLE divergence: sends `SIGTERM`, purges corrupted time directories, halves `maxCo` / `maxAlphaCo` via `PyFoam`, increases `nAlphaSubCycles`, and resumes from `latestTime`.

3. **Real-Time WebGL Visualization (`viz_stream/`)**
   - Extracts 2D `alpha.water` and `vorticity` slices via OpenFOAM `functionObjects` (`surfaces`).
   - Lightweight Go/FastAPI watcher compresses frames into binary streams over WebSocket for 60fps WebGL colormap rendering.

## Status
- [x] Architecture & numerical stability strategy finalized
- [ ] Solver case files & `auto_tuner.py` implementation (Pending Devin session)
- [ ] WebSocket binary streamer & React WebGL dashboard
