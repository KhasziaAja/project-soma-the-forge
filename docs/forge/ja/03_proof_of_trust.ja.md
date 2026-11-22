# 第3章 信頼証明レイヤー：ROOTS（物理代償）とTFA（3層監査）プロトコル

[← 第2章 SATORI補空間相転移とZKP文脈剥離](./02_satori_zkp.ja.md) | [目次に戻る](../../../README.md) | [第4章 経済・トークノミクス数理：D-LOGプロトコル →](./04_tokenomics.ja.md)

## 概要と目的

本モジュールは、デジタルデータ単体の改ざん可能性を排除するため、データ生成時に支払われた「時間・電力消費・GPU熱・ハードウェア固有署名」という改ざん不可能な物理的領収書（レシート）を検証・ハッシュ化する鑑定システム **ROOTS (Reliable Origin & Ontological Trust System)** と、AI 偽造データや捏造ログを3段階の代謝エンジンで自動検閲・排除する **TFA (Triple-Filter Audit)** プロトコルの仕様を定義する。

## 3.1 物理実存証明と多重検閲アーキテクチャ

ROOTS は、計算機や実験装置が物理世界で消費したエネルギーおよび不可逆な熱力学的痕跡を暗号学的にバインドし、信頼度スコア $\text{ROOTS}_i \in [0, 1]$ を発行する。TFA は以下の3層フィルターを通じてデータ純度を監査する。

```text
[ RAWデータ / 物理ログ ]
           │
           ▼
┌────────────────────────────────────────────────────────┐
│ TFA 第一フィルター: NNS                                │
│ (自然界ノイズ・統計指紋監査)                           │
│ ・ガウス分布過度適合（AI生成特有の均一ノイズ）の検知   │
│ ・ベンフォード法則（第一桁分布）からの統計逸脱検知     │
└──────────┬─────────────────────────────────────────────┘
           │ (Pass)
           ▼
┌────────────────────────────────────────────────────────┐
│ TFA 第二フィルター: PT                                 │
│ (因果チェーン監査モジュール)                           │
│ ・仮説-試行-結論間の論理距離および試行回数 N_trial 監査 │
│ ・「苦悩なき跳躍（AIによる一発捏造）」の異常検知       │
└──────────┬─────────────────────────────────────────────┘
           │ (Pass)
           ▼
┌────────────────────────────────────────────────────────┐
│ TFA 第三フィルター: AO                                 │
│ (物理的実存証明ノード)                                 │
│ ・電源周波数ゆらぎ / GPU熱波形プロファイルの照合       │
│ ・TPM / Intel SGX / ハードウェアPUF固有署名の検証      │
└──────────┬─────────────────────────────────────────────┘
           │ (Pass)
           ▼
[ ROOTS Trust-Hash 鑑定書発行 ]
```

## 3.2 Data Schema & Pipeline

ROOTS 鑑定書および TFA 監査ログのデータ構造を以下に定義する。

### スキーマ 3.1: `ROOTS_TrustHash_Certificate`

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

データ投稿者および追試バリデータ間における Stake & Slash 制度の利得構造を定義する。

投稿者はデータ登録時にステーキング額 $S_{\text{stake}}$ を預託する。捏造データが追試または TFA 監査によって発覚した場合、預託資産は全額没収（Slash）され、告発者（バリデータ）および被害補償プールへ按分される。

| | バリデータ: 不正を検知・報告 | バリデータ: 見逃し・スルー |
| :--- | :---: | :---: |
| **投稿者: 捏造データを提出** | $(-S_{\text{stake}}, \; \mu \cdot S_{\text{stake}} - C_{\text{validation}})$ | $(-S_{\text{stake}}, \; 0)$ （TFA自動検知時） |
| **投稿者: 本物データを提出** | $(R_{\text{mint}}, \; -C_{\text{validation}})$ | $(R_{\text{mint}}, \; 0)$ |

