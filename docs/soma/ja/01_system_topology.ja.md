# 第1章 総論：フィジカルAI時代の「存在の価値化」アーキテクチャと人道型ナラティブ

[← 目次に戻る](../../README.md) | [第2章 PAIOS物理・エッジセンシング層 →](./02_paios_physical_mining.ja.md)

## 概要と目的

本モジュールは、従来の労働市場および社会保障インフラの構造的機能不全を解体し、人間の生存活動（身体動態・生理シグナル・非同期マイクロタスク）そのものを直接マイニング対象として資産化する統合社会OS **『Project SOMA / PAIOS』** の全体トポロジーを定義する。パブリック公開層とコア秘匿層の境界設計、ESG/SIB（ソーシャル・インパクト・ボンド）適合型の人道ナラティブ翻訳マトリクス、および現物クレジット相殺による実質生活維持コストのゼロ化（$\text{Net\_LMC}(t) = 0$）の数理仕様を確定する。

## 1.1 従来型社会インフラ・労働市場の破綻と「存在の価値化（Value in Existence）」

現代の高度資本主義社会における労働市場および社会保障インフラは、致命的な二重の構造的不整合に直面している。

第一に、産業界（特にエンタープライズ IT、金融、製造業）では、レガシーコードのリファクタリング、仕様書ドキュメント整備、非構造化データのクレンジングといった「重要だが緊急性が低く、正社員の帯域を圧迫する塩漬け業務（技術的負債・データの墓場）」が不可逆的に堆積している。これらは高度な専門知識を要するため、従来の単純派遣労働では処理できず、企業の生産性を著しく毀損している。一方で、同時刻・同空間での同期型労働や過剰な対人コミュニケーションに適応できずドロップアウトした高度 IT・専門人材が社会的に孤立・遊休化している。

第二に、物理世界で動作する「フィジカルAI（ヒューマノイド、自動運転作業機、身体拡張機器）」の急速な進展に伴い、AI 開発の主戦場はテキスト・画像から「ミリ秒単位で完全同期した生身の身体動態・環境相互作用データ」の調達へと移行した。従来型のシミュレーション環境（Sim2Real）や実験室での被験者データ収集は、人件費の高騰、現実世界のエントロピー（摩耗、揺らぎ、不整地）の再現限界、および傷害事故リスクによってスケール不全に陥っている。

『Project SOMA』は、人間の存在（Being）そのものをシステム駆動の基本モジュールへと再定義する。道徳や善意への期待という脆弱な精神的基盤を完全に排し、個人の根源的な「自己利得追求（打算）」を燃料としてインフラ（電力、通信、物流、労働）を駆動させる。人間が時間の対価として身体を切り売りする既存の強制労働モデルを破砕し、心拍・視線・筋肉電位・床反力・思考ログといった「生存活動（的存在）」を直接マイニング対象（Physical-AI OS: PAIOS）として資産化する。これにより、個人の生活維持コスト（Living Maintenance Cost: LMC）を現物クレジット還元によって実質自己負担ゼロ（$\text{Net\_LMC}(t) = 0$）へ相殺し、真の時間の脱商品化と包摂型社会インフラの樹立を達成する。

## 1.2 『Project SOMA』統合プラットフォームの全体システム設計

『Project SOMA』統合プラットフォームは、物理空間の肉体マイニングからデジタル空間の高度タスク処理、ならびに経済還元配管までをミリ秒単位で結合する 5 大コアサブシステムによって構成される。

