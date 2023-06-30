# Iterable, Iterator, Generator

## iterable

* [Glossary - iterable](https://docs.python.org/3/glossary.html#term-iterable)
* [@IT - Python入門 - イテレータと反復可能オブジェクト](https://atmarkit.itmedia.co.jp/ait/articles/1906/11/news007_2.html#whatsiterator)

iterableとは、日本語でいうと「反復可能オブジェクト」である。  
iterableという単語は形容詞としても名詞としても使える。

「オブジェクトに含まれる要素を1つずつ取り出せるType」はiterableである。  
具体的には:

* subscriptingが可能なもの
  * 言い方を変えれば、`x[]`のように特定要素を取り出せるオブジェクト
  * 更に別の言い方をするなら、`__getitem__()`メソッドを持つオブジェクト
  * Sequence (str, list, tuple)
  * range
  * dict (keyを順に取り出す)
  * file (1行単位)
* iterator → [#iterator](#iterator)を参照

補足すると、`__getitem__()`は、subscriptingをするときに内部的に呼ばれるメソッド。  
→ [object.__getitem__(self, key)](https://docs.python.org/3/reference/datamodel.html?highlight=__getitem__#object.__getitem__)

```py
l = ['hi', 'hello', 'hey']
print(l[0])
# hi

print(l.__getitem__(0))
# hi
```

* [6_if_match_for_while_range.md](./6_if_match_for_while_range.md#for)

iterableは、`for`、`zip`、`map`などSequenceと同様の場面で使うことができる。  
特に`for`は内部的にiterableに対して`iter()`関数を実行することでiteratorを生成し、ループのたびに`next()`関数を実行している。  
そして`StopIteration`例外が発生するとループを停止する。

## iterator

* [Glossary - iterator](https://docs.python.org/3/glossary.html#term-iterator)
* [The Python Standard Library - Built-in Types - #Iterator Types](https://docs.python.org/3/library/stdtypes.html#iterator-types)

iteratorの特徴は2つ。  
特に重要なのは`next()`関数の挙動。

[iter()](https://docs.python.org/3/library/functions.html#iter)

* `iter(x)`
  * オブジェクト`x`のiterableを返す
  * iteratorに対して`iter()`を実行すると、自分自身を返す
  * 他Typeのオブジェクトを`iter()`に渡すと、新規のiteratorオブジェクトを返す
  * オブジェクト`x`は、iterator生成のために以下のいずれかの特徴を持つ必要がある (`x`にはiterableを渡すと考えて良いと思う)
    * `x.__iter__()`メソッドが定義されていること
    * `x.__getitem__()`メソッドが定義されており、要素が`0`から始まるシーケンス番号で指定できること
* `iter(x, sentinel)`
  * `x`はcallableである必要がある
  * `next()`が実行されるたびに、`x`が実行される
  * `x`の戻り値が`sentinel`と等しくなったときに`StopIteration`例外を返す

[next()](https://docs.python.org/3/library/functions.html#next)

* `next(iter[, default])`
  * iteratorオブジェクト`iter`の「次の値」を返す
  * 次の値が存在しないときは、`default`が指定されていれば`default`を返し、そうでなければ`StopIteration`例外を返す
  * 内部的には`iter.__next__()`メソッドを実行している

```py
l = ['hi', 'hey', 'howdy', "what's up"]
it = iter(l)

print(next(it))
# hi

print(next(it))
# hey

print(next(it))
# howdy

print(next(it))
# what's up

print(next(it))
# Traceback (most recent call last):
#   File "/home/stopendy/Documents/projects/python/python_study/test.py", line 8, in <module>
#     print(next(it))
# StopIteration
```

iteratorはiterableの一種。  
iterableが扱える場面では、多くの場合iteratorを代わりに使うことができる。

しかし、for文に対して使うときは例外がある。  
`for`は与えられたiterableに対して内部的に`iter()`を実行してiterableオブジェクトを生成する。

渡されたオブジェクトが`list`などiterator以外のオブジェクトの場合、ここで新規にiteratorオブジェクトが生成する。  

一方で、渡されたオブジェクトが`iter`型だった場合、「自分自身」が返る。  
つまり、新規にインスタンスを生成しない。

結果として、以下のような挙動差分が出る。

listをforに渡したときは、都度iterオブジェクトが生成するため、何度forを実行しても都度ループが回る。

```py
vegetables = ['lettuce', 'spinach', 'bell pepper']

for vegetable in vegetables:
    print(vegetable)
# lettuce
# spinach
# bell pepper

for vegetable in vegetables:
    print(vegetable)
# lettuce
# spinach
# bell pepper
```

iterをforに渡したときは、1度目のforでしかループが回らない。  
これは、1度目のforで渡したiterオブジェクトと2度目のforで渡したiterオブジェクトが全く同じであるためである。

```py
vegetables = ['lettuce', 'spinach', 'bell pepper']
it = iter(vegetables)

for vegetable in it:
    print(vegetable)
# lettuce
# spinach
# bell pepper

for vegetable in it:
    print(vegetable)
```

## generator

* [Glossary - generator](https://docs.python.org/3/glossary.html#term-generator)

## generator iterator

* [Glossary - generator iterator](https://docs.python.org/3/glossary.html#term-generator-iterator)

## asynchronous iterable

* [Glossary - asynchronous iterable](https://docs.python.org/3/glossary.html#term-asynchronous-iterable)

## asynchronous iterator

* [Glossary - asynchronous iterator](https://docs.python.org/3/glossary.html#term-asynchronous-iterator)

## asynchronous generator

* [Glossary - asynchronous generator](https://docs.python.org/3/glossary.html#term-asynchronous-generator)

## asynchronous generator iterator

* [Glossary - asynchronous generator iterator](https://docs.python.org/3/glossary.html#term-asynchronous-generator-iterator)