* **配分条件:** $\mu = 0.50$ （没収額の $50\%$ を検知者へ懸賞金付与、 $50\%$ をプラットフォーム補償基金へ充当）
* $C_{\text{validation}}$: 物理追試および計算検証にかかる実コスト（ $C_{\text{validation}} > 0$ ）
* $R_{\text{mint}}$: 認証通過時に付与される D-LOG トークン報酬

本利得構造により、以下のステーキング下限条件が満たされる限り、悪意ある投稿の期待値は負となり、偽造投稿インセンティブが構造的に消滅する：

$$S_{\text{stake}} > \frac{C_{\text{validation}}}{\mu}$$

## 3.4 Mathematical Formula

ROOTS 総合鑑定スコア算定式、TFA 3層個別スコア（NNS / PT / AO）、および動的ステーキング要求額の数理モデルを確定する。

### 式 3.1: ROOTS 総合判定スコア算定式（値域保証版）

データの物理的実存純度を示すスコア $\text{ROOTS}_i \in [0, 1]$ ：

$$\text{ROOTS}_i = \text{clamp}_{[0, 1]} \left( w_1 \cdot S_{\text{NNS}} + w_2 \cdot S_{\text{PT}} + w_3 \cdot S_{\text{AO}} \right)$$

$$\sum_{m=1}^{3} w_m = 1.0, \quad w_m > 0 \quad (\text{標準重み}: w_1 = 0.25, \; w_2 = 0.25, \; w_3 = 0.50)$$

* $\text{clamp}_{[0, 1]}(x) = \min(1.0, \max(0.0, x))$: 浮動小数点誤差による値域逸脱を防ぐ境界クランプ演算子

### 式 3.2: TFA 各層フィルター監査スコア算定式

1. **第一フィルター（NNS: 自然界ノイズ・統計指紋監査）:**

$$S_{\text{NNS}} = \exp\left( -\beta_1 \cdot \Delta_{\text{Benford}} \right) \cdot \left( 1 - \sigma_{\text{Gaussian}}^2 \right) \cdot \left( 1 - P_{\text{residual}} \right)$$

* $\Delta_{\text{Benford}} \ge 0$: ベンフォード法則からのカルバック・ライブラー情報量（KLダイバージェンス）
* $\sigma_{\text{Gaussian}}^2 \in [0, 1]$: ガウス分布への過度適合度（合成ノイズ指標）
* $P_{\text{residual}} \in [0, 1]$: 末端桁の人工的丸め処理・周期パターン検知ペナルティ
* $\beta_1 > 0$: 統計逸脱感度係数

2. **第二フィルター（PT: 因果チェーン監査）:**

$$S_{\text{PT}} = 1 - \exp\left( -\beta_2 \cdot \frac{N_{\text{trial}}}{D_{\text{causal}} + \epsilon_{\text{causal}}} \right)$$

* $N_{\text{trial}} \ge 1$: 記録された中間試行・失敗ログのステップ数
* $D_{\text{causal}} \ge 0$: 初期仮説ベクトルと最終結論ベクトル間の意味的・論理的距離
* $\epsilon_{\text{causal}} = 10^{-4}$: ゼロ除算防止の正則化定数
* $\beta_2 > 0$: 試行密度スケーリング係数

3. **第三フィルター（AO: 物理的実存証明）:**

$$S_{\text{AO}} = \mathbb{I}_{\text{PUF}} \cdot \exp\left( -\beta_3 \cdot \left| \frac{E_{\text{measured}} - \hat{E}_{\text{model}}(T_{\text{exec}}, \Delta T_{\text{GPU}})}{\max(E_{\text{measured}}, \epsilon_E)} \right| \right)$$

* $\mathbb{I}_{\text{PUF}} \in \{0, 1\}$: ハードウェア PUF / TPM 暗号署名の検証結果（一致なら $1$ 、不一致なら即座に $0$ ）
* $E_{\text{measured}} > 0$: 実測消費電力量（ $\text{kWh}$ ）
* $\hat{E}_{\text{model}}(\cdot)$: 実行時間 $T_{\text{exec}}$ および GPU 熱変化 $\Delta T_{\text{GPU}}$ から熱力学モデルが予測する理論消費電力量
* $\epsilon_E = 10^{-9}$: ゼロ除算防止定数

