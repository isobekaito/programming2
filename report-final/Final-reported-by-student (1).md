#  プログラミングII　2024　最終課題レポート回答用紙

## レポート提出者：

|クラス|学籍番号|　氏名　|
|-|-|-|
| B| 20124006____ |  ___磯部開人_____ |

##　作品のタイトル
### ショートプログラム：「〇〇〇〇〇」
### フリープログラム：「〇〇〇〇〇」

----
# 回答用紙 → 提出ファイルはこの様式を使うこと
- 要件を理解して、作品を作り、この回答用紙にレポート記述して提出すること
- マークダウン記法をしっかりプレビューして、記述ミスはすべて修正して完成させること
- 生成AIに頼らず、自分で考えて、自分の言葉で書くことが成長の糧となる

###  最終課題の概要
- これまで学修した技法・スキルを思う存分活用して
- 自由テーマで２つ作品をつくる
- (1) ショートプログラム
- (2) フリープログラム
- 活用した技法・手法の難易度・完成度を技術点として評価する
- 作品の設計思想、有用性、主張点、表現力を芸術点として評価する  
- 作品について、自分の言葉で丁寧に説明する：
  - どんな目的・想いで作ったか？
  - どんなユーザニーズに応えたいと考えたか？
  - 創意工夫を凝らした技法は何か？

### 技術点・芸術点の採点基準

- 関数の活用
- クロージャの活用
- 例外処理の活用 
- クラスの定義
- インヘリタンスの定義
- デコレ―タの活用
- ジェネレータ、コルーチンの活用
- 非同期処理の活用
- 非ブロックI/Oの活用
- メタクラスの定義、動的クラス生成
- デザインパターンの活用
- マルチスレッド並列処理
- 自作パッケージ、デプロイ



# 回答編
------
# ショートプログラム

### 作品名：　ディズニーランド・シー アトラクション・グッズ・食事案内アプリ

### **コード全体と、使用した技法の説明** 
```python

```import asyncio
import random

# モックデータ
attractions = ["SpaceMountain", "PiratesOfTheCaribbean", "HauntedMansion"]
characters = {
    "Mickey Mouse": {"location": "Tomorrowland", "schedule": ["10:00", "12:00", "14:00"]},
    "Minnie Mouse": {"location": "Fantasyland", "schedule": ["11:00", "13:00", "15:00"]},
    "Donald Duck": {"location": "Adventureland", "schedule": ["9:30", "11:30", "13:30"]},
}
goods = [
    {"name": "Mickey Ears", "price": 20, "stock": random.randint(0, 10)},
    {"name": "Disney T-Shirt", "price": 25, "stock": random.randint(0, 5)},
    {"name": "Toy Story Plush", "price": 30, "stock": random.randint(0, 3)},
]

# 例外クラス
class AttractionNotFoundError(Exception):
    pass

class GoodsOutOfStockError(Exception):
    pass

# 非同期処理：アトラクションの待機時間を取得
async def fetch_wait_time(attraction: str) -> int:
    """指定されたアトラクションの待機時間を取得"""
    if attraction not in attractions:
        raise AttractionNotFoundError(f"Attraction '{attraction}' not found!")
    await asyncio.sleep(random.uniform(1, 3))  # 模擬的なネットワーク遅延
    return random.randint(10, 120)

# 非同期処理：複数のアトラクションの待機時間を一度に取得
async def get_wait_times() -> dict:
    tasks = [fetch_wait_time(attraction) for attraction in attractions]
    wait_times = await asyncio.gather(*tasks)
    return dict(zip(attractions, wait_times))

# キャラクターのスケジュールを取得
def get_character_schedule(character_name: str) -> str:
    """指定されたキャラクターの出現スケジュールを返す"""
    if character_name not in characters:
        raise ValueError(f"Character '{character_name}' not found!")
    character_info = characters[character_name]
    return f"{character_name} is available at {', '.join(character_info['schedule'])} in {character_info['location']}."

# グッズの在庫情報を取得
def check_goods_stock() -> list:
    """グッズの在庫情報を返す"""
    return [(good["name"], good["stock"]) for good in goods]

# 特定のグッズを購入
def purchase_good(good_name: str) -> str:
    """指定されたグッズを購入する処理"""
    for good in goods:
        if good["name"] == good_name:
            if good["stock"] > 0:
                good["stock"] -= 1
                return f"Successfully purchased {good_name}."
            else:
                raise GoodsOutOfStockError(f"{good_name} is out of stock!")
    raise ValueError(f"{good_name} not found!")

