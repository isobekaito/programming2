##  プログラミングII　2024　課題レポート（第4回） 

### レポート提出者：

|クラス|学籍番号|　氏名　|
|-|-|-|
| B| 20124006___ |  _磯部開人_______ |

### 選択する回答コース:  「〇〇〇〇」コース

**コース選択：アプローチの違い（道筋の険しさ）によって難易度・努力値を考慮する**
- **骨太コース：SA級**：生成AIを使わず，ノーヒントで自力で解く骨太な勇者・猛者・トリックスターコース
- **庶民コース：AB級**：生成AIを使わず，ヒントにしたがってアプローチする一般的コース
- **ゆるふわコース：CD級**：生成AIや他力本願を前提とするゆったりコース


----

# 回答用紙 → 提出ファイルはこの様式を使うこと
- 問題用紙をよく読み、この回答用紙にレポート記述して提出すること
- マークダウン記法を、しっかりプレビューして、記述ミスはすべて修正して完成させること
レポート提出者：

# 学生による回答編
- それぞれの問題をよく読み、以下の項目に自分の言葉でしっかり記述する
- マークダウン記法を、しっかりプレビューして、記述ミスはすべて修正して完成させる
- 生成AIに頼らず、自分で考えて、自分の言葉で書くことに重要な意味がある

------

### **問題1: シングルトンと属性制約を保証するメタクラスの設計**
- **概要**: メタクラスを使い、下記要件を満たす「AppConfigクラス」を**空欄を埋めて**設計せよ:
- **要件**
  - シングルトンパターン: クラスのインスタンスは1つしか生成できない
  - 属性制約: クラス内の属性名に制約:「属性名はすべて小文字」を課す
  - デフォルト値: 属性が指定されていない場合にデフォルト値を自動設定
- **課題**:
  1. **コード設計**:  空欄を埋めて、コードを完成させよ
  2. **分析**: なぜこの設計が有用か？実行結果の確認と検証
  3. **考察**: メタクラスを用いることで得られる利点を論じる

---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

class SingletonMeta(type):
    """Singleton を強制するメタクラス"""
    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]


class AttributeConstraintMeta(type):
    """属性名がすべて小文字で命名されることを保証するメタクラス"""
    def __new__(cls, name, bases, dct):
        for attr_name in dct:
            if not attr_name.islower() and not attr_name.startswith("__"):
                raise TypeError(f"Attribute '{attr_name}' in class '{name}' must be in lowercase.")
        return super().__new__(cls, name, bases, dct)


class DefaultValuesMeta(type):
    """属性のデフォルト値を自動設定するメタクラス"""
    DEFAULT_VALUES = {
        'database': 'sqlite'
    }

    def __call__(cls, *args, **kwargs):
        instance = super().__call__(*args, **kwargs)
        for attr, default in cls.DEFAULT_VALUES.items():
            if not hasattr(instance, attr) or getattr(instance, attr) is None:
                setattr(instance, attr, default)
        return instance


class CombinedMetaClass(SingletonMeta, AttributeConstraintMeta, DefaultValuesMeta):
    """Singleton, Attribute Constraints, and Default Valuesを合成したメタクラス"""
    pass


class AppConfig(metaclass=CombinedMetaClass):
    app_name = "MyApplication"  
    version = "1.0"             
    port = 8080                 
    database = None             


# インスタンス生成
configA = AppConfig()
configB = AppConfig()

# 同一インスタンス(シングルトン)を確認
assert configA is configB
print(configA, configB)

# 属性デフォルト値の設定を確認
print(configA.database)  # デフォルト値として "sqlite" を出力


```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

<__main__.AppConfig object at 0x7c124afbd840> <__main__.AppConfig object at 0x7c124afbd840>
sqlite

エラー: Attribute 'Port' in class 'InvalidConfig' must be in lowercase.


```

  
**分析**

