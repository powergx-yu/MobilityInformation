# MobilityInformation

沿線の発車標を、そのままスマホに。

東京の通勤者のための「発車標」PWA。乗り換え案内のように行程を計算して
押しつけるのではなく、道中で目にする掲示板そのものを表示します——
バス停のバス発車標、駅の電車発車標。そして各発車時刻から、物理的に
乗り継げる最初の接続へ、1本だけ正直な矢印を引きます。
おすすめ（推奨）は矢印とは別の枠で示し、矢印に混ぜ込むことはしません。

## できること

- **自宅ページ** — 左に次のバス3本、右に駅の全発車標（通過列車は細い
  ストリップに折り畳んで表示）。各バスから「物理的に乗り継げる最初の
  電車」へ細い矢印を引きます。推奨は別枠で表示し、ルールは
  「直通優先」：乗り換え案内が推奨になるのは、直通より15分以上早く
  着く場合だけです。
- **駅ページ** — 両方向を1ページに上下表示。駅にいるときは行き先が
  どちらか分からないため、両方の発車標を出し、時間帯で可能性の高い
  方向を上にします。
- **職場ページ** — 帰り道のための鏡像。自宅の最寄り駅まで行かない
  電車は、その行に乗り換え駅と到着時刻を明記します。隠れた乗り換えは
  ありません。
- ページ切り替えは完全手動です。位置情報は使いません。開いたときは
  前回見ていたページがそのまま表示されます。
- 静的時刻表だけで完全オフライン動作。ODPTのコンシューマーキーを
  設定すれば、対応区間でリアルタイム層も有効化できます。

## インストール（スマホ）

Chrome（Android）または Safari（iOS）で Pages サイトを開き、
「ホーム画面に追加」を選んでください。ネイティブアプリのように
全画面で動きます。

Live site: https://powergx-yu.github.io/MobilityInformation/

## 駅ライブラリ

駅と時刻表は `data/` にプレーンな JSON として置かれ、アプリのロジックは
駅に依存しません。同梱駅：保谷（西武池袋線＋地域バス）、池袋、
小竹向原、市ヶ谷（有楽町線）。既定のサンプルプロファイルは
保谷 ⇄ 市ヶ谷です。設定で両端の駅を変更できます。

駅を追加するには、ドキュメント化された形式で時刻表ファイルを作成します。
[tools/DATA.md](tools/DATA.md) を参照してください。

## 設計上の約束

- 矢印の意味は1つだけ：「物理的に乗り継げる」。好みのロジック
  （直通優先、15分ルール、同一ホーム乗り換え、1駅手前で降りて
  乗り過ごしを防ぐ手筋）は推奨枠の中に置き、発車標が嘘をつかない
  ようにしています。
- 通過列車を表示するのは、本物の発車標がそうしているからです。
  通過時刻は推定値（±1分、隣接停車駅から算出）で、並び順のためだけに
  使います。
- データ出典：各事業者の公式時刻表（西武バス／西武鉄道／東京メトロ）。
  各ファイルは2つの独立した掲載と照合済みで、`_meta` に出典と
  ダイヤ改正日を保持しています。

---

## English

A departure-board PWA for Tokyo commuters. Instead of computing an
itinerary for you, it shows you the boards you would see along the way —
the bus board at your stop, the train board at your station — and draws
one honest arrow from each departure to the first connection it can
physically catch. Recommendations are marked separately, never smuggled
into the arrows.

- **Home page** — your next 3 buses on the left; the full departure board
  at your station on the right (including trains that pass without
  stopping, folded into thin strips). A thin arrow links each bus to the
  first train it can catch; a separate "推奨" frame marks the recommended
  train, computed with a direct-train-first rule: a transfer only wins if
  it arrives at least 15 minutes earlier.
- **Station page** — both directions on one page, stacked. When you're at
  the station you may be going either way, so both boards are there; the
  likely direction for the time of day comes first.
- **Work page** — the mirror image for the way home. Trains that don't
  reach your home station say so on their row, with the transfer point
  and the resulting arrival time printed inline. No hidden transfers.
- Page switching is fully manual — the app never uses your location.
  It reopens on the page you last looked at.
- Works fully offline on static timetables. An optional ODPT consumer key
  (Settings) enables a live layer where supported.

Stations and timetables live in `data/` as plain JSON — the app logic is
station-agnostic. Bundled today: Hoya (Seibu Ikebukuro line + local
buses), Ikebukuro, Kotake-Mukaihara, Ichigaya (Yurakucho line). See
[tools/DATA.md](tools/DATA.md) for the data format.
