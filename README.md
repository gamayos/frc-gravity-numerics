# Numeric validation suite — *Gravity as Phase Synchronisation on a Finite Substrate*

Independent numerical verification of every quantitative claim of the manuscript
(`../main.tex`). Each script is self-contained, prints the computed values against
the manuscript's targets, and ends with a single `PASS`/`FAIL` line.

## Requirements

Python 3.10+, `numpy`, `sympy` (which provides `mpmath`). No other dependencies.

## Usage

```
python3 run_all.py          # full suite with summary
python3 validate_<x>.py     # any single suite
```

Total runtime ≈ 1–2 minutes; `validate_fluxnoise.py` (a stochastic simulation)
dominates.

## Scripts and the claims they validate

| Script | Manuscript claim | What is checked |
|---|---|---|
| `validate_newton.py` | Lemma (stationary bias field); Theorem (Newtonian limit); Lemma (coherent additivity) | Exact lattice Green's function of Z³ via the Montroll–Bessel representation: 4πr·G(r) → 1 with the stated O(r⁻²) correction; potential power law r⁻¹ (hence force r⁻²); locked ensemble couples as m, unlocked as √m. |
| `validate_ppn.py` | Prop. (full light deflection); Prop. (classical tests); Prop. (exact static profile, series) | Symbolic: Fermat deflection 4Gm/c²b in the index n = 1+2u; PPN β = γ = 1 from the exponential metric expansion; perihelion factor (2+2γ−β)/3 = 1; static-profile series Gm/r·(1+(Gm/r²)²/30). |
| `validate_strongfield.py` | Prop. (exact static profile); Prop. (horizonless, operationally black); Prop. (photon sphere and shadow); area law; echo suppression | Slip-core boundary r\* = √(Gm); exact potential at the photon sphere; numerical extremum of b(r) = r·e^{2u}: r_ph = 2Gm, b_c = 2e·Gm; shadow deviation +4.63 %, ringdown −4.42 %; r_f = r_s/ln Ω, S/S_BH = ln⁻²Ω; echo delay factor Ω/ln²Ω (no observable echoes); Sgr A\*/M87\* angular diameters (53.3 → 55.7 µas; 39.7 → 41.5 µas). |
| `validate_fp_gauge.py` | Eq. (discrete Fierz–Pauli functional); exact discrete gauge invariance | The four-term FP form on a periodic 3-d lattice with random fields: invariance under h → h + Δξ (symmetrised) to machine precision for **central** differences, and its **failure** for one-sided differences — confirming that the anti-self-adjointness Δᵀ = −Δ is what carries the continuum proof, exactly as the manuscript states. |
| `validate_fluxnoise.py` | Lemma (flux conservation under noise) | A driven, pinned Kuramoto chain in the statistically stationary regime: the time-averaged transport through every link equals the source demand, with and without drive noise. (The lemma's stationarity hypothesis is essential: overdriving the chain produces a running state, visible by raising `b` or the noise.) |
| `validate_rar.py` | Prop. (floor acceleration); Cor. (turnaround) | a₀ = cH₀/2π for H₀ ∈ {67.4, 70, 73} against the SPARC fit 1.20 ± 0.24 × 10⁻¹⁰ m s⁻² (all within one systematic sigma); Local-Group turnaround radius (Gm/H²)^{1/3} ≈ 1.6 Mpc against the observed ~1 Mpc zero-velocity surface. |

## What is *not* validated here

The suite validates derived numbers, not premises: the five premises of the
manuscript (mass as locked cardinality, distance as decoherence, the dilation as
common drive, unit capacity, channel unity) are inputs. The two open
constructions — the second-order self-coupling evaluation of the discrete
Fierz–Pauli functional (the strong-field branch decision) and the sub-threshold
dynamics of the weak-acceleration regime — are not yet computable and therefore
not yet testable here; they are the suite's intended growth points.

## Provenance note

The gauge-invariance test in `validate_fp_gauge.py` was responsible for a
correction to the manuscript: an earlier draft stated the discrete functional
with forward differences, for which invariance fails (their lattice adjoint is
not their negative); the exhibited functional uses central differences, for
which invariance is exact. The failing configuration is retained in the test
deliberately, as a control.
