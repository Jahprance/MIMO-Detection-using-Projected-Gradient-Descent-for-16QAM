# MIMO Detection Using Projected Gradient Descent for 16-QAM

**Praneshraj Tiruppur Nagarajan Dhyaneswar**  
Research Internship — Ericsson R&D Budapest  
July 2025 – September 2025  
Supervisor: Tamas Borsos

## Project overview

This repository contains the research code and technical presentation from an internship project on MIMO symbol detection for 16-QAM modulation.

The project investigated the recovery of transmitted symbols from the linear MIMO system

y = Hx + w

where 
y is the received signal, 
H is the channel matrix, 
x contains the transmitted 16-QAM symbols,
w represents additive white Gaussian noise.

The objective was to study the trade-off between symbol-error-rate performance and computational complexity 
through model-driven and learning-based detection methods.

The experiments use a 4 × 8 MIMO configuration and evaluate
Kronecker and CDL-B channel settings. The reported CDL benchmark uses
noise variance σ^2 = 0.25

## Research directions

The repository evaluates three detector families:

### 1. Classical projected gradient descent

The first direction implements iterative projected gradient descent (PGD) for MIMO detection:

x_(k+1) = Proj(x_k + δ Hᴴ(y − Hx_k))

Three projection strategies are investigated:

- **No projection:** unconstrained gradient updates.
- **Hard 16-QAM projection:** mapping estimates to the nearest
  constellation point.
- **Bounding-box projection:** clipping real and imaginary components
  before damping.

The analysis investigates the sensitivity of convergence and SER to
the step size δ, the spectral constraint associated with
\(\mathbf{H}^{H}\mathbf{H}\), and damping.

### 2. PGDNet: deep-unfolded MIMO detection

The second direction develops a deep-unfolded PGD network in which
each neural-network layer corresponds to one PGD iteration.

Rather than learning a large fully connected detector, PGDNet learns
two parameters per layer:

- Step size δ_k
- Damping factor

This preserves the optimization structure of the detector while
learning layer-dependent update behaviour from data.

The implementation includes model training, validation, layer-depth
experiments, checkpointing, and evaluation against conventional
detectors.

### 3. DetNet: exploratory deep MIMO detector

The third direction implements an exploratory DetNet-style detector
based on the *Deep MIMO Detection* architecture.

The tested implementation uses DetNet layers with learned fully
connected transformations and approximately 4,900 trainable
parameters. It is included as a comparison with the more
parameter-efficient PGDNet approach.

## Reported results

The following results are reported for the evaluated CDL configuration
with \(\sigma^2 = 0.25\):

| Detector | Symbol Error Rate (SER) |
|---|---:|
| Maximum Likelihood detector | 14.8% |
| K-Best detector, \(k=64\) | 15.75% |
| PGD with bounding-box projection | 20.95% |
| PGDNet, 25 layers | Approximately 20.5% |
| LMMSE baseline | 25.75% |
| DetNet, 5 layers | Approximately 34% |

Key observations from the reported experiments:

- Bounding-box PGD achieved 20.95% SER, improving on the 25.75%
  LMMSE baseline in the evaluated CDL experiment.
- PGDNet achieved approximately 20.5% SER with 20–30 layers,
  compared with hundreds of iterations for the classical PGD
  experiments reported in the presentation.
- The layer-depth study showed diminishing performance gains beyond
  approximately 20 layers.
- The learned PGDNet parameter schedules were non-monotonic, with
  some learned step sizes exceeding the classical fixed-step
  convergence bound.
- The tested DetNet configuration did not outperform LMMSE, showing
  that increased parameter count alone did not improve detection
  performance in this setup.

These figures are specific to the reported simulation configuration
and should not be interpreted as universal detector performance.

## Repository guide

Start with the technical presentation:

- [`PGD_MIMO_19112025_Readme.pdf`](Summer_Intern_BME/PGD_MIMO_19112025_Readme.pdf)

Then explore the notebooks within `Summer_Intern_BME` in this order:

1. **Non ML - Iterative PGD**  
   Classical PGD experiments, projection strategies, step-size
   threshold analysis, and SER evaluation.

2. **PGDNet - ML Based detection for 16qam using Boundary Box Projection**  
   Deep-unfolded PGDNet implementation with learned step size and
   damping parameters.

3. **Detnet - Trivial Attempt**  
   Exploratory DetNet implementation, training, evaluation, and
   failure analysis under the tested configuration.

## Technical stack

The notebooks use:

- Python
- TensorFlow / Keras
- NVIDIA Sionna
- NumPy
- Matplotlib
- Plotly
- Optuna

## Reproducibility note

This repository contains the original research code and presentation. 
Some analysis, checkpoint-loading, and plotting cells reference dependencies, files, or paths from the original experimental environment.
Running all notebooks from a fresh clone may require installing dependencies, updating paths, and accessing the corresponding saved artifacts. 
The included materials document the project methodology, experiments, evaluation workflow, and conclusions.
