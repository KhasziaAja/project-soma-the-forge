# Chapter 2: Physical & Edge Sensing Layer — PAIOS (Physical-AI OS) Data Mining & Hardware Fortress

[← Chapter 1: Executive Summary & System Architecture](./01_system_topology.en.md) | [Back to Index](../../../README.md) | [Chapter 3: Autonomous B2B Task Guild →](./03_soma_guild_b2b.en.md)

## Overview & Purpose

This module defines the specifications of **PAIOS (Physical-AI OS)**, an edge sensing infrastructure that captures human physical kinematics and involuntary physiological signals with zero friction (15-second attachment / camera-free 2-point mode), refining them into high-purity training datasets for Physical AI foundation models (VLA). It formalizes the **PoU V2.1 (Proof of Usefulness) Anti-Cheat Fortress** against mechanical spoofing and data poisoning, along with the composite validity scoring function $\text{PoU}_{\text{valid}}(t)$ integrating the DFA chaos scaling exponent $\alpha$ and electromechanical delay $T_{\text{rise}}$.

## 2.1 Ambient Sensing & "Zero-Touch / 15-Second Deployment" Protocol

The operational success of physical kinematic mining via PAIOS depends on completely eliminating psychological and physical friction for workers and residents. Because designs requiring manual data entry, button presses, manual calibration, or frequent charging trigger immediate field rejection, the system enforces the following "Zero-Touch / 15-Second Deployment" architecture:

```text
+-----------------------------------------------------------------------------------+
|                [ PAIOS Edge Sensing & Zero-Touch Architecture ]                   |
+-----------------------------------------------------------------------------------+
|  [ Sensor Array: 5-Module / Camera-Free 2-Point Mode ]                            |
|   1. Smart Insole (Ground Reaction Force, Center of Pressure, Gait Entropy)       |
|   2. Eraser-Sized 9-Axis IMU (Micro-Kinematics of Wrists & Ankles)                |
|   3. Neck/Lumbar Patch Sensor (Surface EMG, Mechanomyogram, Sweat, Skin Temp)     |
|   4. Ambient Millimeter-Wave / Thermal (Non-Contact Vitals & Spatial Trajectories)|
+-----------------------------------------------------------------------------------+
                                         |
                                         v (Direct Silicon Die Trace: Zero PCB Exposure)
+-----------------------------------------------------------------------------------+
|  [ SoC-Embedded Hardware Enclave (TPM / PUF) ]                                    |
|   - Real-Time Hardware-Key Signing & Encryption                                   |
|   - Store & Forward (512MB Edge NOR Flash / Autonomous Mesh Offloading)           |
+-----------------------------------------------------------------------------------+
                                         |
                                         v (PoU V2.1 Anti-Cheat Pipeline)
+-----------------------------------------------------------------------------------+
|  [ PoU V2.1 Multi-Layer Validation ]                                              |
|   - DFA Chaos Exponent Verification (0.75 < alpha < 1.25)                         |
|   - Electromechanical Delay Verification (EMG -> Muscle Rise: 80 - 120 ms)        |
|   - 3-Month Payout Holdback & Full Slashing upon Cheat Detection                  |
+-----------------------------------------------------------------------------------+
```

* **SoC-Embedded Hardware Enclave:** Integrates primary biometric sensing elements and cryptographic processors (TPM / PUF) onto a single monolithic silicon die (SoC). Raw sensor array signals are never exposed on printed circuit board (PCB) traces, routing directly into the cryptographic engine to block physical interposer attacks, electrical eavesdropping, and waveform injection at the hardware layer.
* **Camera-Free 2-Point Mode:** In privacy-sensitive residential spaces or classified industrial facilities, the system automatically switches to a camera-free configuration that reconstructs full-body inverse kinematics solely from the smart insole and neck patch.
* **Store & Forward (512MB NOR Flash):** In offline zones or subterranean work sites, up to 72 hours of encrypted feature packets are buffered locally in non-volatile memory and automatically synchronized upon mesh network reconnection.

## 2.2 Chapter 2 Technical Highlights & 4-Core Pillars

### 1) Data Schema & Pipeline

