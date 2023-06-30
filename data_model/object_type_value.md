# オブジェクト、型、値

[The Python Language Reference - Data model - #Objects, values and types](https://docs.python.org/3/reference/datamodel.html#objects-values-and-types)

## オブジェクト

Pythonのソースコードは、オブジェクトとオブジェクト間の関係性を表現したものの集合体と言える。

オブジェクトは、IDによって識別される。  
IDとは、CPythonおいてはオブジェクトが格納されているメモリアドレスのことである。  
各オブジェクトのIDは、`id()`によって確認できる。

オブジェクトはPython実行のたびに再生成され、そのたびにメモリアドレスが書き換わる。  
したがって、以下のコードの実行結果はPython実行のたびに変化する。

```py
print(id(1))
# 140540285862128
print(id(2))
# 140540285862160
```

一度生成されたオブジェクトのIDは、同じPythonのコード内で変わることはない。

```py
print(id(1))
# 139694177730800
print(id(1))
# 139694177730800
```

`is`という演算子は、左辺と右辺のオブジェクトが「同一」であるかを判定する演算子である。  
内部的には、「IDが同じかどうか」を判定している。  
**「同一のオブジェクトはIDが等しい関係にある」** という事実は重要そう。

```py
print(1 is 1)
# True
```

## 型

オブジェクトは型を持つ。

型とはクラスのこと。  
`1`の型は`int`。  
`'hi'`の型は`str`など。

クラスとは、attributeとmethodの定義をまとめたものである。  
つまりオブジェクトがどのクラスに属するかを理解すれば、「そのクラスがどのような値を持ち (attribute)」、「どのようなメソッドを使えるか、何の演算子に対応するか (method)」を理解できる。

型は`type()`関数で確認できる。

```py
print(type(1))
# <class 'int'>

print(type('hi'))
# <class 'str'>
```

ちなみに`type()`が返すオブジェクトは、`type`型である。

```py
print(type(type(1)))
# <class 'type'>
```

## 値

[#(参考) 値って何？](#(参考)-値って何？)に書いたが、Pythonの値が厳密に何なのかはよくわからない。  
しかし、Python公式ドキュメントに「値 (value)」という言葉が出てきてしまう以上、この言葉の存在を無視することは難しい。

一旦、以下のようなふんわりした感じの「値」をイメージする。

* オブジェクトは値を持つ
* 値はオブジェクトではないので、値は配下にプロパティやメソッドを持たない (C言語のリテラルをイメージしてほしい)
* `1`と`1`という2つのint型オブジェクトは、同じ値を持つ
* `1 == 1`は`True`である

値らしきものは、`print()`関数によって確認できる。  
`repr()`関数にオブジェクトを渡せば、型情報をよりわかりやすく表現した文字列を返してくれる (いわゆるstring representation)。  
[print()](https://docs.python.org/3/library/functions.html#print)  
[repr()](https://docs.python.org/3/library/functions.html#repr)

```py
print('1')
# 1

print(repr('1'))
# '1'
```

基本的な型の多くは`print()`で「同じ値を持つリテラルオブジェクト」を作るための表記と同じ文字列を返してくれるため、オブジェクトがどんなリテラルであるかを判断できる。  
しかし型によっては専用の文字列表現を用意していないことがある。  
そういった場合はデフォルトで`<クラス名 object at メモリアドレス>`のように表示されるが、この表記からは値を読み取ることができない。

```py
print(3)
# 3

print(range(3))
# range(0, 3)

print(iter(range(5)))
# <range_iterator object at 0x7f5836766c40>
```

`iter()`のようなオブジェクトは、`print()`によってどのような中身か知ることが難しくなっている。  
`print()`で知ることができるのは「`range`型のイテレータである」という情報のみで、`range()`がどのようなリテラルなのか (どんな範囲のrangeなのか) を知ることはできない。

こういった場合は、`print()`で値のstring representationを表示可能な型に一旦変換するのが有効である。  
Iterator型はiterableなので、`list()`でlist型に変換できる。  
定義を見ればわかるが、`list()`関数はiterableなオブジェクトを渡せばリスト型のオブジェクトを返してくれる。  
[list()](https://docs.python.org/3/library/stdtypes.html#list)

以下の例では、型が`range_iterator`で、値が「`[0, 1, 2, 3, 4]`に近い何か」であることがわかる。

```py
print(list(iter(range(5))))
# [0, 1, 2, 3, 4]

print(type(iter(range(5))))
# <class 'range_iterator'>
```

同様に、keyword argumentsの集合体を内部構造として持つような型であれば`dict()`によってdict型に変換できると考えられる。  
[dict()](https://docs.python.org/3/library/stdtypes.html#dict)

## (参考) string representation

オブジェクトを文字列で表現する関数として、`str()`と`repr()`の2種類が挙げられる (※)。  
どちらもオブジェクトを引数に取って文字列を返す関数だが、表記が「プログラムのユーザーの見やすさ重視」と「開発者向けの厳密な表示か」で若干異なる。  
(※) string representationを「文字列表現」と訳している  
(※) `str()`は、厳密には関数ではなくクラス。インスタンス生成時に`str.__new__()`メソッドによって文字列オブジェクトを生成するので、「文字列に変換する関数」と理解しても大体問題ない。`print(type(str))`を実行すると、`function`ではなく`type`と表記されるという違いがあるが

* `str()`
  * オブジェクトの文字列表現をstr型で返す
  * 厳密さよりもユーザー向けの見やすさを重視して表記する
  * 内部的には、引数に渡したオブジェクトの`__str__()`メソッドを呼び出している
  * [str()](https://docs.python.org/3/library/stdtypes.html#str)
  * [object.\_\_str\_\_()](https://docs.python.org/3/reference/datamodel.html#object.__str__)
* `repr()`
  * オブジェクトの文字列表現をstr型で返す
  * 「フォーマル」とは、string representationの厳密な表記を重視しているという意味
  * 開発やデバッグには`print()`単体よりも`print(repr(...))`こちらのほうが向いている
  * 内部的には、引数に渡したオブジェクトの`__repr__()`メソッドを呼び出している
  * `__str__()`が未定義で`__repr__()`のみが定義されている場合、`__str__()`を呼んだときには変わりに`__repr__()`が使用される
  * [repr()](https://docs.python.org/3/library/functions.html#repr)
  * [\_\_repr\_\_()](https://docs.python.org/3/reference/datamodel.html#object.__repr__)

`print()`関数でオブジェクトを引数に渡した場合は、内部的にはオブジェクトの`__str__()`メソッドを呼んでから標準出力している。  
したがって`print()`を単体で使用すると、いわゆる「見やすい表示」になる。  
[print()](https://docs.python.org/3/library/functions.html#print)

表示の違いが出る最たる例は、str型のオブジェクトである。

`print()`を単体で呼び出すと`3`と表示される。  
カジュアルな用途ではこれで全く問題ないが、開発者目線では表示されたオブジェクトがint型なのかstr型なのかわからない。

`repr()`を呼び出すと、`'3'`というオブジェクトを表す文字列として`"'3'"`が返される。  
つまり、文字列の中身が`3`から`'3'`とシングルクォーテーションも含むようになり、ソースコード中にリテラルを記載するのと同じ表記になる。

```py
print('3')
# 3

print(repr('3'))
# '3'
```

## (参考) 値って何？

値の定義はPython公式ドキュメントに書いていなかった。

非公式情報ではあるものの、Pythonにおいて値を単体で取り出すことができないこともわかっている。  
したがって、Pythonのソースコード上であれこれ実験して「値とはどのようなものか」の理解を深めるにも限界がある。  
[stack overflow - How to determine an object's value in Python](https://stackoverflow.com/questions/57443045/how-to-determine-an-objects-value-in-python)

では`print()`で表示しているのは何かというと、**値に近い意味を持つ文字情報 (string representation)**である。  
`print()`は値を表示しているわけではない。  
`print()`は、内部的にクラスの`__str__`メソッドを呼び出し、得られた文字列を標準出力しているに過ぎないことは[#(参考) string representation](#参考-string-representation)で既に述べた。

このように、自分の中で「値」という言葉に定義を与えることは今のところできない。  
したがって、自分はPythonの文脈であまり「値」という言葉を積極的に使わないようにしようと考えた。

とはいえ、実際にPythonの公式ドキュメントに「値 (Value)」という用語は出てくるので、最低限のイメージは持っておきたい。  
正確ではないものの、大体の人に通じる「値」という概念は恐らくこういったもの...という情報を以下にメモしておく。

### オブジェクトとは

Pythonにおける主な登場人物は以下の通り。

* オブジェクト (リテラル、変数、クラス、関数など。具体的には、`1`, `'hi'`, `x`, `int`など)
* 演算子 (`=`, `==`, `!=`, `>`, `+`, `-`, `*`, `/`など)
* 構文の要素 (`import`, `if`, `for`, `while`など)

値に最も近い概念はオブジェクトだが、そのオブジェクトもPythonの登場人物の一つである。

例えば、Pythonのソースコード上に出てくる`1`というリテラルも、int型 (intクラス) に属するオブジェクトである。  
オブジェクトとはクラスによって定義され、配下にプロパティやメソッドなどの要素を持つものである。

### 値を取り出せない

`1`というオブジェクトの値は何か？

`1`という値だと思うが、Pythonにおいてintクラスのプロパティやメソッドを持たない、C言語のint型の`1`のような値を作ることはできない。  
Pythonのソースコード上で、`1.value`のようにプロパティとして存在するわけでもない。  
仮に`1.value`が何らかの値を返したとしても、それは何かしらのクラスに属するオブジェクトであり、私達の思い描いているような「値」ではない。  
(※) 「私達の思い描いているような値」とは、プロパティやメソッドを持たない存在...ということにしておく

Pythonにおいてオブジェクトから値を取り出せないというのはそういうこと。

もしかしたらCPythonを実装しているC言語においては「値」を評価するような処理があるのかもしれない。  
しかし、そこまで追いかける技術力も時間もないので、諦めることとする。

### 値のイメージ

いくつか具体例を示す。

```py
print(1 is 1)
# True

print(1 == 1)
# True

print('a' is 'a')
# True

print('a' == 'a')
# True

print([1] is [1])
# False

print([1] == [1])
# True

print((1) is (1))
# True

print((1) == (1))
# True
```

`is`は「同一オブジェクトか」を判定する演算子である。  
CPythonにおいては、オブジェクトを格納するメモリアドレスが同一か否かを比較している。

整数型、文字列型、タプル型はImmutable (同一オブジェクトを保ったまま中身を変更不可) である。  
Immutableなオブジェクトは、同じようなリテラルを繰り返し表記しても、同一オブジェクトを繰り返し参照するという動きを取る。  
恐らく、Immutableであれば同じ見た目のオブジェクトが同じ中身を持つので、メモリアドレスを同一にしたほうが効率が良いという考え方なのだろう。

一方で、リスト型はMutable (同一オブジェクトの中身を変更できる。具体的にはリストの中身のオブジェクトを変更可能) である。  
Mutableなオブジェクトは、同じようなリテラルを繰り返し表記すると、都度新しいメモリアドレスを割り当てる。  
2つの`[1]`が異なるメモリアドレスを持つことで、それぞれのリストは独立して中身を変化させることができるということである。

`[1]`と`[1]`は異なるオブジェクトではあるが、`[1] == [1]`は`True`と評価される。  
`[1]`と`[1]`は何が等しいのか？  
それは「値」なのか？

日常会話においては、「`[1]`と`[1]`はMutableだから異なるオブジェクトになるが、値は同じ」と言っても大体は差し支えない気はする。

しかし、「`==`の評価結果が`True`であれば必ず同じ値なのか」と言われると疑問が残る。  
`==`演算子は、そのクラスに属する`__eq__`メソッドを呼び出すという動きをする。  
もし、`__eq__`が無条件に`False`を返すとしたらどうか？

以下に具体的なコードを2例示す。

1つ目のコードでは、`MyClass`というクラスを定義している。  
`str`クラスを継承しており、`MyClass()`特有の定義は一切していない (`pass`)。

この`MyClass`は`str`と異なる名前空間は持つものの、挙動としては基本同じになる。  
(しかし、`myobject1 is myobject2`が`False`になるのは`str`と挙動が異なる。`str`で同じことをすると`True`になる。この理由はよくわからない)

例えば、`myobject1 = MyClass('Hey')`は、`myobject1 = str('Hey')`と同じ挙動である。  
`myobject1`と`myobject2`は`MyClass`型のオブジェクトになるが、中身は`'Hey'`という文字列と実質的に同じである。  
**当然、`myobject1 == myobject2`は`True`になる。**

```py
class MyClass(str):
    pass


myobject1 = MyClass('Hey')
myobject2 = MyClass('Hey')

print(f'{myobject1=}')
# myobject1='Hey'

print(f'{myobject2=}')
# myobject2='Hey'

print('type(myobject1) -->', type(myobject1))
# type(myobject1) --> <class '__main__.MyClass'>

print('myobject1 is myobject2 -->', myobject1 is myobject2)
# myobject1 is myobject2 --> False

print('myobject1 == myobject2 -->', myobject1 == myobject2)
# myobject1 == myobject2 --> True
```

もう一つのコード例を示す。  
今度は、`MyClass`が`__eq__`メソッドの定義を上書きする。  
この`__eq__`メソッドの中身が`return False`だった場合、`myobject1 == myobject2`も当然`False`になる。

「`==`の評価結果が`True`なら同じ値」でだいたい話が通じそうなものだが、こういった例外もあるということは認識しておきたい。

```py
class MyClass(str):
    def __eq__(self, MyClass):
        return False


myobject1 = MyClass('Hey')
myobject2 = MyClass('Hey')

print(f'{myobject1=}')
# myobject1='Hey'

print(f'{myobject2=}')
# myobject2='Hey'

print('type(myobject1) -->', type(myobject1))
# type(myobject1) --> <class '__main__.MyClass'>

print('myobject1 is myobject2 -->', myobject1 is myobject2)
# myobject1 is myobject2 --> False

print('myobject1 == myobject2 -->', myobject1 == myobject2)
# myobject1 == myobject2 --> False
```

オブジェクトの初期化の仕方が全く同じであっても、`print()`に渡したときの表示方法が全く同じであっても、それは`__eq__`や`__str__`の処理結果がたまたま同じだっただけのこと。  
これらの事象が「値が同じである」と同じ意味かというと、実際には全く関係ない...というのが自分の理解である。

では結局Pythonの値とは何かというと、「今の自分には全然わからない」である。

もしかしたらC言語においては値という概念があるかもしれないが、そこまで追うことは自分には無理。  
そしてPythonのコード上でも「値」を説明するに足るような検証結果は得られなかった。

値って難しい。

### 値の意味は型によって異なる

値のの性質は型によって異なる。

int型の場合は数の大小、等・不等などによって値を表現する。  
str型は文字列によって値の等・不等を表現し、大小関係は定義されていない。

クラスにおいて「どんなメソッドが定義されているか」が値を決めるという感覚がある。  
前セクションの`__eq__`の例のように、デタラメなクラスを作ってしまうと値という概念そのものを全くイメージできないこともある。

結局クラスとは何かをモデリングして設計するもので、モデリングの仕方によっては大小や等・不等の関係が矛盾することもありうる。  
値はオブジェクトの性質を抽象的に表現するのに便利な言葉の表現であり、でもPythonのソースコード上には存在しないものとして扱うのが良い気がする。

int型なら`5`の値は5。  
`5`は`4`よりも大きい。

str型なら`'a'`の値は'a'。  
値の間に大小関係はない。

int型もstr型も、リテラルの文字列表現が等しければ値も大体等しいし、`==`で結んだときは`True`となる。

「値」という言葉の定義は不明なものの、型によってどういった性質を持っているかの具体例を集めていけば理解が深まる。

特殊なクラスを自作した場合に「値」の特徴がつかめないものもできることがあるが、そういったケースはあまり考えなくても良い。  
そういったクラスをそもそも設計すべきではないので、値の議論以前の問題としてクラスを作り直すべきである。

クラスの定義次第で「値」がどんなものかは玉虫色に変わるが、「このクラスにおいて値とはこんな感じ」という柔軟な捉え方をするのが大事。  
厳密な理解としては、「Pythonのソースコードにおいてはオブジェクトが最小単位であり、値を取り出すことはできない」ぐらいで十分だと思う。

## (まとめ) 値とstring representation

自分の中での理解を一言でいうと...

* Pythonのソースコードから「値」にアクセスすることはできない
* `print()`で表示できるのはstring representationという、値を連想させる文字列
* より厳密なstring representationを得たいなら、`repr()`を使うことでリテラルと同じ見た目の文字列を生成できる (ことが多い)。デバッグするときは`repr()`を使うべき

CPythonのコア部分はC言語で実装されている。  
[The Python Standard Library - Introduction](https://docs.python.org/3/library/intro.html)によると、C言語で実装されているものの例としては:

* 演算の優先順位やオブジェクトの命名規則など構文周りのルール
* 一部の標準ライブラリ

「一部の標準ライブラリ」で値の部分が出てきている可能性はある。  
現に、CPythonのソースコードを見ると`Value`という単語はたくさん出てくる。  
C言語のレイヤーではオブジェクトではないint型が存在するため、やはりValueという概念はちゃんと存在するように見受けられる。  
Pythonのレイヤーでは見えないので値についてあまり突っ込んだ理解はできないが、値は確かに存在する...と思って差し支えない。  
[GitHub - python/cpython - longobject.c](https://github.com/python/cpython/blob/main/Objects/longobject.c)
