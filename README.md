# Muon-Based Pulse-Position Modulation (PPM) Simulation

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Ya-n0/muon-ppm-simulation/blob/main/Muon_PPM_Simulation.ipynb)

> A Monte Carlo simulation evaluating the feasibility of muometric communication through dense matter using pulse-position modulation.

## Overview

Radio communication in obstructed subsurface environments (such as collapsed mines) is frequently rendered unreliable by the absorption and scattering of electromagnetic signals. This repository contains the simulation code to evaluate an alternative: **using highly penetrating muons as an active information carrier**. 

The simulation models a complete encode-transmit-decode chain using a Pulse-Position Modulation (PPM) scheme, where binary information is encoded in the precise timing intervals between successive muon detections. 

## Features

This Jupyter Notebook (`Muon_PPM_Simulation.ipynb`) implements a discrete-event Monte Carlo model to evaluate the following parameters:

* **Bit Error Rate (BER) vs. Timing Jitter:** Evaluates signal degradation caused by physical scattering and detector resolution.
* **Bit Error Rate vs. Signal-to-Noise Ratio (SNR):** Models the impact of random cosmic-ray or beam-halo muon backgrounds on symbol recovery.
* **Majority-Vote Redundancy:** Tests a 5-fold redundancy scheme to evaluate error-correcting benefits across various jitter and background conditions.
* **Depth Reconstruction:** Reconstructs receiver depth from local muon counting statistics to model statistical precision over finite counting windows.
* **Throughput/Bandwidth Limits:** Estimates the achievable data rate as a function of the PPM slot width and beam muon detection rate.

## Mathematical Models

The simulation relies on the following simplified physics and probabilistic models:

### 1. PPM Encoding & Jitter
Binary bits are encoded into nominal time gaps ($g_1 = 100\text{ ns}$ for '1', $g_0 = 200\text{ ns}$ for '0'). Timing jitter is parameterized by a Gaussian standard deviation ($\sigma_t$) to represent velocity spread and multiple Coulomb scattering.

### 2. Background Contamination
Background noise is parameterized by a probability $p_{bg}$ per repeated gap, representing random muons that mimic a genuine interval. Signal-to-Noise Ratio (SNR) is defined as the reciprocal of this probability:
$$SNR = \frac{1}{p_{bg}}$$

### 3. Depth Reconstruction
The downstream muon flux is approximated using a simplified exponential attenuation law:
$$\Phi(H) = \Phi_0 \exp\left(-\frac{H}{L}\right)$$
Where $\Phi_0$ is the normalized surface intensity, $H$ is the true absorber depth, and $L$ is the attenuation length. The simulation draws a Poisson-distributed muon count over a fixed integration window to model counting statistics and depth uncertainty.

## Dependencies

The code is written in standard Python 3 and relies on the following libraries for simulation and visualization:
* `numpy`
* `matplotlib`
* `scipy`

*Note: Future implementations for physical beam tests will require `uproot` and/or `ROOT` for parsing real Time-to-Digital Converter (TDC) data, alongside GEANT4 for full kinematic modeling.*
