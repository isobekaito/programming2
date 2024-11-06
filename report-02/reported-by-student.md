
##  プログラミングII　2024　課題レポート（第2回）

# 解答用紙 → 提出ファイルはこちら　
- 問題用紙をよく読み、この回答用紙にレポート記述して提出すること
- マークダウン記法を、しっかりプレビューして、記述ミスはすべて修正して完成させること


レポート提出者：

|クラス|学籍番号|　氏名　|
|-|-|-|
| B| 20124006____ |  ___磯部開人_____ |

------

# 解答編

- それぞれの問題をよく読み、以下の項目に自分の言葉でしっかり記述する
- マークダウン記法を、しっかりプレビューして、記述ミスはすべて修正して完成させる
- 生成AIに頼らず、自分で考えて、自分の言葉で書くことに重要な意味がある

------

## **課題1. 関数の作成とスコープルールの理解**

### **問題 1-1: 温度変換関数**

**作成したpythonコード**

```# 絶対零度の定義
# 絶対零度の定義
absolute_zero_celsius = -273.15  # 摂氏
absolute_zero_fahrenheit = -459.67  # 華氏

# 摂氏 → 華氏 (Celsius to Fahrenheit)
def celsius_to_fahrenheit(celsius: float) -> float:
    return celsius * 9 / 5 + 32

# 華氏 → 摂氏 (Fahrenheit to Celsius)
def fahrenheit_to_celsius(fahrenheit: float) -> float:
    return (fahrenheit - 32) * 5 / 9

# 関数合成: 摂氏 → 華氏 → 摂氏
def c_2_f_2_c(c: float) -> float:
    return fahrenheit_to_celsius(celsius_to_fahrenheit(c))

# 合成: 華氏 → 摂氏 → 華氏
def f_2_c_2_f(f: float) -> float:
    return celsius_to_fahrenheit(fahrenheit_to_celsius(f))

# 絶対零度を使った動作確認
print("摂氏 → 華氏 → 摂氏:", c_2_f_2_c(absolute_zero_celsius))  # -273.15 に戻るべき
print("華氏 → 摂氏 → 華氏:", f_2_c_2_f(absolute_zero_fahrenheit))  # -459.67 に戻るべき

# 浮動小数の誤差許容範囲をもって判定
tolerance = 1e-8  # 浮動小数点の誤差の許容範囲
assert abs(c_2_f_2_c(absolute_zero_celsius) - absolute_zero_celsius) < tolerance, "摂氏変換エラー"
assert abs(f_2_c_2_f(absolute_zero_fahrenheit) - absolute_zero_fahrenheit) < tolerance, "華氏変換エラー"

print("動作確認：成功")







```

**動作確認結果**
```
摂氏 → 華氏 → 摂氏: -273.15
華氏 → 摂氏 → 華氏: -459.66999999999996
動作確認：成功


```

**分析** 

<!-- 
この問題は、温度返還の数式と関数の組み合わせて処理を行う合成関数の概念を学び、浮動小数点の精度理解し、関数プログラミングの習得が求められている。
-->

**考察**  

<!-- 
  - 温度返還の関数を定義し、それを組み合わせることが重要です。関数プログラミングの利点は温度変換の計算を関数として独立させることで、他の計算でも再利用できる。応用シーンでは計算やデータ処理が頻繁に行われるデータ分析に使われるのではないかと思う。
  - 
-->

**改善点**  
<!-- 
   - エラー処理を追加して誤った数値を入力してもプログラムが適切に対応できるようにするべきだと思いました。
-->

---

### **★チャレンジ問題 (1-1-aux)　長さ・重さの単位変換**

**作成したpythonコード**

```python

def inch2cm(inch: float) -> float:   
    return inch * 2.54

def cm2inch(cm: float) -> float:   
    return cm / 2.54


def pound2gram(pound: float) -> float:
    return pound * 453.592

def gram2pound(gram: float) -> float:
    return gram / 453.592

# 動作確認
inch_value = 7
cm_value = inch2cm(inch_value)
print(f"{inch_value} インチは {cm_value:.2f} センチメートル")
print(f"{cm_value:.2f} センチメートルは {cm2inch(cm_value):.2f} インチ")

pound_value = 5
gram_value = pound2gram(pound_value)
print(f"{pound_value} ポンドは {gram_value:.2f} グラム")
print(f"{gram_value:.2f} グラムは {gram2pound(gram_value):.2f} ポンド")



```

