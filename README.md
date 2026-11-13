# 3.5 GHz Two-Stage MOSFET Low-Noise Amplifier (LNA)

A transistor-level RFIC design and simulation project targeting the **3.2–3.8 GHz** band, centered at **3.5 GHz**. The amplifier was designed in **Qucs-S** using the **IHP SG13G2 process design kit (PDK)** and SPICE device models.

> **Project status:** Simulation-based design. All performance figures in this README are simulated results, not silicon measurements. Band-edge output matching and wide-frequency stability still require further verification and optimization.

## Circuit schematic and simulation

![LNA schematic and simulation snapshot](images/lna_schematic_and_simulation.png)

The screenshot shows the circuit schematic and representative frequency-domain simulation plots from the Qucs-S design environment.

## Design objectives

- Achieve useful forward gain around 3.5 GHz.
- Maintain a low noise figure for receiver-front-end use.
- Design input, interstage, and output RF matching networks.
- Establish suitable MOSFET DC operating points from a 1.2 V supply.
- Evaluate S-parameters, noise, stability, and preliminary nonlinear performance.
- Assess performance across the intended 3.2–3.8 GHz operating band.

## Circuit architecture

The implementation uses two MOSFET active-device stages with passive RF matching and DC bias networks:

1. Input matching and coupling network.
2. First MOSFET gain stage.
3. Interstage coupling/matching network.
4. Second MOSFET gain stage.
5. Output matching network.
6. DC bias, RF-choke, and decoupling elements.

The topology and component values are visible in the schematic above. The native Qucs-S project files are intended to provide the editable circuit and simulation setup.

## Tools, libraries, and model dependencies

| Tool / library | Role in the project |
|---|---|
| **Qucs-S** | Schematic capture, simulation setup, and plotting/post-processing |
| **IHP SG13G2 PDK** | Technology design kit supplying process-specific device models |
| **SPICE MOSFET models** | Transistor model definitions used for circuit simulation |
| **ngspice / compatible SPICE engine** | Simulation backend, depending on the local Qucs-S configuration |

### Reproducing the simulation

1. Install a compatible version of Qucs-S.
2. Obtain the IHP SG13G2 PDK from its official distribution and review its license.
3. Install/configure the PDK and its SPICE model files locally.
4. Configure Qucs-S to use the compatible SPICE simulation backend and locate the required model files.
5. Open the native Qucs-S project included in this repository and run the simulations.
6. Compare the generated results with the performance summary in this README.

**Environment details to add when confirmed:** Qucs-S version, simulator/backend version, IHP SG13G2 PDK release or commit, and the exact device model corner. These details are not guessed here.

> **PDK licensing:** The PDK and foundry model files are external dependencies. They should not be copied into this repository unless their license explicitly permits redistribution.

## Simulation results

### Performance at 3.5 GHz

| Parameter | Reported simulated result |
|---|---:|
| Forward gain, S21 | 18.96 dB |
| Noise figure (NF) | 1.477 dB |
| Minimum noise figure | 1.368 dB |
| Input return loss, S11 | -10.61 dB |
| Output return loss, S22 | -15.61 dB |
| Reverse isolation, S12 | -34.94 dB |
| Supply voltage | 1.2 V |
| Total DC power | approximately 17.35 mW |

### Performance across 3.2–3.8 GHz

| Parameter | Reported simulated result |
|---|---:|
| Worst-case noise figure | 1.538 dB |
| Forward gain range | 17.01–18.96 dB |
| Worst input return loss | -10.53 dB |
| Worst output return loss | -3.71 dB |

The output match degrades away from the center frequency and is the main currently identified band-wide limitation.

### Stability analysis

The following are preliminary reported stability results:

| Parameter | Reported value |
|---|---:|
| Minimum Rollett K in operating-band check | 2.6195 |
| Minimum K in wide sweep | 1.5416 |
| Wide-sweep stability-related determinant maximum | 0.9987 |
| Minimum μ in wide sweep | 1.0000 |

The wide-sweep values approach a stability boundary. A denser frequency sweep and a careful review of the exact stability metrics and definitions are required before claiming a robust stability margin.

### Preliminary linearity results

| Parameter | Reported result |
|---|---:|
| Input IIP3 | approximately -7.7 dBm |
| Sum-frequency IIP2 | approximately +10.5 dBm |

These are preliminary estimates. The two-tone test conditions, tone spacing, input-power sweep, intermodulation products, and extraction procedure should be documented before treating them as final characterization results.

## Current assessment and future work

- Improve output matching across the complete 3.2–3.8 GHz band.
- Re-run stability analysis with a denser frequency sweep and document K, μ, and determinant definitions consistently.
- Verify the two-tone testbench and IIP2/IIP3 extraction procedure.
- Run process, voltage, temperature, and passive-component variation/corner analysis where supported by the PDK.
- Record exact Qucs-S, simulator, PDK, and model-corner versions.
- Add final verification plots to this README as the design is updated.

## Repository contents

```text
3.5GHz-MOSFET-LNA/
├── README.md
├── images/
│   └── lna_schematic_and_simulation.png
└── <native Qucs-S circuit and simulation project files>
```

The repository is intentionally kept compact: the circuit and native simulation project are stored alongside this README, while the complete explanation and reported results are documented here.

## Disclaimer

This is an educational and portfolio RFIC design project. All performance figures are simulated and depend on the selected PDK models, simulator configuration, device corners, and testbench. They should not be interpreted as fabricated-chip measurements or production-qualified specifications.
