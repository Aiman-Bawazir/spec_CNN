# spec_CNN
Verilog-based Spectral CNN Layer Hardware Accelerator repository, featuring multiple versions (V2–V5) with sequential, parallel, and hybrid architectures for high-performance frequency-domain convolution. Includes FFT, EWM, activation, and register/SRAM memory implementations.


# 🚀 Spectral CNN Layer Hardware Accelerator Repository

Welcome to the **Spectral CNN Layer Hardware Accelerator** repository!  
This repo consolidates all major versions (V2 → V5) of a Verilog-based accelerator for a single Spectral Convolutional Neural Network (CNN) Layer. The project implements convolution in the **frequency domain** for high-performance hardware acceleration.

---

## 🧩 Project Objective

The goal of this project is to design, implement, and optimize a **Spectral CNN Layer** in Verilog using different architectural approaches:

- **V2 (Sequential EWM)** – Baseline sequential execution.  
- **V3 (Parallel EWM)** – Parallel element-wise multiplication for speedup.  
- **V4 (Serial Register-Based)** – Area-optimized, single-pipeline execution using internal registers.  
- **V5 (Hybrid Parallel)** – Final parallel throughput design with register-based memory for maximum performance.

Each version improves either **throughput**, **area efficiency**, or **control simplicity**, while preserving functionality.

---

## 🏛️ Repository Structure

spectral-cnn-accelerator/
│
├── V2/ # Version 2.0 – Sequential EWM
│ ├── rtl/ # Verilog source files
│ ├── tb/ # Testbenches
│ └── docs/ # Documentation & schematics
│
├── V3/ # Version 3.0 – Parallel EWM
│ ├── rtl/
│ ├── tb/
│ └── docs/
│
├── V4/ # Version 4.0 – Serial Register-Based
│ ├── rtl/
│ ├── tb/
│ └── docs/
│
├── V5/ # Version 5.0 – Hybrid Parallel Throughput
│ ├── rtl/
│ ├── tb/
│ └── docs/
│
└── common/ # Shared modules across all versions
├── fft/
├── activation/
├── multiplier/
└── helpers/

yaml
Copy code

---

## ⚙️ Key Components

| Component | File(s) | Function |
|-----------|---------|---------|
| **FFT Block** | `fft_4x4_2d.v` | 2D FFT using Row-Column decomposition |
| **EWM Block** | `ewm_block_v*.v` | Element-wise complex multiplication |
| **Complex Multiplier** | `fixed_point_complex_multiplier_16b_32b_out.v` | 3-cycle pipelined multiplication of two 16-bit complex numbers |
| **Activation Unit** | `activation_block_32b.v` | Combinatorial ReLU applied to complex outputs |
| **Control Unit** | `control_unit_v*.v` | FSM for sequencing memory access and computation |
| **Memory Modules** | `sram_sync_single_port.v` / internal register files | Stores FFT results, kernels, and outputs depending on version |

---

## 🧠 Version Overview

| Version | Architecture | Key Feature |
|---------|--------------|-------------|
| **V2** | Sequential EWM | Baseline, simple pipeline |
| **V3** | Parallel EWM | 6 parallel multipliers for increased throughput |
| **V4** | Serial Register | Area-optimized, single pipeline, internal register memory |
| **V5** | Hybrid Parallel | Parallel multipliers + register memory, maximum throughput |

---

## 🏗️ System Architecture

All versions share a **common workflow**:

1. Load **kernel/filter values** into memory (SRAM or registers).  
2. Feed input data to the **FFT block** and store 16 complex outputs.  
3. Execute the **EWM block** (sequential, parallel, or hybrid).  
4. Apply **activation function** (ReLU) to outputs.  
5. Store results in output memory or registers.  
6. Read results via testbench for verification.

Each version has unique optimizations:

- **V2:** Sequential EWM, simple FSM.  
- **V3:** Parallel EWM blocks, FFT arbitration for shared memory.  
- **V4:** Serial register-based, reduced area and simplified control.  
- **V5:** Hybrid parallel, multiple multipliers per step, register memory retained.