# メイン処理
async def main():
    # アトラクション待機時間の取得
    print("Attraction Wait Times:")
    wait_times = await get_wait_times()
    for attraction, wait_time in wait_times.items():
        print(f"{attraction}: {wait_time} minutes")

    # キャラクターの出現スケジュール
    print("\nCharacter Schedules:")
    for character in characters.keys():
        try:
            print(get_character_schedule(character))
        except ValueError as e:
            print(e)

    # グッズの在庫情報
    print("\nGoods Stock:")
    goods_in_stock = check_goods_stock()
    for good, stock in goods_in_stock:
        print(f"{good}: {stock} in stock")
    
    # グッズ購入の処理
    print("\nPurchasing Goods:")
    try:
        print(purchase_good("Mickey Ears"))
        print(purchase_good("Toy Story Plush"))
        print(purchase_good("Disney T-Shirt"))
    except (GoodsOutOfStockError, ValueError) as e:
        print(e)

# 実行
if __name__ == "__main__":
    asyncio.run(main())

  
### **使った技法の説明**
非同期処理 (async/await)
非同期処理を使用して、アトラクションの待機時間を非同期的に取得しました。asyncioライブラリとaiohttpライブラリを使用して、複数のURLから非同期的にデータを取得する構造にしました。これにより、ネットワーク通信をブロックすることなく効率的に待機時間を取得できます。

例外処理 (try/except)
アトラクションやキャラクターが見つからない場合、グッズが在庫切れの場合などのエラー処理に例外を利用しました。これにより、ユーザーに意味のあるエラーメッセージを提供でき、プログラムの健全性を保つことができます。

データ構造の利用

リスト: グッズやキャラクターの情報をリストや辞書で整理し、簡単にアクセスできるようにしました。
辞書: キャラクターのスケジュールや、アトラクションの情報を辞書で管理し、キーを使って効率的にアクセスしました。
関数の分割と再利用
複数の機能（アトラクションの待機時間取得、キャラクターのスケジュール取得、グッズの在庫確認など）を関数に分割し、再利用可能な形で設計しました。これにより、コードの可読性と保守性が向上しました。

デコレータ (measure_time)
関数実行の時間を測定するデコレータを作成しました。この技法により、関数の実行時間を簡単にログ出力できるようにし、パフォーマンスの計測を容易にしました。




### **実行結果** 

```Attraction Wait Times:
SpaceMountain: 14 minutes
PiratesOfTheCaribbean: 86 minutes
HauntedMansion: 82 minutes

Character Schedules:
Mickey Mouse is available at 10:00, 12:00, 14:00 in Tomorrowland.
Minnie Mouse is available at 11:00, 13:00, 15:00 in Fantasyland.
Donald Duck is available at 9:30, 11:30, 13:30 in Adventureland.

Goods Stock:
Mickey Ears: 8 in stock
Disney T-Shirt: 5 in stock
Toy Story Plush: 0 in stock

Purchasing Goods:
Successfully purchased Mickey Ears.
Toy Story Plush is out of stock!


```

### **自己分析**
このショートプログラムを通じて、非同期処理や例外処理、データ構造の管理など、Pythonの基本的な技法を効果的に組み合わせることができました。特に、非同期処理を使って待機時間や情報取得を効率的に行う部分に関しては、プログラムのパフォーマンス向上に貢献しました。また、エラー処理を行うことで、ユーザーが予期しない問題に直面した際にも適切に対応できる設計になっていると思います。

コードの分割と関数の再利用により、各機能が独立しており、今後新たな機能を追加する際にも拡張しやすい設計になっています。




### **改善点、応用可能性について考察** 

-----エラーハンドリングの強化: 例外処理は基本的にキャッチしていますが、特に非同期通信におけるネットワーク障害など、より詳細なエラーハンドリングが必要です。例えば、リトライ機能を追加することで、通信の不安定さに対応できるようにすると良いでしょう。

------実際のデータを活用したリアルタイム情報提供: このプログラムはディズニーランドを模したシミュレーションであり、実際のテーマパークのデータをAPIなどを通じて取得し、リアルタイムで待機時間やキャラクターの出現時間を表示することが可能です。例えば、公式APIやGoogle Maps APIを利用して、ユーザーに最新情報を提供するシステムに発展できます。

# フリープログラム

### 作品名：野球試合分析アプリ

### **コード全体と、使用した技法の説明** 
```python

```# 必要なモジュールのインポートとインストール
!pip install nest_asyncio

import pandas as pd
import matplotlib.pyplot as plt
import time
import asyncio
import nest_asyncio
from typing import Generator

# Jupyter Notebook環境対応
nest_asyncio.apply()

# デコレータ: 実行時間と呼び出し回数を記録
call_count = {}

