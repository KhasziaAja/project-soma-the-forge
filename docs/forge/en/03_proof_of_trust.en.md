# Chapter 3 Proof-of-Trust Layer: ROOTS (Physical Cost) and TFA (Triple-Filter Audit) Protocol

[← Chapter 2 SATORI Complementary Space Phase Transition & ZKP](./02_satori_zkp.en.md) | [Back to Index](../../README.md) | [Chapter 4 Economic & Tokenomics Mathematics: D-LOG Protocol →](./04_tokenomics.en.md)

## Overview & Purpose

To eliminate the alterability of digital data in isolation, this module defines the specifications of **ROOTS (Reliable Origin & Ontological Trust System)**—an appraisal system that verifies and hashes unalterable physical receipts (time, energy consumption, GPU thermal dissipation, and hardware-unique signatures) paid at the time of data generation—alongside the **TFA (Triple-Filter Audit)** protocol, which automatically censors and eliminates AI-generated fake data and fabricated logs via a three-stage metabolic engine.

## 3.1 Physical Existence Proof and Multi-Censorship Architecture

ROOTS cryptographically binds the energy expended and irreversible thermodynamic traces left in the physical world by computing clusters or laboratory apparatus, issuing a trust score $\text{ROOTS}_i \in [0, 1]$. TFA audits data purity through the following three-tier filter pipeline:

```text
[ RAW Data / Physical Logs ]
           │
           ▼
┌────────────────────────────────────────────────────────┐
│ TFA First Filter: NNS                                  │
│ (Natural Noise & Statistical Fingerprint Audit)        │
│ ・Detects Gaussian overfitting (uniform AI noise)      │
│ ・Detects statistical deviations from Benford's Law    │
└──────────┬─────────────────────────────────────────────┘
           │ (Pass)
           ▼
┌────────────────────────────────────────────────────────┐
│ TFA Second Filter: PT                                  │
│ (Causal Chain Audit Module)                            │
│ ・Audits logical distance & trial count N_trial        │
│ ・Detects "Leaps Without Suffering" (one-shot AI fakes)│
└──────────┬─────────────────────────────────────────────┘
           │ (Pass)
           ▼
┌────────────────────────────────────────────────────────┐
│ TFA Third Filter: AO                                   │
│ (Physical Existence Proof Nodes)                       │
│ ・Matches power grid frequency / GPU thermal waveforms │
│ ・Verifies TPM / Intel SGX / Hardware PUF signatures   │
└──────────┬─────────────────────────────────────────────┘
           │ (Pass)
           ▼
[ Issuance of ROOTS Trust-Hash Certificate ]
```

## 3.2 Data Schema & Pipeline

We define the data structures for the ROOTS Trust-Hash Certificate and the TFA Audit Scores below.

