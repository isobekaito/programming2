##  プログラミングII　2024　課題レポート（第3回） 

### レポート提出者：

|クラス|学籍番号|　氏名　|
| B| 20124006___ |  磯部開人________ |

### 選択する回答コース:  「〇〇」コース

- **骨太コース：SA級**：生成AIを使わず，ノーヒントで自力で解く骨太な勇者・猛者・トリックスターコース
- **庶民コース：AB級**：生成AIを使わず，ヒントにしたがってアプローチする一般的コース
- **ゆるふわコース：CD級**：生成AIや他力本願を前提とする ゆとりコース


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

### **問題1: ジェネレータを使ったコルーチンの設計と実装**
- **概要**: Pythonジェネレータを使って生産者-消費者問題を解決するコードを設計せよ
- **課題内容**:
  1. 要件：生産者コルーチンがデータを生成し、消費者コルーチンがデータを消費するコード
  2. `send`と`receive = yield`を用いた協調的並列処理の実装
  3. **分析**: なぜこの設計が必要か？実行結果の確認と検証
  4. **考察**: コルーチンを用いることで得られる効率性や設計上の利点を論じる
  5. **応用（チャレンジ）** 　ジェネレータに対する`StopIteration`例外を使うことで、生産者が生産を終了した後、消費者も消費を停止するコードに進化せよ

---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 生産者(producer)
   ・役割　消費者に対してデータを逐次生成して送信する。
   ・ロジック　消費者コルーチンを起動
   ループ内で3つのアイテムを生産し、消費者に渡す。
   生産を終了したら、消費者コルーチンを明示的に閉じる

   消費者(consumer)
   ・役割　生産者から送られてきたデータを受信し、消費する。
   ・ロジック　yieldによって外部からデータを受信。
   データを処理中に一定の待ち時間を設ける。
   生産者が終了を宣言するとGeneratorExitを補足し、消費終了を出力。

   コルーチン
   send 生産者がアイテムを消費者に受信
   yield 消費者がデータを受け取るポイントを定義
   close 生産者が消費者を明確に終了。

   例外処理
   GeneratorExit処理
   生産者がclose()を呼び出した際、コルーチンはGeneratorExit例外を発生させる。


   - 
-->

```python
import time
import random

# 生産者Coroutine: 無からアイテムを生産する専門家
def producer(consumer_coroutine):
    print("[マリオ]: 生産準備OK...")
    consumer_coroutine.send(None)  
    for i in range(3):  # アイテム（データ）を3個生産
        duration = random.uniform(0.5, 1.5)
        time.sleep(duration)  # 生産に要した時間
        item = f"アイテム {i}"
        print(f"\n---[マリオ]: 所要時間{duration:.2f}秒で, {item} を生産")
        consumer_coroutine.send(item)  
    consumer_coroutine.close()  # 消費者コルーチンを終了
    print("\n---[マリオ]: 生産終了")

# 消費者Coroutine: アイテムを受け取り、別の製品を生み出す専門家
def consumer():
    print("<ルイージ>: 準備OK...")
    try:
        while True:
            item = yield  
            print(f"<ルイージ>: {item} 受領... 消費中.........")
            duration = random.uniform(0.5, 1.5)
            time.sleep(duration)  # 消費に要した時間
            print(f"<ルイージ>: 所要時間{duration:.2f}秒で, {item} 消費完了")
    except GeneratorExit:
        print("\n<ルイージ>: 消費完了")  # この出力を最後に出すために、StopIteration例外を処理

if __name__ == "__main__":
    consumer_coroutine = consumer()  
    producer(consumer_coroutine)  


```


# 消費者


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

[マリオ]: 生産準備OK...
<ルイージ>: 準備OK...

---[マリオ]: 所要時間0.60秒で, アイテム 0 を生産
<ルイージ>: アイテム 0 受領... 消費中.........
<ルイージ>: 所要時間1.35秒で, アイテム 0 消費完了

---[マリオ]: 所要時間1.27秒で, アイテム 1 を生産
<ルイージ>: アイテム 1 受領... 消費中.........
<ルイージ>: 所要時間1.00秒で, アイテム 1 消費完了