<!--
   - メタクラスを理解し、複数の異なる機能（シングルトンパターン、属性名の制約）を合成して使う方法を修得する問題。
   ・シングルトンパターン: インスタンスが一つしか生成されない設計。
   ・属性名の制約: クラス定義時に属性名が小文字であることを保証する。
-->

-
-
-
  
**考察**  
<!--
   - 複数の異なる機能を一つのクラスに適用するために、メタクラスの多重継承を活用する。CombinedMetaClass を作成し、必要なメタクラス（SingletonMeta、AttributeConstraintMeta、DefaultValuesMeta）を合成して一つのメタクラスにまとめた。
   -
-->

-
-
-

**改善点**
<!--
   - 現状の AttributeConstraintMeta は、属性名が小文字でなければ即エラーになる。将来的には、警告のみを出すオプションや許容する例外ケースを設けることで、柔軟性が高まる。
-->

-
-
-

---
---

### **問題2: 「チューリングマシンの停止問題」「万能デバッガが存在しない理由」**
- **課題**:  
- メタ循環インタプリタに関係する「チューリングマシンの停止問題」について、
- 講義中に説明した比喩「どんなバグもみつける完璧なデバッガが存在しない」を交えて
- 理解したことを自分の言葉で説明せよ
- **考察**: 
- あわせて「ゲーデルの不完全性定理」をふまえてどんな教訓を示しているか分析・考察せよ
 
---

**説明・分析**

<!--
   - チューリングマシンの停止問題は、「あるチューリングマシン（計算モデル）とその入力が与えられたとき、そのマシンが最終的に停止するかどうかを判断する一般的なアルゴリズムは存在しない」という問題です。
   停止問題をソフトウェアデバッグの視点から考えると、次の比喩が成り立ちます：
「どんなバグも見つけ出す完璧なデバッガは存在しない」。
理由は、デバッガがソフトウェアのすべての動作や状態を事前に解析して、無限に動作するかどうか（停止するかどうか）を完璧に判断することが不可能だからです。
-->

-
-
-
  
**考察**  
<!--
   - チューリングの停止問題とゲーデルの不完全性定理は、次の重要な教訓を示しています：
・システムやアルゴリズムには必ず「限界」が存在する。
・「すべての問題を解決する万能な手段」や「完全に正しいシステム」を構築することは不可能である。
・自己言及性やシステムの複雑さが増すほど、その限界は明確に現れる。
   - 
-->

-
-
-

**改善点**
<!--
   - システム設計では、可能な限り自己言及的な構造を避け、矛盾や限界が現れないよう工夫することが考えられます。
-->

-
-
-

---
---

### **問題3: オブザーバパターンを保証するメタクラスの設計**
- **概要**: メタクラスを使い、特定のクラスが「モデル（Observable）」として動作し、他の「オブザーバ（Observer）」に変更を通知する仕組みを確実に実施するサンプルコード設計を**空欄を埋めて**完成させよ
  
- **要件**
1. **`ObserverMeta` メタクラス**  
   - Observable クラスが必ず、 `add_observer`、`remove_observer`、`notify_observers` メソッドを持つことを強制し、実装されていない場合、例外をスローする

2. **`Observable` クラス**  
   - `ObserverMeta`メタクラスを適用した**基底クラス**で、観察される対象を管理する仕組みを提供、オブザーバを登録・解除し、状態変化時に通知する

3. **`Observer` クラス**  
   - `update` メソッドを持つ**抽象基底クラス**で、具体的なオブザーバクラスが継承して実装

4. **具体例（`TemperatureModel` と `TemperatureDisplay`）**  
   - `TemperatureModel` は(温度という)状態を持つモデルで、`temperature` プロパティが変更されると自動的にオブザーバへ通知
   - `TemperatureDisplay` はモデルから通知を受け取り、画面に表示を更新するオブザーバの具体例
   - 観察対象モデル（`TemperatureModel`）の状態変更がオブザーバ(`TemperatureDisplay`)に確実に通知される仕組みが可能
 
