# 第2章 物理・エッジセンシング層：PAIOS（Physical-AI OS）データマイニングとハードウェア要塞

[← 第1章 総論・存在の価値化](./01_system_topology.ja.md) | [目次に戻る](../../../README.md) | [第3章 自律型B2B業務ギルド：SOMA Guild →](./03_soma_guild_b2b.ja.md)

## 概要と目的

本モジュールは、人間の身体動作および不随意生理シグナルをゼロ摩擦（15秒装着・カメラ不要2点モード対応）でエッジ採掘し、フィジカルAI基盤モデル（VLA）用の高純度トレーニングデータへと精製する **PAIOS（Physical-AI OS）** の仕様を定義する。洗濯機・EMS・偽造スクリプトによるデータポイズニングを物理・数理的に完封する **PoU V2.1（Proof of Usefulness）アンチチート防壁**、および DFA カオス指数 $\alpha$ と筋伝導応答遅延 $T_{\text{rise}}$ を統合した総合妥当性判定関数 $\text{PoU}_{\text{valid}}(t)$ の数理仕様を確定する。

## 2.1 アンビエントセンシングと「Zero-Touch / 15秒装着」プロトコル

PAIOS による物理動態マイニングの成否は、作業員および居住者の心理的・身体的摩擦の完全消去にかかっている。手動でのデータ入力、ボタン操作、手動キャリブレーション、頻繁な充電を要求する設計は現場での拒絶反応を招くため、以下の「Zero-Touch / 15秒装着」アーキテクチャを採用する。

```text
+-----------------------------------------------------------------------------------+
|                [ PAIOS Edge Sensing & Zero-Touch Architecture ]                   |
+-----------------------------------------------------------------------------------+
|  [ Sensor Array: 5-Module / Camera-Free 2-Point Mode ]                            |
|   1. Smart Insole (足底床反力・重心移動・歩容エントロピー)                        |
|   2. Eraser-Sized 9-Axis IMU (手首・足首の微細キネマティクス)                     |
|   3. Neck/Lumbar Patch Sensor (筋電位EMG・筋脈波・皮膚発汗・体温)                 |
|   4. Ambient Millimeter-Wave / Thermal (非接触バイタル・空間動線)                 |
+-----------------------------------------------------------------------------------+
                                         |
                                         v (Direct Silicon Die Trace: Zero PCB Exposure)
+-----------------------------------------------------------------------------------+
|  [ SoC-Embedded Hardware Enclave (TPM / PUF) ]                                    |
|   - ハードウェア固有鍵によるリアルタイム署名 & 暗号化                             |
|   - Store & Forward (エッジ NOR Flash 512MB / 自律メッシュ転送)                   |
+-----------------------------------------------------------------------------------+
                                         |
                                         v (PoU V2.1 Anti-Cheat Pipeline)
+-----------------------------------------------------------------------------------+
|  [ PoU V2.1 Multi-Layer Validation ]                                              |
|   - DFA カオス指数検証 (0.75 < alpha < 1.25)                                      |
|   - 筋伝導応答遅延検証 (EMG -> 筋体積変化: 80 - 120 ms)                           |
|   - 3ヶ月報酬留保 (Holdback) & チート検知時全額没収 (Slashing)                    |
+-----------------------------------------------------------------------------------+
```

* **SoC内包型 Hardware Enclave**: 主要な生体センシング素子と暗号プロセッサ（TPM / PUF）を単一のシリコンダイ（SoC）上に統合する。生センサーアレイの信号は基板配線（PCBトレース）に一切露出せず、暗号エンジンへ直結されるため、物理インターポーザ攻撃や電気的盗聴・波形注入を物理層で遮断する。
* **Camera-Free 2-Point Mode**: プライバシー感受性の高い居住空間や機密工場においては、光学カメラを一切使用せず、インソールと首裏パッチの2点のみで全身の逆運動学（Inverse Kinematics）を再構成するモードへ自動切替する。
* **Store & Forward（512MB NOR Flash）**: 通信圏外や地下現場においても最大 72 時間分の特徴量パケットをローカル不揮発性メモリに暗号化保持し、メッシュネットワーク復帰時に自動同期する。

