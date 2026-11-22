# Chapter 2 SATORI Complementary Space Phase Transition Algorithm and ZKP Context Detachment Engine

[← Chapter 1 System Overview](./01_system_overview.en.md) | [Back to Index](../../README.md) | [Chapter 3 Proof-of-Trust Layer: ROOTS & TFA →](./03_proof_of_trust.en.md)

## Overview & Purpose

This module defines the specifications of the SATORI engine—which completely strips social and organizational context (such as corporate names, patent exposure, and reputational risks) from raw corporate experimental and failure logs (ingredient labeling) to perform manifold compression in high-dimensional parameter spaces and complementary space phase transitions (automated derivation of successful recipes)—alongside the Zero-Knowledge Proof (ZKP) pipeline enabling stealth queries.

## 2.1 Algorithmic Principles and Geometric Interpretation

SATORI is an AI engine that completely detaches social and organizational contexts—such as "company names," "trademarks," "patent risks," and "reputational risks"—from submitted raw experimental and failure data, abstracting them into 100% pure "physical and logical boundary conditions (Ingredient Labeling)."

```text
[ RAW Failure Logs / Lab Notes ] ──(Contains corporate/personal names & jargon)
              │
              ▼
┌────────────────────────────────────────────────────────┐
│ 1. Context Detachment                                  │
│   ・Complete purge of named entities & sensitive styles│
│     via NER/LLM                                        │
│   ・Dimensionless tensor transformation of physical    │
│     quantities and boundary conditions                 │
└─────────────────────────┬──────────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────────┐
│ 2. Principal Manifold Reduction (R^D -> R^d)           │
│   ・Auto-discarding irrelevant noise dimensions via    │
│     Sobol sensitivity analysis                         │
│   ・Topology-preserving compression into a 3–5D        │
│     essential physical manifold z                      │
└─────────────────────────┬──────────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────────┐
│ 3. Complementary Space Phase Transition                │
│   ・Monitoring spatial enclosure rate S_rate via       │
│     failure balls K_fail                               │
│   ・Upon S_rate >= 0.99, differentially deriving the   │
│     unique success solution x* passing through the gap │
│   ・Immediate on-chain timestamping as Prior Art       │
└────────────────────────────────────────────────────────┘
```

## 2.2 Data Schema & Pipeline

We define the data structures for raw data input, feature vectors post-context detachment (ingredient labeling), and ZKP proof payloads.

### Schema 2.1: `RAW_ExperimentLog_Schema`

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "RAW_ExperimentLog_Schema",
  "type": "object",
  "properties": {
    "raw_log_id": { "type": "string", "format": "uuid" },
    "organization_metadata": {
      "type": "object",
      "properties": {
        "company_name": { "type": "string" },
        "researcher_ids": { "type": "array", "items": { "type": "string" } },
        "project_codename": { "type": "string" }
      },
      "required": ["company_name", "researcher_ids", "project_codename"]
    },
    "experimental_parameters": {
      "type": "object",
      "properties": {
        "temperature_kelvin": { "type": "number", "minimum": 0.0 },
        "pressure_pascal": { "type": "number", "minimum": 0.0 },
        "chemical_formula_raw": { "type": "string" },
        "voltage_volts": { "type": "number" }
      },
      "required": ["temperature_kelvin", "pressure_pascal", "chemical_formula_raw"]
    },
    "execution_result": {
      "type": "object",
      "properties": {
        "is_success": { "type": "boolean", "enum": [false] },
        "error_code": { "type": "string" },
        "raw_output_telemetry": { "type": "string" }
      },
      "required": ["is_success", "error_code", "raw_output_telemetry"]
    }
  },
  "required": ["raw_log_id", "organization_metadata", "experimental_parameters", "execution_result"]
}
```

### Schema 2.2: `SATORI_IngredientVector_Schema` (ZKP Ingredient Labeling Payload)

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "SATORI_IngredientVector_Schema",
  "type": "object",
  "properties": {
    "ingredient_id": { "type": "string", "pattern": "^ING-[a-f0-9]{64}$" },
    "manifold_dimension_d": { "type": "integer", "minimum": 3, "maximum": 5 },
    "latent_vector_z": {
      "type": "array",
      "items": { "type": "number" },
      "minItems": 3,
      "maxItems": 5
    },
    "roots_trust_score": { "type": "number", "minimum": 0.0, "maximum": 1.0 },
    "zkp_proof": {
      "type": "object",
      "properties": {
        "protocol": { "type": "string", "enum": ["groth16", "plonk"] },
        "proof_bytes_hex": { "type": "string", "pattern": "^0x[a-f0-9]+$" },
        "public_signals_commitment": { "type": "string", "pattern": "^0x[a-f0-9]{64}$" }
      },
      "required": ["protocol", "proof_bytes_hex", "public_signals_commitment"]
    }
  },
  "required": ["ingredient_id", "manifold_dimension_d", "latent_vector_z", "roots_trust_score", "zkp_proof"]
}
```

