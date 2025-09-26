# Harsha Equation — Advanced QuTiP Notebook

This repository contains the computational supplement to the paper:

**Routhu, Harsha Vardhan (2025).  
_The Harsha Equation: A Canonical Law for Quantum Biology._**

The notebook implements and explores the **Harsha Equation**, a proposed canonical dynamical law of quantum biology that unifies coherence, tunneling, environment, and adaptive feedback into one framework.

## Features
- Two-site exciton transport model (photosynthesis case)
- Radical-pair magnetoreception model
- Enzyme tunneling toy model
- Adaptive feedback Hamiltonian (`H_bio(t)`)
- CPTP verification via Choi eigenvalues
- Entropy production analysis (thermodynamic consistency)
- Entanglement diagnostics (log-negativity, concurrence)
- Example parameter sweeps and visualizations

# Summary: Harsha Equation and Simulation Connections

## Harsha Equation

![Master equation](http://he.axqte.com/he.jpeg)

## Term Mapping
| Term | Meaning | Where it appears in my code / plots |
|------|---------|--------------------------------------|
| $\rho(t)$ | Density matrix of the system (possibly system + mode) | `rho0`, evolving `states` in `self_consistent_evolve` or `qt.mesolve` |
| $H_{\mathrm{bio}}(t)$ | System Hamiltonian (can include feedback) | `H_base + lambda(t)*O_bio` in `run_example_two_site_with_mode()` |
| $-i[H, \rho]$ | Unitary evolution term | Handled automatically by QuTiP in `mesolve` or `self_consistent_evolve` |
| $\gamma_k(t)$ | Time-dependent decoherence/damping rates | `gamma`, `k_mode`, `gamma_dephase` in two-site / vibronic / Pauli-limit runs |
| $L_k$ | Collapse operators | Examples: `pop1`, `pop2` (pure dephasing), `model["a"]` (mode damping) |
| $L_k \rho L_k^\dagger - \frac12 \{L_k^\dagger L_k, \rho\}$ | Lindblad dissipator | Implemented automatically in QuTiP via `c_ops` |
| $\lambda(t) = \lambda_0 + \kappa \mathrm{Tr}[O_{\mathrm{bio}} \rho(t)]$ | Feedback-dependent shift of Hamiltonian | Implemented in `lambda_update_fn` inside `self_consistent_evolve` |
| Populations (`pops1`, `pops2`) | Expectation values of projectors ($\langle \Pi_k \rangle = \mathrm{Tr}[\Pi_k \rho]$) | Plotted in two-site dynamics, vibronic runs, Pauli-limit plots |
| Log-negativity / entropy | Measures of entanglement and coherence | Plotted in `plot_vibronic_dynamics()` as `ln_list` and `S_sys` |
| Mode quadrature ($\langle X \rangle$) | Observable for vibrational mode | Plotted in vibronic runs (`mode_x_exp`) |
| Pauli-limit dynamics | Populations under very strong dephasing | `pauli_limit_trajectory()` — corresponds to $(\gamma_k \to \infty)$ in Lindblad term |

## Connections to Plots
| Plot | Equation Connection |
|------|-------------------|
| `two_site_entropy_example.png` | Shows $S(\rho_\mathrm{sys}(t))$, derived from $\rho(t)$ evolving under $H_{\mathrm{bio}}(t) + c\_ops$ |
| `two_site_choi_heatmap.png` | Checks CPTP validity of the channel defined by Lindblad evolution |
| `two_site_choi_spectrum.png` | Spectrum of Choi matrix → all eigenvalues ≥0 ensures complete positivity |
| `vibronic_run_summary.png` | Shows populations and mode observables under master equation with mode + feedback |
| `vibronic_run_extended.png` | Entanglement (log-negativity) + system entropy ($S_\mathrm{vN}$) from $\rho(t)$ |
| `mode_wigner.png` | Wigner function of vibrational mode → partial trace over system |

## Summary / Intuition
- **Unitary part**: $-i[H_\mathrm{bio}(t), \rho]$ → drives coherent oscillations.  
- **Dissipative part**: $\sum_k \gamma_k(t) D[L_k]$ → decoherence, population relaxation, damping.  
- **Feedback** ($\lambda(t)$) → makes $H_\mathrm{bio}(t)$ time-dependent, introducing non-linear self-consistency.  
- **Observables** (populations, quadratures, log-negativity) → measurable consequences of the full master equation.
The code uses **[QuTiP](https://qutip.org/)** for quantum dynamics simulation.

