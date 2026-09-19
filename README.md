# Monte Carlo BER Simulation of Digital Modulation Schemes

## 📌 Overview

This project implements a **Monte Carlo Bit Error Rate (BER) simulation** for multiple digital modulation schemes over an **AWGN (Additive White Gaussian Noise) channel**.

The simulation compares:

* **BPSK**
* **QPSK**
* **BFSK**
* **8-PSK**
* **16-QAM**
* **64-QAM**

The simulated BER is compared with the corresponding **theoretical BER** to analyze the performance of different modulation techniques as the **Eb/N0** changes.

---

## 🎯 Objectives

The main objectives of this experiment are:

1. To understand the concept of Bit Error Rate (BER).
2. To simulate digital modulation schemes using Python.
3. To model an AWGN communication channel.
4. To perform Monte Carlo-based BER estimation.
5. To compare simulated BER with theoretical BER.
6. To study the effect of Eb/N0 on communication performance.
7. To understand the trade-off between modulation order and error performance.
8. To visualize error counts and transmitted bits during simulation.

---

## 🧰 Technologies and Libraries

The project is implemented in **Python**.

### Libraries Used

| Library      | Purpose                                                  |
| ------------ | -------------------------------------------------------- |
| `NumPy`      | Numerical calculations, arrays, random number generation |
| `Matplotlib` | Plotting BER curves and simulation results               |
| `SciPy`      | Provides the complementary error function `erfc`         |

### Installation

Install the required libraries using:

```bash
pip install numpy matplotlib scipy
```

---

## 📡 Modulation Schemes

### 1. BPSK

Binary Phase Shift Keying uses two constellation points and transmits:

**1 bit per symbol**

The theoretical BER is:

```text
BER = Q(√(2Eb/N0))
```

---

### 2. QPSK

Quadrature Phase Shift Keying contains four constellation points and transmits:

**2 bits per symbol**

The theoretical BER used in the simulation is:

```text
BER = Q(√(2Eb/N0))
```

---

### 3. BFSK

Binary Frequency Shift Keying uses two orthogonal signal representations.

The implementation represents the two signals as two-dimensional basis vectors.

The theoretical BER used is:

```text
BER = Q(√(Eb/N0))
```

---

### 4. 8-PSK

8-Phase Shift Keying contains eight phase states.

Therefore:

```text
log₂(8) = 3 bits/symbol
```

A Gray-mapped phase arrangement is used.

The theoretical BER approximation is:

```text
BER = (2/k) Q(√(2kEb/N0) sin(π/8))
```

where:

```text
k = 3
```

---

### 5. 16-QAM

16-QAM uses sixteen constellation points.

Therefore:

```text
log₂(16) = 4 bits/symbol
```

The constellation is normalized to maintain unit average symbol energy.

The theoretical BER approximation used is:

```text
BER = (3/4) Q(√((4/5)Eb/N0))
```

---

### 6. 64-QAM

64-QAM contains sixty-four constellation points.

Therefore:

```text
log₂(64) = 6 bits/symbol
```

The constellation is normalized using the average energy normalization factor.

The theoretical BER approximation used is:

```text
BER = (7/12) Q(√((1/7)Eb/N0))
```

---

## ⚙️ Simulation Parameters

The simulation uses the following configuration:

```python
schemes_to_run = [
    'BPSK',
    'QPSK',
    'BFSK',
    '8-PSK',
    '16-QAM',
    '64-QAM'
]
```

The Eb/N0 range is:

```python
ebno_db_array = np.arange(0, 16, 2)
```

Therefore, the simulated values are:

```text
0, 2, 4, 6, 8, 10, 12, 14 dB
```

The default stopping conditions are:

```text
Maximum errors = 200
Maximum transmitted bits = 1,000,000
```

---

## 🔄 Simulation Flow

The overall simulation follows this process:

```text
Generate Random Symbols
        ↓
Convert Symbols to Bits
        ↓
Modulation
        ↓
Add AWGN Noise
        ↓
Receive Noisy Symbols
        ↓
Nearest-Neighbor Demodulation
        ↓
Convert Detected Symbols to Bits
        ↓
Compare Transmitted & Received Bits
        ↓
Count Bit Errors
        ↓
Calculate BER
        ↓
Compare with Theoretical BER
        ↓
Plot Results
```

---

## 🧮 BER Calculation

The Bit Error Rate is calculated using:

```text
BER = Number of Bit Errors / Total Number of Transmitted Bits
```

For example, if:

```text
Bit errors = 50
Transmitted bits = 100,000
```

then:

```text
BER = 50 / 100,000
    = 0.0005
```

---

## 🌐 AWGN Channel

The simulation uses an **Additive White Gaussian Noise (AWGN)** channel.

AWGN adds random Gaussian noise to the transmitted signal.

The noise level is determined from the selected Eb/N0 value.

The program first converts Eb/N0 from dB to linear scale:

```python
ebno_lin = 10**(ebno_db / 10)
```

The noise power is then calculated according to the number of bits per symbol.

For complex modulation schemes, Gaussian noise is generated independently for the real and imaginary components.

---

## 🔍 Modem Class

The `Modem` class contains the main information required for each modulation scheme.

It stores:

* Modulation type
* Number of bits per symbol
* Constellation points
* Theoretical BER equation
* Bit mapping information

### Main Methods

#### `setup_constellation()`