---[マリオ]: 所要時間1.11秒で, アイテム 2 を生産
<ルイージ>: アイテム 2 受領... 消費中.........
<ルイージ>: 所要時間1.08秒で, アイテム 2 消費完了

<ルイージ>: 消費完了

---[マリオ]: 生産終了



```

  
**分析**

<!--
   - pythonのコルーチンを利用し、データの生成・消費モデルを構築する技術を理解し、sendやyieldを用いてデータをやり取りしながら制御フローを操作する方法を習得する。
-->

-
-
-
  
**考察**  
<!--
   - データ生成とコンシューマーの役割を分け、sendによるデータ送信やyieldでの受け取りのタイミングを制御し、データ処理の流れを円滑にする。
   - 
-->

-
-
-

**改善点**
<!--
   - sendで初期化が必要な点を忘れる可能性があるため、デコレータを導入して自動化する。
-->

-
-
-

---
---

### **問題2: 基本的なオブジェクト指向の例題**
- **概要**: 図形クラスとその派生クラスを設計し、オブジェクト指向の原則の理解度を確認する
- **課題内容**:
  1. 抽象基底クラス`Shape`を定義し、以下のクラスを継承して設計実装せよ
     - 四角形クラス`Rectangle`
     - 三角形クラス`Triangle`
     - 楕円クラス`Ellipse`
     - 五角形クラス`Pentagon`
  2. 各クラスに`rotate`メソッドを実装し、「指定角度（degree）だけ時計回りに回転させる」機能を追加しなさい（シミュレーションでよい、厳密な行列・ベクトルを用いた幾何学的回転の実装は不要）
  3. ポリモーフィズムの概念を用いた操作を記述せよ
  4. **考察**: 継承、抽象基底クラスによるインターフェースの強制、ポリモーフィズムのメリットと、設計上の効果を分析しなさい



---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - Shapeクラス
   役割　図形を表現する抽象基底クラス
   Shape2Dクラス
   役割　2次元図形用の基本クラス。rotateメソッドを実装し、共通回転処理を提供。
   ロジック
   図形の現在の角度(angle)を指定された回転量で更新
   角度は360度単位で正規化するため、angle ％= 360を使用。
   - 
-->

```python

from abc import ABC, abstractmethod

# rotate()メソッドを義務づける抽象基底クラス Shape
class Shape(ABC):
    @abstractmethod  
    def rotate(self, angle):
        pass

# Shapeを継承した2次元図形クラス Shape2D
class Shape2D(Shape):
    def rotate(self, angle):  
        print(f'{self.__class__.__name__} @ angle({self.angle}) rotated by ({angle}) degree ->', end='')
        self.angle += angle
        self.angle %= 360
        print(f'to angle({self.angle})')

# 長方形クラス Rectangle
class Rectangle(Shape2D):
    def __init__(self, width, height, angle=0):
        self.width = width
        self.height = height
        self.angle = angle

    def rotate(self, angle):
        super().rotate(angle)  
        print(f'calc rotate (width, height) = ({self.width}, {self.height}) @ {self.angle}')

# 三角形クラス Triangle
class Triangle(Shape2D):
    def __init__(self, base, height, angle=0):
        self.base = base
        self.height = height
        self.angle = angle

    def rotate(self, angle):
        super().rotate(angle)  
        print(f'calc rotate (base, height) = ({self.base}, {self.height}) @ {self.angle}')

# 楕円クラス Ellipse
class Ellipse(Shape2D):
    def __init__(self, major_axis, minor_axis, angle=0):
        self.major = major_axis
        self.minor = minor_axis
        self.angle = angle

    def rotate(self, angle):
        super().rotate(angle)  
        print(f'calc rotate (major, minor) = ({self.major}, {self.minor}) @ {self.angle}')

# 五角形クラス Pentagon
class Pentagon(Shape2D):
    def __init__(self, side_length, angle=0):
        self.side = side_length
        self.angle = angle

    def rotate(self, angle):
        super().rotate(angle) 
        print(f'calc rotate (side) = ({self.side}) @ {self.angle}')

# 多様な図形インスタンスのリスト
shapes = [
    Rectangle(10, 5, angle=270),
    Triangle(6, 8, angle=90),
    Ellipse(5, 3, angle=330),
    Pentagon(7, angle=-180)
]


for shape in shapes:
    shape.rotate(45)  # 45度回転