**動作確認結果**
```

7 インチは 17.78 センチメートル
17.78 センチメートルは 7.00 インチ
5 ポンドは 2267.96 グラム
2267.96 グラムは 5.00 ポンド

```

**分析** 

<!-- 
出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

**考察**  

<!-- 
  - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
  - 用いた技法、例えば、関数プログラミングの利点、プロトコルの活用場面について、応用シーンを考察する
-->

**改善点**  
<!-- 
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

----

### **問題 1-2: スコープルールの確認**

**作成したpythonコード**

```python

# グローバルスコープの変数
x = 24


def outer():
    # outer関数内のローカル変数
    x = 12


    def inner():
        nonlocal x # どの 'x'を意味するか？ X=12
        x += 1  # どの 'x' を 1 増加するか？X=12
        print(f"Inner: {x}")

    inner() # inner関数を実行
    print(f"Outer: {x}") # どの 'x'を意味するか？X=13
    return inner  # inner関数をリターンする：クロージャ（関数閉包）

# outer関数を実行
o = outer() # 何が出力されるか? それはなぜか？  Inner()を実行してxが12から13に増加するため Inner: 13 print(f"outer: {x}")が実行されOuter: 13が出力されます。
o()         # 何が出力されるか? それはなぜか？　Inner: 14　o()はouter()の戻り値として返されたinner()を再度実行します。nonlocal xによりxは1増加し、14になります。print(f"Inner: {x}")が実行され、Inner: 14が出力されます。
print(f"Global: {x}")   # どの 'x'を意味するか？ x =24
p = outer()  # 何が出力されるか? それはなぜか？　p = outer()を実行すると、xは新たに12に初期化されてinner()が呼び出され、Inner: 13 Outer: 13が出力されます。
p()          # 何が出力されるか? それはなぜか？ p()を実行すると、pに対応するinner()が再度実行され、xは1増加して14となり、Inner: 14
print(f"Global:{x}")     # どの 'x'を意味するか？ Global:　24
o()   # 何が出力されるか? それはなぜか？ xが1増加し15になり Inner: 15
o()   # 何が出力されるか? それはなぜか？ xがさらに1増加し16になり　Inner: 16
p()   # 何が出力されるか? それはなぜか？ xが1増加し15になり、Inner:15
print(f"Global: {x}")     # どの 'x'を意味するか？ グローバルスコープのx = 24を指していて　Global: 24と出力される。

```

**動作確認結果**
```

Inner: 13
Outer: 13
Inner: 14
Global: 24
Inner: 13
Outer: 13
Inner: 14
Global:24
Inner: 15
Inner: 16
Inner: 15
Global: 24


```

**分析** 

<!-- 
この問題はスコープの名前空間の有効範囲を理解、習得する。
-->

**考察**  

<!-- 
  - スコープの階層構造を意識し、再帰的関数を複数回呼び出すのに、変数がどのように変化するかを理解する。
  
-->

**改善点**  
<!-- 
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

----


### **★チャレンジ問題 (1-2-aux)**　グローバルコンテキスト、ローカルコンテキストの仕組み


**作成したpythonコード**

```python




```

**動作確認結果**
```



```

**分析** 

<!-- 
出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

**考察**  

<!-- 
  - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
  - 用いた技法、例えば、関数プログラミングの利点、プロトコルの活用場面について、応用シーンを考察する
-->

**改善点**  
<!-- 
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

----
----

## **課題2. 関数プログラミング**

### **問題 2-1: 高階関数と部分関数**

**作成したpythonコード**

```python
Three = 3
from functools import partial

# n が d の倍数かどうか
def divisible_by(n: int, d=Three) -> bool:
    return n % d == 0

# n の 10 進数表記に '3' が含まれるかどうか
def has_char(n: int, d=str(Three)) -> bool:
    return d in str(n)

# n を m 倍する関数
def multiply_by(n: int, m=Three) -> int:
    return n * m

# m=3倍する部分関数
triples = partial(multiply_by, m=Three)

# 合成されたフィルタ関数：n が 3 の倍数かつ '3' を含む
def combined_filter(n: int) -> bool:
    return divisible_by(n) and has_char(n)

# 3の倍数かつ '3' を含む数を 3 倍する
def triple_three(numbers: list[int]) -> list[int]:
    # 合成フィルタ関数を使用
    filtered_numbers = filter(combined_filter, numbers)
    # 各要素を 3 倍する
    result = map(triples, filtered_numbers)
    return list(result)

# 動作確認
if __name__ == "__main__":
    numbers = list(range(1, 100))
    result = triple_three(numbers)
    print(f"Result: {result}")



```

