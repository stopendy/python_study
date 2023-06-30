# リテラル

[@IT - Python入門 - Hello Python：一番簡単なプログラムを作ってみよう](https://atmarkit.itmedia.co.jp/ait/articles/1904/05/news021.html#simplehelloworld)  
→ `print()`関数

## サマリ

| 型名       | scalar/compound | mutability |
| ---------- | --------------- | ---------- |
| int        | scalar          | immutable  |
| float      | scalar          | immutable  |
| bool       | compound        | immutable  |
| str        | compound        | immutable  |
| bytes      | compound        | immutable  |
| bytearray  | compound        | mutable    |
| list       | compound        | mutable    |
| tuple      | compound        | immutable  |
| dict       | compound        | mutable    |
| set        | compound        | mutable    |
| frozenset  | compound        | immutable  |

## int

### intの概要

* [Built-in Types - Numeric Types - int, float, complex](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)
* [The Python Language Reference - Data model - The standard type hierarchy - #Integers (int)](https://docs.python.org/3/reference/datamodel.html#the-standard-type-hierarchy#:~:text=Integers%20(int))
* [class int()](https://docs.python.org/3/library/functions.html#int)

整数を表現する。

int型は、シフト演算をサポートするために内部的には二進数で管理されている。  
負の数は2の補数で表現されている。

### intの基本演算

[Python Tutorial - 3.1.1. Numbers](https://docs.python.org/3/tutorial/introduction.html#numbers)

```py
print(2 + 2)
# 4

print(10 - 2)
# 8

print(2 * 3)
# 6

print(2 ** 3)
# 8

print(6 / 3)
# 2.0

print(10 // 7)
# 1

print(10 % 7)
# 3
```

### intの初期化

* [class int()](https://docs.python.org/3/library/functions.html#int)

intの初期化の仕方は2つある。

1つ目は小数点を伴わない数字をリテラルとして記載すること。

```py
print(5)
# 5
```

2つ目はclass constructorを使うこと。  
class constructorとは、classをcall (カッコ付きで使う) することでインスタンスを生成すること。

`int(x)`は、オブジェクト`x`を元に`int`型のインスタンスを返す。  
内部的には、`x`の`__int__()`、`__index__()`、`__trunc__()`メソッドのいずれかが呼ばれる。  
引数なしで`int()`を実行すると`0`を返す。

実用上は、以下のように型変換に使うことが多いと思う。

```py
print(int('5'))
# 5
```

## float

### floatの概要

* [Built-in Types - Numeric Types - int, float, complex](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)
* [The Python Language Reference - Data model - The standard type hierarchy](https://docs.python.org/3/reference/datamodel.html#the-standard-type-hierarchy) → numbers.Real (float)
* [class float()](https://docs.python.org/3/library/functions.html#float)

小数を表現する。

C言語でいうdouble型 (倍精度浮動小数点数)。  
`.`を含めることでfloatリテラルを作れる。

```py
print(1.)
# 1.0

print(2.0)
# 2.0

print(3.5)
# 3.5
```

単精度浮動小数点数は、Pythonには存在しない。

### floatの基本演算

基本intと同じ。

### floatのconstructor

[float(x)](https://docs.python.org/3/library/functions.html#float)

`float(x)`はオブジェクト`x`を元に、`float`型のインスタンスを返す。  
内部的には`x`の`__float__()`か`__index__()`を呼び出す。

引数なしで`float()`が実行された場合、`0.0`が返される。

型変換に使うことがある...かもしれない。

```py
print(float(5))
# 5.0

print(float('2.3'))
# 2.3
```

## (参考) complex

### complexの概要

* [Built-in Types - Numeric Types - int, float, complex](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)
* [The Python Language Reference - Data model - The standard type hierarchy](https://docs.python.org/3/reference/datamodel.html#the-standard-type-hierarchy) → numbers.Complex (complex)
* [class complex()](https://docs.python.org/3/library/functions.html#complex)

複素数。

内部的には実数部と虚数部でそれぞれfloat型の値を持っている (倍精度浮動小数点数2つで1セットになっている)。

実数部と虚数部の足し算で表現する。  
虚数部は末尾に`j`か`J`をつける。  

### complexの基本演算

```py
print(1 + 3j)
# (1+3j)

print(1.0 + 5.3j)
# (1+5.3j)
```

実数部が`0`の場合は表記を省略してよい (虚数)。  
その場合も必ず`j`か`J`をつける。  
[The Python Language Reference - Lexical analysis - #Imaginary literals](https://docs.python.org/3/reference/lexical_analysis.html#imaginary-literals)

```py
print(0j)
# 0j

print(3j)
# 3j

print(2j ** 2)
# -4 + 0j
```

complex `z`の実数部は`z.real`、虚数部は`z.imag`で取得できる。  
いずれも読み取り専用の値。

### complexのconstructor

オブジェクト`x`を引数に`complex(x)`を呼び出すと、内部的には`x.__complex__()`、または`x.__float__()`、または`x.__index__()`が呼ばれる。

## str

### strの概要

* [The Python Language Reference - Data model - The standard type hierarchy](https://docs.python.org/3/reference/datamodel.html#the-standard-type-hierarchy) → Strings
* [Python Tutorial - #String](https://docs.python.org/3/tutorial/introduction.html#strings)

strとは、Unicode code pointsを表すオブジェクト。  
code pointは、日本語訳すると符号点という。  
code pointとは、`\u0041`などの整数値のこと。  
Pythonにおいてはいくつかの表記があり、表記方法によって10進数、16進数、8進数の場合がある。  
詳細は[Bytes Literal](./4_string_advanced.md#bytes-literal)を参照。

strが表現できるUnicodeの範囲は`\u0000`〜`\U0010ffff`。  
PythonはC言語と違ってchar型 (※) を持たないが、特定要素を取り出すことは可能 (例: `'abcde'[0]`)  
(※) C言語のcharとは1文字を格納する1バイトの型のこと

### strの基本演算

* [Python Tutorial - #String](https://docs.python.org/3/tutorial/introduction.html#strings)

YAMLやbashと違って、`"`と`'`に差はない。  
`\`によるエスケープが可能な限り少なくなるように書くのが推奨される。

```py
print("a")
# a

print('a')
# a

print("can't")
# can't

print('can\'t')
# can't

print("said \"I'm serious.\"")
# said "I'm serious."

print('said "I\'m serious."')
# said "I'm serious."

print("a\nb")
# a
# b

print('a\nb')
# a
# b
```

複数行。

```py
print("""Shopping List
- apple
- python""")
# Shopping List
# - apple
# - python

print('''
New line at both the start and the end of lines.
''')
# 
# New line at both the start and the end of lines.
# 

print('''\
Escape new lines.\
''')
# Escape new lines.
```

足し算。

```py
print('a' + '0123')
# a0123
```

indexing、またはsubscripting。  
特定要素を取り出すこと。

```py
print('1234'[3])
# 4

print('yes'[0])
# y
```

slicing。  
`:` (コロン) で区切った数値によって要素の範囲を表現する。  
引数の順序と意味は、range関数と似ている (というか同じ)。

1要素目が始点、2要素目が終点、3要素目が間隔。

値を省略した場合はデフォルト値が入る。  
始点と終点は最初と最後 (間隔が負の数の場合は反転)。  
間隔はデフォルト1。

'python'のインデックスの範囲は`-6`〜`5`まで。

```py
print('python'[0:3])
# pyt

print('python'[:2])
# py

print('python'[::2])
# pto

print('python'[::-1])
# nohtyp
```

**文字列はImmutable。**  
つまり、文字列オブジェクトのメンバーを変更することはできない。

```py
s = 'Python'
s[0] = 'p'
# TypeError: 'str' object does not support item assignment
```

別オブジェクトの代入なら可能。  
「同一オブジェクトを保ったまま中身を変えることができない」だけ。

```py
s = 'Python'
s = 'python'
print(s)
# python
```

### strのconstructor

[str()](https://docs.python.org/3/library/stdtypes.html#str)

strはクラス名。  
`str()`のようにクラスを呼び出すことでインスタンスを生成できる。

厳密には異なるが、「str型に変換する関数」として使っても使う分には違和感なく使える。  
しかし繰り返しになるが`str()`はクラスを呼び出しているので、実際にはクラスの初期化処理が行われている。

`str(x)`を呼び出すと、オブジェクトを元にstrインスタンスを生成して返す。  
`str()`は空白の文字列オブジェクト (`''`) を返す。

内部的には`x.__str__()`を実行している。  
`x.__str__()`が未定義なら`repr(x)`を代わりに実行する。

## bool

* [The Python Language Reference - Data model - The standard type hierarchy](https://docs.python.org/3/reference/datamodel.html#the-standard-type-hierarchy) → Booleans (bool)

`True`と`False`の2種類のみのオブジェクトを持つ。  
先頭のみ大文字にする。

内部的にはint型のsubtype。  
中身としては、`0`と`False`、`1`と`True`は同じ。

`1 == 1`のような演算子の結果として返ってくることが多い。

### boolの基本演算

```py
print(True)
# True

print(False)
# False

print(3 < 5)
# True
```

### bool() constructor

[bool()](https://docs.python.org/3/library/functions.html#bool)

`bool(x)`は、オブジェクト`x`がTrusyなら`True`、Falsyなら`False`を返す。  
例えば`str(False)`は`'False'`になるが、`bool('False')`は`True`なので注意。  
「1文字以上の文字列はTruthy」と扱われるためである。  
[Truth Value Testing](https://docs.python.org/3/library/stdtypes.html#truth)

過去の経験上、「空のオブジェクト」はFalsyで、他はTruthyであると見なされることが多い。  
ここでいう「空のオブジェクト」とは、引数を渡さずにconstructorを呼び出して生成したインスタンスのこと。  
Falsyな値の具体例は以下のとおりである。

* `int()`で返される`0`
* `str()`で返される`''`
* `list()`で返される`[]`

Truth Value Testingの結果は、そのオブジェクトの`__bool__()`の戻り値、または`__len__()`が0を返すか否かによって決まる。

```py
print(bool(''))
# False

print(bool('0'))
# True

print(bool(0))
# False

print(bool(-1))
# True
```

## list

### listの概要

* [Python Tutorial - #Lists](https://docs.python.org/3/tutorial/introduction.html#lists)
* [Built-in Types - Sequence Types - list, tuple, range](https://docs.python.org/3/library/stdtypes.html#sequence-types-list-tuple-range)

compound data typesの一つ。  
順番の概念を持つ。

```py
print([1, 2, 3][2])
# 3

print(['Hello', 'guys.', "How's", 'it', 'going?'][3])
# it
```

Mutable。

```py
l = ['tomato', 'cabbage', 'spinach']
l[1] = 'Vegeta'
print(l)
# ['tomato', 'Vegeta', 'spinach']

del(l[0])
print(l)
# ['Vegeta', 'spinach']
```

ネスト。  
外側配列の3要素目は`[3, 4]`というリスト。  
「`[3, 4]`で1つのリスト型の値」と考える。

```py
print([1, 2, [3, 4], 5])
# [1, 2, [3, 4], 5]
```

### listの初期化

list型オブジェクトの初期化には3つの方法がある。

* リテラル表記 (`[]`)
* constructorの利用 (`list()`)
* dictionary comprehension (リスト内包表記)

#### listのリテラル記述

`[]`で囲う。

```py
print([3, 1, 2])
# [3, 1, 2]
```

#### list() constructor

* [class list()](https://docs.python.org/3/library/functions.html#func-list)
* [iterable](./9_iterable_iterator_generator.md#iterable)

`list(iterable)`によって、iterableをlistに変換する。

```py
print(list(range(3)))
# [0, 1, 2]
```

#### list comprehension

[list comprehension (リスト内包表記)](./6_if_match_for_while_range.md#list-comprehension-リスト内包表記)を参照。

## tuple

* [Built-in Types - Sequence Types - list, tuple, range](https://docs.python.org/3/library/stdtypes.html#sequence-types-list-tuple-range)
* [The Python Standard Library - Built-in Types - #Tuple](https://docs.python.org/3/library/stdtypes.html#tuple)

基本は[#リスト](#リスト-配列)と同じ性質を持つ。  
ただしリストは[mutable](./data_model/mutable_immutable.md#mutable)であるのに対し、タプルは[immutable](./data_model/mutable_immutable.md#immutable)である。

タプルのリテラルには必ず`,`を伴う。  
タプルのリテラルを木j痛するときは、`()`で囲っても囲わなくても良い。  

`()`が必要なケースは2つ。

* 空のtupleを作成する時 → `()`
* 構文上制限があるとき

```py
t1 = (1, 2)
t2 = 1, 2
t3 = 1,
t4 = ()

print(t1)
# (1, 2)

print(t2)
# (1, 2)

print(t3)
# (1,)

print(t4)
# ()
```

クラス呼び出しでもタプルのリテラルを生成できる。  
[tuple(iterable)](https://docs.python.org/3/library/stdtypes.html#tuple)

```py
t = tuple(range(5))

print(t)
# (0, 1, 2, 3, 4)
```

タプルの表現は、複数変数の同時代入にも使える。

```py
x, y = 1, 2

print(x)
# 1

print(y)
# 2
```

タプルはimmutableなので、要素を指定した代入演算子や`append()`のようなメソッドには対応しない。

```py
t = (1,)
t[0] = 5
# TypeError: 'tuple' object does not support item assignment
```

```py
t = (1,)
t.append(5)
# AttributeError: 'tuple' object has no attribute 'append'
```

## range

* [Built-in Types - Sequence Types - list, tuple, range](https://docs.python.org/3/library/stdtypes.html#sequence-types-list-tuple-range)
* [The Python Standard Library - Built-in Types - Ranges](https://docs.python.org/3/library/stdtypes.html#ranges)

rangeは、イテラブル (iterable) として使われるオブジェクト。  
rangeのリテラルは、`range()`クラス呼び出しによって作成する。

```py
r1 = range(5)
r2 = range(2, 5)
r3 = range(0, 100, 10)

print(r1)
# range(0, 5)

print(r2)
# range(2, 5)

print(r3)
# range(0, 100, 10)
```

rangeのリテラル表記に見慣れない場合は、listやtupleに変換してからprintすると見やすくなるかもしれない。

```py
l1 = list(range(5))
l2 = list(range(2, 5))
l3 = list(range(0, 100, 10))

print(l1)
# [0, 1, 2, 3, 4]

print(l2)
# [2, 3, 4]

print(l3)
# [0, 10, 20, 30, 40, 50, 60, 70, 80, 90]
```

## dict

### dictの概要

* [Built-in Types - Mappings - dict](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict)
* [class dict()](https://docs.python.org/3/library/functions.html#func-dict)
* [Reference manual - Data model - #Mappings](https://docs.python.org/3/reference/datamodel.html#:~:text=Mappings)

1つまたは複数のkeyとvalueを紐付ける型。

dict型変数`a`に対し、`a[key]`のようにkeyをsubscriptで指定することでvalueを取り出すことができる。

```py
d = {'name': 'endy', 'age': 30, 5: 'five'}

print(d)
# {'name': 'endy', 'age': 30, 5: 'five'}

print(d['name'])
# endy

print(d['age'])
# 30

print(d[5])
# five
```

keyにはhashableなTypeを指定できる。  
immutableなTypeは大抵指定可能。  
strやintを指定することが多い印象。

unhashableなTypeはkeyとして指定できない。  
具体的には、idではなくvalueによって値を比較するようなmutableな型はkeyとして指定できない。  
例えば、listやdictはkeyとして指定できない。

hashableの意味について、詳細は[(参考) hashableとは](#参考-hashableとは)を参照。

```py
print({[0]: 3})
# TypeError: unhashable type: 'list'
```

Python 3.7以降では、dictに登録したkeyの順序が内部的に記憶される。  
ただ、順序が異なっていても`==`演算子の結果は「等しい」という判定になる。

```py
d1 = {'k1': 'v1', 'k2': 'v2'}
d2 = {'k2': 'v2', 'k1': 'v1'}

print(f'{d1=}')
# d1={'k1': 'v1', 'k2': 'v2'}

print(f'{d2=}')
# d2={'k2': 'v2', 'k1': 'v1'}

print(f'{d1==d2=}')
# d1==d2=True
```

dictはmutableなのでkeyを指定した代入演算子が使える。  
また、`add()`メソッドによる要素の追加も可能。

代入演算子によってdict型の要素を編集した場合は、以下の挙動になる。

* 既存のkeyに対応するvalueを上書きした場合、順序は維持される
* 新規にkey, valueの組み合わせを追加した場合、末尾に追加される

```py
d1 = {'k1': 'v1', 'k2': 'v2'}
d2 = {'k2': 'v2', 'k1': 'v1'}

d1['k1'] = 1
print(d1)
# {'k1': 1, 'k2': 'v2'}

d2['k3'] = 'v3'
print(d2)
# {'k2': 'v2', 'k1': 'v1', 'k3': 'v3'}
```

### dictの初期化

dict型オブジェクトの初期化には3つの方法がある。

* リテラル表記 (`{}`)
* constructorの利用 (`dict()`)
* dictionary comprehension (辞書内包表記)

#### dictのリテラル記述

```py
print({'five': 5, 'six': 6})
# {'five': 5, 'six': 6}
```

* [class dict()](https://docs.python.org/3/library/stdtypes.html#dict)

`dict()`による初期化。  
`dict()`は3つの引数を取りうる。

1つ目はkeyword argumentsを渡す方法。  
keyword argumentsの場合、keyは必ずstr型になる...はず。

リテラル記述と比べて、keyを`''`で囲わなくて良い分楽に感じる。

```py
print(dict(one=1, two=2))
# {'one': 1, 'two': 2}

d = {'a': 'apple', 'b': 'blueberry'}
print(dict(**d))
# {'a': 'apple', 'b': 'blueberry'}
```

#### dict() constructor

2つ目はdict以外のMappingを渡す方法。  
dictへの変換に使う。

dict以外のMappingを知らないので、サンプルは無し。

3つ目は、iterableのネストからの変換。  
内側のiterableは、要素数がちょうど2である必要がある。  
内側のiterableは、1要素目がkey、2要素目がvalueとして解釈される。

```py
l1 = [('k1', 'v1'), ('k2', 'v2')]

print(dict(l1))
# {'k1': 'v1', 'k2': 'v2'}
```

`zip()`関数を使うことでもiterableのネストを作成できる。  
keyのみを含むiterableとvalueのみを含むiterableを`zip()`で結合する。  
それを`list()`や`tuple()`などに渡すと、iterableのネストになっていることがわかる。  
更にそれを`dict()`に渡せば`dict`型に変換できる。

**Sequenceをdictに変換する**ときに使えそうなテクニック。  
元々あるSequenceに、後付けで各要素に対応するkey名を格納したSequenceを用意する。  
それをzip、Sequence、dictへと順に変換すれば良い。

```py
z = zip(('k1', 'k2'), ('v1', 'v2'))
print(f'{z=}')
# z=<zip object at 0x7faf529f5180>

l2 = list(z)
print(f'{l2=}')
# l2=[('k1', 'v1'), ('k2', 'v2')]

print(f'{dict(l2)=}')
# dict(l2)={'k1': 'v1', 'k2': 'v2'}
```

#### dictionary comprehension (辞書内包表記)

[dictionary-comprehension](./6_if_match_for_while_range.md#dictionary-comprehension)を参照。

## bytes

* [The Python Standard Library - Built-in Types - #Bytes](https://docs.python.org/3/library/stdtypes.html#bytes)

リテラルの形式は文字列と同じ。  
先頭に`b`か`B`がつくだけ。

```py
print(b'byte strings')
# b'byte strings'

print(b'''
multiline
byte
stream''')
# b'\nmultiline\nbyte\nstream'
```

bytesに含めて良い文字列はASCII文字列のみ。  
ASCIIコードが128以上になる場合は、適切にエスケープして格納する必要がある。

```py
print(b'Ω')
# SyntaxError: bytes can only contain ASCII literal characters
```

```py
print(ord('Ω'))
# 937
```

bytes型について、もっと高度な話は以下を参照。  
[4_string_advanced.md#bytes-literal](./4_string_advanced.md#bytes-literal)

## (参考) bytearray

[bytearray()](https://docs.python.org/3/library/stdtypes.html#bytearray)

bytesとほぼ同じ。  

ただし...

* mutable
* 0〜255のcode pointを表現可能

## set

### setの概要

* [class set()](https://docs.python.org/3/library/stdtypes.html#set)
* [Reference manual - Data model - #Set types](https://docs.python.org/3/reference/datamodel.html#:~:text=Set%20types)

以下の特徴を持つ。

一意の要素を持つ。  
一意じゃない値は直ちに削除される。

重複削除のために`set()` constructorを使うケースもある。  
`class set(iterable)`という定義の通り、`set()`は1つのイテラブルを引数に取る。

```py
print({0, 0, 1})
# {0, 1}

l = [0, 1, 2, 2, 1]
print(set(l))
# {0, 1, 2}
```

数値を要素に含む場合、`1`と`1.0`のように`__eq__()`に渡したときに`True`が返るような関係の場合も **重複** と見なされる。

```py
print({1, 1.0})
# {1}
```

順序を持たない。  
順序を持つlistやtupleとは対照的。

```py
s1 = {'hi', 'hello', 'hey'}
s2 = {'hey', 'hi', 'hello'}

print(s1)
# {'hi', 'hello', 'hey'}

print(s2)
# {'hi', 'hello', 'hey'}

print(s1 == s2)
# True
```

順序を持たないので、「setオブジェクトのn番目の要素を取り出す」といったこともできない。

```py
print({0, 1}[0])
# TypeError: 'set' object is not subscriptable
```

mutable。  
つまり、作成済みのオブジェクトの値を変更可能。  
setの場合、要素を構成するオブジェクトのidを変更できるということ。  
→ [mutable_immutable.md](./data_model/mutable_immutable.md)

以下の例では、setに属するオブジェクトのidを1つ「追加」している。

```py
s = {0, 1}
s.add('hey')

print(s)
# {0, 1, 'hey'}
```

setの要素はhashableでなければならない。  
他にも、dictionaryのkeyはhashableでなければならない。  
hashableの意味について、詳細は[(参考) hashableとは](#参考-hashableとは)を参照。

### setリテラルの生成

3つの方法がある。

1つ目は、`{}`で囲うこと。

```py
print({0, 1})
# {0, 1}
```

2つ目は、`set(iterable)` constructorを使うこと。  
イテラブルを渡さない場合、`set()`は`{}`という空のsetを生成する。

```py
print(set(range(5)))
# {0, 1, 2, 3, 4}
```

3つ目は、set comprehensionを使うこと。  
詳細は[6_if_match_for_while_range.md#set comprehension](./6_if_match_for_while_range.md#set-comprehension)を参照。

```py
print({s for s in range(10) if s % 2 == 0})
# {0, 2, 4, 6, 8}
```

## (参考) frozenset

* [class frozenset](https://docs.python.org/3/library/stdtypes.html#frozenset)
* [Reference manual - Data model - #Set types](https://docs.python.org/3/reference/datamodel.html#:~:text=Set%20types)

基本setと同じ。  
でもimmutable。  
→ [mutable_immutable.md](./data_model/mutable_immutable.md)

リテラルの形式は`frozenset({})`。

```py
print(frozenset(range(5)))
# frozenset({0, 1, 2, 3, 4})
```

immutableなので、オブジェクト生成後に値 (要素のid) を変更できない。  
値を変更したい場合は、オブジェクトを作り直すしかない。

```py
fs = frozenset({0, 1})
fs.add('hey')
# AttributeError: 'frozenset' object has no attribute 'add'
```

set of set...のようなネスト構造を組むとき、子要素はfrozensetにする必要がある...と公式ドキュメントに書いてあった。  
「setの要素はhashableである必要がある」という制約によるものと思われる。

## (参考) hashableとは

hashableの意味はざっくりこんな感じ。  
※難しいので、あまりちゃんと理解していない

* `__hash__()`メソッドを持つ
* `__hash__()`メソッドで生成したハッシュ値は、以下の特徴を持つ
  * 値が等しいならば、ハッシュ値も等しい
  * ハッシュ値はオブジェクトが生成してから消えるまで変化しない
* `__hash__()`を自前で定義しない場合、ユーザー定義のオブジェクトはデフォルトでhashableであり、`id()`の値をベースにしたハッシュ値を生成する
* immutableなオブジェクトにはhashableなものが多い
  * immutableなオブジェクトは「値が同じだけどidは異なる」関係のオブジェクトはなさそうだし、ハッシュ値に`id()`相当の変更できない値を使ってそう...と理解した
* 参考リンク
  * [python glossary - #hashable](https://docs.python.org/3/glossary.html#term-hashable)
  * [Qiita - Python における hashable](https://qiita.com/yoichi22/items/ebf6ab3c6de26ddcc09a)

`__hash__()`はどうやら`__eq__()`よりも計算コストが低いらしい。  
したがって、`__eq__()`よりも先に`__hash__()`の値が等しいかを確認し、等しければ`__eq__()`を実行するらしい。  
`in`演算子など、要素の数だけ何度も`==`相当の演算を実施しなければならないケースにおいて、この最適化が効いてくると理解した。  
※詳細は上述のQiitaのリンクを参照
