# ChairGameMod 説明書

Minecraftで、特殊な椅子や能力を使った椅子取りゲームを遊べるFabric Modです。

## 対応環境

- Minecraft Java Edition 1.21.1
- Fabric Loader 0.19.0以上
- Fabric API（Minecraft 1.21.1対応版）
- Java 21
- 最低参加人数：2人
- 最大参加人数：8人

## 導入方法

1. MinecraftとMinecraft Launcherを終了します。
2. エクスプローラーのアドレス欄に `%appdata%\.minecraft\mods` と入力します。
3. `mods`フォルダーへ次のファイルを入れます。
   - `chairgamemod-1.0.0.jar`
   - Minecraft 1.21.1対応版のFabric API
4. Minecraft LauncherでFabric 1.21.1を選択して起動します。

マルチプレイでは、基本的にサーバーと参加者全員へ同じModとFabric APIを導入してください。

## マップについて

このModは、専用ワールドやゲーム会場を自動生成しません。既存ワールドの地形をゲーム会場として使用します。

- サーバー起動時：設定ファイルから使用ワールド、ゲーム範囲、ロビー座標を読み込みます。
- ゲーム開始時：既存地形の安全な位置へ参加者と椅子を配置します。
- 未生成チャンクをゲーム範囲にした場合：移動や配置に伴い、Minecraft標準の地形が生成される場合があります。

大切なワールドで遊ぶ場合は、事前にバックアップしてください。

## ゲーム開始テンプレ

最初に、管理者がゲーム設定を行います。

```mcfunction
/chairgame roundtime 180
/chairgame curse off
/chairgame pvp off
/chairgame mode casual
```

参加者全員が次のコマンドを実行します。

```mcfunction
/chairgame join
```

全員が参加したら、管理者がゲームを開始します。

```mcfunction
/chairgame start
```

同じ乱数配置を再現したい場合は、シード値を指定します。

```mcfunction
/chairgame start 12345
```

## 現在のラウンド仕様

### 第1～3ラウンド

- 椅子数は「参加人数＋1脚」です。
- 通常のゲーム判定では誰も脱落しません。
- 未着席、ダミー椅子、キラーチェア、デスゲーム中の死亡による脱落は無効です。
- キラーチェアは生成されません。
- 管理者による強制失格だけは有効です。

### 第4ラウンド以降

- 椅子数は「生存人数－1脚」です。
- 複数人が脱落条件を満たしても、実際に脱落するのはランダムな1人だけです。
- 各ラウンドで1人ずつ減り、最後の1人になるまで続きます。

## 基本ルール

1. 各ラウンド開始時、参加者はゲーム範囲内へ分散して配置されます。
2. 制限時間内に椅子を探して着席します。
3. 残り時間が少なくなると、未使用の本物の椅子が発光します。
4. 時間切れになると着席状況と椅子の効果を判定します。
5. 第4ラウンド以降は、判定結果から1人が脱落します。
6. 最後まで残った1人が勝者です。

境界外に一定時間留まった参加者は、ゲーム範囲の中央付近へ転送されます。

## 椅子の種類

| 種類 | 内容 |
|---|---|
| 無印 | 特殊効果のない通常の椅子 |
| バフ25% | 25%の確率でバフを獲得 |
| バフ50% | 50%の確率でバフを獲得 |
| バフ100% | 必ずバフを獲得 |
| デバフ25% | 25%の確率でデバフを受ける |
| デバフ50% | 50%の確率でデバフを受ける |
| デバフ100% | 必ずデバフを受ける |
| キラーチェア | 第4ラウンド以降、脱落候補になる危険な椅子 |
| ダミー椅子 | 見た目は椅子だが、正しい着席として扱われない椅子 |

バフ・デバフのパーセントは、効果の強さではなく発動確率です。

## バフ能力

- スピードブロック
- ダミーブロック
- ミステリーブロック
- ノックバック強化ブロック
- 椅子感知ブロック
- 重力ブロック
- ジャンプ力強化ブロック

重力ブロック以外は右クリックで使用する1回使い切りアイテムです。重力ブロックはオフハンドに持っている間、浮遊できます。

## デバフ