**動作確認結果**
```

Result: [9, 90, 99, 108, 117, 189, 279]


```

**分析** 

<!-- 
部分関数(functools.partial)と高階関数(combined_filter)の活用の仕方を理解し習得する。
-->

**考察**  

<!-- 
  - 関数の分割をし複数の単純な関数に分割することで個々の関数を独立してテストができる。次に、高階関数を利用し、ループを使わずに条件を満たす要素を簡単に抽出し、変換できるようにする。
  最後に、部分関数を利用し、特定の引数を固定した新しい関数を生成し、引数を毎回指定する必要がなくなった。
  -
-->

**改善点**  
<!-- 
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

----

### **問題 2-2: デコレータの実装**

**作成したpythonコード**

```python

import time
from functools import wraps

def timer(func):
    @wraps(func)  # 関数メタデータを保持するためのデコレータ
    def wrapper(*args, **kwargs):  # 実装する
        # 実行開始前の時刻を記録
        start_time = time.time()
        print(f"{func.__name__}(args={args}, kwargs={kwargs})")  # 呼び出し時の引数を表示
        # 対象関数を実行
        result = func(*args, **kwargs)
        # 実行終了後の時刻を記録
        end_time = time.time()
        # 実行時間を計算
        duration = end_time - start_time
        print(f"実行時間: {duration:.4f} 秒")
        return result
    return wrapper

# デコレータを適用した関数
@timer
def example(sec: int = 1) -> None:
    time.sleep(sec)  # sec秒間待機
    print("関数が実行されました")

# テスト実行
if __name__ == "__main__":
    example(sec=2)


```

**動作確認結果**
```

example(args=(), kwargs={'sec': 2})
関数が実行されました
実行時間: 2.0032 秒


```

**分析** 

<!-- 
デコレータの基本的な使い方と実行時間の計測を習得する。
-->

**考察**  

<!-- 
  - デコレータによる関数の拡張を意識したり、timeを利用して処理前後の時間を記録し、実行時間を計算する。
  - 
-->

**改善点**  
<!-- 
   - エラーハンドリングを追加して、例外が出た場合でも実行できるようにした方がよいと思う。
-->


----
----

## **課題3. プロトコルの実装**

### **問題 3-1: イテレータの実装**


**作成したpythonコード**

```python

class FibonacciIterator:
    def __init__(self, size: int):
        # 初期化
        self.size = size  # 生成するフィボナッチ数の個数
        self.count = 0  # 何個目かを追跡
        self.a, self.b = 0, 1  # フィボナッチ数列の初期値（ゼロ始まりと仮定）

    def __iter__(self):
        # イテレータを返す
        return self

    def __next__(self):
        # 次のfibonacci数を返す
        if self.count >= self.size:
            raise StopIteration  # 指定件数に達したら停止

        current = self.a  # 現在のフィボナッチ数を取得
        self.a, self.b = self.b, self.a + self.b  # 次のフィボナッチ数へ更新
        self.count += 1  # カウントを進める
        return current  # 現在のフィボナッチ数を返す

# 20個のフィボナッチ数
fib_iter = FibonacciIterator(20)
for num in fib_iter:
    print(num, end=',')


```

**動作確認結果**
```

0,1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,1597,2584,4181

```

**分析** 

<!-- 
イテレータの基礎を理解し、フィボナッチ数列を効率的に行う手法を学ぶ。
-->

**考察**  

<!-- 
  - まずフィボナッチ数列が何かを理解し、イテレータプロトコルのメソッドである_iter_,_next_を使用し、反復処理を可能にする。

  - 
-->

**改善点**  
<!-- 
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

---

### **問題 3-2: コンテキストマネージャの作成**

**作成したpythonコード**

```python

class FileManager:
    def __init__(self, filename: str, mode: str):
        self.filename = filename
        self.mode = mode
        self.file = None  # ファイルオブジェクトを初期化

    def __enter__(self):
        # ファイルを指定モードで開く
        self.file = open(self.filename, self.mode)
        return self.file  # ファイルオブジェクトを返す

    def __exit__(self, exc_type, exc_value, traceback):
        # ファイルを閉じる
        if self.file:
            self.file.close()

# ファイルを書き込みモードで開き、文字列を書き込む
with FileManager('test.txt', 'w') as f:
    f.write('Hello, World!')


```

