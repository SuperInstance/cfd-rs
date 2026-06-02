# cfd-rs

CFD simulations in Rust. Navier-Stokes to Lattice Boltzmann.

A from-scratch computational fluid dynamics library that solves the fundamental equations of fluid mechanics on structured grids — all serializable and composable.

## Modules

| Module | Solver | Method |
|---|---|---|
| `navier_stokes` | `NavierStokes1D`, `NavierStokes2D` | Incompressible Navier-Stokes (projection) |
| `euler` | `EulerSolver` | 2D compressible Euler (Lax-Friedrichs) |
| `lattice_boltzmann` | `LatticeBoltzmannD2Q9` | D2Q9 Lattice Boltzmann (BGK collision) |
| `advection_diffusion` | `AdvectionDiffusionSolver` | Advection-diffusion with SUPG stabilization |
| `vortex` | `PointVortex`, `VortexSheet`, `VortexSystem` | Biot-Savart vortex dynamics (RK2) |
| `potential` | `StreamFunction`, `VelocityPotential` | Potential flow on a grid |
| `flow_regime` | `ReynoldsNumber`, `FlowRegime` | Re computation and regime classification |
| `cavity` | `LidDrivenCavity` | Lid-driven cavity benchmark |

## Install

```toml
[dependencies]
cfd-rs = "0.1"
```

## Quick Start

```rust
use cfd_rs::*;

// 1D Navier-Stokes (Burgers' equation)
let mut ns = NavierStokes1D::new(100, 0.01, 1.0);
ns.init_sine(1.0);
let dt = ns.stable_dt(0.5);
ns.advance(dt, 100);

// 2D Navier-Stokes with Taylor-Green vortex
let mut ns2d = NavierStokes2D::new(32, 32, 0.01, 1.0, 1.0);
ns2d.init_taylor_green(1.0);
ns2d.advance(0.001, 100, 50);

// Lattice Boltzmann D2Q9
let mut lbm = LatticeBoltzmannD2Q9::new(50, 50, 0.8);
lbm.init_equilibrium(1.0, 0.1, 0.0);
lbm.add_rectangular_obstacle(20, 20, 30, 30);
lbm.advance(200);

// Vortex dynamics
let mut sys = VortexSystem::new();
sys.add_vortex(0.0, 0.5, 1.0);
sys.add_vortex(0.0, -0.5, -1.0);
sys.advance(0.01, 50);

// Lid-driven cavity
let mut cavity = LidDrivenCavity::new(32, 100.0, 1.0);
let dt = cavity.stable_dt(0.3);
cavity.advance(dt, 500, 50);
```

## Testing

```sh
cargo test
```

## License

MIT OR Apache-2.0
