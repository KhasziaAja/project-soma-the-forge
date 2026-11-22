# 第2章 SATORI補空間相転移アルゴリズムとZKP文脈剥離エンジン

[← 第1章 システム総論](./01_system_overview.ja.md) | [目次に戻る](../../../README.md) | [第3章 信頼証明レイヤー：ROOTSとTFA →](./03_proof_of_trust.ja.md)

## 概要と目的

本モジュールは、企業から投入された生の実験・失敗ログから企業名や特許・レピュテーションリスク等の「社会的・組織的文脈」を完全に剥離（成分表示化）し、高次元パラメータ空間の多様体圧縮と補空間相転移（成功レシピの自動導出）を実行する SATORI エンジン、およびステルス照合を実現するゼロ知識証明（ZKP）パイプラインの仕様を定義する。

## 2.1 アルゴリズム原理と幾何学的解釈

SATORI は、投入された生の実験・失敗データから「企業名」「商標」「特許リスク」「レピュテーションリスク」といった社会的・組織的文脈を完全に切り離し、100%純粋な「物理的・論理的境界条件（成分表示: Ingredient Labeling）」へと抽象化する AI エンジンである。

```
[ RAW失敗ログ / 実験ノート ] ──(企業名・個人名・社内用語を含む)
              │
              ▼
┌────────────────────────────────────────────────────────┐
│ 1. 文脈剥離（Context Detachment）                      │
│   ・NER/LLMによる固有表現・機密文体の完全パージ        │
│   ・物理量・境界条件の無次元化テンソル変換             │
└─────────────────────────┬──────────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────────┐
│ 2. 主成分多様体圧縮（Manifold Reduction: R^D -> R^d）  │
│   ・Sobol感度解析による無関係ノイズ次元の自動破棄      │
│   ・3〜5次元の本質的物理多様体 z への位相保存圧縮      │
└─────────────────────────┬──────────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────────┐
│ 3. 補空間相転移（Complementary Space Phase Transition）│
│   ・失敗ボール K_fail による空間包囲率 S_rate の監視   │
│   ・S_rate >= 0.99 到達時、隙間を通る成功解 x* を微分導出│
│   ・即座に Prior Art（先行技術）としてオンチェーン刻印 │
└────────────────────────────────────────────────────────┘
```

## 2.2 Data Schema & Pipeline

生データの投入、文脈剥離後の特徴ベクトル（成分表示）、および ZKP 証明ペイロードのデータ構造を以下に定義する。

### スキーマ 2.1: `RAW_ExperimentLog_Schema`

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

### スキーマ 2.2: `SATORI_IngredientVector_Schema`（ZKP成分表示ペイロード）

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

買い手企業とデータ提供ノード間における、ZKP ステルス照合および相転移成果物の権利獲得に関するゲーム理論設計を提示する。

データ購入企業は、自社の研究開発ロードマップ（検索クエリ）が外部に漏洩するインサイダーリスクを抱えている。SATORI では暗号化クエリ $\mathbf{q}$ を用いた ZKP 逆引き照合（Stealth Query）を実行できるため、検索意図を秘匿したまま自社が踏み抜こうとしている「地雷（既知の失敗領域）」の有無を検証できる。

一方、データ提供側（ $N$ ノード）においては、単体では無価値な廃棄ログであっても、他社の失敗ログと結合して相転移臨界点（包囲率 $S_{\text{rate}} \ge 0.99$ ）を超えた瞬間、相転移成功レシピ生成への寄与度（BRS: Bottleneck Resolution Score）に応じた永続ロイヤリティ（一括分配プール）を獲得する。

**各データ提供ノードの非利己的最適行動ポリシー:**
* **生失敗データを隠蔽・破棄する** $\rightarrow$ 利得 $0$
* **SATORI 経由で即座にクレンジング投稿する** $\rightarrow$ 相転移発生時の按分報酬 $\sum R_{\text{split}} \cdot w_i > 0$ を獲得

## 2.4 Mathematical Formula

SATORI エンジンのコアである「高次元多様体圧縮」および「補空間相転移（成功レシピ自動生成）」の幾何学算定式を確定する。