## 2.2 第2章における 4-Core Pillars

### 1) Data Schema & Pipeline

ミリ秒完全同期された床反力・IMU・不随意生体反応のデータパケット構造 `PaiosKinematicsPacket.json` 仕様：

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

作業員・居住者（マイナー）と検証プロトコル間における「Work-to-Earn 装着モチベーション＆3ヶ月留保（Holdback）ゲーム」の利得行列：

| | 検証プロトコル: 正常認証（90日後に報酬解放） | 検証プロトコル: ポイズニング検知（即時 Slashing） |
| :--- | :---: | :---: |
| **マイナー: 正当装着・日常稼働** | $(+R_{\text{mining}} + R_{\text{R2S}} - C_{\text{wear}}, \; +V_{\text{DaaS}})$ | $(0, \; 0)$ （誤検知率は極小） |
| **マイナー: 機械振動・チート工作** | $(0, \; 0)$ （PoU V2.1 により通過確率 $\approx 0$ ） | $(-S_{\text{holdback}} - C_{\text{cheat}}, \; +\text{SlashedPool})$ |

* **均衡条件:** 報酬の 3 ヶ月間ロック（Holdback）期間中に複数回（オフライン時系列クロスチェック含む）の逆解析監査が走るため、一時的にセンサーを騙せたとしても 90 日以内に発覚し、蓄積された留保報酬 $S_{\text{holdback}}$ が全額没収される。よって、正規装着して日常動作を行うことが唯一の厳密支配戦略となる。

### 3) Mathematical Formula

#### 式 2.1: DFA カオス検証スケーリング指数（自律神経揺らぎ解析）

心拍 RR 間隔時系列 $\{x_k\}_{k=1}^{N}$ の累積偏差 $Y(i) = \sum_{k=1}^{i} (x_k - \bar{x})$ に対し、ウィンドウサイズ $n$ ごとの局所トレンド $Y_n(i)$ からの二乗平均平方根ゆらぎ $F(n)$ を算出し、スケーリング指数 $\alpha$ を導出する：

$$F(n) = \sqrt{\frac{1}{N} \sum_{i=1}^{N} \left( Y(i) - Y_n(i) \right)^2} \propto n^{\alpha}$$

$$\alpha = \frac{\Delta \ln F(n)}{\Delta \ln n}, \quad \mathbb{I}_{\text{DFA}}(\alpha) = \begin{cases} 1 & \text{if } 0.75 < \alpha < 1.25 \\ 0 & \text{otherwise} \end{cases}$$

* 人工的なピンクノイズ生成器や単調な機械振動（洗濯機・マッサージチェア等）はホワイトノイズ（ $\alpha \approx 0.50$ ）またはブラウン運動（ $\alpha \ge 1.50$ ）へ偏るため、生体自律神経特有の $1/f$ カオス領域（ $0.75 < \alpha < 1.25$ ）から逸脱し即座に排除される。

#### 式 2.2: 筋伝導応答遅延（Electromechanical Delay: EMD）判定式

大脳運動野からの随意指令（表面筋電図 EMG 立上がり時刻 $t_{\text{EMG}}$ ）と、実際の筋体積変化・筋脈波立上がり時刻 $t_{\text{mech}}$ の生理学的位相差 $T_{\text{rise}}$ ：

$$T_{\text{rise}} = t_{\text{mech}} - t_{\text{EMG}}, \quad \mathbb{I}_{\text{EMD}}(T_{\text{rise}}) = \begin{cases} 1 & \text{if } 80\text{ ms} \le T_{\text{rise}} \le 120\text{ ms} \\ 0 & \text{otherwise} \end{cases}$$

* 低周波治療器（EMS）や外部からの機械的圧迫・振動では、物理変位が先行（ $T_{\text{rise}} \le 0$ ）するか、神経伝達経路を経由しないため $80\text{--}120\text{ ms}$ の生理的遅延ウィンドウから外れ、受動的モーション（Passive Motion）として無効化される。

#### 式 2.3: PoU V2.1 総合妥当性判定関数（統合スコア式）