### 式 3.3: 動的最低ステーキング要求額（Sybil スパム抑止式）

$$S_{\text{stake}} = \max\left( S_{\text{floor}}, \; k_{\text{risk}} \cdot \hat{C}_{\text{phys}} \right)$$

* $S_{\text{floor}} > 0$: プロトコル最低預託保証金
* $\hat{C}_{\text{phys}} \ge 0$: AI が算出した当該実験規模の推定物理コスト
* $k_{\text{risk}} > 1.0$: スパム抑止リスク乗数

## 3.5 Anti-Fragile Red-Teaming

ROOTS および TFA プロトコルに対して想定される攻撃ベクトルと防衛弁を提示する。

1. **攻撃ベクトル: 物理ゆらぎ生成スプーフィング攻撃（Generative Spoofing Attack）**
   * **手法:** GAN や高度な物理シミュレータを用い、CPU/GPU の熱波形、消費電力プロファイル、計器ノイズを合成して TFA を突破しようとする。
   * **防御弁:** 物理複製困難関数（PUF）およびセキュア・エンクレーブ（Intel SGX / TPM）のハードウェア直結を採用する。生センサーデータは生成された瞬間にプロセッサの Enclave 内部で暗号化・署名されるため、外部ソフトウェアによる波形改ざん・注入を $100\%$ 遮断する。
2. **攻撃ベクトル: ステーキング迂回スパム攻撃（Sybil Spaghetti Attack）**
   * **手法:** 低コストな計算で無価値な失敗データを大量生成し、多数のアカウントに $S_{\text{stake}}$ を分散させてシステムストレージと監査ノードを麻痺させる。
   * **防御弁:** 式 3.3 に基づき、必要最低ステーキング額 $S_{\text{stake}}$ をデータの推定物理コスト $\hat{C}_{\text{phys}}$ に動的連動させる。実質的な物理コストがゼロに近い AI 生成ゴミデータに対しては要求デポジット比率が指数関数的に跳ね上がるため、経済的に攻撃が成立しない。

## 3.6 境界条件と既知の限界（Open Issues）

* **前提条件（Assumptions）:** 本章の物理実存証明（AO）は、データ生成ノードがハードウェア信頼基点（TPM / PUF / TEE）を搭載した物理計算機またはセンサー直結環境で稼働していることを前提とする。
* **想定される破綻モード（Failure Modes）:**
  * 高精度量子センサー・分光器等における微細ノイズが NNS フィルターにより AI 過剰適合と誤判定されるリスク（関連: [ISSUE-001](../../../issues/FORGE_ISSUES_JP.md)）。
  * 研究者の直感的・天才的ブレイクスルー（中間試行 $N_{\text{trial}}$ が極小）が PT フィルターで「苦悩なき跳躍」として誤ペナルティを受けるリスク（関連: [ISSUE-002](../../../issues/FORGE_ISSUES_JP.md)）。
  * クラウド仮想環境（AWS / Docker / Google TPU）における熱・電力波形のハイパーバイザ正規化による AO 検証失敗（関連: [ISSUE-003](../../../issues/FORGE_ISSUES_JP.md)）。
  * 高ノイズ領域（プラズマ物理・バイオ等）における指数重み $k \ge 3$ による過減衰（関連: [ISSUE-004](../../../issues/FORGE_ISSUES_JP.md)）。
  * オフライン実験装置のタイムスタンプ不連続問題（関連: [ISSUE-005](../../../issues/FORGE_ISSUES_JP.md)）。
* **監査・PoC検証要件:** 実機 GPU クラスターおよび物理センサー環境における熱力学電力モデル $\hat{E}_{\text{model}}$ の誤差許容幅キャリブレーション。

[← 第2章 SATORI補空間相転移とZKP文脈剥離](./02_satori_zkp.ja.md) | [目次に戻る](../../../README.md) | [第4章 経済・トークノミクス数理：D-LOGプロトコル →](./04_tokenomics.ja.md)