### 式 2.1: 主成分多様体圧縮（Manifold Dimension Reduction）

高次元パラメータ $\mathbf{x}_{\text{raw}} \in \mathbb{R}^D$ （ $D > 100$ ）を、本質的物理多様体 $\mathbf{z} \in \mathbb{R}^d$ （ $d \ll D$ 、通常 $d \in [3, 5]$ ）へ圧縮するエンコーダー関数 $f_{\theta}$ ：

$$\mathbf{z} = f_{\theta}(\mathbf{x}_{\text{raw}})$$

$$\min_{\theta, \phi} \mathbb{E} \left[ \left\lVert \mathbf{x}_{\text{raw}} - g_{\phi}(f_{\theta}(\mathbf{x}_{\text{raw}})) \right\rVert^2 \right] + \mu \cdot \mathcal{L}_{\text{topology}}(f_{\theta})$$

* $g_{\phi}$: デコーダー関数
* $\mathcal{L}_{\text{topology}}$: 位相構造保存正則化項（Persistent Homology 損失）
* $\mu$: 正則化ハイパーパラメータ（ $\mu > 0$ ）

### 式 2.2: 失敗凸包・非凸包の包囲限界と飽和判定（正則化・値域保証版）

確定失敗データ点群 $\mathcal{S}_{\text{fail}} = \{\mathbf{z}_1, \mathbf{z}_2, \dots, \mathbf{z}_N\}$ がパラメータ空間 $\Omega \subset \mathbb{R}^d$ に形成する絶対立ち入り禁止領域 $\mathcal{K}_{\text{fail}}$ ：

$$\mathcal{K}_{\text{fail}} = \bigcup_{i=1}^{N} B_{\epsilon_i}(\mathbf{z}_i), \quad \epsilon_i = r_0 \cdot (\text{ROOTS}_i)^k$$

空間領域 $\Omega$ における失敗飽和率（Failure Saturation Rate） $S_{\text{rate}} \in [0, 1]$ の正則化定義式：

$$S_{\text{rate}} = \min \left( 1.0, \; \frac_{\text{Vol}(\mathcal{K}_{\text{fail}} \cap \Omega)}{\max \left( \text{Vol}(\Omega) - \text{Vol}(\mathcal{X}_{\text{core}}), \; \epsilon_{\text{vol}} \right)} \right)$$

* $B_{\epsilon_i}(\mathbf{z}_i)$: 中心 $\mathbf{z}_i$ 、ROOTS スコアに応じた半径 $\epsilon_i$ の失敗ボール（ $r_0 > 0, \; k = 3$ ）
* $\text{Vol}(\cdot)$: $\mathbb{R}^d$ 上のルベーグ測度による超体積
* $\mathcal{X}_{\text{core}}$: 物理的に存在し得る未踏の心臓部（補空間、 $\mathcal{X}_{\text{core}} \subset \Omega$ ）
* $\epsilon_{\text{vol}}$: 分母のゼロ除算および過飽和発散を防ぐ正則化下限定数（ $\epsilon_{\text{vol}} = 10^{-6} \cdot \text{Vol}(\Omega)$ ）

### 式 2.3: 補空間相転移（成功レシピ）直接算出式

飽和率 $S_{\text{rate}} \ge 0.99$ （臨界点超過）かつ $\Omega \setminus \mathcal{K}_{\text{fail}} \neq \emptyset$ において、失敗の壁の隙間をすり抜ける唯一の成功パラメータセット $\mathbf{x}^*$ をポテンシャル場最小化（逆数最大化）により直接導出する最適化式：

$$\mathbf{x}^* = \arg\max_{\mathbf{x} \in \Omega \setminus \mathcal{K}_{\text{fail}}} \left[ \sum_{i=1}^{N} \frac{(\text{ROOTS}_i)^k}{\left\lVert \mathbf{x} - \mathbf{z}_i \right\rVert^2 + \delta} \right]^{-1}$$

