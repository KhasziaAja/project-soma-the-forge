# Project SOMA × The Forge

[English](#english) | [日本語](#japanese)

---

<a name="english"></a>
## ── English Overview

**Autonomous Physical-Sensing Infrastructure & Negative-IP Liquidation Engine**

A unified, self-sovereign systemic framework combining edge physical sensing, a dual-mode legal fortress, and a negative-IP clearance protocol.

### Architecture Highlights

* **Project SOMA (Physical-AI OS):** Zero-touch physical data mining, DFA chaos verification, asynchronous B2B task relay, and dual-mode legal boundaries (Japan Civil Code Art. 593 / US 1099-NEC).
* **The Forge:** Clearance protocol for negative IP (failed research logs), topological state transition (SATORI Engine), zero-knowledge proof binding, and dynamic royalty distribution (D&A Model).

---

### Repository Architecture

```text
repository-root/
├── README.md                              # Unified Master Portal (SOMA & The Forge)
├── ARCHITECT_NOTES.md                     # System Architect Behavioral Parameters
├── DISCLAIMER.md                          # Research & Legal Disclaimers
├── LICENSE                                # MIT License
├── docs/
│   ├── soma/
│   │   ├── ja/                            # Project SOMA Japanese Specs (6 files)
│   │   │   ├── 01_system_topology.ja.md
│   │   │   ├── 02_paios_physical_mining.ja.md
│   │   │   ├── 03_soma_guild_b2b.ja.md
│   │   │   ├── 04_tokenomics_finance.ja.md
│   │   │   ├── 05_legal_fortress_0yen.ja.md
│   │   │   └── 06_resilience_nasa_mode.ja.md
│   │   └── en/                            # Project SOMA English Specs (6 files)
│   │       ├── 01_system_topology.en.md
│   │       ├── 02_paios_physical_mining.en.md
│   │       ├── 03_soma_guild_b2b.en.md
│   │       ├── 04_tokenomics_finance.en.md
│   │       ├── 05_legal_fortress_0yen.en.md
│   │       └── 06_resilience_nasa_mode.en.md
│   └── forge/
│       ├── ja/                            # The Forge Japanese Specs (8 files)
│       │   ├── 01_system_overview.ja.md
│       │   ├── 02_satori_zkp.ja.md
│       │   ├── 03_proof_of_trust.ja.md
│       │   ├── 04_tokenomics.ja.md
│       │   ├── 05_d_and_a_model.ja.md
│       │   ├── 06_governance.ja.md
│       │   ├── 07_the_pulse.ja.md
│       │   └── 08_legal_compliance.ja.md
│       └── en/                            # The Forge English Specs (8 files)
│           ├── 01_system_overview.en.md
│           ├── 02_satori_zkp.en.md
│           ├── 03_proof_of_trust.en.md
│           ├── 04_tokenomics.en.md
│           ├── 05_d_and_a_model.en.md
│           ├── 06_governance.en.md
│           ├── 07_the_pulse.en.md
│           └── 08_legal_compliance.en.md
└── issues/
    ├── SOMA_ISSUES_JP.md                  # SOMA Priority Friction Points (JP)
    ├── SOMA_ISSUES_EN.md                  # SOMA Priority Friction Points (EN)
    ├── FORGE_ISSUES_JP.md                 # The Forge Audit Issues (JP)
    └── FORGE_ISSUES_EN.md                 # The Forge Audit Issues (EN)
```

---

### System Architecture Diagrams

#### 1. Project SOMA Topology

```mermaid
flowchart TD
    subgraph PAIOS_LAYER [1. PAIOS 物理・エッジセンシング層]
        USER[作業員 / 居住者]

        subgraph SENSORS [センサモジュール]
            INSOLE[Smart Insole]
            IMU[消しゴムサイズ 9軸IMU]
            PATCH[首裏/腰パッチセンサ]
        end

        subgraph RED_TEAM [Red-Teaming 防壁]
            ENCLAVE[SoC内包型 Hardware Enclave]
            DFA[DFA カオス検証回路]
            RISETIME[筋伝導 Response Rise Time 検証]
            POU_CHECK{PoU 総合妥当性判定}
            HOLDBACK[3ヶ月 報酬留保 Holdback]
        end

        subgraph STORE_FORWARD [ストア＆フォワード]
            MEM[エッジ NOR Flash 512MB]
            MESH[自律メッシュネットワーク]
        end
    end

    subgraph DUAL_LEGAL [3. Dual-Mode Legal Fortress]
        subgraph JP_MODE [日本モード - 完全分離境界]
            ENT_A[Entity A: 住居管理法人 民法593条]
            ENT_B[Entity B: データ買収法人]
            LOCK_API[スマートロックAPI主権]
            ENT_A -. 法的・資本分離 .-> ENT_B
            LOCK_API ==> ENT_A
        end
    end

    USER --> INSOLE & IMU & PATCH
    INSOLE & IMU & PATCH --> ENCLAVE --> DFA --> RISETIME --> POU_CHECK
```

#### 2. The Forge Pipeline

```mermaid
flowchart TD
    subgraph SG1 [1. 生データ投入 & コア秘匿層]
        RAW[生の失敗データ FDR-V2 ログ RAW Memo] --> CLEANSING[SATORI AI 文脈剥離エンジン]
        CLEANSING --> MANIFOLD[主成分多様体圧縮 100次元超 ➔ 3-5次元物理多様体 z]
    end

    subgraph SG2 [2. 信頼証明 & ROOTS TFA 3層監査回路]
        MANIFOLD --> TFA_NNS{TFA 第一フィルター: NNS ノイズ監査}
        TFA_NNS -- Pass: ベンフォード法則適合 --> TFA_PT{TFA 第二フィルター: PT 因果監査}
        TFA_NNS -- Fail: ガウス過度適合 --> SLASH[Stake & Slash 実行 預託没収]

        TFA_PT -- Pass: 仮説-結論距離 適合 --> TFA_AO{TFA 第三フィルター: AO 物理実存証明}
        TFA_PT -- Fail: AI自動生成 --> SLASH

        TFA_AO -- Pass: 熱 電力波形 TPM PUF一致 --> ROOTS_HASH[ROOTS Trust-Hash 鑑定書発行 R_score 確定]
        TFA_AO -- Fail: 物理レシート不在 --> SLASH
    end

    subgraph SG3 [3. パブリック層 & D-LOG 1:3 物々交換]
        ROOTS_HASH --> ZKP_GEN[ZKP ゼロ知識証明バインディング Groth16]
        ZKP_GEN --> DLOG_SELECT{D-LOG 二重通貨プロトコル}

        DLOG_SELECT -- TFA ROOTS合格品 --> DLOG_CORE[D-LOG-Core 可換高位トークン]
        DLOG_SELECT -- Tier 3 C2Cジャンク品 --> DLOG_FLEA[D-LOG-Flea 1:3 物々交換専用]

        DLOG_CORE --> DYN_PRICING[動的プライシング算定式 P_total]

        DYN_PRICING --> TIER1[Tier 1 0-72h 限定独占オークション]
        TIER1 -- 72h 経過 --> TIER2[Tier 2 72h-30d 準一般ライセンス]
        TIER2 -- 30d 経過 --> TIER3[Tier 3 30d+ パブリック AIコモンズ]
    end

    subgraph SG4 [4. SATORI 補空間相転移 & D&A 動的ロイヤリティ分配]
        ZKP_GEN --> SAT_DENSITY[失敗包囲率 S_rate リアルタイムスキャン]
        SAT_DENSITY --> SAT_CRIT{臨界点判定 S_rate >= 0.99}

        SAT_CRIT -- 未飽和 --> TIER1
        SAT_CRIT -- 臨界点超過 相転移発動 --> RECIPE_GEN[補空間微分 成功レシピ自動導出 x*]

        RECIPE_GEN --> PRIOR_ART[Prior Art オンチェーン刻印 先行技術公知化]
        PRIOR_ART --> DNA_MODEL[D&A 開発モデル The Forge ✕ IPEX 結合]

        DNA_MODEL --> DAG_BUILD[有向非巡回グラフ DAG 依存関係構築]
        DAG_BUILD --> ROY_SPLIT[Dynamic Royalty Split 自動一括分配]

        ROY_SPLIT --> MILD_FMP[マイルド FMP v2.0 パトロン還元 ROI Cap 200%]
    end

    subgraph SG5 [5. The Pulse リアルタイム予測 & 異常波形遮断]
        ROOTS_HASH --> PULSE_STREAM[The Pulse テレメトリ ストリーム]
        PULSE_STREAM --> PULSE_ZCAP{動的 Zスコア上限キャップ Z_i > 3.0}

        PULSE_ZCAP -- 人工スパイク --> PULSE_TRUNC[過剰波形切捨て 相場操縦ブロック]
        PULSE_ZCAP -- 正常波形 --> PULSE_TOPO{SATORI トポロジー相転移監査}

        PULSE_TOPO -- 位相変化なし --> PULSE_REJ[偽クエンチ拒否 シグナル不活性化]
        PULSE_TOPO -- 位相変化確定 --> PULSE_SIG[真の市場予測シグナル P_signal 出力]
    end

    subgraph SG6 [6. EU適合 4フェーズ タイムロック ガバナンス回路]
        SLASH --> GOV_TRIG[不正検知 焦土作戦トリガー発動]
        GOV_TRIG --> GOV_P1{Phase 1: 着信確認 ACK 14日間}

        GOV_P1 -- ACKなし --> SCORCHED[レベル3: 焦土作戦 自動執行 権利永久ロック]
        GOV_P1 -- ACK完了 --> GOV_P2{Phase 2: フォレンジック調査 Cure 30日間}

        GOV_P2 -- Cure Fee 決済 --> CURE_SOFT[レベル2: 事後ライセンス和解 Soft-Landing]
        GOV_P2 -- 反証なし 和解拒否 --> SCORCHED
        GOV_P2 -- 反証データ提出 --> GOV_P3{Phase 3: IQ-FTC 国際審理 21日間}

        GOV_P3 -- 5/9以上 拒否権投票 --> VETO_FREE[凍結解除 バグ修正]
        GOV_P3 -- 拒否権不成立 --> GOV_P4[Phase 4: サイレント承認 デフォルト執行]

        GOV_P4 --> AUDIT_TRAIL[EU-Audit Trail 暗号化レポート生成]
        AUDIT_TRAIL --> SCORCHED
    end
```

---

### Repository Navigation (Modular Specifications)

#### 1. Project SOMA Specification (Physical-AI OS & Social Infrastructure)

| Module / Chapter | Focus Area | Japanese (日本語) | English (英語) |
| :--- | :--- | :--- | :--- |
| **Ch.1 System Topology** | 総論・存在の価値化・ESG/SIB | [01_system_topology.ja.md](./docs/soma/ja/01_system_topology.ja.md) | [01_system_topology.en.md](./docs/soma/en/01_system_topology.en.md) |
| **Ch.2 PAIOS Edge Mining** | 物理センシング・PoU V2.1・DFAカオス | [02_paios_physical_mining.ja.md](./docs/soma/ja/02_paios_physical_mining.ja.md) | [02_paios_physical_mining.en.md](./docs/soma/en/02_paios_physical_mining.en.md) |
| **Ch.3 SOMA Guild B2B** | 24時間時差リレー・FDR-V2文脈剥離 | [03_soma_guild_b2b.ja.md](./docs/soma/ja/03_soma_guild_b2b.ja.md) | [03_soma_guild_b2b.en.md](./docs/soma/en/03_soma_guild_b2b.en.md) |
| **Ch.4 Tokenomics & Finance** | R2S現物レバレッジ・CWE・GPR指数 | [04_tokenomics_finance.ja.md](./docs/soma/ja/04_tokenomics_finance.ja.md) | [04_tokenomics_finance.en.md](./docs/soma/en/04_tokenomics_finance.en.md) |
| **Ch.5 Legal Fortress & 0-Yen** | 日米デュアル法務・民法593条・0円生活 | [05_legal_fortress_0yen.ja.md](./docs/soma/ja/05_legal_fortress_0yen.ja.md) | [05_legal_fortress_0yen.en.md](./docs/soma/en/05_legal_fortress_0yen.en.md) |
| **Ch.6 Resilience & NASA Mode** | 4段階縮退・救済オラクル・創発バウンティ | [06_resilience_nasa_mode.ja.md](./docs/soma/ja/06_resilience_nasa_mode.ja.md) | [06_resilience_nasa_mode.en.md](./docs/soma/en/06_resilience_nasa_mode.en.md) |

#### 2. The Forge Specification (Negative-IP Liquidation Engine)

| Module / Chapter | Focus Area | Japanese (日本語) | English (英語) |
| :--- | :--- | :--- | :--- |
| **Ch.1 System Overview** | アーキテクチャ総論・知能金融 | [01_system_overview.ja.md](./docs/forge/ja/01_system_overview.ja.md) | [01_system_overview.en.md](./docs/forge/en/01_system_overview.en.md) |
| **Ch.2 SATORI & ZKP** | 文脈剥離・補空間相転移アルゴリズム | [02_satori_zkp.ja.md](./docs/forge/ja/02_satori_zkp.ja.md) | [02_satori_zkp.en.md](./docs/forge/en/02_satori_zkp.en.md) |
| **Ch.3 Proof-of-Trust** | ROOTS物理代償・TFA 3層監査回路 | [03_proof_of_trust.ja.md](./docs/forge/ja/03_proof_of_trust.ja.md) | [03_proof_of_trust.en.md](./docs/forge/en/03_proof_of_trust.en.md) |
| **Ch.4 Tokenomics** | D-LOG二重通貨・3軸動的境界線 | [04_tokenomics.ja.md](./docs/forge/ja/04_tokenomics.ja.md) | [04_tokenomics.en.md](./docs/forge/en/04_tokenomics.en.md) |
| **Ch.5 D&A Model** | 開発モデル・動的ロイヤリティ分配 | [05_d_and_a_model.ja.md](./docs/forge/ja/05_d_and_a_model.ja.md) | [05_d_and_a_model.en.md](./docs/forge/en/05_d_and_a_model.en.md) |
| **Ch.6 Governance** | ガバナンス防腐・EU適合4フェーズ回路 | [06_governance.ja.md](./docs/forge/ja/06_governance.ja.md) | [06_governance.en.md](./docs/forge/en/06_governance.en.md) |
| **Ch.7 The Pulse** | 予測インデックス・市場操作防腐 | [07_the_pulse.ja.md](./docs/forge/ja/07_the_pulse.ja.md) | [07_the_pulse.en.md](./docs/forge/en/07_the_pulse.en.md) |
| **Ch.8 Legal Compliance** | 地政学・知財金庫シェルター・輸出管理 | [08_legal_compliance.ja.md](./docs/forge/ja/08_legal_compliance.ja.md) | [08_legal_compliance.en.md](./docs/forge/en/08_legal_compliance.en.md) |

---

### Domain-Specific Gateways (For External Specialists)

* 📊 **For Quants & Tokenomics Designers:**
  $\to$ **SOMA:** [Ch.4 Tokenomics & Finance](./docs/soma/en/04_tokenomics_finance.en.md) | **Forge:** [Ch.4 Tokenomics](./docs/forge/en/04_tokenomics.en.md) & [Ch.5 D&A Model](./docs/forge/en/05_d_and_a_model.en.md)
  *(R2S in-kind leverage, clamped GPR hedge pools, dynamic Proof-of-Burn, DAG royalty split, Mild FMP v2.0)*
* 🔐 **For Cryptographers, Hardware & Security Engineers:**
  $\to$ **SOMA:** [Ch.2 PAIOS Edge Mining](./docs/soma/en/02_paios_physical_mining.en.md) | **Forge:** [Ch.2 SATORI & ZKP](./docs/forge/en/02_satori_zkp.en.md) & [Ch.3 Proof-of-Trust](./docs/forge/en/03_proof_of_trust.en.md)
  *(Monolithic SoC enclaves, DFA chaos & EMD gating, Groth16 stealth queries, PUF/SGX physical receipts, TFA 3-tier audit)*
* ⚖️ **For Legal Engineers & Regulatory Analysts:**
  $\to$ **SOMA:** [Ch.5 Legal Fortress & 0-Yen](./docs/soma/en/05_legal_fortress_0yen.en.md) | **Forge:** [Ch.6 Governance](./docs/forge/en/06_governance.en.md) & [Ch.8 Legal Compliance](./docs/forge/en/08_legal_compliance.en.md)
  *(JP Civil Code Sec. 593 corporate separation, US AB5 / HUD FMR 1099 automation, EU AI Act 4-phase timelock, Wassenaar dual-use quarantine, multi-polar IP Vault)*
* 🧠 **For AI & Complex Systems Researchers:**
  $\to$ **SOMA:** [Ch.3 SOMA Guild B2B](./docs/soma/en/03_soma_guild_b2b.en.md) & [Ch.6 Resilience](./docs/soma/en/06_resilience_nasa_mode.en.md) | **Forge:** [Ch.2 SATORI & ZKP](./docs/forge/en/02_satori_zkp.en.md) & [Ch.7 The Pulse](./docs/forge/en/07_the_pulse.en.md)
  *(Manifold reduction $\mathbb{R}^D \to \mathbb{R}^d$, complementary space phase transition, bounded task re-chunking, NASA Safe-Mode degradation, clamped Z-score market signals)*

---

### Audit Issues & Core Governance

| Category | Japanese (日本語) | English (英語) |
| :--- | :--- | :--- |
| **SOMA Audit Issues** | [SOMA_ISSUES_JP.md](./issues/SOMA_ISSUES_JP.md) | [SOMA_ISSUES_EN.md](./issues/SOMA_ISSUES_EN.md) |
| **The Forge Audit Issues** | [FORGE_ISSUES_JP.md](./issues/FORGE_ISSUES_JP.md) | [FORGE_ISSUES_EN.md](./issues/FORGE_ISSUES_EN.md) |
| **Architect Manifesto** | [ARCHITECT_NOTES.md](./ARCHITECT_NOTES.md) | [ARCHITECT_NOTES.md](./ARCHITECT_NOTES.md) |
| **Disclaimer & License** | [DISCLAIMER.md](./DISCLAIMER.md) | [DISCLAIMER.md](./DISCLAIMER.md) |

---

<a name="japanese"></a>
## ── 日本語概要

本プロジェクトは、人間の悪意や制度のバグを前提とし、物理・金融・法務の3レイヤーで防腐化された自律型社会インフラ・システムです。

* **Project SOMA:** 物理行動データ採掘（PAIOS）、24時間時差B2Bギルド、および民法593条（使用貸借）・米国AB5法適合による法的実効境界（全6章）。
* **The Forge:** 失敗データ（負の知財）のSATORI補空間相転移および暗号学的動的ロイヤリティ分配エンジン（全8章）。

各専門領域のモジュール（計28ファイル・日英完全対応）および課題リストは、上記の **Repository Navigation** テーブルより直接参照できます。

---

## ── Maintenance & Response Protocol (連絡・応答方針)

### English
* **Communication Style:** Asynchronous & Irregular.
* **Availability:** Responses to Issues, Discussions, and Pull Requests are handled on an irregular basis (primarily during weekends) depending on capacity.
* **Policy:** Urgent demands or sentimental pressure will be ignored. Contributions and critical technical feedback are always reviewed asynchronously when bandwidth permits.

### 日本語
* **通信スタイル:** 完全非同期・不定期応答。
* **稼働状態:** Issue、Discussion、Pull Request への確認・返信は、本業およびプライベートの合間（主に週末）に不定期で行われます。
* **方針:** 返信の催促や情緒的な要求には対応しません。提案や技術的フィードバックは、リソースが確保できたタイミングで非同期に確認します。

---

## ── License & Disclaimer

* **License:** MIT License
* **Disclaimer:** See [DISCLAIMER.md](./DISCLAIMER.md) for research & legal disclaimers.