## 2.3 Game Theory & Incentives

We present the game-theoretic design between buyer enterprises and data-providing nodes regarding ZKP stealth matching and rights acquisition over phase-transition outputs.

Data-purchasing enterprises face insider risks where their R&D roadmaps (search queries) leak externally. Because SATORI executes ZKP reverse matching (Stealth Query) using an encrypted query $\mathbf{q}$, enterprises can verify whether they are about to step onto a "landmine (known failure region)" while keeping their search intent strictly confidential.

Meanwhile, for data providers ($N$ nodes), even discarded logs that hold zero standalone value automatically earn perpetual royalties (lump-sum distribution pool) proportional to their Bottleneck Resolution Score (BRS) the moment they combine with other companies' failure logs to cross the phase-transition critical point (enclosure rate $S_{\text{rate}} \ge 0.99$).

**Non-Selfish Optimal Behavioral Policy for Each Data-Providing Node:**

* **Concealing or discarding raw failure data** $\rightarrow$ Payoff $0$
* **Immediately submitting cleansed data via SATORI** $\rightarrow$ Acquires proportional reward $\sum R_{\text{split}} \cdot w_i > 0$ upon phase transition

## 2.4 Mathematical Formula

We establish the geometric calculation formulas for "high-dimensional manifold compression" and "complementary space phase transition (automated success recipe generation)," which form the core of the SATORI engine.

### Formula 2.1: Principal Manifold Dimension Reduction

Encoder function $f_{\theta}$ compressing high-dimensional parameters $\mathbf{x}_{\text{raw}} \in \mathbb{R}^D$ ($D > 100$) into an essential physical manifold $\mathbf{z} \in \mathbb{R}^d$ ($d \ll D$, typically $d \in [3, 5]$):

$$\mathbf{z} = f_{\theta}(\mathbf{x}_{\text{raw}})$$

$$\min_{\theta, \phi} \mathbb{E} \left[ \left\lVert \mathbf{x}_{\text{raw}} - g_{\phi}(f_{\theta}(\mathbf{x}_{\text{raw}})) \right\rVert^2 \right] + \mu \cdot \mathcal{L}_{\text{topology}}(f_{\theta})$$

* $g_{\phi}$: Decoder function
* $\mathcal{L}_{\text{topology}}$: Topology-preserving regularization term (Persistent Homology loss)
* $\mu$: Regularization hyperparameter ($\mu > 0$)

### Formula 2.2: Enclosure Limit of Failure Convex/Non-Convex Hulls and Saturation Judgment (Regularized & Bounded)

Absolute no-go zone $\mathcal{K}_{\text{fail}}$ formed in parameter space $\Omega \subset \mathbb{R}^d$ by verified failure data point cloud $\mathcal{S}_{\text{fail}} = \{\mathbf{z}_1, \mathbf{z}_2, \dots, \mathbf{z}_N\}$:

$$\mathcal{K}_{\text{fail}} = \bigcup_{i=1}^{N} B_{\epsilon_i}(\mathbf{z}_i), \quad \epsilon_i = r_0 \cdot (\text{ROOTS}_i)^k$$

Regularized definition of the Failure Saturation Rate $S_{\text{rate}} \in [0, 1]$ in spatial domain $\Omega$:

$$S_{\text{rate}} = \min \left( 1.0, \; \frac{\text{Vol}(\mathcal{K}_{\text{fail}} \cap \Omega)}{\max \left( \text{Vol}(\Omega) - \text{Vol}(\mathcal{X}_{\text{core}}), \; \epsilon_{\text{vol}} \right)} \right)$$

* $B_{\epsilon_i}(\mathbf{z}_i)$: Failure ball centered at $\mathbf{z}_i$ with radius $\epsilon_i$ scaled by the ROOTS score ($r_0 > 0, \; k = 3$)
* $\text{Vol}(\cdot)$: Hypervolume by Lebesgue measure on $\mathbb{R}^d$
* $\mathcal{X}_{\text{core}}$: Physically viable unexplored core (complementary space, $\mathcal{X}_{\text{core}} \subset \Omega$)
* $\epsilon_{\text{vol}}$: Regularization lower-bound constant preventing denominator zero-division and oversaturation divergence ($\epsilon_{\text{vol}} = 10^{-6} \cdot \text{Vol}(\Omega)$)

