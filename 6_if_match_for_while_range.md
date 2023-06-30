# if, for, while

## if

[Python Tutorial - More Control Flow Tools - #if](https://docs.python.org/3/tutorial/controlflow.html#if-statements)

`if` statementに渡した条件式が`True`と評価される場合のみ、配下の処理が実行される。

if文はインデントを伴う。  
Pythonはインデントによって階層構造を表現する。

階層構造の始まりの目印として、Pythonでは`:`が使われる。  
このルールはクラス定義、関数定義、while文、for文など他の階層構造においても全て同様である。

```py
if 3 > 2:
    print(1)
# 1

if 3 < 2:
    print(2)
```

* [Truth Value Testing](https://docs.python.org/3/library/stdtypes.html#truth)

各オブジェクトはTruthy、Falsyの概念がある。  
Booleanとしてそもそも評価すべきでないオブジェクトも存在する。

数値の`0`、`len()`に渡すと`0`になるものはFalsyである (`False`相当である) ことが多い。  
他は大体Truthy (`True`相当)。

```py
if True:
    print(1)
# 1

if False:
    print(2)

if '0':
    print(3)
# 3

if '':
    print(4)

if -1:
    print(5)
# 5

if 0:
    print(6)
```

`elif` statementは、直前の`if`が`False`扱いとなって実行されなかった場合のみ評価される。  
`elif`は連続して使えるが、その先頭には必ず`if`が来る。

```py
if True:
    print(1)
elif True:
    print(2)
# 1

if False:
    print(3)
elif True:
    print(4)
# 4
```

`else`は、`if`や`elif`とセットで使われる。  
同一階層で先に呼ばれた`if`や`elif`がすべて`False`扱いで実行されなかった場合のみ、`else`配下が実行される。

```py
if False:
    print(1)
elif False:
    print(2)
else:
    print(3)
# 3

if True:
    print(4)
else:
    print(5)
# 4
```

## for

### 概要

* [Python Tutorial - More Control Flow Tools - #for](https://docs.python.org/3/tutorial/controlflow.html#for-statements)
* [@IT - Python入門 - #for文](https://atmarkit.itmedia.co.jp/ait/articles/1905/10/news023.html#for)

`for` statementにiterableを渡すと、iterableの全ての要素に対してループ処理を行う。

iterableとは、要素を1つずつ取り出せるようなオブジェクトのこと。  
具体的にはSequence (list, str, tuple)やrange, dict、file (1行単位のループ) がiterableである。  
詳細は[9_iterable_iterator_generator.md#iterable](./9_iterable_iterator_generator.md#iterable)にて補足する。  

以下の例ではlist, range, dictに対して`for`文ループを実行している。

dictはそのまま`for`に渡すとkeyに対してループする。  
書き方を変えて、`for key in d.keys()`と書いてもよい。  
`dict.keys()`は、dict型の全てのkeyをlist型で返すメソッドである。

```py
names = ['Mary', 'John', 'Harry', 'Elizabeth']

for name in names:
    print(name)
# Mary
# John
# Harry
# Elizabeth

for i in range(3):
    print(i)
# 0
# 1
# 2

d = {'name': 'endy', 'gender': 'mail', 'age': None}

for key in d:
    print(key)
# name
# gender
# age

for value in d.values():
    print(value)
# endy
# mail
# None
```

for文のループ変数をtupleによって2つ使うこともできる。  
for文によってiterableを展開した結果、各要素が2つの要素を持つtupleやlistにすれば良い。

このようなiterableを作る方法は、知っている範囲では2つある。

1つ目は、`zip()`によって2つのiterableを結合する方法。  
for文に渡すときはzipオブジェクトのままで良い。

```py
l1 = ['one', 'two', 'three']
l2 = [1, 2, 3]

for a, b in zip(l1, l2):
    print(f'{a} is {b}')

# one is 1
# two is 2
# three is 3

print(list(zip(l1, l2)))
# [('one', 1), ('two', 2), ('three', 3)]
```

2つ目は、`dict.items()`を使う方法。  
dictionary型のオブジェクトが既にある場合はこの方法が有効。

```py
d = {'one': 1, 'two': 2, 'three': 3}

for x, y in d.items():
    print(f'{x} is {y}')
# one is 1
# two is 2
# three is 3
```

* [@IT - Python入門 - イテレータと反復可能オブジェクト](https://atmarkit.itmedia.co.jp/ait/articles/1906/11/news007_2.html#whatsiterator)
* [The Python Language Reference - Compound statements - #for](https://docs.python.org/3/reference/compound_stmts.html#the-for-statement)

for文は、内部的にiterableからiteratorを生成して、next関数によって次の要素を取得している。  
最後の要素まで繰り返した後に`next()`を実行すると`StopIteration`例外が発生するが、`for`文は内部的にこのエラーをキャッチしてループ終了を検知している。  
iterable, iteratorについては[9_iterable_iterator_generator.md](9_iterable_iterator_generator.md)を参照。

* [The Python Language Reference - Simple statements - #continue](https://docs.python.org/3/reference/simple_stmts.html#continue)

`continue` statementにより、次のループに行く。  
`continue` statementは、`while` statementか`for` statementの内側でのみ使われる。  

```py
numbers = [0, 1, 2, 3]

for number in numbers:
    if number % 2 == 0:
        continue
    print(number)
# 1
# 3
```

* [The Python Language Reference - Simple statements - #break](https://docs.python.org/3/reference/simple_stmts.html#break)

`break` statementを実行すると、ループ処理を中断する。
`break` も`continue`と同様に`for`か`while`の配下で使われる。

```py
numbers = [0, 1, 2, 3]

for number in numbers:
    if number == 2:
        break
    print(number)
# 0
# 1
```

`for`の後に`else`が存在するとき、`for`ループが最後に実行された場合のみ`else`配下が実行される。

```py
numbers = [0, 1, 2, 3]

for number in numbers:
    print(number)
else:
    print('Finished.')
# 0
# 1
# 2
# 3
# Finished.

for number in numbers:
    if number == 2:
        break
    print(number)
else:
    print('Finished.')
# 0
# 1
```

### list comprehension (リスト内包表記)

* [list comprehension](https://docs.python.org/3/glossary.html#term-list-comprehension)

for文ループを使ってlistリテラルを生成する記法。  
要素数の多いlistも短い記述で簡潔に記述できる。

`[]`の中にスペース区切りで2つの記述を行う。  

1. 要素を表現する数式を記述する
2. for文

```py
print([i for i in range(5)])
# [0, 1, 2, 3, 4]

print([i ** 2 for i in range(3, 8)])
# [9, 16, 25, 36, 49]

print([0 for i in range(5)])
# [0, 0, 0, 0, 0]
```

通常のfor文で記述すると、以下のように長い表現になる。

```py
l = list()

for i in range(5):
    l.append(i)

print(l)
```

if文を組み合わせることで、条件を満たしたループのみ実行されるようにできる。  
if文はfor文の次にスペース区切りで記述する。

```py
print([i for i in range(5) if i % 2 == 0])
# [0, 2, 4]
```

内包表記を使わず表記すると、以下のようにfor文とif文のネストと同義になる。

```py
l = list()

for i in range(5):
    if i % 2 == 0:
        l.append(i)

print(l)
# [0, 2, 4]
```

### set comprehension

* [Glossary - set comprehension](https://docs.python.org/3/glossary.html#term-set-comprehension)

基本的な考え方はlist comprehensionと同じ。

```py
print({s for s in range(10) if s % 2 == 0})
# {0, 2, 4, 6, 8}
```

setなので、被る値があれば即時破棄される。

```py
print({i for i in [0, 1, 0, 0, 1, 2]})
# {0, 1, 2}
```

### dictionary comprehension

* [Glossary - dictionary comprehension](https://docs.python.org/3/glossary.html#term-dictionary-comprehension)
* [@IT - Python入門 - #辞書の内包表記](https://atmarkit.itmedia.co.jp/ait/articles/1906/19/news017_2.html#dictcomprehension)

dictionary comprehensionは、以下の2つの知識があることを前提とする。

* [for文](./6_if_match_for_while_range.md#for)
* [#list comprehension (リスト内包表記)](#list-comprehension-リスト内包表記)

`{}`の中に独自の記法で`for`や`if`を使って1行でdictオブジェクトを表現する方法。  
ループや条件分岐を使うため、手書きだと長くなってしまいそうなdictも手軽に生成できるのが特徴。

`for`のみを使った方法。  
冒頭に`n: n ** 2`のようにループ変数を使って`{key: value}`ペアを表現する。  
その後にスペース区切りで`for`文を記述する。

ループ変数が1つの場合。

```py
print({n: n ** 2 for n in range(5)})
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

ループ変数が2つの場合。  
iterableのネストを作り、内側のiterableの要素数が2 (変数の数と同じ) になるようにする。  
イメージとしては、`k, v = 0, 5`のようなtuple形式の代入文と同じような構造を作る。

```py
l = list(zip(range(3), range(5, 8)))
print(f'{l=}')
# l=[(0, 5), (1, 6), (2, 7)]

print({k: v for k, v in l})
# {0: 5, 1: 6, 2: 7}
```

ループ変数が2つの場合、その2。  
出発点がdict型の場合、`dict.items()`メソッドによってiterableの二重構造を作れる。

```py
d = {'one': 1, 'two': 2}
print(f'{d.items()=}')
# d.items()=dict_items([('one', 1), ('two', 2)])

print({k: v for k, v in d.items()})
# {'one': 1, 'two': 2}
```

`for`と`if`を組み合わせる場合。  
`if`文にマッチした場合のみループが実行される。

```py
age = dict(John=30, Mary=18, Elizabeth=20, May=25)

print({k: v for k, v in age.items() if v % 2 == 0})
# {'John': 30, 'Mary': 18, 'Elizabeth': 20}
```

以下のように、for文の中にif文をネストしたのと同じような構造。

```py
age = dict(John=30, Mary=18, Elizabeth=20, May=25)

d = dict()
for k,v in age.items():
    if v % 2 == 0:
        d[k] = v

print(d)
```

## while

* [Python Tutorial - An Informal Introduction to Python - #First Steps Towards Programming](https://docs.python.org/3/tutorial/introduction.html#first-steps-towards-programming)
* [The Python Language Reference - Compound statements - #while](https://docs.python.org/3/reference/compound_stmts.html#the-while-statement)

while文に指定した条件を満たす限りループし続ける。

※個人的には`for`の方が使う場面が多い

```py
i = 0
while i < 10:
    print(i)
    i += 1
# 0
# 1
# 2
# 3
# 4
# 5
# 6
# 7
# 8
# 9
```

* [The Python Language Reference - Simple statements - #continue](https://docs.python.org/3/reference/simple_stmts.html#continue)

`continue` statementにより、次のループに行く。  
`continue` statementは、`while` statementか`for` statementの内側でのみ使われる。  
※例がかなりアレだが無視してほしい...

```py
i = 0
while i < 5:
    if i % 2 == 0:
        i += 1
        continue
    print(i)
    i += 1
# 1
# 3
```

* [The Python Language Reference - Simple statements - #break](https://docs.python.org/3/reference/simple_stmts.html#break)

`break` statementを実行すると、ループ処理を中断する。
`break` も`continue`と同様に`for`か`while`の配下で使われる。

```py
i = 0
while True:
    print(i)
    if i == 2:
        break
    i += 1
# 0
# 1
# 2
```

## match

* [Python Tutorial - More Control Flow Tools - #match](https://docs.python.org/3/tutorial/controlflow.html#tut-match)

C言語でいうところのswitch文相当。  
match対象のオブジェクトの値によって処理を分岐させるときに使う。

`if`、`elif`でも同じことは表現できるが、matchの方が少ない行数でシンプルに書けることがある。  
ただし、後述の通りあまり複雑なマッチ条件は書けない。  
**非Sequenceのオブジェクトか、要素数が少なめのSequnceオブジェクトに対するmatchの利用にとどめたほうが良い気がする。**

match文は上から評価され、条件に一致した時点で内部の処理を実行して走査を終了する。

```py
x = 2

match x:
    case 1:
        print('one')
    case 2:
        print('two')
    case 3:
        print('three')
# two
```

```py
x = 4

match x:
    case 1:
        print('one')
    case 2:
        print('two')
    case 3:
        print('three')
# (出力なし)
```

末尾の`_`はワイルドカードとして動作し、全ての条件にマッチする。  
`raise` statementは例外を発生させる。

```py
x = 4

match x:
    case 1:
        print('one')
    case 2:
        print('two')
    case 3:
        print('three')
    case _:
        raise ValueError("x didn't match any of (1, 2, 3).")
# Traceback (most recent call last):
#   File "/home/stopendy/Documents/projects/python/python_study/test.py", line 11, in <module>
#     raise ValueError("x didn't match any of (1, 2, 3).")
# ValueError: x didn't match any of (1, 2, 3).
```

`|`でOR条件を作れる。

```py
x = 3

match x:
    case 1 | 2 | 3 | 4 | 5:
        print('x is any of 1 to 5')
# x is any of 1 to 5
```

`case`にブロック変数を入れて使うことも可能。  
ブロック変数にはマッチした値が入る。  
これだけだと意味は薄いが...。

```py
x = 3

match x:
    case a:
        print(a)
# 3
```

listやtupleの場合はパターンマッチングが可能。  
内部的には、`x, y = 1, 2`のようなunpacking assignmentのような動きをしているらしい。

`case (x, x, x)`のように、1つのパターンの中に同じ変数を複数回登場させることはできない。  
こういった場合は素直に`if`, `elif`を使ったほうが良さそう。

```py
t = (1, 2, 3)

match t:
    case (7, 7, 7):
        print("Three 7s!")
    case (1, x, y):
        print(f'Start with 1 followed by {x} and {y} ')
# Start with 1 followed by 2 and 3 
```