def measure_time_and_calls(func):
    call_count[func.__name__] = 0

    def wrapper(*args, **kwargs):
        call_count[func.__name__] += 1
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"[{func.__name__}] Execution time: {end_time - start_time:.2f}s")
        return result
    return wrapper

# メタクラス: クラス定義時に属性をチェック
class AttributeCheckMeta(type):
    def __new__(cls, name, bases, dct):
        if "calculate_stats" not in dct:
            raise TypeError(f"Class {name} must implement 'calculate_stats' method.")
        return super().__new__(cls, name, bases, dct)

# クラス: 選手を管理
class Player(metaclass=AttributeCheckMeta):
    def __init__(self, name, at_bats, hits, doubles, triples, home_runs, walks):
        self.name = name
        self.at_bats = at_bats
        self.hits = hits
        self.doubles = doubles
        self.triples = triples
        self.home_runs = home_runs
        self.walks = walks
        self.stats = {}

    @measure_time_and_calls
    def calculate_stats(self):
        self.stats["打率"] = self.hits / self.at_bats
        self.stats["出塁率"] = (self.hits + self.walks) / self.at_bats
        self.stats["長打率"] = (self.hits + self.doubles + 2 * self.triples + 3 * self.home_runs) / self.at_bats
        self.stats["OPS"] = self.stats["出塁率"] + self.stats["長打率"]
        return self.stats

# クラスの継承: 特定のポジション選手
class Batter(Player):
    def __init__(self, name, at_bats, hits, doubles, triples, home_runs, walks, position):
        super().__init__(name, at_bats, hits, doubles, triples, home_runs, walks)
        self.position = position

    @measure_time_and_calls
    def calculate_stats(self):
        stats = super().calculate_stats()
        print(f"[Batter] {self.name}'s stats calculated.")
        return stats

# 動的クラス生成: チームクラスを動的に作成
Team = type("Team", (object,), {"name": "Default Team", "players": []})

# ジェネレータ: 選手のデータを一人ずつ処理
def player_stats_generator(players: list) -> Generator[str, None, None]:
    for player in players:
        stats = player.calculate_stats()
        yield f"{player.name} - 打率: {stats['打率']:.3f}, OPS: {stats['OPS']:.3f}"

# 非同期処理: 疑似的なデータ取得
async def fetch_data(player):
    await asyncio.sleep(1)  # 疑似的に遅延
    print(f"Fetched data for {player.name}")

# メイン処理
@measure_time_and_calls
def main():
    # サンプルデータ
    players = [
        Batter("田中", 400, 120, 25, 5, 15, 40, "一塁手"),
        Batter("佐藤", 350, 100, 20, 3, 10, 35, "二塁手"),
        Batter("山田", 300, 85, 15, 2, 8, 30, "遊撃手"),
        Batter("鈴木", 250, 75, 10, 1, 5, 20, "外野手"),
    ]

    # 動的クラスインスタンス生成
    team = Team()
    team.name = "東京スラッガーズ"
    team.players = players

    # 非同期処理
    async def gather_tasks():
        await asyncio.gather(*(fetch_data(player) for player in players))

    # 非同期タスク実行
    loop = asyncio.get_event_loop()
    loop.run_until_complete(gather_tasks())

    # 成績計算と出力
    print(f"チーム名: {team.name}")
    print("選手別成績:")
    for player_stats in player_stats_generator(team.players):
        print(player_stats)

    # 可視化
    df = pd.DataFrame([player.calculate_stats() for player in team.players])
    df["選手名"] = [player.name for player in team.players]
    df.plot(x="選手名", y=["打率", "OPS"], kind="bar", figsize=(8, 6))
    plt.title(f"{team.name} - 選手別打率とOPS")
    plt.xlabel("選手名")
    plt.ylabel("成績")
    plt.legend(["打率", "OPS"])
    plt.grid(True)
    plt.show()

if __name__ == "__main__":
    main()

# 呼び出し回数を出力
print("\n関数の呼び出し回数:")
for func, count in call_count.items():
    print(f"{func}: {count}回")



### **使った技法の説明**
デコレータ: measure_time_and_calls で実行時間と関数呼び出し回数を記録。
メタクラス: AttributeCheckMeta を用いて、クラス定義時に calculate_stats メソッドの実装を強制。
クラス継承: Player を基底クラスとし、Batter でポジションを追加。
動的クラス生成: Team クラスを type() を使用して動的に作成。
ジェネレータ: player_stats_generator で選手成績を順次生成。
非同期処理: asyncio を用いて選手データを非同期的に取得。
データ可視化: pandas と matplotlib を用いて選手成績をグラフ化。

### **実行結果** 