- **課題**:
  1. **コード設計**:  空欄を埋めて、コードを完成させよ
  2. **分析**: なぜこの設計が有用か？実行結果の確認と検証
  3. **考察**: メタクラスを用いることで得られる利点を論じる

---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

# **ObserverMeta メタクラス**
class ObserverMeta(type):

    def __new__(cls, name, bases, dct):
        required_methods = {"add_observer", "remove_observer", "notify_observers"}
        # 親クラスのメソッドも含めて確認
        implemented_methods = set(dct.keys()).union(*(set(dir(base)) for base in bases))
        if not required_methods.issubset(implemented_methods):
            missing = required_methods - implemented_methods
            raise TypeError(f"Class {name} is missing required methods: {', '.join(missing)}")
        return super().__new__(cls, name, bases, dct)

# **Observable クラス**
class Observable(metaclass=ObserverMeta):

    def __init__(self):
        self._observers = []

    def add_observer(self, observer):
        if observer not in self._observers:
            self._observers.append(observer)

    def remove_observer(self, observer):
        if observer in self._observers:
            self._observers.remove(observer)

    def notify_observers(self):
        for observer in self._observers:
            observer.update(self)

# **Observer クラス**
class Observer:

    def update(self, observable):
        raise NotImplementedError("The 'update' method must be implemented by subclasses.")

# **観察者(View)の具体例**
class TemperatureDisplay(Observer):
    def update(self, observable):
        if isinstance(observable, TemperatureModel):
            print(f"[Display] Model updated to {observable.temperature}°C [Display]")

class TemperatureBrowser(Observer):
    def update(self, observable):
        if isinstance(observable, TemperatureModel):
            print(f"<Web> Model updated to {observable.temperature}°C</Web>")

# **観察対象(Model)の具体例**
class TemperatureModel(Observable):
    def __init__(self):
        super().__init__()
        self._temperature = 0

    @property
    def temperature(self):
        return self._temperature

    @temperature.setter
    def temperature(self, value):
        self._temperature = value
        self.notify_observers()

# **テスト関数**
def test():
    # 観察対象 Model
    model = TemperatureModel()
    # 観察者 View
    display = TemperatureDisplay()
    view = TemperatureBrowser()

    # オブザーバの割り当て
    model.add_observer(display)
    model.add_observer(view)

    # モデルのアップデート
    model.temperature = 25
    model.temperature = 30

    # オブザーバの解除
    model.remove_observer(display)
    model.temperature = 35

    # オブザーバの再割り当て
    model.add_observer(display)
    model.temperature = 44

# **テストの実行**
test()


```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

[Display] Model updated to 25°C [Display]
<Web> Model updated to 25°C</Web>
[Display] Model updated to 30°C [Display]
<Web> Model updated to 30°C</Web>
<Web> Model updated to 35°C</Web>
<Web> Model updated to 44°C</Web>
[Display] Model updated to 44°C [Display]


```

  
**分析**

<!--
   - Observerパターンを理解し、観察者（Observer）と観察対象（Observable）の関係性をプログラムで実装できることを目的としている。
    観察対象（Model）から通知を受け取る観察者（View）の仕組み。
-->

-
-
-
  
**考察**  
<!--
   - Observerパターンは、観察対象（Model）が観察者（View）を直接知らずに通知できる仕組みを提供する。これにより、ModelとView間の依存関係を減らし、変更に強い設計が可能になる。


-->

-
-
-

**改善点**
<!--
   - remove_observer メソッドで、指定されたobserverが登録されていない場合のエラーメッセージやログ出力を追加すると、デバッグが容易になる。
-->

-
-
-

-----
-----


### **問題4: メタクラスではなく、`__init_subclass__()`で代用する方法**
- **課題**: 問題３をシンプルに、`__init_subclass__()`で代用するコードを**空欄を埋めて**完成させよ