上記物理・生理指標および床反力・IMU 運動学整合度 $S_{\text{kin}} \in [0, 1]$ を結合したパケット有効性判定スコア $\text{PoU}_{\text{valid}}(t) \in [0, 1]$ ：

$$\text{PoU}_{\text{valid}}(t) = \mathbb{I}_{\text{PUF}} \cdot \mathbb{I}_{\text{DFA}}(\alpha) \cdot \mathbb{I}_{\text{EMD}}(T_{\text{rise}}) \cdot \text{clamp}_{[0, 1]}\left( w_{\text{kin}} \cdot S_{\text{kin}}(t) + w_{\text{bio}} \cdot \exp\left( -\lambda_{\alpha} |\alpha - 1.0| - \lambda_{T} \frac{|T_{\text{rise}} - 100|}{20} \right) \right)$$

* $\mathbb{I}_{\text{PUF}} \in \{0, 1\}$: SoC Hardware Enclave 署名検証フラグ
* $w_{\text{kin}} + w_{\text{bio}} = 1.0$ （標準値: $w_{\text{kin}} = 0.40, \; w_{\text{bio}} = 0.60$ ）、 $\lambda_{\alpha} = 2.0, \; \lambda_{T} = 1.0$
* 判定閾値 $\text{PoU}_{\text{valid}}(t) \ge 0.80$ を満たすパケットのみが VLA トレーニングデータとして採択され、R2S クレジットを発行する。

### 4) Anti-Fragile Red-Teaming

1. **攻撃ベクトル 2-A: 洗濯機・EMS・サーボロボットによる物理擬態チート攻撃（Mechanical Spoofing Attack）**
   * **手法:** センサーを洗濯機、マッサージ機、あるいはプログラムされたロボットアームに装着し、人間の労働動作を偽装して大量マイニングしようとする。
   * **防御弁:** 式 2.1（DFA カオス指数 $\alpha \in (0.75, 1.25)$ ）と式 2.2（EMG 先行遅延 $T_{\text{rise}} \in [80, 120]\text{ ms}$ ）の二重物理ロックにより、周期的機械振動および外部電気刺激（EMS）は $100\%$ 排除される。さらに 3 ヶ月間の Holdback 期間中に時系列全体の歩容・姿勢エントロピー監査が実行され、異常検知時は累積報酬が全額没収される。
2. **攻撃ベクトル 2-B: 基板プロービングによる生データ改ざん注入攻撃（Hardware Interposer Attack）**
   * **手法:** ウェアラブル端末を分解し、センサーとマイコン間の通信線に偽造データ生成器を割り込ませる。
   * **防御弁:** センサー素子と TPM/PUF 暗号コアを同一シリコンダイ（SoC Enclave）内に封止し、外部バスへの平文出力を物理的に廃止しているため、プロービングによる信号注入は物理的に不可能である。

## 2.3 境界条件と既知の限界（Open Issues）

* **前提条件（Assumptions）:** 本章の EMD（筋伝導遅延）判定は、対象ユーザーが重度の神経筋疾患やペースメーカー装着者ではなく、標準的な神経-筋伝達系を有していることを前提とする（該当者向けには福祉免除プロファイルが適用される）。
* **想定される破綻モード（Failure Modes）:**
  * 極端な発汗・皮膚乾燥・体毛による首裏パッチの EMG 電極インピーダンス上昇に伴う信号欠損（関連: [ISSUE-002](../../../issues/SOMA_ISSUES_JP.md)）。
  * 激しい現場作業時のモーションアーチファクトによる DFA $\alpha$ 値の一時的ドリフト（関連: [ISSUE-003](../../../issues/SOMA_ISSUES_JP.md)）。
* **監査・PoC検証要件:** 実機 SoC プロトタイプを用いた 100 名規模のフィールド試験における偽陽性率（誤拒絶率 FRR $< 1.5\%$ ）および偽陰性率（チート突破率 FAR $< 0.01\%$ ）の実測検証。

[← 第1章 総論・存在の価値化](./01_system_topology.ja.md) | [目次に戻る](../../../README.md) | [第3章 自律型B2B業務ギルド：SOMA Guild →](./03_soma_guild_b2b.ja.md)