```text
+-----------------------------------------------------------------------------------+
|                         [ Project SOMA System Topology ]                          |
+-----------------------------------------------------------------------------------+
|  [Layer 1: Physical & Vital Mining]                                               |
|   - PAIOS (Physical-AI OS) Edge Sensors (5-Module / Camera-Free 2-Point Mode)     |
|   - Zero-Touch Sensing / Store & Forward / PoU V2.1 Anti-Cheat Enclave            |
+-----------------------------------------------------------------------------------+
                                         |
                                         v (Filtered Physical Kinematics & Vitals)
+-----------------------------------------------------------------------------------+
|  [Layer 2: Autonomous B2B Task Execution & Data Refinement]                       |
|   - SOMA Guild (24/7 Asynchronous Relay: Engine A x Engine B)                     |
|   - The Forge (FDR-V2 Failure Log Anonymization & Re-Chunking Engine)             |
|   - DaaS Refinement & Cleansing AI (Patented Black-Box Core)                      |
+-----------------------------------------------------------------------------------+
                                         |
                                         v (Data Sales & SaaS Revenue)
+-----------------------------------------------------------------------------------+
|  [Layer 3: Financial Settlement & Tokenomics Pipeline]                            |
|   - Direct Resilience Pay (Automated Programmable 3-Split Settlement: 40/40/20)    |
|   - Dynamic Factoring & Lien Mechanism (Future Earnings Lien)                     |
|   - GPR Index Cushion & Off-chain SBT Micro-Royalty Settlement                    |
+-----------------------------------------------------------------------------------+
                                         |
                                         v (In-Kind Credit Rebate)
+-----------------------------------------------------------------------------------+
|  [Layer 4: Social Infrastructure & Living In-Kind Rebate]                         |
|   - 0-Yen Life Package (Protocol 0 / SOMA-Lite Tiered Asset Relief)               |
|   - R2S (Resource-to-Service) Cost-Offset Engine (Housing/Energy/Data/Food)      |
+-----------------------------------------------------------------------------------+
```

* **PAIOS (Physical-AI OS)**: 人間の身体動作・生理シグナル（関節角度、トルク、床反力、心拍ゆらぎ、視線）をエッジ側でリアルタイムに収集・構造化し、フィジカル AI の基盤モデル（VLA: Vision-Language-Action モデル等）用データとして精製・供給する物理 OS インフラ。
* **SOMA Guild**: 企業の塩漬けタスクを引き取り、AI ディレクターが細切れの「クエスト」へと解体・プール化。先進国の回復期人材（エンジンA）と準先進国の高度人材（エンジンB）が地球の自転（時差）を利用して 24 時間無摩擦で非同期リレー処理する自律型 B2B 業務ギルド。
* **R2S (Resource-to-Service)**: データ売却益（定価）と自社インフラ提供原価の差益を活用し、ユーザーが提供したデータ対価を「現物サービス（住居、電力、通信、機能性食品、遠隔医療）」として還元する現物決済配管。
* **0-Yen Life Package (Protocol 0)**: 居住・生存に必要な 5 大基本インフラを、生活ログ・生体データの提供深度（ティア 1〜3＋）に応じて段階的に無償化（クレジット相殺）し、住人の実質生活維持費をゼロにする社会的包摂住宅インフラ。
* **Direct Resilience Pay**: データ購入企業（Big Tech、製薬、損保、ロボティクス）から支払われる資金を、仲介者の介在を許さずにスマートコントラクト上で「作業員/個人（40–50%）」「現場施設/事業者（40–50%）」「プラットフォーム（10–20%）」へ即時自動分流する分散型清算プロトコル。

## 1.3 ESG/SIB（ソーシャル・インパクト・ボンド）適合型人道ナラティブ・フレームワーク

本システムは、内部実装においては徹底したゲーム理論と打算・利得計算で駆動しながら、対外的・行政連携においては ESG / SIB（ソーシャル・インパクト・ボンド）基準に完全適合する「包摂型セーフティネット」として機能する二重翻訳構造を備える。

### 対外ナラティブ二重翻訳マトリクス

| 構造要素 | 内部実装コード / 機械的解釈 | 対外ナラティブ（ESG / SIB / 人道型表現） |
| :--- | :--- | :--- |
| **駆動原理** | 個人の利得・損失最適化による行動変容制御 | インセンティブ整合型の持続可能な社会インフラ |
| **生体マイニング** | 運動学・生理パケットの強制エッジ採掘 | アンビエント・ウェルネス＆作業負荷軽減モニタリング |
| **ギルド構造** | タスク細切れ化によるドロップアウト人材の収益化 | 分散型高度 IT キャリア再構築・社会復帰支援 |
| **0円生活** | 家賃・光熱費相殺のための生存活動の資産化 | 地域統合型スマート・ライフケア基盤（Protocol 0） |
| **報酬留保** | 不正検知時の全額没収を伴う 3 ヶ月間報酬ロック | データ品質保証および TFA（信頼要素監査）期間 |
| **デッドマン装置** | 資産価値毀損・事故物件化を防ぐ自動ロックアウト論理 | 孤立死ゼロを目指す見守り型デッドマン生活支援 |

