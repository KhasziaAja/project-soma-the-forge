# 第1章 システム総論：知能金融（Intelligent Finance）市場とアンチフラジャイル設計

[← 目次に戻る](../../README.md) | [第2章 SATORI補空間相転移とZKP文脈剥離 →](./02_satori_zkp.ja.md)

---

## 概要と目的

本モジュールは、従来廃棄されてきた企業の失敗ログ・ボツ実験データ（負の知財）を匿名構造化し、文明のショートカット権として流動化・清算する知能金融プラットフォーム「The Forge」の全体アーキテクチャを定義する。パブリック層（`IPEX/L1-L2`）とコア秘匿層（`SATORI/FDR-V2/L3`）の物理・論理分離構造、および内部告発を誘発するゲーム理論的防腐メカニズムを確立する。

---

## 1.1 システム定義と基本アーキテクチャ

「The Forge」は、従来破棄されてきた「負の知財（失敗データ、ボツ実験、破壊ログ、境界条件ログ）」をゼロ知識証明（ZKP: Zero-Knowledge Proof）および AI エンジン（SATORI）を用いて匿名構造化し、文明のショートカット権として流動化・清算する世界初の知能金融（Intelligent Finance）プラットフォームである。

本システムは、パブリック層（`IPEX/L1-L2`）とコア秘匿層（`SATORI/FDR-V2/L3`）を論理的・物理的に分離するデュアル構造を採用する。

```text
[ 入力: 生の失敗ログ / RAWメモ / センサーログ ]
                        │
                        ▼
┌────────────────────────────────────────────────────────┐
│ コア秘匿層 (SATORI / FDR-V2 / L3)                      │
│ ・文体パラフレーザー                                   │
│ ・TFA 物理ログ / ROOTS 検証                            │
│ ・SATORI 特徴量抽出 / 機密テンソル置換                 │
└───────────────────────┬────────────────────────────────┘
                        │
                        ▼ (ZKP暗号化成分表示)
┌────────────────────────────────────────────────────────┐
│ パブリック層 (IPEX / L1-L2 / DEX)                      │
│ ・D-LOG 1:3 物々交換 & 決済                            │
│ ・Dynamic Pricing & オークション                       │
│ ・The Pulse インデックス公開                           │
└────────────────────────────────────────────────────────┘
```

---

## 1.2 Data Schema & Pipeline

本システムにおける全体システム構成設定のデータ構造を以下に定義する。

