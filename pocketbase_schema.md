# PocketBase コレクション定義

このファイルは、PocketBase 管理画面で手動作成するためのスキーマメモです。  
PocketBase はすべてのコレクションに `id`、`created`、`updated` を自動で持つため、以下では追加フィールドだけを示します。

## rooms

用途: 現場ルーム、集合地、ルーム単位の車両設定。

| フィールド | 型 | 必須 | 備考 |
|---|---|---:|---|
| name | text | はい | 現場名 |
| vehicle_id | text | いいえ | `VEHICLES.id` |
| custom_w | number | いいえ | カスタム全幅 m |
| custom_h | number | いいえ | カスタム全高 m |
| custom_t | number | いいえ | カスタム総重量 t |
| meet_lat | number | いいえ | 集合地緯度 |
| meet_lng | number | いいえ | 集合地経度 |
| meet_label | text | いいえ | 集合地メモ |

推奨 API ルール:

```text
List/Search/View/Create/Update: 空欄（公開）
Delete: 管理者のみ
```

初期検証を優先して公開ルールにしています。社内運用では Tailscale またはリバースプロキシ側で入口を絞ってください。

## presences

用途: フォアグラウンド中の参加者現在地。古い `updated` はクライアント側で非表示。

| フィールド | 型 | 必須 | 備考 |
|---|---|---:|---|
| room_id | text | はい | `rooms.id` |
| display_name | text | はい | 表示名 |
| lat | number | いいえ | 緯度 |
| lng | number | いいえ | 経度 |

推奨 API ルール:

```text
List/Search/View/Create/Update/Delete: 空欄（公開）
```

## markers

用途: 渋滞、通行止め、工事、狭い・離合不可などの共有マーカー。

| フィールド | 型 | 必須 | 備考 |
|---|---|---:|---|
| room_id | text | はい | `rooms.id` |
| kind | select | はい | `jam`, `closed`, `work`, `narrow`, `other` |
| lat | number | はい | 緯度 |
| lng | number | はい | 経度 |
| note | text | いいえ | メモ |
| author | text | いいえ | 設置者表示名 |

推奨 API ルール:

```text
List/Search/View/Create/Update/Delete: 空欄（公開）
```

## passages

用途: セッション終了時に一括送信する通行実績ログ。リアルタイム購読しない。

| フィールド | 型 | 必須 | 備考 |
|---|---|---:|---|
| room_id | text | いいえ | ルームID |
| room_name | text | いいえ | 現場名スナップショット |
| road_name | text | はい | `ROADS.properties.name` |
| road_class | text | いいえ | 重要物流道路/緊急輸送道路/一般道 |
| vehicle_id | text | いいえ | カスタム時は空 |
| vehicle_name | text | いいえ | 車両名スナップショット |
| total_weight | number | いいえ | 総重量 t |
| weight_class | select | いいえ | `light`, `medium`, `heavy`, `veryheavy` |
| load_factor | number | いいえ | 目安係数 |
| traverse_sec | number | いいえ | 区間所要秒 |
| entered_at | date | いいえ | 路線進入時刻 |
| passed_at | date | いいえ | ログ保存時刻 |

推奨 API ルール:

```text
List/Search/View: 管理者のみ
Create: 空欄（公開）
Update/Delete: 管理者のみ
```

## trip_logs

用途: セッション終了時に一括送信する走行サマリ。距離はGPS概算。

| フィールド | 型 | 必須 | 備考 |
|---|---|---:|---|
| room_id | text | いいえ | ルームID |
| room_name | text | いいえ | 現場名スナップショット |
| display_name | text | いいえ | 表示名 |
| vehicle_id | text | いいえ | カスタム時は空 |
| vehicle_name | text | いいえ | 車両名スナップショット |
| distance_km | number | いいえ | GPS概算走行距離 km |
| round_trips | number | いいえ | 集合地到着→離脱の回数 |
| duration_sec | number | いいえ | 位置共有ONから終了まで |
| started_at | date | いいえ | 開始時刻 |
| ended_at | date | いいえ | 終了時刻 |

推奨 API ルール:

```text
List/Search/View: 管理者のみ
Create: 空欄（公開）
Update/Delete: 管理者のみ
```

## インデックス

PocketBase 管理画面のコレクション設定で、必要に応じて以下を追加します。

```sql
CREATE INDEX idx_presences_room_id ON presences (room_id);
CREATE INDEX idx_markers_room_id ON markers (room_id);
CREATE INDEX idx_passages_road_name ON passages (road_name);
CREATE INDEX idx_passages_passed_at ON passages (passed_at);
CREATE INDEX idx_trip_logs_room_id ON trip_logs (room_id);
CREATE INDEX idx_trip_logs_ended_at ON trip_logs (ended_at);
```