Creates the constellation and theoretical BER equation for the selected modulation scheme.

#### `modulate()`

Converts symbol indices into constellation points.

#### `demodulate()`

Determines the closest constellation point to each received symbol.

The demodulator uses a **minimum Euclidean-distance decision rule**.

---

## 📊 Plot 1 — BER vs Eb/N0

The first plot shows:

```text
BER vs Eb/N0
```

The x-axis represents:

```text
Eb/N0 (dB)
```

The y-axis represents:

```text
Bit Error Rate (BER)
```

The y-axis uses a logarithmic scale because BER values can span several orders of magnitude.

### Plot Elements

**Solid lines**

→ Theoretical BER

**Markers**

→ Monte Carlo simulation BER

**Open markers**

→ Upper-bound results when zero errors were observed

---

## 📈 Interpretation of Plot 1

As Eb/N0 increases, the signal becomes stronger relative to the noise.

Therefore, the BER generally decreases.

At low Eb/N0:

```text
More noise
      ↓
More detection errors
      ↓
Higher BER
```

At high Eb/N0:

```text
Less effect of noise
      ↓
More reliable detection
      ↓
Lower BER
```

The simulation points should generally follow the theoretical curves.

Small differences are expected because the simulation uses a finite number of randomly generated symbols.

---

## 📊 Plot 2 — Error Count and Transmitted Bits

The second plot shows:

* Number of bit errors
* Number of transmitted bits

as a function of Eb/N0.

The error count is plotted using solid lines.

The transmitted-bit count is plotted using dashed lines and is scaled by 1000.

Two reference lines are also included:

```text
Maximum Error Target = 200
Maximum Bit Target = 1,000,000
```

---

## 🔍 Interpretation of Plot 2

At low Eb/N0, errors occur frequently.

Therefore, the simulation can reach the maximum error target relatively quickly.

At high Eb/N0, errors become less frequent.

The simulation may therefore continue until the maximum number of transmitted bits is reached.

This is important for Monte Carlo BER simulations because very low BER values require a large number of transmitted bits to obtain statistically meaningful error counts.

---

## ⚠️ Zero-Error Results

Sometimes the simulation may observe:

```text
0 bit errors
```

This does **not** mean that the actual BER is exactly zero.

Instead, the code treats the result as an upper-bound estimate:

```python
ber_sim.append(1.0 / total_bits)
```

This provides a practical indication that the BER is below the resolution demonstrated by the number of transmitted bits.

---

## 📐 Important Communication Concepts Demonstrated

This project demonstrates several important digital communication concepts:

* Digital modulation
* Constellation diagrams
* BPSK
* QPSK
* BFSK
* M-PSK
* M-QAM
* AWGN channel
* Eb/N0
* Symbol energy
* Noise variance
* Gray mapping
* Nearest-neighbor detection
* Monte Carlo simulation
* Bit Error Rate
* Theoretical BER
* Simulation vs theoretical analysis

---

## 📁 Suggested Project Structure

```text
Monte-Carlo-BER-Simulation/
│
├── ber_simulation.py
├── README.md
├── requirements.txt
└── results/
    └── ber_results.png
```

### `requirements.txt`

```text
numpy
matplotlib
scipy
```

---

## ▶️ How to Run

### Step 1 — Clone or download the project

Download the project files to your computer.

### Step 2 — Install dependencies

Open a terminal in the project directory and run:

```bash
pip install -r requirements.txt
```

### Step 3 — Run the Python program

```bash
python ber_simulation.py
```

The simulation will print the modulation schemes being processed and display the final plots.

---

## ⏱️ Simulation Performance

Monte Carlo simulations can take some time because a large number of random symbols may need to be generated, especially at high Eb/N0 values.

The simulation uses adaptive stopping:

```text
Stop when:
    Errors >= 200
OR
    Transmitted bits >= 1,000,000
```

This helps balance simulation accuracy and execution time.

---

## 📚 Key Observation

The simulation demonstrates an important communication-system trade-off:

> Higher-order modulation can transmit more bits per symbol, but the constellation points become more closely spaced, making the system more sensitive to noise.

For example:

```text
BPSK      → 1 bit/symbol
QPSK      → 2 bits/symbol
8-PSK     → 3 bits/symbol
16-QAM    → 4 bits/symbol
64-QAM    → 6 bits/symbol
```

Thus, increasing modulation order improves **spectral efficiency**, but generally requires better signal-to-noise conditions for reliable communication.

---

## 🧪 Experiment Outcome

At the end of the experiment, we obtain two important visualizations:

### Graph 1

**Monte Carlo BER over AWGN**

This compares simulated BER with theoretical BER.

### Graph 2

**Error Count & Transmitted Bits vs Eb/N0**

This shows how many errors were observed and how many bits were required by the simulation.

Together, these graphs provide a practical understanding of digital modulation performance over an AWGN channel.

---

## 👨‍💻 Author

**Digital Communication / ECE Experiment**

Implemented using:

```text
Python
NumPy
SciPy
Matplotlib
```

---

## ⭐ Keywords

```text
Digital Communication
BER Simulation
Monte Carlo Simulation
AWGN
BPSK
QPSK
BFSK
8-PSK
16-QAM
64-QAM
Eb/N0
Python
NumPy
SciPy
Matplotlib
Digital Modulation
Communication Systems
Signal Processing
ECE
```