### スキーマ 1.1: `SystemArchitectureConfig`

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "SystemArchitectureConfig",
  "type": "object",
  "properties": {
    "network_layer": {
      "type": "string",
      "enum": ["L1_Public_DEX", "L2_Industry_Zone", "L3_Strategic_Sanctuary_Darkpool"]
    },
    "access_control": {
      "type": "object",
      "properties": {
        "zkp_verifier_endpoint": { "type": "string", "format": "uri" },
        "encryption_algorithm": { "type": "string", "default": "AES-256-GCM" },
        "key_derivation": { "type": "string", "default": "Argon2id" }
      },
      "required": ["zkp_verifier_endpoint", "encryption_algorithm", "key_derivation"]
    },
    "data_pipeline_isolation": {
      "type": "object",
      "properties": {
        "public_layer_exposure": { "type": "boolean", "default": false },
        "raw_tensor_storage": { "type": "string", "enum": ["Encrypted_IPFS", "Arweave_Vault"] }
      },
      "required": ["public_layer_exposure", "raw_tensor_storage"]
    }
  },
  "required": ["network_layer", "access_control", "data_pipeline_isolation"]
}
```

---

## 1.3 Game Theory & Incentives

MICE（Money, Ideology, Compromise, Ego）フレームワークおよび Prospect Theory（プロスペクト理論）に基づく内部告発・成功隠蔽攻撃対抗のゲーム理論的利得構造（Payoff Matrix）を構築する。

組織内部者 $A$ および $B$ が存在するとき、ブラックボックス成功隠蔽に対する利得（懸賞金 $R_{\text{bounty}}$ vs 企業の口止め料 $C_{\text{bribe}}$）の構造は以下の通り定義される。

| | 内部者 B: 告発する（裏切り） | 内部者 B: 沈黙する（協調） |
| :--- | :---: | :---: |
| **内部者 A: 告発する（裏切り）** | $\left(\frac{R_{\text{bounty}}}{2}, \frac{R_{\text{bounty}}}{2}\right)$ | $(R_{\text{bounty}}, -P_{\text{penalty}})$ |
| **内部者 A: 沈黙する（協調）** | $(-P_{\text{penalty}}, R_{\text{bounty}})$ | $(C_{\text{bribe}}, C_{\text{bribe}})$ |

* **条件設定:** $R_{\text{bounty}} \gg C_{\text{bribe}} > 0$ かつ $P_{\text{penalty}} \gg C_{\text{bribe}}$
* **制度設計:** 最速の $1$ 人目（ファースト・ムーバー）のみが懸賞金 $R_{\text{bounty}}$ を $100\%$ 独占取得するルール（Winner-Take-All）をブロックチェーン上で強制適用する。

これにより、他者の告発タイミングに対する不確実性が存在する環境下において、各アクターの最適戦略は「相手に先越される前に即座に ZKP 匿名告発を行う」ことに収束し、ナッシュ均衡（裏切り、裏切り）が唯一の厳密支配的戦略（Strictly Dominant Strategy）として成立する。

---

## 1.4 Mathematical Formula

システム全体価値（GMV）の非線形増加を示すアンチフラジャイル評価関数および MICE/Prospect Theory 損失回避ペイオフの数理モデルを以下に確定する。

### 式 1.1: アンチフラジャイルシステム評価関数

システムへの「負の入力（失敗データ数および破壊エネルギー総量）」に伴うプラットフォーム総価値 $V_{\text{system}}(t)$ の発展方程式：

$$V_{\text{system}}(t) = V_0 + \gamma \sum_{i=1}^{N_{\text{fail}}(t)} \left( \text{ROOTS}_i \cdot \text{SC}_i \right)^{\eta} - \sum_{j=1}^{M_{\text{leak}}(t)} L_{\text{risk}, j}$$

* $V_0$: 初期基本資本量（$V_0 > 0$）
* $N_{\text{fail}}(t)$: 時刻 $t$ における累積確定失敗データ数（$N_{\text{fail}}(t) \in \mathbb{Z}_{\ge 0}$）
* $\text{ROOTS}_i$: $i$ 番目の失敗データの物理実存証明スコア（$0.0 \le \text{ROOTS}_i \le 1.0$）
* $\text{SC}_i$: AI が算出した $i$ 番目の失敗データのショートカット可能性指数（削減可能開発コスト $\times$ 開発期間、$\text{SC}_i \ge 0.0$）
* $\eta$: アンチフラジャイル増幅係数（$\eta > 1.0$、本仕様では $\eta = 1.25$ に固定）
* $\gamma$: プラットフォーム網羅性スケーリング定数（$\gamma > 0$）
* $M_{\text{leak}}(t)$: 情報漏洩インシデント数（$M_{\text{leak}}(t) \in \mathbb{Z}_{\ge 0}$）
* $L_{\text{risk}, j}$: 情報漏洩に伴う損失額（$L_{\text{risk}, j} \ge 0$）

### 式 1.2: Prospect Theory 損失回避タイマーモデル

告発猶予期間 $t \in [0, T_{\text{max}}]$ における内部通報者の主観的効用関数 $U_{\text{whistle}}(t)$：

$$U_{\text{whistle}}(t) = v(R_{\text{bounty}}) \cdot w(p_{\text{win}}(t)) - \lambda \cdot \left| v(-P_{\text{loss}}) \right| \cdot w(1 - p_{\text{win}}(t))$$

$$p_{\text{win}}(t) = \exp\left( -k_{\text{rival}} \cdot n_{\text{insider}} \cdot t \right)$$

$$v(x) = \begin{cases} x^\alpha & (x \ge 0) \\ -\lambda (-x)^\beta & (x < 0) \end{cases}, \quad w(p) = \frac{p^\gamma}{\left(p^\gamma + (1 - p)^\gamma\right)^{1/\gamma}}$$

* $\lambda$: 損失回避係数（$\lambda \approx 2.25$）
* $\alpha, \beta$: 危険態度係数（$\alpha = \beta = 0.88$）
* $\gamma$: 確率加重曲率パラメータ（標準値 $\gamma \approx 0.65$）
* $P_{\text{loss}}$: 告発に遅れた場合の機会損失および法的・組織的ペナルティ相当額（$P_{\text{loss}} \equiv P_{\text{penalty}} > 0$）
* $p_{\text{win}}(t)$: 時間 $t$ において自らが第 $1$ 通報者であり続ける確率（$p_{\text{win}}(t) \in (0, 1]$）
* $k_{\text{rival}}$: 社内裏切り感度パラメーター（$k_{\text{rival}} > 0$）
* $n_{\text{insider}}$: 成功隠蔽の事実を知る社内関係者の推定人数（$n_{\text{insider}} \ge 1$）

---

## 1.5 Anti-Fragile Red-Teaming

本章のアーキテクチャに対して想定される攻撃ベクトルおよびその物理・アルゴリズム的防御弁を提示する。

1. **攻撃ベクトル: 企業による通報者買収・口止め（Bribe Attack）**
   * **手法:** 成功隠蔽を行った企業が内部関係者全員に対して $C_{\text{bribe}}$ を提示し、合意のもとで沈黙を維持させようとする。
   * **防御弁:** プロトコル層において「ファースト・ムーバー100%独占型懸賞金」を前払いエスクロー設定する。$R_{\text{bounty}}$ は企業の将来利益の一定割合（例: 20%）から自動算出され、$R_{\text{bounty}} > n_{\text{insider}} \cdot C_{\text{bribe}}$ を常時成立させる。企業側の買収予算が無限大とならない限り、囚人のジレンマにより買収協定は崩壊する。
2. **攻撃ベクトル: 通報者の身元逆引き・報復攻撃（Re-Identification Attack）**
   * **手法:** オンチェーンの投稿タイムスタンプや IP トラフィック、トークン引き出しパターンから通報者個人を特定・懲戒する。
   * **防御弁:** ZKP 署名（zk-SNARKs: Groth16 プロトコル）により、アドレスや IP を遮断した状態でのメンバーシップ証明を実現する。さらに、付与された懸賞金は Tornado 型ミックスプールおよびタイムロック分割コントラクトを経由して小分け引き出しされるため、オンチェーン・トラッキングによる個人特定は暗号学的に不可能となる。

---

## 1.6 境界条件と既知の限界（Open Issues）

* **前提条件（Assumptions）:** 本章のゲーム理論的均衡は、内部関係者が複数名（$n_{\text{insider}} \ge 2$）存在し、かつブロックチェーン上の懸賞金エスクローが現実法域の差押えから隔離されていることを前提とする。
* **想定される破綻モード（Failure Modes）:**
  * 伝統的銀行口座（非CBDC）に対する成功ロイヤリティ・罰則の自動強制執行（Clawback）のAPI不在（関連: [ISSUE-022](../../issues/FORGE_ISSUES_JP.md)）。
  * 社内二重投資警告機能に対する現場開発チームのローカル端末隠蔽工作（関連: [ISSUE-020](../../issues/FORGE_ISSUES_JP.md)）。
* **監査・PoC検証要件:** マルチエージェントシミュレーションによる $n_{\text{insider}}$ 変動時のナッシュ均衡収束速度の検証。

---

[← 目次に戻る](../../README.md) | [第2章 SATORI補空間相転移とZKP文脈剥離 →](./02_satori_zkp.ja.md)