```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

Rectangle @ angle(270) rotated by (45) degree -> to angle(315)
calc rotate (width, height) = (10, 5) @ 315

Triangle @ angle(90) rotated by (45) degree -> to angle(135)
calc rotate (base, height) = (6, 8) @ 135

Ellipse @ angle(330) rotated by (45) degree -> to angle(15)
calc rotate (major, minor) = (5, 3) @ 15

Pentagon @ angle(-180) rotated by (45) degree -> to angle(225)
calc rotate (side) = (7) @ 225


```

  
**分析**

<!--
   - 抽象基底クラスを利用して、派生クラスにrotateメソッドの実相を義務付ける仕組みを理解する。
-->

-
-
-
  
**考察**  
<!--
   - 抽象クラスを通じて、「回転可能な図形」という共通の性質を持つオブジェクトの設計を実現する。
   これにより、新しい図形クラスを簡単に追加できる。
   - 
-->

-
-
-

**改善点**
<!--
   - 各クラスのrotateメソッドが正しく動作しているかを確認するためのテストを実装し、コードの信頼性を向上させる。
-->

-
-
-

---
---

### **問題3: **コマンド(Command)** デザインパターンの実装**
- **概要**: テキストエディタに対する編集操作と`Undo`/`Redo`機能をコマンドデザインパターンで実装せよ
- **課題内容**:
  1. 基本的なコマンドクラス`Command`を設計
  2. 具体的なコマンドとして、`InsertText`、`DeleteText`を実装しなさい（シミュレーションでよい）
  3. `Undo`/`Redo`機能を有効にする履歴管理を追加しなさい
  4. **考察**: コマンドパターンがもたらす柔軟性と拡張性について分析

---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

from abc import ABC, abstractmethod

# コマンドクラスがもつ共通インターフェース
class Command(ABC):
    @abstractmethod
    def execute(self):  # 実行
        raise NotImplementedError

    @abstractmethod
    def undo(self):  # キャンセル=実行を打ち消す、逆操作
        raise NotImplementedError


# テキストを挿入するコマンド
class InsertText(Command):
    def __init__(self, document, text, position):
        self.document = document  # 編集対象のドキュメントオブジェクト
        self.text = text  # 追加するテキスト
        self.position = position  # 追加する位置

    def execute(self):  # 実行（挿入）
        self.document.insert(self.text, self.position)

    def undo(self):  # キャンセル=実行を打ち消す、逆操作（削除）
        self.document.delete(self.position, len(self.text))  

    def __repr__(self):
        return f"(挿入:'{self.text}'@{self.position}) "


# テキストを削除するコマンド
class DeleteText(Command):
    def __init__(self, document, position, length):
        self.document = document  # 編集対象のドキュメントオブジェクト
        self.position = position  # 削除する位置
        self.length = length  # 削除する文字数
        self.deleted_text = ""  # Redo用に、削除されたテキストを保存

    def execute(self):  # 実行（削除）
        self.deleted_text = self.document.content[self.position:self.position + self.length]
        self.document.delete(self.position, self.length)

    def undo(self):  # キャンセル=実行を打ち消す、逆操作（挿入）
        self.document.insert(self.deleted_text, self.position)  

    def __repr__(self):
        return f"(削除:'{self.deleted_text}'@{self.position}から{self.length}文字)"


# テキストを管理するドキュメントクラス
class Document:
    def __init__(self):
        self.content = ""  # ドキュメントの本文

    def insert(self, text, position):  # 文書へ、テキスト挿入
        self.content = self.content[:position] + text + self.content[position:]
        print(f"Inserted '{text}' at {position}.\nCurrent content: '{self.content}'")

    def delete(self, position, length):  # 文書から、テキスト削除
        self.content = self.content[:position] + self.content[position + length:]
        print(f"Deleted text at {position} with length {length}.\nCurrent content: '{self.content}'")