この二重構造により、自治体との連携は SIB（Pay-for-Success：成果連動型民間委託契約方式）スキームで運用される。生活保護費、公的医療費補助、失業対策費などの行政支出削減実績（$V_{\text{SIB}}$）の一定割合（例: $50\%$）が成果報酬としてプラットフォームへ還流され、財政健全化と社会的包摂が同時に達成される。

## 1.4 第1章における 4-Core Pillars

### 1) Data Schema & Pipeline

パブリック公開層とコア秘匿層の物理・論理境界を定義するシステム構成ノードスキーマ `SomaTopologyConfig.json` 仕様：

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "SomaTopologyConfig",
  "type": "object",
  "required": [
    "system_version",
    "node_id",
    "architecture_layer",
    "access_control",
    "public_interfaces",
    "core_secret_endpoints"
  ],
  "properties": {
    "system_version": {
      "type": "string",
      "enum": ["V2.1-MASTER"]
    },
    "node_id": {
      "type": "string",
      "pattern": "^SOMA-NODE-[A-Z0-9]{8}$"
    },
    "architecture_layer": {
      "type": "string",
      "enum": ["PAIOS_EDGE", "GUILD_GATEWAY", "R2S_PIPELINE", "DOCK_HOUSING"]
    },
    "access_control": {
      "type": "object",
      "required": ["github_public_exposure", "encryption_standard"],
      "properties": {
        "github_public_exposure": {
          "type": "boolean",
          "const": true
        },
        "encryption_standard": {
          "type": "string",
          "enum": ["AES-256-GCM", "ChaCha20-Poly1305", "zk-SNARKs-PLONK"]
        }
      }
    },
    "public_interfaces": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["interface_name", "protocol_type", "data_schema_ref"],
        "properties": {
          "interface_name": { "type": "string" },
          "protocol_type": { "type": "string", "enum": ["REST", "gRPC", "WebSocket", "EVM_SmartContract"] },
          "data_schema_ref": { "type": "string" }
        }
      }
    },
    "core_secret_endpoints": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["endpoint_id", "abstraction_layer_type", "ip_protection_mechanism"],
        "properties": {
          "endpoint_id": { "type": "string" },
          "abstraction_layer_type": { "type": "string", "enum": ["BLACK_BOX_API", "ENCLAVE_EXECUTION", "ZK_PROOF_GENERATOR"] },
          "ip_protection_mechanism": { "type": "string" }
        }
      }
    }
  }
}
```

### 2) Game Theory & Incentives

オープンソース公開（GitHub 放流）時における「プロトコル透明性の最大化」と「コアアルゴリズム・AI 重み防衛」の境界防衛ゲーム（Boundary Defense Game）：

| | 外部開発者・監査人: プロトコル検証・統合 | 競合・攻撃者: コア重み・クレンジング逆解析 |
| :--- | :---: | :---: |
| **公開層（Schemas / Math / Contracts）** | 検証可能性・採用拡大によるネットワーク価値増大（$+\Delta V_{\text{net}}$） | 仕様は公開済みだが、秘密鍵・学習重みへは到達不能 |
| **秘匿層（Black-Box API / TEE / ZKP）** | ZKP 証明付き入出力のみを受け取り正常動作 | 暗号境界により推論重み・生データ抽出コストが実質無限大（$-C_{\text{attack}} \to -\infty$） |

### 3) Mathematical Formula

#### 式 1.1: 実質生活維持コスト（Net LMC）の現物クレジット相殺ゼロ収束式

物理的・熱力学的基礎代謝およびインフラ維持実費の総和 $C_{\text{basic}}(t) > 0$ に対し、PAIOS 生体採掘および SOMA Guild 活動から生成される R2S 現物クレジット還元額 $R_{\text{rebate}}(t) \ge 0$ を相殺したユーザーの正味自己負担額 $\text{Net\_LMC}(t)$：

$$\text{Net\_LMC}(t) = \max\left( 0, \; \sum_{k=1}^{5} C_{\text{basic}, k}(t) - \sum_{m} R_{\text{rebate}, m}(t) \right)$$

$$\lim_{t \to \infty} \text{Net\_LMC}(t) = 0 \quad \left( \text{when } \sum_{m} R_{\text{rebate}, m}(t) \ge \sum_{k=1}^{5} C_{\text{basic}, k}(t) \right)$$

* $k \in \{1..5\}$: 5 大生存基礎インフラ（1: 住居, 2: 電力・水道, 3: 通信, 4: 機能性食料, 5: 遠隔医療・保険）
* $C_{\text{basic}, k}(t) > 0$: インフラ項目 $k$ の実物調達・供給原価
* $R_{\text{rebate}, m}(t) \ge 0$: データ採掘・ギルド報酬から R2S 配管を通じて充当される現物相殺クレジット

#### 式 1.2: SIB 行政支出削減額およびプラットフォーム成果還流算定式

自治体および公的機関における社会的コスト削減効果 $V_{\text{SIB}}(t)$ およびプラットフォーム受領報酬 $P_{\text{SIB}}(t)$：

$$V_{\text{SIB}}(t) = \sum_{i=1}^{N_{\text{resident}}(t)} \left( \Delta E_{\text{welfare}, i}(t) + \Delta E_{\text{medical}, i}(t) + \Delta E_{\text{unemp}, i}(t) \right)$$

$$P_{\text{SIB}}(t) = \rho_{\text{SIB}} \cdot \max\left( 0, \; V_{\text{SIB}}(t) - B_{\text{baseline}}(t) \right)$$

* $N_{\text{resident}}(t) \in \mathbb{Z}_{\ge 0}$: 対象自治体内の Project SOMA 稼働参加者数
* $\Delta E_{\text{welfare}, i}, \Delta E_{\text{medical}, i}, \Delta E_{\text{unemp}, i}$: 参加者 $i$ に関する生活保護給付削減額、公的医療費削減額、失業対策費削減額
* $B_{\text{baseline}}(t) \ge 0$: 統計的対照群（非参加層）の自然改善ベースライン
* $\rho_{\text{SIB}} \in (0, 1)$: SIB 成果連動分配率（標準値 $\rho_{\text{SIB}} = 0.50$）

### 4) Anti-Fragile Red-Teaming

1. **攻撃ベクトル: オープンソース仕様からのクローン・モデル重み強奪攻撃（Clone & Model Extraction Attack）**
   * **手法:** GitHub 上に公開されたデータスキーマや数理モデルを模倣し、さらに Black-Box API への大量クエリを通じて SATORI / DaaS クレンジング AI の内部重みを蒸留・盗用しようとする。
   * **防御弁:** パブリック層（数理・契約・スキーマ）とコア秘匿層（TEE / Hardware Enclave 内部の推論エンジンおよび ZKP 回路）を物理・暗号分離する。API にはクエリレート制限と差分プライバシーノイズ注入が施されており、モデル蒸留攻撃を構造的に遮断する。
2. **攻撃ベクトル: 行政・自治体による SIB 成果認定の恣意的引き延ばし（Fiscal Clawback Delay Attack）**
   * **手法:** 自治体が財政難を理由に公的支出削減効果 $V_{\text{SIB}}$ の算定・支払いを遅延させ、プラットフォームの運転資金を枯渇させようとする。
   * **防御弁:** Project SOMA の経済循環は民間 B2B 市場（DaaS データ販売および SOMA Guild 業務受託）からのキャッシュフローのみで $100\%$ 自立黒字化する設計となっており、SIB 報酬はあくまで追加余剰（ボーナスプール）として位置づけられるため、行政支払いの遅延がシステム生存を脅かすことはない。

## 1.5 境界条件と既知の限界（Open Issues）

* **前提条件（Assumptions）:** 本章の現物クレジット相殺（$\text{Net\_LMC}(t) = 0$）は、データ販売市場および B2B タスク市場において一定以上の外部需要が存在し、R2S レバレッジ原価率が維持されることを前提とする。
* **想定される破綻モード（Failure Modes）:**
  * 初期立ち上げ期におけるデータ購入企業および B2B クライアント不足による流動性欠乏（関連: [ISSUE-010](../../issues/SOMA_ISSUES_JP.md)）。
  * 生活保護制度等の現行公的扶助と現物クレジット支給の併給調整・収入認定摩擦（関連: [ISSUE-006](../../issues/SOMA_ISSUES_JP.md)）。
* **監査・PoC検証要件:** パイロット自治体（$N \ge 100$ 世帯規模）における 12 ヶ月間の医療・福祉支出削減トラッキングおよび R2S 原価率の実証計測。

[← 目次に戻る](../../README.md) | [第2章 PAIOS物理・エッジセンシング層 →](./02_paios_physical_mining.ja.md)