# GitHub Pages / LP 構想メモ

## 目的

α版を GitHub Pages で公開し、スマホから HTTPS で GPS・地図・ルート案内を検証できる状態にする。将来的にはトップページを LP にして、アプリ本体・技術説明・注意事項へ導線を分ける。

## 想定構成

```text
/
  index.html          # LP。まだ作らない
/app/
  index.html          # 地図アプリ本体
/docs/
  index.html          # データ出典・判定ロジック・制約説明
```

当面は `index.html` をそのまま Pages のトップに置いて公開してもよい。LPを作る段階で、アプリ本体を `/app/` に移す。

## LPに載せる内容

- α版であること
- 対象: 建設・現場搬入・大型車両の事前ルート確認
- できること: GPS現在地、行き先検索、ルート候補、音声案内、通行可否の参考表示
- 準備中: チーム共有、共有マーカー、リアルタイム交通情報API、公的データ連携
- 注意: 正式な通行可否は特殊車両通行確認システムで確認
- データ出典: 地理院タイル、OpenStreetMap、将来のJARTIC/公的データ候補
- CTA: 「α版を開く」

## 公開時の注意

- GitHub Pages は HTTPS なので、Tailscale HTTP より GPS 検証に向く
- PocketBase 共有機能は Pages 単体では動かないため、α版ではグレーアウト
- APIキーやCORS制限がある交通情報は、必要に応じて中継サーバーを検討
- LP作成後も `/app/` のURLをLINE共有で使えるようにする

## 後でやる作業

1. `app/` を作って現在の `index.html` を移動
2. ルートの `index.html` をLPとして新規作成
3. `README.md` にPages公開URLとα版範囲を追記
4. GitHub Pages の公開元を対象ブランチに設定
5. スマホでGPS許可・音声案内・ルート表示を確認