# コマンドを管理し、Undo/Redo機能を提供するクラス
class CommandManager:
    def __init__(self):
        self.undo_stack = []  # Undo履歴 スタックにコマンドオブジェクトをそのまま積み上げ
        self.redo_stack = []  # Redo履歴 スタックにコマンドオブジェクトをそのまま積み上げ

    def execute_command(self, command):  # 編集コマンドの実行
        command.execute()  
        self.undo_stack.append(command)
        self.redo_stack.clear()  # 新しい操作を行ったらRedo履歴をクリア

    def undo(self):  # 編集コマンドの取消し
        if self.undo_stack:
            command = self.undo_stack.pop()
            command.undo() 
            self.redo_stack.append(command)
        else:
            print("Nothing to undo.")

    def redo(self):  # 編集コマンドの再実行＝取消しを取り消す
        if self.redo_stack:
            command = self.redo_stack.pop()
            command.execute()  
            self.undo_stack.append(command)
        else:
            print("Nothing to redo.")


# 活用ケース
if __name__ == "__main__":
    document = Document()
    edit_actions = [  # 挿入、挿入、挿入、削除  編集コマンドのインスタンス
        InsertText(document, "Peace", 0),
        InsertText(document, " and Love ", 5),
        InsertText(document, " for the World", 14),
        DeleteText(document, 6, 9),
    ]
    print(edit_actions)
    manager = CommandManager()
    for ed in edit_actions:
        manager.execute_command(ed)  

    print("\nUndo操作を2回実施:")
    # Undo, Undo
    manager.undo()
    manager.undo()

    print("\nRedo操作を2回実施:")
    # Redo, Redo
    manager.redo()
    manager.redo()

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

[(挿入:'Peace'@0) , (挿入:' and Love '@5) , (挿入:' for the World'@14) , (削除:''@6から9文字)]
Inserted 'Peace' at 0.
Current content: 'Peace'
Inserted ' and Love ' at 5.
Current content: 'Peace and Love '
Inserted ' for the World' at 14.
Current content: 'Peace and Love for the World '
Deleted text at 6 with length 9.
Current content: 'Peace for the World '

Undo操作を2回実施:
Inserted 'and Love ' at 6.
Current content: 'Peace and Love for the World '
Deleted text at 14 with length 14.
Current content: 'Peace and Love '

Redo操作を2回実施:
Inserted ' for the World' at 14.
Current content: 'Peace and Love for the World '
Deleted text at 6 with length 9.
Current content: 'Peace for the World '


```

  
**分析**

<!--
   - コマンドパターンの理解、取り消し(Undo)、再実行(Redo)の機能を理解する。
-->

-
-
-
  
**考察**  
<!--
   - 各クラスの役割を明確に把握し、機能を実装。InsertTextとDeleteTextの逆操作の仕組みを確認。
   - 
-->

-
-
-

**改善点**
<!--
   - 無効な操作を防ぐためのエラーチェック機能の追加。
-->

-
-
-

-----
-----

### **問題4: オブザーバ(Observer) デザインパターンの実装**
- **概要**: Pythonのダックタイピングを活用して**オブザーバパターン**を実装し、MVCモデルについて論じよ
- **課題内容**:
  1. `Observer`と`Subject`を設計し、観察者-被観察者関係を構築
  2. サンプルアプリとして簡易的なMVCモデルを実装
  3. 各コンポーネントの役割を論述
  4. **考察**: ダックタイピングの利点と、オブザーバパターンによる柔軟なシステム設計についてパターンを使わない場合と比較し，効能・使い道，論じる

---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - Model データの保持を管理を担当する被観察者。
　　　observer ビューの基底クラスで、update()メソッドを定義。
　　　subject データの変化を各フォーマットで出力する具象クラス。
　　　Controller モデルのデータ変更を管理し、観察者へ通知する役割。
   - 
-->

```class Model:  # 被観察者（データの保持と管理）
    def __init__(self, name):
        self._name = name
        self._data = None

    def set_data(self, data):
        self._data = data

    def get_data(self):
        return self._data

    def get_name(self):
        return self._name

    def serialize(self):  # Modelのデータを直列化するメソッド
        return {"data": self._data}

    def deserialize(self, data):  # 直列化されたデータからModelを復元するメソッド
        self._data = data["data"]


class Observer:  # 観察者（ビューのインターフェース）
    def update(self, model):  
        pass


class HTMLView(Observer):  # HTML形式のビュー
    def update(self, model):
        print(f"<div> HTMLView: {model.get_name()} updated to {model.get_data()} </div>")


