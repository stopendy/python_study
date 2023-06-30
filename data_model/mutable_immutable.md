# mutableとimmutable

[The Python Language Reference - Data model](https://docs.python.org/3/reference/datamodel.html#objects-values-and-types#:~:text=mutable)

作成済みのオブジェクトの値を後から変更できる場合、そのオブジェクトはmutableである。  
後から変更できない場合、そのオブジェクトはimmutableである。

## mutable

mutableなオブジェクトは、`id`を保ったまま中身を変えることができる。  
mutableなオブジェクトの例としては、listが挙げられる。

以下の例では、`id`を保ったまま様々な方法でlistの中身を変更している。  
※f-stringの使い方については、[#4_string_advanced](../4_string_advanced.md)を参照。

```py
l = [0, 1, 2]

print(f'{id(l)=}')
# id(l)=140569329139968

l[0] = 5

print(f'{l=}')
# l=[5, 1, 2]

print(f'{id(l)=}')
# id(l)=140569329139968

l.append(3)

print(f'{l=}')
# l=[5, 1, 2, 3]

print(f'{id(l)=}')
# id(l)=140569329139968
```

## immutable

numbers, strings, tuplesはimmutableである。  
例えば以下のようにそれぞれの型のオブジェクトを生成する。  
各オブジェクトは`id` (メモリアドレス) を持つ。

```py
print(id(1))
# 140274912166128

print(id('abc'))
# 140274911379952

print(id((1)))
# 140274912166128
```

上記のメモリアドレスを保ったまま、中身の値を変更することができないオブジェクトがimmutableである。  
各オブジェクトを変数に格納すると検証しやすい。

リテラルのままだと一度生成したオブジェクトを参照するのは難しいが、変数として名前をつければ同じオブジェクトを繰り返し参照できる。  
以下に例を示す。

```py
x = 1

print(id(x))
# 139680919994608

print(id(x))
# 139680919994608
```

オブジェクトが変わらない限り、`id`の値も同じである。  
ここでstrを使って実験を行う。

strの文字列オブジェクトは文字の集合体だが、1文字目だけを変更できるかを確認してみる。  
こうすると、strという文字列オブジェクト (つまりメモリアドレス) は保ちつつも、中身の文字 (値) だけは変えることができそう。

しかし、実際にこの操作を行おうとするとエラーになる。  
str型はimmutableなオブジェクトとしてクラスが定義されているので、1文字目に代入しようとすると「特定要素に対する代入操作はサポートされない」となる。  
これは「文字列の1文字目は絶対に代入できない仕様である」ということではなく、「strというクラスがそのように定義されている」である。  
strとよく似たmutableなクラスを自分で作成すれば、1文字目への代入もサポートされるはずである (つまり、同じ文字列オブジェクトのまま中身の文字を変更することも可能となる)。

```py
x = 'abc'
print(x[0])
# a

x[0] = 'A'
# Traceback (most recent call last):
#   File "/home/stopendy/Documents/projects/python/python_study/test.py", line 4, in <module>
#     x[0] = 'A'
# TypeError: 'str' object does not support item assignment
```

同じことはtupleについても言える。

```py
x = (0, 1)
x[0] = 6
# Traceback (most recent call last):
#   File "/home/stopendy/Documents/projects/python/python_study/test.py", line 2, in <module>
#     x[0] = 6
# TypeError: 'tuple' object does not support item assignment
```

immutableなオブジェクトの場合、オブジェクトの値を変えることができない。  
オブジェクトを特定するのは`id`の値、つまりメモリアドレスである。  
つまり言い換えると、「immutableなオブジェクトのメモリアドレス上に別の値を上書きすることはできない」ということである。

しかし、`x = 1`とした後に`x = 3`と変数の値を上書きすることは可能である。  
しかしこれは`1`というオブジェクトを書き換えたのではなく、変数`x`が`1`とは異なる`3`というオブジェクトを指し示すようになっただけである。  
この場合、`x`が指し示すオブジェクトが変わっているので当然`id`の値も変わる。

## mutableなオブジェクトの罠

[Container](https://docs.python.org/3/library/collections.abc.html#collections-abstract-base-classes) objectとは、list, tuple, dictのように複数の要素を持つようなオブジェクトのこと。

Containerオブジェクトの値は、要素として含む「オブジェクト」である。  
更に具体的に言うと、「オブジェクトの`id`」である。

`([1], 2)`というオブジェクトを考えてみる。  
メモリには`[1]`というlistオブジェクト、`2`というintオブジェクト、そして`([1], 2)`というtupleオブジェクトが格納されている。

ここでtupleオブジェクトは`[1]`と`2`という2つのオブジェクトそのものを格納しているわけではない。  
別のメモリアドレス上にある`[1]`と`2`のidを格納している...と自分の中では理解している。  
※出典はないが、そのように理解すると辻褄が合う  
※CPythonではオブジェクトのidとはメモリアドレスのこと。C言語でいうとポインタである

ここで`[1]`というlistは、mutableなのでオブジェクトidを保ちながら値を変えることができる。  
listがどう変化しようと、tupleはlistオブジェクトのidを参照し続けるのみである。  
**結果として、「listオブジェクトを変更するとtupleも追従して値が変わったように見える」という現象が起こる。**

これがmutableなオブジェクトの罠である。  
mutableなオブジェクトをContainerの子要素に含む場合、Containerがmutableなオブジェクトの変更に追従するので要注意である。  
Containerがmutableであるかimmutableであるかに関係なく、こういった挙動になる。

```py
l = [0]
t = (l, 2)

print(f'{t=}')
# t=([0], 2)

print(f'{id(t)=}')
# id(t)=139676908474432

print(f'{l=}')
# l=[0]

print(f'{id(l)=}')
# id(l)=139676909920128

l.append(1)

print(f'{t=}')
# t=([0, 1], 2)

print(f'{id(t)=}')
# id(t)=139676908474432

print(f'{l=}')
# l=[0, 1]

print(f'{id(l)=}')
# id(l)=139676909920128
```

上記の例は親要素がtupleである場合について検証したが、親要素がlistやdictであっても同じことが起こる。

ここで、tupleがimmutableであることを思い出してほしい。  
tupleのidを保ったまま、tupleの値を変えることはできない。

上記の例ではtupleが参照しているオブジェクトのidを変更していなかった。  
こういった方法であれば、tupleがimmutableであっても`print()`した時の表示は変わってしまう。  
直感的には「tupleの値が変わった」と言っても良いぐらいである。  
※しかし、今回の説明では「Containerの値とは要素のオブジェクトidである」という前提で議論を進めているので、「tupleの値は変わっていない」と表現する

[#immutable](#immutable)の再掲にはなるが、以下のようにtupleの要素のidが変わるような操作はエラーになる。

```py
t = (0, 1)
t[0] = 3
# Traceback (most recent call last):
#   File "/home/stopendy/Documents/projects/python/python_study/test.py", line 2, in <module>
#     t[0] = 3
# TypeError: 'tuple' object does not support item assignment
```

変化の追従を防止することで今回の罠を回避するには、「別のオブジェクトidでContainerに格納する」方法が有効である。  
具体的には、`copy`メソッドによってオブジェクトを複製する方法が有効である。

`list.copy()`は、listオブジェクトを複製した別のlistオブジェクトを返すメソッドである。  
一見すると`list`そのものと同じようにも見えるが、idが異なるので全くの別物である。

```py
l = [0]
t = (l.copy(), 2)

print(f'{t=}')
# t=([0], 2)

print(f'{l=}')
# l=[0]

l.append(1)

print(f'{t=}')
# t=([0], 2)

print(f'{l=}')
# l=[0, 1]
```

## mutable、immutableなオブジェクトの特徴

### 複数回参照したときに同じidを持つケースがある

[The Python Language Reference - Data model](https://docs.python.org/3/reference/datamodel.html#objects-values-and-types#:~:text=for%20immutable%20types,%20this%20is%20not%20allowed)

同じ型と値を持つimmutableなリテラルを複数回参照すると、それらのリテラルは同じオブジェクトを参照し、同じidを持つことがある。  
※クラスの実装によっては、同じidを持たないケースもある

```py
a = 1
b = 1

print(f'{id(a)=}')
# id(a)=140452541677808

print(f'{id(b)=}')
# id(b)=140452541677808
```

一方で、mutableなリテラルを複数回参照した場合、それらのリテラルは**絶対に**同じidを持たない。

```py
a = []
b = []

print(f'{id(a)=}')
# id(a)=140653378011392

print(f'{id(b)=}')
# id(b)=140653376962880
```

以下のようなアサイン方法の場合、2つの変数は同じmutable objectを参照する。

```py
a = b = []

print(f'{id(a)=}')
# id(a)=140672211943680

print(f'{id(b)=}')
# id(b)=140672211943680
```
