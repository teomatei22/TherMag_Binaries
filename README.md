# TherMag_Binaries

## TherMag App

TherMag is a cross-platform GUI for setting up, running, and post-processing thermoelectric finite-element simulations with ElmerFEM. A temperature difference drives thermal and electrical transport in a sample surrounded by air, and the workflow extracts the resulting density currents and magnetic fields.

## What It Does

Work through the four toolbar panels:

1. **Geometry** - Cylinder, prism, or sphere; full, hollow, layer-over-full, or layer-over-hollow. Dimensions are stored as SI lengths.
2. **Simulation** - Temperature and electric boundary conditions, materials, and temperature driver (steady, harmonic, or sawtooth).
3. **Results** - Select ParaView maps and probe extractions; set interior and exterior probe points.
4. **Sweep** - Optional coefficient sweep of geometry parameters.

Typical run:

1. Set geometry and simulation options.
2. Choose results scripts and probes.
3. Enter a run name and press **Generate** to write Gmsh `.geo` and Elmer `.sif` inputs.
4. Press **Run** to mesh, solve (optional MPI), and post-process.

**Generate** only writes inputs, while **Run** meshes, solves, and executes the selected scripts.

### Post-Processing

| Output | Role |
|--------|------|
| Temperature Map | Temperature field image / animation |
| Current Density Interior | Interior **J**, temperature, and potential at shared probe points (CSV) |
| Temperature Interior / Potential Interior | UI companions of Current Density Interior (same probes) |
| Current Density Vector Map | Vector field visualization of current density |
| Magnetic Field Exterior | Exterior **B** at probe points (CSV) |
| Magnetic Field Contour Map | Midplane \|B\| contour on the trusted exterior band |
| Magnetic Field Vector Map | Vector field visualization of magnetic field |
| Generate Plots | Matplotlib plots from extracted CSV (time-dependent runs) |
| Sweep Plot | Comparison plots across sweep coefficients |

## Quick Start

### Windows (Installer)

1. Download `TherMag_Setup.exe` from the [GitHub release](https://github.com/teomatei22/ESA_wrapper_binaries/releases).
2. Run the installer and launch from the desktop shortcut.

The installer places the app under `C:\Program Files\TherMag`, bundles Gmsh / Elmer / ParaView under `bin\`, and defaults new runs to the user Documents folder (the Generate dialog chooses the parent).

### Linux 

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python run_gui.py
```

Gmsh, ElmerGrid, ElmerSolver, and ParaView `pvpython` must be on `PATH` (or set in `config.ini`). On Linux, runs default to `./runs/` under the process working directory.

## Directory Structure

```
.
├── run_gui.py                 # GUI entry point
├── config/                    # Application defaults and tool paths
├── domain/                    # Geometry, simulation, sweep, run manifests
├── application/               # Services (project, simulation, sweep, post-processing)
├── infrastructure/            # Gmsh, Elmer, ParaView, workspace adapters
├── presentation/              # Facade between GUI and application services
├── gui/                       # PySide6 tabs, widgets, controllers
├── geometries/                # Parameter defaults and .geo templates
├── simulations/               # SIF templates, generator, tensor utilities
├── materials/                 # Material library (default_materials.json)
├── processing/                # ParaView scripts, probe scaling, geometry params
└── tests/
```

## Supported Geometries

Each shape has four variants: **Full**, **Hollow**, **Layer over Full**, and **Layer over Hollow**.

| Shape | Stored size keys (examples) | Templates |
|-------|-----------------------------|-----------|
| Cylinder | `L`, `R` (+ thicknesses) | `full_cylinder.geo` --- `layer_over_hollow_cylinder.geo` |
| Prism | `L`, `W`, `H` (+ thicknesses) | `full_prism.geo` --- `layer_over_hollow_prism.geo` |
| Sphere | `R_sphere`, `R_cap`, `patch_angle_deg` (+ thicknesses) | `full_sphere.geo` --- `layer_over_hollow_sphere.geo` |

Air-domain radius in the generated `.geo` is `R_sph`, derived from the metal size.

## Material Database

Materials live in `materials/default_materials.json`. Each entry can classify:

- **Simple** - Constant transport properties
- **Temperature-dependent** - Tabulated conductivity vs temperature
- **Anisotropic** - Direction-dependent tensors 
- **Nonlinear H–B** - Magnetic curve when present

## Documentation

- **Technical documentation (API):** open **[TherMag API](https://teomatei22.github.io/ESA_wrapper_binaries/)** in a browser (click the link).

- User manual: `docs/user_manual/user_manual.tex`
- In-app Help: control reference from the manual (preview **? Help** button)

## Requirements

### Python

- Python 3.10+
- Packages in `requirements.txt` (PySide6, NumPy, psutil, matplotlib, pytest)

### External tools

- **GMSH**, **ElmerFEM 9.0**, **ParaView** (pvpython)
- Windows installer and binaries: [`TherMag_Setup.exe`](https://github.com/teomatei22/ESA_wrapper_binaries/releases) in the [ESA_wrapper_binaries](https://github.com/teomatei22/ESA_wrapper_binaries) repo

## License

Copyright (C) 2026 TherMag. Licensed under the [GNU GPL v2](LICENSE) (or later, at your option).