**動作確認結果**
```

上記のコードを実行すると、test.txt  ファイルが作成される
ファイルには文字列︓  Hello, World!  が書き込まれる

```

**分析** 

<!-- 
コンテキストマネジャーを利用して、ファイルの開閉の簡潔に行えるようにする。
-->

**考察**  

<!-- 
  - コンテキストマネジャーの仕組みを理解し、またwith文を使うことでファイルを自動的に閉じる役割しているためwith文の
  基本的な役割を理解するが重要になる。
  - 
-->

**改善点**  
<!-- 
   - _exit_内や_enter_での例外発生する場合の改善方法を考えた方がよい。
-->

----
----

## **課題4. 再帰,lambda式**

### **問題 4-1: クイックソートの再帰的実装**
**作成したpythonコード**

```python

def quicksort(arr: list[int]) -> list[int]:
    if len(arr) <= 1:
        return arr
    pivot = arr[0]
    less = [x for x in arr[1:] if x <= pivot]  # ピボット以下の要素
    greater = [x for x in arr[1:] if x > pivot]  # ピボットより大きい要素
    return quicksort(less) + [pivot] + quicksort(greater)

# 動作確認コード
import random
randoms = [random.randint(-20, 80) for _ in range(15)]
print(randoms)
# [44, 52, 70, -19, 8, -15, 37, 60, 52, 67, 47, 46, -19, 1, -9]
qs = quicksort(randoms)
print(qs)
# [-19, -19, -15, -9, 1, 8, 37, 44, 46, 47, 52, 52, 60, 67, 70]


```

**動作確認結果**
```

[76, -9, -11, 56, 22, 4, -1, 18, -16, 4, 18, 13, 1, -10, 9]
[-16, -11, -10, -9, -1, 1, 4, 4, 9, 13, 18, 18, 22, 56, 76]

```

**分析** 

<!-- 
クイックソートの基本的知識とPythonで再帰的に実行方法を理解する。
-->

**考察**  

<!-- 
  - このクイックソートでは、リストの最初の要素をピボットとして選択し、ピボット以下の要素とそれより大きい要素に分けて
  再帰的にしている。
  - 
-->

**改善点**  
<!-- 
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

----

### **問題 4-2: ハノイの塔の動作原理**
**作成したpythonコード**

```python

def hanoi(n: int, source: str, target: str, auxiliary: str) -> None:
    if n == 1:  
        print(f"ディスク {n} を {source} から {target}へ移動")
        return
    # n-1枚のディスクをsourceからauxiliaryへ移動
    hanoi(n - 1, source, auxiliary, target)
    # 最大のディスクをsourceからtargetへ移動
    print(f"ディスク {n} を {source} から {target}へ移動")
    # n-1枚のディスクをauxiliaryからtargetへ移動
    hanoi(n - 1, auxiliary, target, source)

# 動作検証：3枚のディスクをAからCへ移動
hanoi(3, 'A', 'C', 'B')


```

**動作確認結果**
```
ディスク 1 を A から Cへ移動
ディスク 2 を A から Bへ移動
ディスク 1 を C から Bへ移動
ディスク 3 を A から Cへ移動
ディスク 1 を B から Aへ移動
ディスク 2 を B から Cへ移動
ディスク 1 を A から Cへ移動


```

**分析** 

<!-- 
ハノイの塔の再帰的解法を理解し、再帰の基本的な使い方理解すること。
-->

**考察**  

<!-- 
  - 再帰関数hanoiを使って、全体の問題を分解しています。# n-1枚のディスクをsourceからauxiliaryへ移動
  # 最大のディスクをsourceからtargetへ移動、再び # n-1枚のディスクをauxiliaryからtargetへ移動に分け、再帰的に解決している。
  
  - 
-->

**改善点**  
<!-- 
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->


----

### **★チャレンジ問題 (4-2-aux)　lambdaを使った関数プログラミング**
**多数の英単語をカウント、ソートするlambda関数プログラミング**

**作成したpythonコード**

```python

ここにコードを書く

```

**動作確認結果**
```

ここに実行結果を書く

```

**分析** 

<!-- 
出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

**考察**  

<!-- 
  - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
  - 用いた技法、例えば、関数プログラミングの利点、プロトコルの活用場面について、応用シーンを考察する
-->

**改善点**  
<!-- 
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->


----

## 所感、自由記述欄