### Schema 3.1: `ROOTS_TrustHash_Certificate`

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "ROOTS_TrustHash_Certificate",
  "type": "object",
  "properties": {
    "certificate_id": { "type": "string", "format": "uuid" },
    "data_hash": { "type": "string", "pattern": "^0x[a-fA-F0-9]{64}$" },
    "physical_receipt": {
      "type": "object",
      "properties": {
        "execution_time_seconds": { "type": "number", "minimum": 0.001 },
        "energy_kwh": { "type": "number", "minimum": 0.000001 },
        "gpu_thermal_delta_celsius": { "type": "number" },
        "hardware_puf_signature": { "type": "string", "pattern": "^0x[a-fA-F0-9]{128}$" }
      },
      "required": ["execution_time_seconds", "energy_kwh", "gpu_thermal_delta_celsius", "hardware_puf_signature"]
    },
    "tfa_audit_scores": {
      "type": "object",
      "properties": {
        "s_nns": { "type": "number", "minimum": 0.0, "maximum": 1.0 },
        "s_pt": { "type": "number", "minimum": 0.0, "maximum": 1.0 },
        "s_ao": { "type": "number", "minimum": 0.0, "maximum": 1.0 }
      },
      "required": ["s_nns", "s_pt", "s_ao"]
    },
    "roots_score": { "type": "number", "minimum": 0.0, "maximum": 1.0 },
    "timestamp_unix": { "type": "integer" }
  },
  "required": ["certificate_id", "data_hash", "physical_receipt", "tfa_audit_scores", "roots_score", "timestamp_unix"]
}
```

## 3.3 Game Theory & Incentives

We define the payoff structure of the Stake & Slash mechanism between data submitters and replication validators.

Submitters deposit a staking amount $S_{\text{stake}}$ upon data registration. If fabricated data is uncovered via replication tests or the TFA audit, the deposited assets are completely slashed and distributed proportionally between the reporter (validator) and the victim compensation pool.

| | Validator: Detect & Report Violation | Validator: Ignore / Pass |
| :--- | :---: | :---: |
| **Submitter: Submit Fabricated Data** | $(-S_{\text{stake}}, \; \mu \cdot S_{\text{stake}} - C_{\text{validation}})$ | $(-S_{\text{stake}}, \; 0)$ (Upon automated TFA detection) |
| **Submitter: Submit Authentic Data** | $(R_{\text{mint}}, \; -C_{\text{validation}})$ | $(R_{\text{mint}}, \; 0)$ |

* **Allocation Condition:** $\mu = 0.50$ ($50\%$ of slashed funds awarded to the detector as a bounty; $50\%$ allocated to the platform compensation fund)
* $C_{\text{validation}}$: Actual cost incurred for physical replication testing and computational verification ($C_{\text{validation}} > 0$)
* $R_{\text{mint}}$: D-LOG token reward granted upon passing verification

Under this payoff structure, as long as the following minimum staking condition is satisfied, the expected value of malicious submissions becomes negative, structurally eliminating incentives for fraudulent posts:

$$S_{\text{stake}} > \frac{C_{\text{validation}}}{\mu}$$

## 3.4 Mathematical Formula

We establish the mathematical models for the ROOTS Comprehensive Trust Score, the three individual TFA layer audit scores (NNS / PT / AO), and the dynamic minimum staking requirement.

### Formula 3.1: ROOTS Comprehensive Score Calculation Formula (Bounded)

Score $\text{ROOTS}_i \in [0, 1]$ indicating the physical existence purity of the data:

$$\text{ROOTS}_i = \text{clamp}_{[0, 1]} \left( w_1 \cdot S_{\text{NNS}} + w_2 \cdot S_{\text{PT}} + w_3 \cdot S_{\text{AO}} \right)$$

$$\sum_{m=1}^{3} w_m = 1.0, \quad w_m > 0 \quad (\text{Standard Weights}: w_1 = 0.25, \; w_2 = 0.25, \; w_3 = 0.50)$$

* $\text{clamp}_{[0, 1]}(x) = \min(1.0, \max(0.0, x))$: Boundary clamp operator preventing range overflow from floating-point precision drift

### Formula 3.2: TFA Layer-by-Layer Filter Audit Score Formulas

1. **First Filter (NNS: Natural Noise & Statistical Fingerprint Audit):**

$$S_{\text{NNS}} = \exp\left( -\beta_1 \cdot \Delta_{\text{Benford}} \right) \cdot \left( 1 - \sigma_{\text{Gaussian}}^2 \right) \cdot \left( 1 - P_{\text{residual}} \right)$$

* $\Delta_{\text{Benford}} \ge 0$: Kullback-Leibler (KL) divergence from Benford's Law
* $\sigma_{\text{Gaussian}}^2 \in [0, 1]$: Degree of Gaussian overfitting (synthetic noise indicator)
* $P_{\text{residual}} \in [0, 1]$: Detection penalty for artificial rounding or periodic patterns in terminal digits
* $\beta_1 > 0$: Statistical deviation sensitivity coefficient

2. **Second Filter (PT: Causal Chain Audit):**

$$S_{\text{PT}} = 1 - \exp\left( -\beta_2 \cdot \frac{N_{\text{trial}}}{D_{\text{causal}} + \epsilon_{\text{causal}}} \right)$$

* $N_{\text{trial}} \ge 1$: Number of recorded intermediate trial and failure log steps
* $D_{\text{causal}} \ge 0$: Semantic and logical distance between the initial hypothesis vector and the final conclusion vector
* $\epsilon_{\text{causal}} = 10^{-4}$: Regularization constant preventing zero-division
* $\beta_2 > 0$: Trial density scaling coefficient

3. **Third Filter (AO: Physical Existence Proof):**

$$S_{\text{AO}} = \mathbb{I}_{\text{PUF}} \cdot \exp\left( -\beta_3 \cdot \left| \frac{E_{\text{measured}} - \hat{E}_{\text{model}}(T_{\text{exec}}, \Delta T_{\text{GPU}})}{\max(E_{\text{measured}}, \epsilon_E)} \right| \right)$$

* $\mathbb{I}_{\text{PUF}} \in \{0, 1\}$: Verification result of the hardware PUF / TPM cryptographic signature ($1$ if valid, immediately $0$ if invalid)
* $E_{\text{measured}} > 0$: Measured electrical energy consumption ($\text{kWh}$)
* $\hat{E}_{\text{model}}(\cdot)$: Theoretical energy consumption predicted by the thermodynamic model from execution time $T_{\text{exec}}$ and GPU thermal delta $\Delta T_{\text{GPU}}$
* $\epsilon_E = 10^{-9}$: Zero-division prevention constant

### Formula 3.3: Dynamic Minimum Staking Requirement (Sybil Spam Suppression)

$$S_{\text{stake}} = \max\left( S_{\text{floor}}, \; k_{\text{risk}} \cdot \hat{C}_{\text{phys}} \right)$$

* $S_{\text{floor}} > 0$: Protocol floor deposit guarantee
* $\hat{C}_{\text{phys}} \ge 0$: Estimated physical cost of the given experimental scale calculated by AI
* $k_{\text{risk}} > 1.0$: Spam suppression risk multiplier

## 3.5 Anti-Fragile Red-Teaming

We present anticipated attack vectors and defense valves for the ROOTS and TFA protocols.

1. **Attack Vector: Physical Fluctuation Generative Spoofing Attack**
   * **Method:** Using GANs or advanced physical simulators to synthesize CPU/GPU thermal waveforms, power consumption profiles, and instrument noise to breach TFA.
   * **Defense Valve:** Direct hardware integration of Physical Unclonable Functions (PUF) and Secure Enclaves (Intel SGX / TPM) is adopted. Because raw sensor data is encrypted and signed inside the processor Enclave at the exact moment of generation, external software waveform tampering and injection are $100\%$ blocked.
2. **Attack Vector: Staking Bypass Spam Attack (Sybil Spaghetti Attack)**
   * **Method:** Generating vast quantities of low-value failure data at minimal computational cost and distributing $S_{\text{stake}}$ across numerous accounts to paralyze system storage and auditing nodes.
   * **Defense Valve:** Based on Formula 3.3, the minimum required staking amount $S_{\text{stake}}$ is dynamically pegged to the estimated physical cost $\hat{C}_{\text{phys}}$ of the data. For AI-generated garbage data with near-zero real physical cost, the required deposit ratio spikes exponentially, rendering the attack economically unviable.

## 3.6 Boundary Conditions & Known Limitations (Open Issues)

* **Assumptions:** The physical existence proof (AO) in this chapter assumes that data-generating nodes operate on physical compute hardware or sensor-coupled environments equipped with a hardware Root of Trust (TPM / PUF / TEE).
* **Failure Modes:**
  * Risk of genuine micro-noise from high-precision quantum sensors or spectrometers being misclassified as AI overfitting by the NNS filter (See: [ISSUE-001](../../issues/FORGE_ISSUES_EN.md)).
  * Risk of intuitive or genius research breakthroughs (with minimal intermediate trials $N_{\text{trial}}$) being penalized as "Leaps Without Suffering" by the PT filter (See: [ISSUE-002](../../issues/FORGE_ISSUES_EN.md)).
  * Failure of AO physical verification in cloud virtualized environments (AWS / Docker / Google TPU) due to hypervisor normalization of thermal and power waveforms (See: [ISSUE-003](../../issues/FORGE_ISSUES_EN.md)).
  * Over-attenuation of valid data in inherently noisy fields (e.g., plasma physics, biotechnology) caused by the purity exponent $k \ge 3$ (See: [ISSUE-004](../../issues/FORGE_ISSUES_EN.md)).
  * Timestamp discontinuities caused by network dropouts in offline laboratory automation equipment (See: [ISSUE-005](../../issues/FORGE_ISSUES_EN.md)).
* **Verification / PoC Requirements:** Error margin calibration of the thermodynamic power model $\hat{E}_{\text{model}}$ across physical GPU clusters and direct laboratory sensor hardware.

[← Chapter 2 SATORI Complementary Space Phase Transition & ZKP](./02_satori_zkp.en.md) | [Back to Index](../../README.md) | [Chapter 4 Economic & Tokenomics Mathematics: D-LOG Protocol →](./04_tokenomics.en.md)