* $\delta$: 失敗座標との一致時（ $\mathbf{x} \to \mathbf{z}_i$ ）におけるゼロ除算および勾配爆発を防ぐ数値安定化定数（ $\delta = 10^{-8}$ ）
* $k$: ROOTS 信頼度重み指数（ $k = 3$ 、低信頼データによるポテンシャル歪曲を抑制）

## 2.5 Anti-Fragile Red-Teaming

SATORI エンジンに対して想定される代表的な攻撃・脆弱性およびそのアルゴリズム的防衛弁を提示する。

1. **攻撃ベクトル: 次元の呪い攻撃（Curse of Dimensionality Attack）**
   * **手法:** パラメータ次元数 $D$ を人為的に水増し（無関係な環境変数を大量追加）して空間体積 $\text{Vol}(\Omega)$ を爆発させ、相転移臨界点（ $S_{\text{rate}} \ge 0.99$ ）に永久に到達できなくさせる。
   * **防御弁:** 式 2.1 の前処理ステップとして「多様体学習（Manifold Learning）」および Sobol 感度解析を強制適用し、目的変数に対する寄与率が閾値 $\tau < 0.01$ 未満の次元を自動破棄する。圧縮次元 $d$ を常に $3 \sim 5$ 次元に厳密固定することで幾何学的破綻を遮断する。
2. **攻撃ベクトル: 正解座標の上書き隠蔽攻撃（Phantom Hits Overwriting Attack）**
   * **手法:** 悪意あるノードが「本来成功する座標」の上に捏造した失敗データを大量プロットし、正解パラメータセット $\mathbf{x}^*$ を失敗ボール $\mathcal{K}_{\text{fail}}$ で覆い隠す。
   * **防御弁:** 相転移算定モデルに「TFA 確率的イレイサー（Probabilistic Eraser）」を組み込む。 $\text{ROOTS}_i < 0.85$ のデータは評価半径が $\epsilon_i \to 0$ に減衰する。さらに、独立した他ノードから矛盾する報告（成功実績や上位技術）が到達した場合、該当する失敗座標は自動消去され、多様体の位相再計算（Re-topology）が動的に実行される。
3. **攻撃ベクトル: 自動導出レシピのメガテック先回り特許出願（Patent Troll Attack）**
   * **手法:** メガテック企業が SATORI から自動導出された成功レシピ $\mathbf{x}^*$ を監視し、即座に自社名義で国際特許出願して独占しようとする。
   * **防御弁:** 相転移アルゴリズムが $\mathbf{x}^*$ を出力したミリ秒単位のタイムフレームで、そのパラメータハッシュがパブリック台帳（IP-CLEAR / GIP-NET）へ「先行技術（Prior Art）」として自動刻印される。これにより、第三者による特許出願は国際法（PCT）上の新規性要件を満たせず 100% 構造的に無効化される。

## 2.6 境界条件と既知の限界（Open Issues）

* **前提条件（Assumptions）:** 本章の補空間相転移は、対象技術領域の物理法則が連続的であり、かつ多様体圧縮後の潜在空間 $\mathbb{R}^d$ 上で失敗領域 $\mathcal{K}_{\text{fail}}$ が連結閉集合として近似可能であることを前提とする。
* **想定される破綻モード（Failure Modes）:**
  * 極小ニッチ領域におけるデータ疎性により $S_{\text{rate}} \ge 0.99$ の臨界点へ到達不能となる問題（関連: [ISSUE-009](../../../issues/FORGE_ISSUES_JP.md)）。
  * ZKP ステルス照合（Groth16）の回路制約増加に伴うプローバー側計算オーバーヘッドの増大（関連: [ISSUE-008](../../../issues/FORGE_ISSUES_JP.md)）。
* **監査・PoC検証要件:** 合成ベンチマーク空間（ $d=3, 5$ ）における非凸失敗領域からの $\mathbf{x}^*$ 収束精度および誤検出率の定量評価。

[← 第1章 システム総論](./01_system_overview.ja.md) | [目次に戻る](../../../README.md) | [第3章 信頼証明レイヤー：ROOTSとTFA →](./03_proof_of_trust.ja.md)