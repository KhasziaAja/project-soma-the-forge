# 第5章 統合開発モデル：D&A（Debug & Assembly）と動的ロイヤリティ・スプリット

[← 第4章 経済・トークノミクス数理：D-LOGプロトコル](./04_tokenomics.ja.md) | [目次に戻る](../../../README.md) | [第6章 ガバナンス防腐と国際法適合回路 →](./06_governance.ja.md)

## 概要と目的

本モジュールは、The Forge が提供する「負の知財（地雷マップ・失敗ログ）」と IPEX が提供する「正の知財（休眠特許・成功の処方箋）」を融合させて無駄な二重試行錯誤を消去する **D&A（Debug & Assembly）統合開発モデル**、有向非巡回グラフ（DAG）による依存関係追跡、およびネガティブ IP への最大 80% キャップ制約とマイルド FMP v2.0（パトロン 200% ROI 上限還元）を組み込んだ **Dynamic Royalty Split（動的ロイヤリティ一括分配）** の数理仕様を確定する。

## 5.1 モノづくり世代交代と知能統合アーキテクチャ

D&A（Debug & Assembly）モデルは、The Forge が提供する「負の知財（地雷マップ）」と IPEX が提供する「正の知財（休眠特許・成功の処方箋）」を融合させ、研究開発の無駄な二重試行錯誤を構造的に消去する次世代開発プロトコルである。

従来の R&D 開発が「自社閉塞型の模索」であったのに対し、D&A モデルは「SATORI 境界解析による阻害要因（バグ）の即時デバッグ」と「スマートライセンスによる確立済知財（パーツ）の統合アセンブリ」によって開発期間およびコストを $1/10 \sim 1/100$ に短縮する。

```text
┌─────────────────────────────────────────┐
│ The Forge: 負の知財 (地雷マップ/失敗ログ)│
└────────────────────┬────────────────────┘
                     │ (阻害要因・ボトルネックの特定)
                     ▼
┌─────────────────────────────────────────┐
│ SATORI 寄与度算定エンジン               │
│ ・BRS (ボトルネック解除度) 算出          │ ──► Dynamic Royalty Split
│ ・DAG (有向非巡回グラフ) 因果連鎖解析    │
└────────────────────┬────────────────────┘
                     │ (最適結合処方箋の導出)
                     ▼
┌─────────────────────────────────────────┐
│ IPEX: 正の知財 (休眠特許/成功レシピ)    │
└────────────────────┬────────────────────┘
                     │ (ワンクリック・スマートライセンス)
                     ▼
[ 商業化成功・自動ロイヤリティ一括分配プール ]
```

## 5.2 Data Schema & Pipeline

D&A アセンブリ依存関係グラフ（DAG）およびスマートコントラクト用ロイヤリティ分配ペイロードのデータ構造を以下に定義する。