---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

class Observable:
    def __init__(self):
        self._observers = []  

    def add_observer(self, observer):
        if observer not in self._observers:
            self._observers.append(observer)

    def remove_observer(self, observer):
        if observer in self._observers:
            self._observers.remove(observer)

    def notify_observers(self):
        for observer in self._observers:
            observer.update(self)  

    @classmethod
    def __init_subclass__(cls, **kwargs):  
        super().__init_subclass__(**kwargs)
        required_methods = {"add_observer", "remove_observer", "notify_observers"}  
        for method in required_methods:
            if not callable(getattr(cls, method, None)):
                raise TypeError(f"Class {cls.__name__} is missing required method: {method}")


# Sample Observer Class
class Observer:
    def update(self, observable):
        raise NotImplementedError("The 'update' method must be implemented by subclasses.")


# Example Implementation
class TemperatureModel(Observable):  
    def __init__(self):
        super().__init__()
        self._temperature = 0

    @property
    def temperature(self):
        return self._temperature

    @temperature.setter
    def temperature(self, value):
        self._temperature = value
        self.notify_observers()


class TemperatureDisplay(Observer):  
    def update(self, observable):
        if isinstance(observable, TemperatureModel):
            print(f"Temperature updated to {observable.temperature}°C")


def test():
    temp_model = TemperatureModel()
    display = TemperatureDisplay()

    temp_model.add_observer(display)

    temp_model.temperature = 25
    temp_model.temperature = 30

    temp_model.remove_observer(display)
    temp_model.temperature = 35


test()


```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

Temperature updated to 25°C
Temperature updated to 30°C

```

  
**分析**

<!--
   - __init_subclass__() の仕組みを理解し、それを活用してコードの簡素化やメタクラスの代替手段を実現する能力を養う
   __init_subclass__() を使って、メタクラスを使わずにクラス定義時の検証を行う方法。
-->

-
-
-
  
**考察**  
<!--
   - クラス階層を設計する際、必須のメソッドやプロパティを保証する仕組みが必要。通常はメタクラスを使うが、__init_subclass__() を利用することで、シンプルかつ直接的に実現可能である。
   - 
-->

-
-
-

**改善点**
<!--
   - __init_subclass__() の検証で発生するエラーに、具体的な不足メソッドや対処方法を記載することで、使いやすさが向上する。
-->

-
-
-

----
----

### **問題5: 自作パッケージとデプロイの実践
- **概要**: 自作パッケージを作って、デプロイし、他者が`pip insatll`や、`import`を使って活用できる状態にする
- 第12回の練習課題（プラクティスNo.1，No.2）を参考に、**自作パッケージを作ってデプロイするプラクティスNo.3**を実施する
- 簡単なユーティリティ関数を束ねたパッケージを自作する
- 例えば、次のような簡単な関数や、その他有用な関数を追加する：
- 

    - 1. BMI計算関数: 身長と体重を入力として受け取り、BMI（Body Mass Index）を計算して返す関数 : `calculate_bmi(weight, height)`
    - 2. 温度変換関数: 摂氏温度を華氏温度に変換する関数:`celsius_to_fahrenheit(celsius)`と、その逆変換:`fahrenheit_to_celsius(fahrenheit)`
    - 3. 単語カウント関数: テキスト内の単語数をカウントする関数:`count_words(text)`
    - 4. リストの平均値計算関数:`calculate_average(numbers)`
    - 5. 素数判定関数:`is_prime(number)`
    - 6. フィボナッチ数列生成関数:` generate_fibonacci(n)`
    - 7. 文字列の逆順関数:`reverse_string(s)`
    - 8. 最大公約数（GCD）計算関数:`gcd(a, b)`

----
---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

def calculate_bmi(weight, height):
    return weight / (height ** 2)