class CSVView(Observer):  # CSV形式のビュー
    def update(self, model):
        print(f"CSVView,{model.get_name()},updated,to,{model.get_data()},")


class PDFView(Observer):  # PDF形式のビュー
    def update(self, model):
        print(f"@PDFView: {model.get_name()} {model.get_data()} updated")


class GUIView(Observer):  # GUI形式のビュー
    def update(self, model):
        print(f"□●GUIView: TEXT_FIELD:{model.get_name()} updated to {model.get_data()} CheckBox ✓")


class Subject:  # 観察者と被観察者を仲介するクラス
    def __init__(self):
        self._observers = []  # 観察者リスト

    def register_observer(self, observer):  # 観察者を登録する
        self._observers.append(observer)  

    def unregister_observer(self, observer):  # 観察者を解除する
        self._observers.remove(observer)  

    def notify_observers(self, model):  # すべての観察者に変化を通知する
        for observer in self._observers:
            observer.update(model)  


class Controller(Subject):  # イベント契機に通知を発生させるクラス
    def __init__(self, model):
        super().__init__()
        self.model = model

    def set_data(self, data):  # モデルデータを更新し、観察者に通知する
        self.model.set_data(data)
        self.notify_observers(self.model)  


# オブザーバパターンによるMVCプログラム
if __name__ == "__main__":
    # モデルの作成
    model1 = Model("")ルフィ
    model2 = Model("ゾロ")

    # 各種ビューの作成
    html_view = HTMLView()
    csv_view = CSVView()
    pdf_view = PDFView()
    gui_view = GUIView()

    # コントローラの作成
    controller1 = Controller(model1)
    controller2 = Controller(model2)

    # コントローラにビューを登録
    controller1.register_observer(html_view)
    controller1.register_observer(csv_view)
    controller2.register_observer(pdf_view)
    controller2.register_observer(gui_view)

    # モデルデータの変更と通知
    controller1.set_data("イベント通知:Hi, HTML and CSV表現")
    controller2.set_data("イベント通知: Hello, PDF and GUI表現")



```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```
<div> HTMLView: ルフィ updated to イベント通知:Hi, HTML and CSV表現 </div>
CSVView,ルフィ,updated,to,イベント通知:Hi, HTML and CSV表現,
@PDFView: ゾロ イベント通知: Hello, PDF and GUI表現 updated
□●GUIView: TEXT_FIELD:ゾロ updated to イベント通知: Hello, PDF and GUI表現 CheckBox ✓



```

  
**分析**

<!--
   - オブザーバパターンの理解とMVCアーキテクチャの活用を理解する。
-->

-
-
-
  
**考察**  
<!--
   - ダックタイピングを活用したオブザーバパターンにより、拡張性の高い設計を実現できる。
   - 
-->

-
-
-

**改善点**
<!--
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

-
-
-

----
----

### **チャレンジ問題5: 非同期処理の実装と分析**　（骨太コース必須）
- **概要**: `async`と`await`を使用した非同期処理の設計と実装を通じて、非同期プログラミングを理解する
- **課題内容**:
  1. `async`と`await`を使用して複数の非同期タスクを並列実行するコード（例：Webスクレイピング、ファイル入出力）のサンプル例を実行し、動作原理を理解する
  2. 実行時間の比較実験を行い、同期処理との差を示す
  3. 非同期処理の利点、欠点、適用例を分析し、同期処理との違いを考察して，自分の言葉で説明せよ

----
---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

ここにコードを書く

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

ここに実行結果を書く

```

  
**分析**

<!--
   - 出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

-
-
-
  
**考察**  
<!--
   - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
   - 用いた技法、例えば、関数プログラミング、プロトコル、オブジェクト指向等の技法の効果、活用場面について、応用シーンを考察する
-->

-
-
-

**改善点**
<!--
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

-
-
-

-----

### 所感・今後の目標課題
 (自由記述欄、多様なご意見を尊重します)

- プログラミングに興味が湧かない原因
- プログラミングには興味がないが、それ以外で没頭していること
- 授業中に質問や意見が言えない理由
- いいねツールに期待すること
- コードや英語や文字、活字が好きにならない理由
- 私とプログラミング
- なぜプログラミングを学ぶのだろうか？
- これまで学んだことを振り返り
- これからの学びに活かすべき知見
 
---




---