`PaiosKinematicsPacket.json`: Millisecond-synchronized packet schema for Ground Reaction Force (GRF), 9-axis IMU, and involuntary physiological reactions:

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "PaiosKinematicsPacket",
  "type": "object",
  "required": [
    "packet_id",
    "node_enclave_pubkey",
    "timestamp_utc_ms",
    "sampling_rate_hz",
    "kinematics_9axis",
    "ground_reaction_force_n",
    "involuntary_vitals",
    "pou_v21_metrics",
    "enclave_signature_hex"
  ],
  "properties": {
    "packet_id": { "type": "string", "format": "uuid" },
    "node_enclave_pubkey": { "type": "string", "pattern": "^0x[a-fA-F0-9]{64}$" },
    "timestamp_utc_ms": { "type": "integer", "minimum": 0 },
    "sampling_rate_hz": { "type": "integer", "enum": [100, 250, 1000] },
    "kinematics_9axis": {
      "type": "object",
      "required": ["accel_xyz", "gyro_xyz", "mag_xyz"],
      "properties": {
        "accel_xyz": { "type": "array", "items": { "type": "number" }, "minItems": 3, "maxItems": 3 },
        "gyro_xyz": { "type": "array", "items": { "type": "number" }, "minItems": 3, "maxItems": 3 },
        "mag_xyz": { "type": "array", "items": { "type": "number" }, "minItems": 3, "maxItems": 3 }
      }
    },
    "ground_reaction_force_n": {
      "type": "object",
      "required": ["left_foot_total_n", "right_foot_total_n", "cop_shift_mm"],
      "properties": {
        "left_foot_total_n": { "type": "number", "minimum": 0.0 },
        "right_foot_total_n": { "type": "number", "minimum": 0.0 },
        "cop_shift_mm": { "type": "array", "items": { "type": "number" }, "minItems": 2, "maxItems": 2 }
      }
    },
    "involuntary_vitals": {
      "type": "object",
      "required": ["emg_microvolts", "muscle_rise_delay_ms", "rr_interval_ms"],
      "properties": {
        "emg_microvolts": { "type": "number", "minimum": 0.0 },
        "muscle_rise_delay_ms": { "type": "number", "minimum": 0.0 },
        "rr_interval_ms": { "type": "number", "minimum": 200.0, "maximum": 2500.0 }
      }
    },
    "pou_v21_metrics": {
      "type": "object",
      "required": ["dfa_alpha", "pou_valid_score"],
      "properties": {
        "dfa_alpha": { "type": "number", "minimum": 0.0, "maximum": 2.0 },
        "pou_valid_score": { "type": "number", "minimum": 0.0, "maximum": 1.0 }
      }
    },
    "enclave_signature_hex": { "type": "string", "pattern": "^0x[a-fA-F0-9]{128}$" }
  }
}
```

### 2) Game Theory & Incentives

Payoff matrix for the "Work-to-Earn Attachment Incentive & 3-Month Holdback Game" between workers/residents (miners) and the verification protocol:

| | Verification Protocol: Validated (Released after 90 Days) | Verification Protocol: Poisoning Detected (Immediate Slashing) |
| :--- | :---: | :---: |
| **Miner: Authentic Attachment & Daily Activity** | $(+R_{\text{mining}} + R_{\text{R2S}} - C_{\text{wear}}, \; +V_{\text{DaaS}})$ | $(0, \; 0)$ (Negligible false-positive rate) |
| **Miner: Mechanical Vibration / Spoofing** | $(0, \; 0)$ (Pass probability $\approx 0$ via PoU V2.1) | $(-S_{\text{holdback}} - C_{\text{cheat}}, \; +\text{SlashedPool})$ |

* **Equilibrium Condition:** During the 3-month (90-day) payout holdback window, multiple longitudinal reverse-forensic audits (including cross-session gait entropy checks) run asynchronously. Even if a spoofing attempt bypasses real-time edge checks momentarily, it is detected prior to settlement release, resulting in full forfeiture of accrued holdback funds $S_{\text{holdback}}$. Consequently, authentic attachment during normal daily motion holds as the unique strictly dominant strategy.

### 3) Mathematical Formulae

#### Formula 2.1: Detrended Fluctuation Analysis (DFA) Chaos Scaling Exponent

For an inter-beat RR-interval time series $\{x_k\}_{k=1}^{N}$, compute the integrated cumulative deviation $Y(i) = \sum_{k=1}^{i} (x_k - \bar{x})$ and measure the root-mean-square fluctuation $F(n)$ around local piecewise linear trends $Y_n(i)$ across window scales $n$ to extract the scaling exponent $\alpha$:

$$
F(n) = \sqrt{\frac{1}{N} \sum_{i=1}^{N} \left( Y(i) - Y_n(i) \right)^2} \propto n^{\alpha}
$$

$$
\alpha = \frac{\Delta \ln F(n)}{\Delta \ln n}, \quad \mathbb{I}_{\text{DFA}}(\alpha) = \begin{cases} 1 & \text{if } 0.75 < \alpha < 1.25 \\ 0 & \text{otherwise} \end{cases}
$$

* Synthetic pink-noise generators and periodic mechanical actuators (such as washing machines or massage chairs) collapse toward uncorrelated white noise ($\alpha \approx 0.50$) or over-correlated Brownian motion ($\alpha \ge 1.50$), falling outside the physiological $1/f$ autonomic chaos band ($0.75 < \alpha < 1.25$) and triggering immediate rejection.

#### Formula 2.2: Electromechanical Delay (EMD) Muscle Response Verification

Physiological phase lag $T_{\text{rise}}$ between voluntary motor-cortex excitation onset (surface electromyography onset $t_{\text{EMG}}$) and mechanical muscle-belly expansion / mechanomyogram rise ($t_{\text{mech}}$):

$$
T_{\text{rise}} = t_{\text{mech}} - t_{\text{EMG}}, \quad \mathbb{I}_{\text{EMD}}(T_{\text{rise}}) = \begin{cases} 1 & \text{if } 80\text{ ms} \le T_{\text{rise}} \le 120\text{ ms} \\ 0 & \text{otherwise} \end{cases}
$$

* Under external electrical muscle stimulation (EMS) or passive mechanical compression/vibration, mechanical displacement either precedes neural firing ($T_{\text{rise}} \le 0$) or bypasses corticospinal conduction delays, falling outside the $80\text{--}120\text{ ms}$ physiological window and invalidating passive motion.

#### Formula 2.3: PoU V2.1 Composite Validity Scoring Function

Unified packet validity score $\text{PoU}_{\text{valid}}(t) \in [0, 1]$ combining hardware attestation, physiological gates, and GRF/IMU kinematic consistency $S_{\text{kin}} \in [0, 1]$:

$$
\text{PoU}_{\text{valid}}(t) = \mathbb{I}_{\text{PUF}} \cdot \mathbb{I}_{\text{DFA}}(\alpha) \cdot \mathbb{I}_{\text{EMD}}(T_{\text{rise}}) \cdot \text{clamp}_{[0, 1]}\left( w_{\text{kin}} \cdot S_{\text{kin}}(t) + w_{\text{bio}} \cdot \exp\left( -\lambda_{\alpha} \left\lvert \alpha - 1.0 \right\rvert - \lambda_{T} \frac{\left\lvert T_{\text{rise}} - 100 \right\rvert}{20} \right) \right)
$$

* $\mathbb{I}_{\text{PUF}} \in \{0, 1\}$: SoC Hardware Enclave cryptographic signature verification flag.
* $w_{\text{kin}} + w_{\text{bio}} = 1.0$ (standard weights: $w_{\text{kin}} = 0.40, \; w_{\text{bio}} = 0.60$), $\lambda_{\alpha} = 2.0, \; \lambda_{T} = 1.0$.
* Only packets satisfying $\text{PoU}_{\text{valid}}(t) \ge 0.80$ are accepted into the VLA foundation-model training pipeline and credited with R2S offsets.

### 4) Anti-Fragile Red-Teaming

1. **Attack Vector 2-A: Mechanical Mimicry Spoofing via Washing Machines, EMS, or Servo Robots**
   * **Attack Scenario:** Malicious actors attach wearable modules to washing machines, massage chairs, or programmed robotic arms to simulate human labor trajectories and farm mining credits at scale.
   * **Defensive Countermeasures:** Dual physical gating via Formula 2.1 (DFA chaos exponent $\alpha \in (0.75, 1.25)$) and Formula 2.2 (EMG-preceding delay $T_{\text{rise}} \in [80, 120]\text{ ms}$) eliminates periodic mechanical oscillation and external electrical stimulation ($100\%$ rejection). Additionally, longitudinal gait and postural entropy audits executed during the 90-day holdback period slash all accrued rewards upon detecting synthetic repetition.
2. **Attack Vector 2-B: PCB Probing & Synthetic Waveform Injection (Hardware Interposer Attack)**
   * **Attack Scenario:** Attackers disassemble wearable modules and splice a synthetic signal generator onto the trace between sensor transducers and the microcontroller.
   * **Defensive Countermeasures:** Sensing transducers and the TPM/PUF cryptographic core are co-packaged within a single monolithic silicon die (SoC Enclave), eliminating any external plaintext bus and physically preventing hardware interposer injection.

## 2.3 Boundary Conditions & Known Limitations (Open Issues)

* **Assumptions:** Electromechanical delay (EMD) gating in this chapter assumes users possess intact neuromuscular conduction pathways without implanted cardiac pacemakers or severe neurodegenerative conditions (designated welfare-exemption profiles apply to eligible individuals).
* **Failure Modes:**
  * Surface EMG electrode impedance spikes caused by heavy perspiration, dry skin, or body hair under the neck/lumbar patch, leading to intermittent signal dropout (See: [ISSUE-002](../../../issues/SOMA_ISSUES_EN.md)).
  * Temporary drift in DFA $\alpha$ estimates during high-intensity industrial motion artifacts (See: [ISSUE-003](../../../issues/SOMA_ISSUES_EN.md)).
* **Verification / PoC Requirements:** Field validation across $N = 100$ human subjects using monolithic SoC prototypes to verify False Rejection Rate ($\text{FRR} < 1.5\%$) and False Acceptance Rate ($\text{FAR} < 0.01\%$).

[← Chapter 1: Executive Summary & System Architecture](./01_system_topology.en.md) | [Back to Index](../../../README.md) | [Chapter 3: Autonomous B2B Task Guild →](./03_soma_guild_b2b.en.md)