### スキーマ 5.1: `DAndA_Assembly_DAG`

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "DAndA_Assembly_DAG",
  "type": "object",
  "properties": {
    "assembly_id": { "type": "string", "format": "uuid" },
    "target_product_hash": { "type": "string", "pattern": "^0x[a-fA-F0-9]{64}$" },
    "positive_ip_nodes": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "ipex_license_id": { "type": "string", "format": "uuid" },
          "patent_number": { "type": "string" },
          "weight_score": { "type": "number", "minimum": 0.0, "maximum": 1.0 }
        },
        "required": ["ipex_license_id", "patent_number", "weight_score"]
      }
    },
    "negative_ip_nodes": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "forge_asset_id": { "type": "string", "format": "uuid" },
          "brs_score": { "type": "number", "minimum": 0.0, "maximum": 1.0 },
          "roots_score": { "type": "number", "minimum": 0.0, "maximum": 1.0 },
          "parent_node_ids": { "type": "array", "items": { "type": "string", "format": "uuid" } }
        },
        "required": ["forge_asset_id", "brs_score", "roots_score", "parent_node_ids"]
      }
    },
    "patron_fmp_pool": {
      "type": "object",
      "properties": {
        "patron_address": { "type": "string", "pattern": "^0x[a-fA-F0-9]{40}$" },
        "principal_invested": { "type": "number", "minimum": 0.0 },
        "cumulative_payout": { "type": "number", "minimum": 0.0 },
        "roi_cap_multiplier": { "type": "number", "const": 2.0 }
      },
      "required": ["patron_address", "principal_invested", "cumulative_payout", "roi_cap_multiplier"]
    },
    "created_timestamp": { "type": "integer" }
  },
  "required": ["assembly_id", "target_product_hash", "positive_ip_nodes", "negative_ip_nodes", "created_timestamp"]
}
```

## 5.3 Game Theory & Incentives

D&A 統合エコシステムにおける「特許パテントトロールの無効化」および「マイルド FMP v2.0（パトロン還元と発明者保護の均衡）」のゲーム理論設計を提示する。

1. **商業化企業（製品メーカー）の最適行動:**
   * 自社でゼロから R&D を回した場合の期待コストを $C_{\text{solo}}$ 、D&A 統合ライセンスの総ロイヤリティ負担を $R_{\text{royalty}}$ とする。
   * $R_{\text{royalty}} \ll C_{\text{solo}}$ かつ商業化リードタイム短縮による先行者利益 $\Delta \Pi_{\text{time}} > 0$ が成立するため、自前主義に固執するより D&A アセンブリを利用することが厳密支配戦略となる。
2. **マイルド FMP v2.0（パトロン投資家 vs 発明者・失敗提供者）:**
   * 初期の失敗実験や検証コストを資金支援したパトロン（初期投資家）には、商業化ロイヤリティから優先配分が行われるが、**投資元本に対する累積還元上限（ROI Cap = $200\%$ ）** がスマートコントラクト上でハードコードされる。
   * 累積配当が元本の $2.0$ 倍に達した瞬間、パトロン枠の配当権は自動消滅し、以後の永続ロイヤリティは 100% 現場の発明者・失敗データ提供ノードへと還流する。これにより、金融資本による永続的な知財搾取（レントシーキング）を構造的に遮断する。

## 5.4 Mathematical Formula

有向非巡回グラフ（DAG）上の寄与度伝播と、ネガティブ IP 配分上限（ $80\%$ キャップ）およびマイルド FMP v2.0 還元上限を組み込んだ動的ロイヤリティ分配式を確定する。

### 式 5.1: Dynamic Royalty Split（80% キャップ保証・DAG 遡及分配式）

商業化売上 $G_{\text{rev}}(t)$ に総ロイヤリティ料率 $r_{\text{total}} \in (0, 1)$ を乗じた分配原資 $P_{\text{pool}}(t) = r_{\text{total}} \cdot G_{\text{rev}}(t)$ の配分算定式：

まず、ネガティブ IP 群（ $\mathcal{N}_{\text{neg}}$ ）とポジティブ IP 群（ $\mathcal{N}_{\text{pos}}$ ）の総配分比率 $\theta_{\text{neg}}, \theta_{\text{pos}}$ を以下の通り上限クランプ付きで決定する：

$$
\theta_{\text{neg}} = \min\left( 0.80, \; \frac{\sum_{i \in \mathcal{N}_{\text{neg}}} W_{\text{neg}, i}}{\sum_{i \in \mathcal{N}_{\text{neg}}} W_{\text{neg}, i} + \sum_{j \in \mathcal{N}_{\text{pos}}} W_{\text{pos}, j} + \epsilon_W} \right), \quad \theta_{\text{pos}} = 1.0 - \theta_{\text{neg}}
$$

各ネガティブ IP ノード $i \in \mathcal{N}_{\text{neg}}$ の raw 重み $W_{\text{neg}, i}$ および最終受取ロイヤリティ $R_i(t)$ ：

$$
W_{\text{neg}, i} = \text{BRS}_i \cdot (\text{ROOTS}_i)^k + \sum_{m \in \text{Desc}(i)} \gamma_{\text{dag}}^{\text{dist}(i, m)} \cdot W_{\text{neg}, m}
$$

$$
R_i(t) = P_{\text{pool}}(t) \cdot \theta_{\text{neg}} \cdot \frac{W_{\text{neg}, i}}{\sum_{u \in \mathcal{N}_{\text{neg}}} W_{\text{neg}, u} + \epsilon_W}
$$

* $\text{BRS}_i \in [0, 1]$: SATORI が算出した $i$ 番目の失敗データのボトルネック解除度（Bottleneck Resolution Score）

* $\text{ROOTS}_i \in [0, 1]$: 物理実存証明スコア（ $k = 3$ ）

* $\text{Desc}(i)$: DAG 上でノード $i$ の失敗知見を前提として派生した子孫ノード集合

* $\gamma_{\text{dag}} \in (0, 1)$: DAG 遡及減衰係数（例: $\gamma_{\text{dag}} = 0.50$ ）

* $\text{dist}(i, m) \in \mathbb{Z}_{\ge 1}$: DAG 上の有向パス最短距離

* $0.80$: ネガティブ IP 群への総分配上限キャップ（正の特許保有者へ最低 $20\%$ の取り分を保証し、特許訴訟・ボイコットを抑止）

* $\epsilon_W = 10^{-9}$: ゼロ除算防止定数

### 式 5.2: マイルド FMP v2.0（パトロン 200% ROI キャップ）償還制御式

パトロン $p$ に対する時刻 $t$ における実効配当額 $D_p(t)$ ：

$$
D_p(t) = \min\left( \rho_p \cdot P_{\text{pool}}(t), \; \max\left(0, \; 2.0 \cdot I_p - \sum_{\tau < t} D_p(\tau)\right) \right)
$$

* $I_p > 0$: パトロン $p$ の初期拠出元本

* $\rho_p \in (0, 1)$: 契約上の優先配分シェア

* $2.0 \cdot I_p$: $200\%$ ROI 上限キャップ（超過分は自動的に発明者・データ提供者プールへ再配分）

## 5.5 Anti-Fragile Red-Teaming

D&A モデルおよび動的ロイヤリティ分配に対して想定される攻撃ベクトルと防衛弁を提示する。

1. **攻撃ベクトル: DAG 循環参照・自己増殖スピン攻撃（DAG Sybil Loop Attack）**
   * **手法:** 悪意あるノードが自らの失敗ログを微修正して大量の子孫ノード $\text{Desc}(i)$ をぶら下げ、式 5.1 の遡及重み $\sum \gamma_{\text{dag}}^{\text{dist}} W$ を水増ししてロイヤリティを独占しようとする。
   * **防御弁:** スマートコントラクトおよび SATORI グラフ検証器において、トポロジカルソートによる厳格な非巡回性（DAG 制約）を強制するとともに、特徴ベクトル空間上のコサイン類似度が閾値（例: $0.95$ ）以上の派生ノードは「重複派生（Duplicate Fork）」として重みを $0$ に縮退させる。
2. **攻撃ベクトル: 商業化商品のステルス実装・ロイヤリティ中抜き（Stealth Commercialization Attack）**
   * **手法:** 企業が D&A の処方箋を閲覧して製品化したにもかかわらず、ライセンス契約を締結せず自社独自開発と偽ってロイヤリティ支払いを回避する。
   * **防御弁:** SATORI エンジンは製品出荷後の公開スペック・成分分析・特許公報を継続クロールし、D&A 処方箋のトポロジー指紋（Topological Fingerprint）との一致率を自動監視する。無断商業化が検知された場合、第 6 章の「EU 適合 4 フェーズガバナンス回路」が自動起動し、和解金（Cure Fee）の徴収または特許・サプライチェーン凍結（焦土作戦）へ移行する。

## 5.6 境界条件と既知の限界（Open Issues）

* **前提条件（Assumptions）:** 本章のロイヤリティ自動分配は、対象製品の売上高または出荷数量がオラクル（ERP 連携・決済スマートコントラクト・監査証明）を通じて検証可能であることを前提とする。
* **想定される破綻モード（Failure Modes）:**
  * 複数の基盤技術が絡み合う複合製品における、個々の失敗データの正確な貢献度（ $\text{BRS}_i$ ）帰属の曖昧性（関連: [ISSUE-016](../../../issues/FORGE_ISSUES_JP.md)）。
  * B2B クローズド取引や非公開軍事・社内インフラ転用時における商業化売上トラッキングの困難性（関連: [ISSUE-017](../../../issues/FORGE_ISSUES_JP.md)）。
  * 有向非巡回グラフ（DAG）の深さ増大に伴うオンチェーンガス代の高騰（関連: [ISSUE-018](../../../issues/FORGE_ISSUES_JP.md)）。
  * 既存メガテック特許プールとのクロスライセンス衝突（関連: [ISSUE-019](../../../issues/FORGE_ISSUES_JP.md)）。
* **監査・PoC検証要件:** L2 ZK-Rollup 上での大規模 DAG（ノード数 $N > 10,000$ ）一括ロイヤリティ分配のガスコストおよび計算複雑度ベンチマーク。

[← 第4章 経済・トークノミクス数理：D-LOGプロトコル](./04_tokenomics.ja.md) | [目次に戻る](../../../README.md) | [第6章 ガバナンス防腐と国際法適合回路 →](./06_governance.ja.md)