# 大型車両ルート確認アプリ 取扱説明書案

> Notion掲載用の叩き台です。本文はNotionの通常ブロック、画面イメージはHTMLブロックに貼り付ける想定です。

## これは何をするアプリですか

現在地と行き先を入れると、大型車が通れそうな道を地図上に表示します。国土地理院の地図を使い、曲がり角や道幅の確認、音声案内にも対応した試作版です。

## 使う前に

1. 車両を選びます。
2. 位置情報の使用を許可します。
3. 行き先を入力します。
4. 行き先候補を地図で確認します。
5. ルート候補を選びます。
6. 問題なければ案内を開始します。

## 画面の流れ

### 1. 車両を選ぶ

最初に、使う車両を選びます。

- 中型
- 大型
- 特殊
- その他

車両が分からない場合は、近い区分を選びます。詳しい条件は管理画面で変更できます。

### 2. 行き先を入れる

住所、施設名、現場名などを入力します。候補が複数出る場合があります。

候補を選ぶと地図にピンが出ます。場所が正しいか確認してから確定します。

### 3. ルートを選ぶ

ルート候補が表示されます。

表示される情報:

- 距離
- 目安時間
- 高速道路を含むか
- 通行可能道路との整合状況

候補を選ぶと地図上で確認できます。確定するまで案内は始まりません。

### 4. 運転時

運転時は、直近の案内だけを大きく表示します。

- 次に曲がる方向
- 次の区間までの距離
- 案内開始
- ルート再検索
- 曲がり確認

運転中は、関係ない道路を押して通行可否を見る機能は出ません。誤操作を減らすためです。

### 5. 曲がり確認

曲がり角で大型車が曲がれそうか、地図上に車両の軌跡を重ねて確認できます。

これは参考表示です。実際には現地確認や誘導員の判断を優先してください。

## 管理画面

管理画面では次を変更できます。

- 車両条件
- 画面のライト/ダーク表示
- 音声案内
- 縮尺表示
- ホーム画面に追加
- 表示レイヤ

## 注意

- このアプリは正式な通行許可を出すものではありません。
- 正式な確認は、特殊車両通行確認システムなどで行ってください。
- GPSが使えない場合は、HTTPSのページで開いているか確認してください。
- ルートや曲がり角の表示は参考です。現地状況を優先してください。

## HTMLブロック用モック

下のHTMLをNotionのHTMLブロックに貼り付ける想定です。  
Notion側でJavaScriptが使えない場合でも、静的な画面見本として読めるようにしています。

```html
<div style="font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; max-width: 880px; border: 1px solid #d7dee7; border-radius: 16px; overflow: hidden; background: #f6f8fb; color: #17212b;">
  <div style="display: grid; grid-template-columns: 1.2fr .9fr; min-height: 420px;">
    <div style="position: relative; background: linear-gradient(135deg, #dfeaf5, #f8fbff); border-right: 1px solid #d7dee7; padding: 18px;">
      <div style="position: absolute; inset: 18px; background-image: linear-gradient(#b8c7d6 1px, transparent 1px), linear-gradient(90deg, #b8c7d6 1px, transparent 1px); background-size: 44px 44px; opacity: .45;"></div>
      <div style="position: relative; height: 100%;">
        <div style="position: absolute; left: 10%; top: 66%; width: 68%; height: 10px; background: #32c57a; border-radius: 999px; transform: rotate(-8deg);"></div>
        <div style="position: absolute; left: 20%; top: 28%; width: 62%; height: 10px; background: #ff9f1c; border-radius: 999px; transform: rotate(30deg);"></div>
        <div style="position: absolute; left: 52%; top: 22%; width: 10px; height: 58%; background: #32c57a; border-radius: 999px;"></div>
        <div style="position: absolute; left: 48%; top: 48%; width: 34px; height: 34px; border-radius: 50% 50% 50% 0; background: #ffd400; transform: rotate(-45deg); box-shadow: 0 4px 12px rgba(0,0,0,.25);"></div>
        <div style="position: absolute; left: 50%; top: 51%; width: 12px; height: 12px; background: #243447; border-radius: 50%;"></div>
        <div style="position: absolute; right: 16px; top: 16px; background: #111827; color: white; padding: 12px 14px; border-radius: 12px; box-shadow: 0 10px 28px rgba(0,0,0,.25);">
          <div style="font-size: 12px; color: #ffd400; font-weight: 700;">出発地点</div>
          <div style="font-size: 15px; font-weight: 800; margin-top: 4px;">富山県庁</div>
          <div style="font-size: 12px; opacity: .7; margin-top: 4px;">重要物流道路・直線約148m</div>
        </div>
      </div>
    </div>
    <div style="background: #111827; color: #e9eef3; padding: 18px; display: flex; flex-direction: column; gap: 14px;">
      <div>
        <div style="font-size: 12px; color: #ffd400; font-weight: 800;">STEP 1</div>
        <div style="font-size: 22px; font-weight: 900; margin-top: 4px;">ルート選定</div>
        <div style="font-size: 13px; color: #9aa7b3; margin-top: 4px;">車両・行き先・高速道路条件を確認します。</div>
      </div>
      <div style="background: #1b242d; border: 1px solid #2b3742; border-radius: 12px; padding: 12px;">
        <div style="font-size: 12px; color: #9aa7b3;">あなたの車両</div>
        <div style="display: flex; gap: 8px; margin-top: 8px;">
          <span style="flex: 1; text-align: center; padding: 10px; border-radius: 10px; background: #222d38;">中型</span>
          <span style="flex: 1; text-align: center; padding: 10px; border-radius: 10px; background: rgba(255,212,0,.16); color: #ffd400; border: 1px solid #ffd400;">大型</span>
          <span style="flex: 1; text-align: center; padding: 10px; border-radius: 10px; background: #222d38;">特殊</span>
        </div>
      </div>
      <div style="background: #1b242d; border: 1px solid #2b3742; border-radius: 12px; padding: 12px;">
        <div style="font-size: 12px; color: #9aa7b3;">行き先</div>
        <div style="margin-top: 8px; background: #0d1117; border-radius: 8px; padding: 12px;">富山県庁</div>
        <div style="display: flex; gap: 8px; margin-top: 10px;">
          <span style="background: #ffd400; color: #111827; padding: 10px 14px; border-radius: 10px; font-weight: 900;">ルート表示</span>
          <span style="background: #222d38; padding: 10px 14px; border-radius: 10px; font-weight: 900;">共有</span>
        </div>
      </div>
      <div style="background: #1b242d; border: 1px solid #2b3742; border-radius: 12px; padding: 12px;">
        <div style="font-size: 13px; font-weight: 800;">候補1 約4.2km / 12分</div>
        <div style="font-size: 12px; color: #9aa7b3; margin-top: 4px;">高速道路なし / 通行可能道路と概ね整合</div>
      </div>
    </div>
  </div>
</div>
```

