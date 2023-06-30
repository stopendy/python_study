# Built-in Types

## 組み込み型の階層構造

[The Python Language Reference - Data model - #The standard type hierarchy](https://docs.python.org/3/reference/datamodel.html#the-standard-type-hierarchy)

組み込みの型をまとめる。

### None

「値がない」ことを意味する型。  
具体例としては、`return`を伴わない関数は形式的に`None`を返す。

```py
def myfunc():
    pass


print(myfunc())
# None
```

`None`は、予め定義された名前として存在する。  
リテラルではなく名前であり、Syntax上は区別されるらしい。  
名前であってもリテラルであっても、「`None`を代入する」などの基本的な使い方であれば変わりない気はする。

`None`はFalsy valueである。 → [True value testing](https://docs.python.org/3/library/stdtypes.html#truth)

### NotImplemented

数値計算系のメソッドが対応していない演算子に対して返す値。

`NotImplemented`は、組み込みの名前として存在する。

Booleanとして評価されるべきではない。  
Python 3.9ではTruthy Valueとして評価されるが、Deprecation Warningを伴う。  
将来的にはTypeErrorを返すようになる。

### Ellipsis

* [3 Uses of the Ellipsis in Python](https://medium.com/techtofreedom/3-uses-of-the-ellipsis-in-python-25795aac723d)
* [stack overflow - What does the Ellipsis object do?](https://stackoverflow.com/questions/772124/what-does-the-ellipsis-object-do)

使い方はいくつかある。  
type hintかNumPyを使うときに思い出せば良い。

* PEP484によると、Type Hintにおける表記として`...`を使うことがある
* NumPyで多次元配列を作るときに使われる。任意サイズの要素が任意個数並んでいることを表す
* 未実装の関数定義にプレイスホルダーとして使う (これは恐らく慣習的なもので、本来の使い方ではない気がする。個人的には`pass`と書くほうが好み)

`Ellipsis`は組み込みの名前として存在する。  
`...`というリテラルも存在する。  
`...`の使い方は上記箇条書きの通り。

Truthy valueとして評価される。

### Numbers

* [PEP 3141](https://peps.python.org/pep-3141/)
* [#(参考) 抽象基底クラスとは](#参考-抽象基底クラスとは)

Numbersは抽象基底クラスであり、以下の階層構造を持つ。

`Number :> Complex :> Real :> Rational :> Integral`  
`数 :> 複素数 :> 実数 :> 有理数 :> 整域` (翻訳)

(※) `:>`は、「左辺は右辺のsuper classである」を意味する記号  
(※) Integralが整数というのは嘘らしい。つまりIntegralとIntegerは全く異なる ([リンク](https://www.quora.com/What-is-the-difference-between-the-integer-and-integral))

抽象基底クラスについては[#(参考) 抽象基底クラスとは](#参考-抽象基底クラスとは)を参照。  
**一旦詳細は省くが、インフラ自動化を目指す自身のユースケースにおいては、Numbersという抽象基底クラスを使ってプログラムを書くことはない。**  
int, float, bool, complexなどの組み込みの型があれば十分。

とはいえ、上記組み込みの型が共通して持つ「`Number`型の特徴」を理解しておくと実用上役に立つ。  
`Number`は以下の特徴を持つ。

* immutableである
* `+`などの数値計算用の演算子や組み込みのメソッドを持つ

#### 組み込み型とNumbersの関係

* [intはNumbers.Integralの仮想的サブクラス](https://github.com/python/cpython/blob/v3.10.5/Lib/numbers.py#L393)
* boolはintのsubtype
* [floatはnumbers.Realの仮想的サブクラス](https://github.com/python/cpython/blob/v3.10.5/Lib/numbers.py#L264)
* [complexはnumbers.Complexの仮想的サブクラス](https://github.com/python/cpython/blob/v3.10.5/Lib/numbers.py#L144)

※仮想的サブクラスという用語については、[#(参考) intとIntegralの関係](#参考-intとIntegralの関係)にて説明する。

### Sequences

Sequenceの共通の特徴は以下の通り。

* `len()`関数の引数に渡すと要素数を返す
* Sequence `a` のi番目の要素は`a[i]`で表現できる (`i >= 0`)
* Sliceを使える → [literal - 文字列](../1_literal.md#文字列)

#### Immutable Sequences

immutableなSequnceオブジェクト。  
Immutable Sequenceオブジェクトが一度作成されると、それが要素として参照しているオブジェクトを別オブジェクトで置き換えることができない。  
詳細は以下記事を参照。  
[mutableとimmutable - #immutable](./mutable_immutable.md#immutable)

以下の型がimmutable sequenceに分類される。

* str
* tuple
* bytes

#### Mutable Sequences

mutableなSequenceオブジェクト。  
以下の型がmutable sequenceに分類される。

* list
* bytearray

### Set types

* [1_literal.md - #set](../1_literal.md#set)
* [1_literal.md - #(参考) frozenset](../1_literal.md#参考-frozenset)

setとfrozensetがこのカテゴリに入る。

### Mappings

* [1_literal.md - #dic](../1_literal.md#)

x

## (参考) 抽象基底クラスとは

* [PythonのABC - 抽象クラスとダック・タイピング](https://qiita.com/kaneshin/items/269bc5f156d86f8a91c4)
* [PEP 3119](https://peps.python.org/pep-3119/)

英語でいうとAbstract Base Class。  
ABCと略される。

一般的なクラスは、直接callされてインスタンスを生成する。

```py
class MyClass():
    pass


c = MyClass()
print(isinstance(c, MyClass))
# True
```

抽象基底クラスは、ざっくり以下のような特徴を持つらしい。  
※あまりちゃんと調べていないので、ざっくりとした情報として捉えて欲しい

* 抽象基底クラスは、別のクラスに継承されるためのクラス
* 継承先のクラスの仕様をゆるく決定する (メソッドの中身は定義していないけど、子クラスはXXXというメソッドを持たなければならない...など)
* 大規模なソフトウェア開発において利用される
* 特徴
  * 抽象基底クラスを直接呼び出してインスタンス化することはない
  * 抽象基底メソッドを抽象基底クラスで定義しておけば、子クラスも同じメソッドを定義していないとエラーになる
* 抽象基底クラスを定義するためのクラスは、[abc module](https://docs.python.org/3.10/library/abc.html)で定義されている

インフラエンジニアに読ませるようなコードでABCを使うと小難しくなってしまうので、自分がABCを書くことはあまりなさそうに思える。  
抽象基底クラスから複数の子クラスが派生するような大きめのコードも正直まだ書けない。

ただ、GitHub上のOSSのソースコードではABCがいっぱい使われていると思うので、他人が書いたABC利用のコードは理解できるようにしておきたい。  
現段階では、`@abstractmethod`のようなdecoratorが出てきたときは「インスタンスを生成しない親クラスなんだな」と理解する程度にとどめておく。

PEP 3119のリンクは貼ったが、まだあまり読んでいない。  
Pythonの経験をもっと高めてから読んだほうが効率が良いため。

### (参考) intとIntegralの関係

* [GitHub - python/cpython - numbers.py - #L393](https://github.com/python/cpython/blob/v3.10.5/Lib/numbers.py#L393)
* [Stackoverflow - Difference between int and numbers.Integral in Python](https://stackoverflow.com/a/58859688/14387327)

`int`は`Integral`の仮想的サブクラスだった。  
`int`クラスの定義は`Integral`を継承しているわけではない。

言い換えると、`class int(Integer):`のように定義されているわけではない。  
`Integral.register(int)`と記述されている。

クラスが継承関係にあれば、`クラス名.__mro__`で親クラスを確認できる。  
しかし、`int`は`Integral`を継承しているわけではないので、この方法では確認できない。

```py
print(int.__mro__)
# (<class 'int'>, <class 'object'>)
```

`issubclass()`であれば、仮想的サブクラスの場合にTrueと表示される。

```py
from numbers import Integral


print(issubclass(int, Integral))
# True
```
