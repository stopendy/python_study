# 関数

## 関数の定義、呼び出し

* [Python Tutorial - #Defining Functions](https://docs.python.org/3/tutorial/controlflow.html#defining-functions)
* [pass statements](https://docs.python.org/3/tutorial/controlflow.html#pass-statements)

関数は`def 関数名():`で定義し、`関数名()`で呼び出すことで使う。

関数定義の部分は`if`, `while`, `for`などと同様にインデントによって階層構造を表現する。

```py
def myfunction():
    print('aaa')


myfunction()
# aaa
```

引数を要求する関数。

```py
def myfunc(greet):
    print(greet)


myfunc('Hello')
# Hello
```

`return`の特徴。

* 関数定義の中で使う
* `return`が実行されると関数から抜ける
* `return`の後に渡したオブジェクトを返す
* `return`のみを実行すると`None`を返す。単純に関数の処理を中断させたいときに`return`を単体で使う

関数を呼び出すと、関数内の処理を実行した上で、関数呼び出しの部分が戻り値として評価される。  
つまり、`greet_to_given_name('John')`は、`'Hi, John.'`という文字列オブジェクトと同じ意味になる。  
文字列オブジェクトなので、当然ながら`print()`に渡しても`greet`という変数に代入しても文法上問題ない。

```py
def greet_to_given_name(name):
    return f'Hi, {name}.'


print(greet_to_given_name('John'))
# Hi, John.

greet = greet_to_given_name('Mary')
print(greet, "How are you doing?")
# Hi, Mary. How are you doing?
```

## pass statements

* [Python Tutorial - pass statements](https://docs.python.org/3/tutorial/controlflow.html#pass-statements)

関数の定義内に何も書かないときは`pass`と記述する。  
`pass`は以下の場面で有用である。

* 後で実装するけど、とりあえず備忘的に関数を定義しておきたいとき
* 空っぽの関数でもエラーにせず処理を継続させたいとき
* 実際に、空っぽの関数として実装したいとき (Abstract Base Classのメソッド定義の場合はこういった実装もあり得る気がする)

```py
def myfunc():
    pass


myfunc()
```

`pass`を書かずに空っぽの関数を作ろうとするとSyntax Errorになってしまう。

```py
def myfunc():


myfunc()
#     myfunc()
#     ^
# IndentationError: expected an indented block after function definition on line 1
```

## 引数にデフォルト値を持たせる

* [Python Tutorial - #Default Argument Values](https://docs.python.org/3/tutorial/controlflow.html#default-argument-values)

```py
def to_the_power_of_two(num=3):
    return num ** 2


print(to_the_power_of_two(5))
# 25

print(to_the_power_of_two())
# 9
```

デフォルト値は、関数宣言時の1回のみ評価される。  
デフォルト値がmutableなオブジェクトの場合、関数を複数回呼び出したときに同じオブジェクトが参照され続けてしまう。  
結果として、以下のような予想外な動きになる。

```py
def append_to_list(a, l = []):
    l.append(a)
    print(l)


append_to_list(3)
# [3]

append_to_list(4)
# [3, 4]

append_to_list(5)
# [3, 4, 5]
```

上記のような事象を回避するには、関数の初期値を一旦immutableな値とし、関数内部で再び初期化すれば良い。  
例えば以下のようなコードであれば、関数呼び出しのたびに新規インスタンスが発行されるようになる。

```py
def append_to_list(a, l=None):
    if l is None:
        l = []

    l.append(a)
    print(l)


append_to_list(3)
# 3

append_to_list(4)
# 4

append_to_list(5)
# 5
```

## keyword arguments

* [Python tutorial - keyword arguments](https://docs.python.org/3/tutorial/controlflow.html#keyword-arguments)

関数を呼び出す際、変数の順番を意識せずに定義時の変数名を使って引数を指定できる。  
(keyword argument)

以下のような場面でkeyword argumentsという指定方法が役に立つ想定。

* デフォルト値を持つ関係で全ての引数を指定する必要がないとき
* 引数の順序を思い出しづらいとき
* 引数が多いとき
* 任意個数の引数を取りうるとき → 詳細は[#特殊なパラメータ指定](#特殊なパラメータ指定)を参照

```py
def f(a, b=10, c=100):
    print(a, b, c)


f(a=1, c=300)
# 1 10 300
```

引数の対応関係を順序で表現するpositional argumentと、今回のkeyword argumentは組み合わせて使うことができる。

```py
def f(a, b, c):
    print(a, b, c)


f(1, c=3, b=2)
# 1 2 3
```

ただし、keyword argumentを1度指定したら、以降の引数は全てkeyword argumentで指定する必要がある。  
keyword argumentの後にpositional argumentを指定するのはNG。  
これはpositional argumentが何番目を表すかわかりづらくなるのを防ぐためと思われる。

```py
def f(a, b, c):
    print(a, b, c)


f(a=1, 2, 3)
# SyntaxError: positional argument follows keyword argument
```

## unpackingを利用した引数指定

### Sequenceのunpacking

* [Python tutorial - Unpacking Argument Lists](https://docs.python.org/3/tutorial/controlflow.html?highlight=unpack#unpacking-argument-lists)

Sequence (list, tuple, strなど) の手前に`*`をつけると、unpackingになる。  
unpackingとは、「ソースコード上で外側のカッコを外す」イメージである。

引数の情報をSequence型の変数として格納し、関数に対してその変数をunpackingして渡すという使い方が一般的である。

以下の例において、`f(*t)`は`f(1, 2, 3)`と同じ意味である。

```py
def f(a, b, c):
    print(a, b, c)


t = (1, 2, 3)
f(*t)
# 1 2 3
```

`*`とSequenceの間にはスペースを入れないのが一般的だが、スペースを入れても動作はする。

```py
def f(a, b, c):
    print(a, b, c)


t = (1, 2, 3)
f(* t)
# 1 2 3
```

### Mappingのunpacking

Mapping (dictなど) 型の変数の手前に`**`をつけると、`key1=value1, key2=value2`という文言に置き換えられる。  
これもSequenceのunpackingと同様に、unpackによってオブジェクトを生成するのではなく、単にソースコード上の文言としてそのまま解釈される。

このunpackingは、関数やメソッドなどを呼び出す際のkeyword argumentsとして使われる。  
dict型のオブジェクトにkeyword argumentsを格納し、そのオブジェクトを関数に渡すことで多数のパラメータを手軽に関数に渡すことができる。

```py
def f(greeting, name, apple_counts):
    print(f"{greeting} {name}. You've got {apple_counts} apples.")


d = dict(greeting='Hi', name='Jessy', apple_counts=5)

print(f'{d=}')
# d={'greeting': 'Hi', 'name': 'Jessy', 'apple_counts': 5}

f(**d)
# Hi Jessy. You've got 5 apples.
```

## 特殊パラメータ

* [Python tutorial - #Special parameters](https://docs.python.org/3/tutorial/controlflow.html#special-parameters)

パラメータと引数という用語は、以下のように区別している。

* [parameter](https://docs.python.org/3/glossary.html#term-parameter)
  * 関数定義の`()`内に指定するもの
* [argument](https://docs.python.org/3/glossary.html#term-argument)
  * 関数呼び出しの`()`内に指定するもの

ここでは、関数定義におけるパラメータ指定で使う特殊な記号を扱う。  
扱う記号は以下の通り。

* `/`
* `*`

要点を示す。  
以下のような関数定義があったとする。

```py
def f(a, /, b, *, c):
    pass
```

このとき、各変数は以下の意味を持つ。

* `a`: positional argumentでしか指定できない (`/`より左にあるため)
* `b`: positional argument、keyword argumentのどちらでも指定できる
* `c`: keyword argumentでしか指定できない (`*`よりも右にあるため)

* [Python tutorial - #recap](https://docs.python.org/3/tutorial/controlflow.html#recap)

使い方として、公式では以下のガイドラインが示されている。

* `/`を使う場面
  * 変数名に意味がなくて順序が大事であることを示したいとき
  * `/`のあとに任意のkeyword argumentを取る構成で、positional argumentと変数名の衝突が起こらないようにしたいとき
  * APIの場合など、positional argumentを強制することで、変数名を変えてもAPI利用元が壊れないことを保障したいとき
* `*`を使う場面
  * 関数のユーザーに対し、引数の順序に依存してほしくないとき

### / (forward-slash)

`/`より左に指定されたパラメータは、「positional argumentでしか指定できない」という意味になる。

`/`は特殊パラメータであるため、引数としては数えられない。  
例えば`f(a, /, b):`という関数定義があった場合、positional argumentで指定するなら第一引数は`a`と、第二引数は`b`と対応する。  
2番目に`/`が存在しているが、`/`は数えない。

```py
def f(a, /, b):
    print(a)
    print(b)


f(1, 2)
# 1
# 2

f(a=1, b=2)
# TypeError: f() got some positional-only arguments passed as keyword arguments: 'a'
```

### * (asterisk)

`*`より右のパラメータはkeyword argumentでしか指定できなくなる。

```py
def f(a, b, *, c):
    print(a)
    print(b)
    print(c)


f(1, 2, c=5)
# 1
# 2
# 5

f(1, 2, 3)
# TypeError: f() takes 2 positional arguments but 3 were given
```

`*`も特殊パラメータという扱い。  
positional argumentの観点では「そもそもカウントされない」。  
`*arg`などとは異なり「任意個数の引数」という意味ではないので、注意が必要。

```py
def f(a, *, b):
    pass


f(1, 2, 3)
# TypeError: f() takes 1 positional argument but 3 were given
```

## 任意数の引数を受け取るパラメータ

### *args

* [Arbitrary Argument Lists](https://docs.python.org/3/tutorial/controlflow.html#arbitrary-argument-lists)

`*args`のように、`*`で始まる変数名を関数定義のパラメータとして指定すると、tupleとして扱われる。

```py
def f(a, *x):
    print(a)
    print(x)


f(1, 2, 3, 4, 5, 6)
# 1
# (2, 3, 4, 5, 6)
```

`*args`は任意個数のpositional argumentsと紐づくため、`*args`より右に定義されたパラメータはkeyword argumentsでしか指定できなくなる。  
普通、関数定義するときは`*args`のようなパラメータをなるべく右の方に持ってくる。  
(次セクションの`**kwargs`は例外で、一番右に来ることが多いが)

```py
def f(*x, a=0):
    print(x)
    print(a)


f(1, 2, 3)
# (1, 2, 3)
# 0

f(1, 2, a=3)
# (1, 2)
# 3
```

### **kwargs

`**kwargs`は任意個数のkeyword argumentsと紐づく。  
指定されたkeyword argumentsは、関数内では`dict`と見なされる。

```py
# def f(a, **x):
#     print(a)
#     print(x)


# f(1, b=2, c=3, d=4)
```

`**kwargs`より右にはパラメータを指定できない。  
仮に指定できたとしても、`**kwargs`より右のパラメータに引数を渡すことができないのでナンセンスである。

```py
def f(a, **x, y):
    pass

# def f(a, **x, y):
#               ^
# SyntaxError: invalid syntax
```

## Documentation strings (docstring)

* [Python Tutorial - #Documentation Strings](https://docs.python.org/3/tutorial/controlflow.html#documentation-strings)
* [@IT - Python入門 - docstringの書き方](https://atmarkit.itmedia.co.jp/ait/articles/1912/06/news025.html)

関数定義の冒頭に配置した文字列は、関数オブジェクトの`__doc__`属性に自動的に代入される。  
`__doc__`の値は`help(関数名)`で表示できる。

ここには関数の説明を書く。

```py
def myfunc():
    """Returns None.
    """
    return


print(myfunc())
# None

print(myfunc.__doc__)
# Returns None.
```

docstringは、`help()`関数によって表示される関数の説明文にも反映される。

```py
def add(num1, num2):
    """Returns num1 + num2

    Args:
        num1: an int/float number to add to num2.
        num2: an int/float number to add to num1.
    """
    return num1 + num2


help(add)
# add(num1, num2)
#     Returns num1 + num2
    
#     Args:
#         num1: an int/float number to add to num2.
#         num2: an int/float number to add to num1.
```

docstringの書きっぷりについては、いくつかガイドラインがある。

* わかりやすい情報
  * [@IT - PEP 8とPEP 257で定められているdocstringの形式](https://atmarkit.itmedia.co.jp/ait/articles/1912/06/news025.html#docstringstyles)
* 公式情報
  * [PEP 8](https://peps.python.org/pep-0008/)
  * [PEP 257](https://peps.python.org/pep-0257/)
* より具体的な団体ごとのルール
  * [Google Python Style Guide - #Comments and Docstrings](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings)
  * [numpydoc - Style guide](https://numpydoc.readthedocs.io/en/latest/format.html)

最低限、以下は意識したい。

* `""" """`で囲う
* 1行、または複数行で書く
* 1行目はサマリを書く
* 1行目と他行の間には空行を1つ入れる

## Annotation

* [Python tutorial - #Function Annotations](https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions)
* [PEP 3107](https://peps.python.org/pep-3107/)
* [PEP 484](https://peps.python.org/pep-0484/)

Annotationは、関数に付与するメタデータの一種。  
関数の使い方に関する情報を補足する効果があるが、関数の動作そのものには一切影響を与えない。

Annotationは、以下2通りの記号を使って表現できる。

* `: 数式` → 関数のパラメータについて補足する。パラメータ名の後に記述する
* `-> 数式` → 関数の戻り値について補足する。関数定義の`)`の後に記述する

以下にサンプルを示す。

以下のサンプルでは、関数`f`に3つのannotationを付与している。

* パラメータ`b`は`int`型を期待している
* パラメータ`c`は`str`型を期待している
* 戻り値は`None`型を期待している

Pythonは型を宣言することなく変数を使えるが、上記のようにannotationをつけることで関数の意味をわかりやすくできる。  
必須ではないが、マニュアルやdocstringで長々と書くよりは簡潔でわかりやすい印象がある。

```py
def f(a, b: int, c: str='hi') -> None:
    return None


help(f)
# f(a, b: int, c: str = 'hi') -> None
```

annotationの情報を無視しても別にエラーにはならない。

```py
def f(a: int, b: str) -> str:
    print(a)
    print(b)
    return


f('Hi', 5)
# Hi
# 5
```

「コーディング規約に基づかない書き方に対して警告を出す」ようなエディタの拡張機能を利用している場合、annotationをつけることで早期にエラーに気付けるというメリットがある。  
annotationをつけることで、「実行は通るけど挙動はおかしい」コードを作ってしまうのを防げる...と思う。

例えばVS CodeのPython拡張機能を導入しつつ、mypyの利用を有効化しているとannotationとの矛盾を検知し、赤い下線で警告してくれる。

また、VS Codeの`Ctrl+Space`でIntellisenseを呼び出したとき、自作関数の説明がリッチになるというメリットもある。  
※これはdocstringも同様である

Syntaxの観点で最後に一点補足する。  
`:`と`->`の後に続くものはExpression (式) なので、演算を伴う式や、Type以外のオブジェクトを入れても動作する。  
意味はない気がするが。  
PEPを読めばより高度な使い方に言及されているかもしれないが、PEPの本文が長いので今はスルーしておく...。

```py
def f(a: 5, b: 3 > 2) -> max(9, 3):
    print(a)
    print(b)
    return


help(f)
# f(a: 5, b: True) -> 9
```

## Lambda expressions

* [Python tutorial - #Lambda Expressions](https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions)

lambdaとは、1行で無名の関数オブジェクトを生成する表現。  
`lambda パラメータ名: 戻り値`という表記で記述する。

関数オブジェクトは、lambdaではない通常の形式では「関数名」で参照できるもの。

```py
def f():
    pass


print(f)
# <function f at 0x7f0d72163d90>
```

lambdaだと以下のような表記になる。  
`gen_func()`関数が返すものが`lambda`が返す関数オブジェクトである。  
この関数オブジェクトを変数`f`に代入する (名前`f`に参照させる)。  
関数を実行するには、`f()`のように関数オブジェクトをCallする必要がある。  
※Callするとは、`()`をつけること

```py
def gen_func():
    return lambda x: x + 1

f = gen_func()
print(f(5))
```

`lambda`は複数変数を受け付けることも可能。

```py
f = lambda a, b: a + b

print(f(1, 2))
# 3
```

簡単な関数を簡潔に表現したいとき、lambdaを使うと便利そう。  
または、`map()`関数や`list.sort()`のように関数オブジェクトを必要とする場面において「一度きり」の関数を手軽に生成するのに便利かもしれない。  

`map()`は、第一引数に関数オブジェクト、第二引数にiterableを取る。  
iterableの各要素を引数に関数を実行し、その戻り値を参照するiteratorを返す。  
[map()](https://docs.python.org/3/library/functions.html#map)

以下の例では、lambdaによって「引数として文字列を受け取り、先頭を大文字にして末尾に`' there'`を付け加えた文字列を返す関数オブジェクト」を生成している。  
その関数をtuple `t`の各要素に対して実行する。  
その戻り値をiterableとして返している。

iterableはそのままだと「mapオブジェクト」となってしまい`print()`で表示できないため、`list()`によってリスト型に変換した。

```py
t = ('hi', 'hello')
l = list(map(lambda s: s.capitalize() + ' there.', t))

print(l)
# ['Hi there.', 'Hello there.']
```

`list.sort()`を利用している。  
`list.sort()`は、listの要素を`<`演算子によってソートする。  
`key=`パラメータに関数オブジェクトを渡すと、ソート前にlistの各要素に対して関数を実行する。  
[list.sort()](https://docs.python.org/3/library/stdtypes.html#list.sort)

以下の例では、lambdaで生成した関数を`list.sort()`の`key`に指定した。  
`lambda`は、dictから特定のkeyを取り出す関数を作っている。

1度目の`scores.sort()`では、リストの各要素のdictから`Math`というkeyに対応するvalueを取り出してから逆順にソートした。  
これにより、数学の点数が高い順にソートした。  
2度目の`scores.sort()`では、`English`について同様のソートを行った。

```py
scores = [dict(Math=100, English=50), dict(Math=30, English=70), dict(Math=80, English=100)]

scores.sort(key=lambda d: d['Math'], reverse=True)
print(scores)
# [{'Math': 100, 'English': 50}, {'Math': 80, 'English': 100}, {'Math': 30, 'English': 70}]

scores.sort(key=lambda d: d['English'], reverse=True)
print(scores)
# [{'Math': 80, 'English': 100}, {'Math': 30, 'English': 70}, {'Math': 100, 'English': 50}]
```