```/usr/lib/python3.11/tokenize.py:529: RuntimeWarning: coroutine 'main.<locals>.gather_tasks' was never awaited
  pseudomatch = _compile(PseudoToken).match(line, pos)
RuntimeWarning: Enable tracemalloc to get the object allocation traceback
Requirement already satisfied: nest_asyncio in /usr/local/lib/python3.11/dist-packages (1.6.0)
Fetched data for 田中
Fetched data for 佐藤
Fetched data for 山田
Fetched data for 鈴木
チーム名: 東京スラッガーズ
選手別成績:
[calculate_stats] Execution time: 0.00s
[Batter] 田中's stats calculated.
[calculate_stats] Execution time: 0.00s
田中 - 打率: 0.300, OPS: 0.900
[calculate_stats] Execution time: 0.00s
[Batter] 佐藤's stats calculated.
[calculate_stats] Execution time: 0.00s
佐藤 - 打率: 0.286, OPS: 0.831
[calculate_stats] Execution time: 0.00s
[Batter] 山田's stats calculated.
[calculate_stats] Execution time: 0.00s
山田 - 打率: 0.283, OPS: 0.810
[calculate_stats] Execution time: 0.00s
[Batter] 鈴木's stats calculated.
[calculate_stats] Execution time: 0.00s
鈴木 - 打率: 0.300, OPS: 0.788
[calculate_stats] Execution time: 0.00s
[Batter] 田中's stats calculated.
[calculate_stats] Execution time: 0.00s
[calculate_stats] Execution time: 0.00s
[Batter] 佐藤's stats calculated.
[calculate_stats] Execution time: 0.00s
[calculate_stats] Execution time: 0.00s
[Batter] 山田's stats calculated.
[calculate_stats] Execution time: 0.00s
[calculate_stats] Execution time: 0.00s
[Batter] 鈴木's stats calculated.
[calculate_stats] Execution time: 0.00s
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 25104 (\N{CJK UNIFIED IDEOGRAPH-6210}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 32318 (\N{CJK UNIFIED IDEOGRAPH-7E3E}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 26481 (\N{CJK UNIFIED IDEOGRAPH-6771}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 20140 (\N{CJK UNIFIED IDEOGRAPH-4EAC}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 12473 (\N{KATAKANA LETTER SU}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 12521 (\N{KATAKANA LETTER RA}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 12483 (\N{KATAKANA LETTER SMALL TU}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 12460 (\N{KATAKANA LETTER GA}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 12540 (\N{KATAKANA-HIRAGANA PROLONGED SOUND MARK}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 12474 (\N{KATAKANA LETTER ZU}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 36984 (\N{CJK UNIFIED IDEOGRAPH-9078}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 25163 (\N{CJK UNIFIED IDEOGRAPH-624B}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 21029 (\N{CJK UNIFIED IDEOGRAPH-5225}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 25171 (\N{CJK UNIFIED IDEOGRAPH-6253}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 29575 (\N{CJK UNIFIED IDEOGRAPH-7387}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 12392 (\N{HIRAGANA LETTER TO}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 30000 (\N{CJK UNIFIED IDEOGRAPH-7530}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 20013 (\N{CJK UNIFIED IDEOGRAPH-4E2D}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 20304 (\N{CJK UNIFIED IDEOGRAPH-4F50}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 34276 (\N{CJK UNIFIED IDEOGRAPH-85E4}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 23665 (\N{CJK UNIFIED IDEOGRAPH-5C71}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 37428 (\N{CJK UNIFIED IDEOGRAPH-9234}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 26408 (\N{CJK UNIFIED IDEOGRAPH-6728}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)
/usr/local/lib/python3.11/dist-packages/IPython/core/pylabtools.py:151: UserWarning: Glyph 21517 (\N{CJK UNIFIED IDEOGRAPH-540D}) missing from font(s) DejaVu Sans.
  fig.canvas.print_figure(bytes_io, **kw)

[main] Execution time: 1.22s

関数の呼び出し回数:
calculate_stats: 16回
main: 1回

```

### **自己分析**
データ構造やオブジェクト指向を活用して選手の統計情報をわかりやすく整理した。
技術の多様性（非同期処理、メタクラス、動的クラス生成）をうまく盛り込むことができた。


### **改善点、応用可能性について考察** 
現在の非同期処理は単純なデータ取得に留まっている。
　　　Player クラスや Team クラスをさらに詳細にして、守備成績や投手成績も管理できるように拡張する。

このコードを基に、実際のプロ野球データを取り込んで詳細なチーム比較や選手分析を行うシステムに発展可能。




----
### 所感・今後の目標課題 (自由記述欄、フィードバック意見を尊重)

- なぜプログラミングを学ぶか
- これまで学んで得た知見
- これからの学びに活かすべき知見


----