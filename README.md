# 大型車両 通行ルート共有ツール

フェーズ0の車両諸元による通行可否ビューアを土台に、PocketBase で現場ルーム、現在地、集合地、規制マーカーを共有するフェーズ1試作です。

## 成果物

- `index.html`: Leaflet + 素の JavaScript + PocketBase JS SDK の単体HTML
- `pocketbase_schema.md`: PocketBase 管理画面で作成するコレクション定義
- `public_traffic_sources.json`: JARTIC/国交省系の無償交通データ接続候補
- `public_traffic_data_plan.md`: 利用規約確認、申請、実装順のメモ

PocketBase 未接続でも、車両選択、路線色分け、現場ピン、最寄り通行可ルート表示は動きます。

## PocketBase 起動

1. PocketBase を取得します。

   [PocketBase Releases](https://github.com/pocketbase/pocketbase/releases) から Windows 用 zip をダウンロードし、任意のフォルダへ展開します。

2. `pb_public` を作り、`index.html` を配置します。

   ```powershell
   New-Item -ItemType Directory -Force .\pb_public
   Copy-Item "C:\Users\tatelogmk\Documents\いろいろ\index.html" .\pb_public\index.html
   ```

3. PocketBase を起動します。

   ```powershell
   .\pocketbase.exe serve --http 0.0.0.0:8090
   ```

4. 管理画面を開きます。

   ```text
   http://127.0.0.1:8090/_/
   ```

5. `pocketbase_schema.md` に沿って、`rooms`、`presences`、`markers`、`passages`、`trip_logs` を作成します。

6. アプリを開きます。

   ```text
   http://127.0.0.1:8090/
   ```

## HTTPS と Tailscale

Geolocation API は HTTPS が必要です。ローカル検証以外では、次のどちらかで HTTPS 化してください。

- Tailscale の MagicDNS + TLS を使う
- Caddy などのリバースプロキシで PocketBase に HTTPS を付与する

本番方針は、現場位置情報と通行ログを外部クラウドに置かず、自社管理の PocketBase に集約することです。

## 検証手順

1. PocketBase を起動し、`http://127.0.0.1:8090/` を開きます。
2. ルームID、表示名、現場名を入力して「新規作成」を押します。
3. 共有URLを別ブラウザ、別端末、または同じPCの別ウィンドウで開きます。
4. 別の表示名で同じルームに「参加」します。
5. 「位置共有 ON」を押します。GPSが使えない環境では「現在地を手動設置」を押し、地図をタップします。
6. 互いの現在地ラベルが地図と参加者リストに表示されることを確認します。
7. 「集合地を設定」を押して地図をタップし、全員に集合地が表示されることを確認します。
8. 「渋滞」「通行止め」「工事」「狭い」のいずれかを押して地図をタップし、全員に共有マーカーが表示されることを確認します。
9. 位置共有をONにしたまま路線近傍を移動し、退出または位置共有OFFで `passages` と `trip_logs` が作成されることを管理画面で確認します。

## 注意

- このツールは一次検討・現場共有のための補助です。正式な特殊車両通行可否は国の「特殊車両通行確認システム」を確認してください。
- 交通情報は無償の公的・準公的データを優先して連携します。JARTIC/国交省交通量APIやJARTIC交通規制オープンデータは、利用規約、出典表記、取得頻度、保存・再配布可否を確認してから有効化してください。
- 位置共有は、この画面を開いている間だけ動きます。タブを閉じる、またはバックグラウンドに回ると更新されません。
- GPSが使えないセッションでは通行実績ログを保存しません。誤データ防止のため、手動設置のみのセッションは `passages` / `trip_logs` の対象外です。
- 走行距離はGPS概算です。月報や交渉資料では「GPS概算」と明示してください。