### Formula 2.3: Direct Calculation of Complementary Space Phase Transition (Success Recipe)

Optimization formula directly deriving the unique successful parameter set $\mathbf{x}^*$ slipping through gaps in the failure wall via potential field minimization (reciprocal maximization) when saturation rate $S_{\text{rate}} \ge 0.99$ (exceeding the critical threshold) and $\Omega \setminus \mathcal{K}_{\text{fail}} \neq \emptyset$:

$$\mathbf{x}^* = \arg\max_{\mathbf{x} \in \Omega \setminus \mathcal{K}_{\text{fail}}} \left[ \sum_{i=1}^{N} \frac{(\text{ROOTS}_i)^k}{\left\lVert \mathbf{x} - \mathbf{z}_i \right\rVert^2 + \delta} \right]^{-1}$$

* $\delta$: Numerical stabilization constant preventing zero-division and gradient explosion upon coincidence with failure coordinates ($\mathbf{x} \to \mathbf{z}_i$, $\delta = 10^{-8}$)
* $k$: ROOTS trust weight exponent ($k = 3$, suppressing potential distortion from low-trust data)

## 2.5 Anti-Fragile Red-Teaming

We present representative attacks and vulnerabilities anticipated against the SATORI engine along with their algorithmic defense valves.

1. **Attack Vector: Curse of Dimensionality Attack**
   * **Method:** Artificially inflating parameter dimensionality $D$ (adding massive numbers of irrelevant environmental variables) to explode spatial volume $\text{Vol}(\Omega)$, preventing the system from ever reaching the phase-transition critical point ($S_{\text{rate}} \ge 0.99$).
   * **Defense Valve:** Enforcing "Manifold Learning" and Sobol sensitivity analysis as a preprocessing step in Formula 2.1, automatically discarding dimensions whose contribution rate to the target variable falls below threshold $\tau < 0.01$. Strictly fixing the compressed dimension $d$ to $3\text{--}5$ dimensions blocks geometric collapse.
2. **Attack Vector: Phantom Hits Overwriting Attack**
   * **Method:** Malicious nodes plot massive amounts of fabricated failure data over "coordinates that actually succeed," concealing the true parameter set $\mathbf{x}^*$ beneath failure balls $\mathcal{K}_{\text{fail}}$.
   * **Defense Valve:** Integrating a "TFA Probabilistic Eraser" into the phase-transition calculation model. Data with $\text{ROOTS}_i < 0.85$ has its evaluation radius attenuated toward $\epsilon_i \to 0$. Furthermore, if conflicting reports (successful outcomes or superior technologies) arrive from independent nodes, the corresponding failure coordinates are automatically purged and manifold re-topology is dynamically executed.
3. **Attack Vector: Preemptive Patent Filing of Auto-Derived Recipes by Mega-Tech (Patent Troll Attack)**
   * **Method:** Mega-tech corporations monitor successful recipes $\mathbf{x}^*$ automatically derived by SATORI and immediately file international patent applications under their own names to monopolize them.
   * **Defense Valve:** Within milliseconds of the phase-transition algorithm outputting $\mathbf{x}^*$, its parameter hash is automatically stamped onto the public ledger (IP-CLEAR / GIP-NET) as "Prior Art." Consequently, any third-party patent application fails the novelty requirement under international law (PCT) and is 100% structurally invalidated.

## 2.6 Boundary Conditions & Known Limitations (Open Issues)

* **Assumptions:** The complementary space phase transition in this chapter assumes that physical laws in the target technological domain are continuous and that the failure region $\mathcal{K}_{\text{fail}}$ can be approximated as a connected closed set within the compressed latent space $\mathbb{R}^d$.
* **Failure Modes:**
  * Inability to reach the $S_{\text{rate}} \ge 0.99$ critical threshold in ultra-niche domains due to data sparsity (See: [ISSUE-009](../../issues/FORGE_ISSUES_EN.md)).
  * Increased prover-side computational overhead resulting from growing circuit constraints in ZKP stealth matching (Groth16) (See: [ISSUE-008](../../issues/FORGE_ISSUES_EN.md)).
* **Verification / PoC Requirements:** Quantitative evaluation of $\mathbf{x}^*$ convergence accuracy and false-positive rates from non-convex failure regions in synthetic benchmark spaces ($d = 3, 5$).

[← Chapter 1 System Overview](./01_system_overview.en.md) | [Back to Index](../../README.md) | [Chapter 3 Proof-of-Trust Layer: ROOTS & TFA →](./03_proof_of_trust.en.md)