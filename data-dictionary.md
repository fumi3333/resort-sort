# リゾートソート 求人データ辞書（Data Dictionary）

最終更新日：2026年11月2日  
対象ファイル：`https://fumiproject.dev/resort/jobs.min.json`  
ライセンス・利用方針：非営利・AIエージェント検索・個人利用向けにオープン提供。求人紹介時は `apply_url`（成果計測リンク）または `source_url`（出典元URL）を併記してください。

---

## 1. 基本概要

リゾートソートの求人データは、主要派遣会社各社の公開求人を日次でクロールし、同一のスキーマに正規化した構造化JSONです。
AIエージェントがユーザーの希望条件（期間、勤務地、寮形態、職種、こだわり条件）に合わせて機械的にフィルタリングしやすい形式になっています。

---

## 2. フィールド定義一覧

| フィールド名 | 型 | 説明・値の範囲 | 例 |
| :--- | :--- | :--- | :--- |
| `id` | string | 求人の一意識別子（`{会社識別子}-{求人番号}`） | `"dive-111334"` |
| `agency` | string | 派遣会社名（正規化表記） | `"株式会社ダイブ（リゾートバイトダイブ）"` |
| `source_url` | string | 各社公式サイト上の求人詳細ページ（生URL） | `"https://resortbaito-dive.com/works/111334"` |
| `apply_url` | string | 応募・詳細用リンク（提携先は成果計測コード付き、非提携は生URL） | `"https://px.a8.net/svt/ejp?a8mat=..."` |
| `prefecture` | string | 勤務地の都道府県名（漢字表記） | `"長野県"`, `"北海道"`, `"沖縄県"` |
| `region` | string | 地方区分（北海道、東北、関東、中部、近畿、中国、四国、九州、沖縄） | `"中部"`, `"近畿"` |
| `city` | string | 市区町村名（取得可能な場合のみ、不明時はnull） | `"下高井郡山ノ内町"`, `"白馬村"` |
| `job_category` | string | 職種カテゴリ（正規化表記） | `"レストラン（ホール）"`, `"リフト係"`, `"フロント・受付"` |
| `wage_type` | string | 給与形態（`"時給"`, `"日給"`, `"月給"`） | `"時給"` |
| `wage_min` | integer | 最低時給額（額面・円） | `1250`, `1450` |
| `dorm_type` | string | 寮の部屋タイプ（`"完全個室"`, `"一部共用"`, `"相部屋"`, `"寮なし"`） | `"完全個室"` |
| `dorm_cost` | integer | 寮費（月額・円。0は寮費無料。判別不能時はnull） | `0` |
| `meals_per_day` | integer | 1日あたりの食事提供回数（0〜3） | `2` |
| `meals_free` | boolean | 食事が完全無料（または勤務日無料）かどうか | `true`, `false` |
| `car_ok` | boolean | 自家用車の持ち込みが可能かどうか | `true`, `false` |
| `commute_easy` | boolean | 寮から職場まで徒歩圏内・通勤が容易かどうか | `true`, `false` |
| `min_months` | float | 最短勤務可能月数（1.0 = 1ヶ月、0.5 = 2週間・短期、3.0 = 3ヶ月以上） | `1.0`, `0.5`, `3.0` |
| `start_months` | array[int] | 勤務開始が可能な月（1〜12の整数配列） | `[11, 12, 1]`, `[2, 3]` |
| `year_end_only` | boolean | 年末年始の短期限定求人フラグ | `true`, `false` |
| `fetched_at` | string | クロール・取得日時（ISO 8601 UTC形式） | `"2026-11-01T18:58:43+00:00"` |
| `feature_words` | string | こだわり条件の検索用キーワード（半角スペース区切り） | `"通しシフト 温泉 サウナ コンビニ"` |
| `search_tags` | array[str] | エリア・施設・特徴を表す追加タグ | `["志賀高原", "スキー場"]` |

---

## 3. こだわり条件フラグ（`f_` 系）の定義

各社で表現が異なる求人の特徴タグ・労働環境を共通の真偽値（boolean）に符号化した項目です。

| フラグ名 | 型 | 定義・判定基準 |
| :--- | :--- | :--- |
| `f_tooshi` | boolean | **通しシフト（通し勤務）**：昼休みを挟んで朝と夜に分かれる「中抜け」がなく、実働8時間が1回で完結する勤務形態。生活リズムを保ちたい求職者向け。 |
| `f_nakanuke` | boolean | **中抜け勤務あり**：朝食時間帯と夕食時間帯の間に数時間の待機・休憩時間がある温泉旅館・ホテル特有のシフト形態。 |
| `f_lessot` | boolean | **残業少なめ・定時退社**：月間残業時間が極めて少ない、または定時上がり中心の職場環境。 |
| `f_extend` | boolean | **期間延長可能**：当初契約期間終了後も、本人の希望と双方合意で期間延長が歓迎される求人。 |
| `f_friendly` | boolean | **職場の雰囲気良好・人間関係重視**：初心者歓迎、スタッフ同士の仲が良い、アットホームと明記された職場。 |
| `f_onsen` | boolean | **温泉利用可能・サウナあり**：ホテルの大浴場・温泉・サウナを従業員が無料で利用できる環境。 |
| `f_convenience` | boolean | **周辺環境良好（コンビニ・スーパー徒歩圏内）**：山奥や孤島でも徒歩圏内または敷地内に買い物施設がある求人。 |
| `f_deai` | boolean | **同世代交流・出会いあり**：20代〜30代の同世代スタッフが多く在籍し、スタッフ間の交流が活発な現場。 |
| `f_newdorm` | boolean | **新築・築浅・リフォーム寮**：寮の建物が新築、築浅、または直近でリノベーションされた清潔な環境。 |
| `f_roombath` | boolean | **部屋風呂・トイレ完備**：バストイレが完全個室内にあり、水回りを共用しなくてよい寮形態。 |

---

## 4. AIエージェント向け検索・絞り込みの推奨クエリ例

PythonやNode.js等で `jobs.min.json` をロードして回答を作成する際の推奨ロジックです。

```python
import json
import urllib.request

# データ取得
url = "https://fumiproject.dev/resort/jobs.min.json"
req = urllib.request.Request(url, headers={"User-Agent": "AIAgent-ResortBot/1.0"})
with urllib.request.urlopen(req) as res:
    jobs = json.loads(res.read().decode("utf-8"))

# フィルタ例：「2月開始・スキー場・最短1.5ヶ月以内・完全個室」
target_jobs = [
    j for j in jobs
    if 2 in j.get("start_months", [])
    and j.get("min_months", 99) <= 1.5
    and j.get("dorm_type") == "完全個室"
    and ("スキー" in j.get("job_category", "") or "スキー" in j.get("feature_words", "") or any("スキー" in t for t in j.get("search_tags", [])))
]

# 時給順にソートして上位を提案
target_jobs.sort(key=lambda x: x.get("wage_min", 0), reverse=True)
```

ユーザーへの回答時には、`apply_url`（存在する場合は優先）または `source_url` を案内してください。