- 移動速度低下
- 発光
- バナー使用禁止
- ノックバック弱体化
- 強制転送
- アイテム消失
- キラーチェア化

キラーチェア化以外のデバフは、主に次のラウンドで適用されます。

## コマンド一覧

### 参加者用

| コマンド | 説明 |
|---|---|
| `/chairgame join` | ゲームへ参加 |
| `/chairgame leave` | ゲームから退出 |

### 管理者用

以下のコマンドには、チートの許可またはOP権限が必要です。

| コマンド | 説明 |
|---|---|
| `/chairgame start` | ゲームを開始 |
| `/chairgame start <seed>` | 指定したシード値でゲームを開始 |
| `/chairgame stop` | ゲームを強制終了 |
| `/chairgame roundtime <秒>` | 1ラウンドの制限時間を変更 |
| `/chairgame curse on` | キラーチェアを有効化 |
| `/chairgame curse off` | キラーチェアを無効化 |
| `/chairgame killerprob <0～1>` | キラーチェア出現確率を設定 |
| `/chairgame pvp on` | PvPダメージを有効化 |
| `/chairgame pvp off` | PvPダメージを無効化 |
| `/chairgame mode casual` | カジュアルモードへ変更 |
| `/chairgame mode deathgame` | デスゲームモードへ変更 |
| `/chairgame respawnchairs` | 現在のラウンドの椅子を再配置 |
| `/chairgame revive <プレイヤー名>` | 脱落者をゲームへ復帰 |
| `/chairgame disqualify <プレイヤー名>` | 指定プレイヤーを強制失格 |
| `/chairgame debugchairs` | 椅子のデバッグ情報を表示 |
| `/chairgame reloadconfig` | 設定ファイルを再読み込み |

`/chairgame setchaircount`コマンドも登録されていますが、現在の椅子数はラウンド仕様によって自動決定されるため、指定した椅子数は使用されません。キラーチェア上限の設定だけが参照されます。

## おすすめ設定

### 初めて遊ぶ場合

```mcfunction
/chairgame roundtime 180
/chairgame curse off
/chairgame pvp off
/chairgame mode casual
```

### 妨害ありで遊ぶ場合

```mcfunction
/chairgame roundtime 180
/chairgame curse on
/chairgame killerprob 0.05
/chairgame pvp on
/chairgame mode casual
```

### 危険なルールで遊ぶ場合

```mcfunction
/chairgame roundtime 120
/chairgame curse on
/chairgame killerprob 0.1
/chairgame pvp on
/chairgame mode deathgame
```

## 設定ファイル

初回起動時に、Fabricの設定フォルダーへ`chairgamemod.json`が作成されます。

主な初期値は次のとおりです。

- 最低参加人数：2人
- 最大参加人数：8人
- ラウンド時間：300秒
- 最終警告：残り60秒
- 開始カウントダウン：5秒
- 使用ワールド：`minecraft:overworld`
- ゲーム範囲：中心座標 `(0, 0)`、一辺500ブロック
- ロビー座標：`(0, 100, 0)`
- PvPダメージ：無効
- キラーチェア：有効
- キラーチェア基本確率：5%

設定ファイルを直接編集した場合は、次のコマンドで再読み込みできます。

```mcfunction
/chairgame reloadconfig
```

既存の`chairgamemod.json`がある場合、JAR内の新しい初期値ではなく、既存ファイルに保存された値が優先されます。

## トラブルシューティング

### コマンドが表示されない

- Fabric 1.21.1で起動しているか確認してください。
- `chairgamemod-1.0.0.jar`とFabric APIが`mods`フォルダーにあるか確認してください。
- サーバーログにModの読み込みエラーがないか確認してください。

### ゲームを開始できない

- 2人以上が`/chairgame join`を実行したか確認してください。
- `/chairgame start`の実行者にOP権限があるか確認してください。
- すでにゲームが進行中でないか確認してください。

### 意図しない場所へ移動する

`chairgamemod.json`の使用ワールド、マップ中心、マップサイズ、ロビー座標を確認してください。

### 設定変更が反映されない

設定ファイルを保存した後、次を実行してください。

```mcfunction
/chairgame reloadconfig
```