def celsius_to_fahrenheit(celsius):
    return (celsius * 9/5) + 32

def fahrenheit_to_celsius(fahrenheit):
    return (fahrenheit - 32) * 5/9

def count_words(text):
    words = text.split()
    return len(words)

def calculate_average(numbers):
    return sum(numbers) / len(numbers)

def is_prime(number):
    if number <= 1:
        return False
    for i in range(2, int(number ** 0.5) + 1):
        if number % i == 0:
            return False
    return True

def generate_fibonacci(n):
    fib_sequence = [0, 1]
    while len(fib_sequence) < n:
        fib_sequence.append(fib_sequence[-1] + fib_sequence[-2])
    return fib_sequence[:n]

def reverse_string(s):
    return s[::-1]

def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

from my_utils import (
    calculate_bmi,
    celsius_to_fahrenheit,
    fahrenheit_to_celsius,
    count_words,
    calculate_average,
    is_prime,
    generate_fibonacci,
    reverse_string,
    gcd
)

rom setuptools import setup, find_packages



# Example usage
print(calculate_bmi(70, 1.75))
print(celsius_to_fahrenheit(30))
print(fahrenheit_to_celsius(86))
print(count_words("Hello world!"))
print(calculate_average([1, 2, 3, 4, 5]))
print(is_prime(29))
print(generate_fibonacci(10))
print(reverse_string("hello"))
print(gcd(48, 18))

from my_utils import utils

# BMIの計算
print(utils.calculate_bmi(70, 1.75))  # 体重70kg、身長1.75mのBMIを計算

# 摂氏→華氏温度の変換
print(utils.celsius_to_fahrenheit(30))  # 摂氏30度を華氏に変換

# 単語カウント
print(utils.count_words("Hello world! This is a test."))  # 単語数をカウント

# 平均値の計算
print(utils.calculate_average([10, 20, 30, 40, 50]))  # 数値リストの平均を計算

# 素数判定
print(utils.is_prime(29))  # 29が素数かどうか判定

# フィボナッチ数列の生成
print(utils.generate_fibonacci(10))  # 最初の10個のフィボナッチ数を生成

# 文字列の逆順
print(utils.reverse_string("hello"))  # 文字列 "hello" を逆順に

# 最大公約数（GCD）の計算
print(utils.gcd(48, 18))  # 48と18の最大公約数を計算



```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

22.857142857142858    # BMIの計算結果
86.0                  # 摂氏30度 → 華氏86度
6                     # "Hello world! This is a test." の単語数
30.0                  # 平均値
True                  # 29は素数
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34]  # フィボナッチ数列
"olleh"               # 文字列 "hello" の逆順
6                     # 48と18の最大公約数


```

  
**分析**

<!--
   - Pythonの自作パッケージ作成や、関数の実装方法を学び、それを他者が利用可能な形で公開するまでの流れを理解し実践すること
-->

-
-
-
  
**考察**  
<!--
   - 小さく、独立性の高い関数を実装することで、モジュール化された構造を作る。これにより、関数の再利用性を高める。
自作関数を公開するにあたり、他者がどのように利用するかを想定してインターフェースをわかりやすく設計する必要がある。
   - 
-->

-
-
-

**改善点**
<!--
   - 関数の引数に対する型チェックやエラーハンドリングを追加する。例えば、BMI計算時にheightがゼロの場合のエラー処理や、温度変換関数での入力値検証を行う。
-->

-
-
-

-----

### 所感・今後の目標課題 (自由記述欄、フィードバックご意見を尊重します)

- プログラミングに興味が湧かない原因
- プログラミングには興味がないが、それ以外で没頭していること
- 授業中に質問や意見が言えない理由
- コードや英語や文字、活字が好きにならない理由
- 私とプログラミング
- なぜプログラミングを学ぶのか？
- これまで学んだことを振り返り
- これからの学びに活かすべき知見
 
---




---

