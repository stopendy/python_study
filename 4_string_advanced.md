# 文字列 (応用編)

## プレフィックス付きのリテラル

[The Python Language Reference - String and Bytes literals](https://docs.python.org/3/reference/lexical_analysis.html#string-and-bytes-literals)

文字列リテラルの手前に`f' '`のようにアルファベット一文字のプレフィックスをつけることで、特殊な意味をもたせることができる。

プレフィックスは大文字でも小文字でも同じ意味になる。  
例えば、`f' '`と`F' '`は同じ。

### f-string (Formatted String)

Python 3.6以降よりサポート。

文字列をフォーマットする方法はいくつかあるが、2022年時点では最も新しくてわかりやすく、可読性も高い書き方。

以下のように記述する。  

* `f' '`
* `f" "`
* `F' '`
* `F" "`

f-stringの中で`{ }`に囲われた部分は変数置換の対象となる。

```py
name = 'endy'
print(f"I'm {name}.")
# I'm endy.
```

構文上の例外は一部あるものの、`{ }`で囲われた部分は通常のPythonコードとほぼ同様に動作する。  
もちろん演算子も使える。

```py
num = 5
print(f"I'm {num} years old.")
# I'm 5 years old.

print(f"I'll be {num + 1} years old next year.")
```

#### 参考

置換部分の末尾に`=`、`!`、`:`を加えることで特殊な意味を加えることができる。  
`=`と`:`は知っておくと便利そうな印象を持った。

`=`をつけると、`変数=値`の形式に置換される (Python 3.8以降)。  
`repr()`関数のように、型がわかりやすいように値部分を修飾する。  
デバッグに役立つ ([repr](https://docs.python.org/3/library/functions.html#repr))。

(※) `repr()`は、引数に渡したオブジェクトに型修飾をつけた文字列オブジェクトを返す

```py
name='endy'
print(f'{name=}')
# name='endy'
```

`!s`、`!r`、`!a`で型変換系の関数を呼ぶ。  
ただ、`!s`は明らかに意味がないし、`!r`や`!a`にしても素直に`repr()`と`ascii()`を呼べば良い。  
したがって、これらの構文は基本使わない。  
これらの表記が存在する理由は、`format()`メソッドを使っていた頃との互換性のため。  
([参考: PEP498](https://peps.python.org/pep-0498/#s-r-and-a-are-redundant))

* `!s`: `str()`によって文字列型に変換する([str](https://docs.python.org/3/library/stdtypes.html#str))
* `!r`: `repr()`によって、型修飾を伴う文字列型に変換する ([repr](https://docs.python.org/3/library/functions.html#repr))
* `!a`: `ascii()`を呼ぶ。`ascii()`は基本`repr()`と同様だが、ASCII文字で表現できない文字を`\x`, `\u`, `\U`によってエスケープする

`!r`のみ例を示す。

```py
name = 'endy'
print(f"I'm {name}.")
# I'm endy.

print(f"I'm {name!r}.")
# I'm 'endy'.

print(f"I'm {repr(name)}.")
# I'm 'endy'.
```

`:`以降にフォーマット指定子 (format specifier) を記述することで、「右揃え」や「ゼロ埋め」のような書きっぷり変更ができる。  
([参考: Format Specification Mini-Language](https://docs.python.org/3/library/string.html#formatspec))

以下に例を示す。

2つ目の例は、字幅を`4`字分確保しつつ、`右揃え`で変数`num`を表示している。  
結果として、`num`の左3字分にスペースが入った。

```py
num = 5
print(f'No.{num}')
# No.5

print(f'No.{num:>4}')
# No.   5
```

### raw string

`r' '`のように記述すると、バックスラッシュ (`\`) がただの文字になる。

```py
print("Hey.\nWhat's up?")
# Hey.
# What's up?

print(r"Hey.\nWhat's up?")
# Hey.\nWhat's up?
```

ただし、raw stringにおいても`\`はクォーテーションをエスケープしてしまう。  
そして、エスケープ後の文字列として`\`は残り続ける。  
具体例を2つ示す。

`r'\'`は、構文上成立しない。  
`\`が2つ目のシングルクォーテーションをエスケープしてしまうため、クォーテーションが閉じていないと認識されてしまうためである。

```py
print(r'\')
# SyntaxError: unterminated string literal (detected at line 1)
```

`r'\''`は成立する。  
raw stringの働きによって`\`も含めて文字列と見なされるため、`\'`という文字列として認識される。

```py
print(r'\'')
# \'

print('\'')
# '
```

Pythonが認識するエスケープシーケンスは、[The Python Language References - String and Bytes literals](https://docs.python.org/3/reference/lexical_analysis.html#string-and-bytes-literals)の表を参照。

### Bytes Literal

#### bytes型の概要

* [The Python Language Reference - Data model - The standard type hierarchy](https://docs.python.org/3/reference/datamodel.html#the-standard-type-hierarchy) → Bytes
* [bytes()](https://docs.python.org/3/library/stdtypes.html#bytes)

`b' '`はbytes型のリテラルを生成する。  
bytes型は、内部的には8-bitの整数値 (0〜127) が連続したデータ構造。  
この整数値はUnicode code pointである。

bytes型のstring representationは、str型とよく似ている。  
Syntaxや演算の特性もstrとほぼ同様である。

```py
print(b'test')
# b'test'
```

bytes型で表示可能な文字列はASCIIのみ。  
つまりUnicode code pointでいうと10進数で0〜127までで、これは英数字と一部の記号のみに相当する。  
[(参考) ASCIIは1Byte、UTF8は4Byte。UTF8の若番はASCIIと同じ値に揃えてある](https://qiita.com/heeroo_ymsw/items/c6e15d5f9246b4e842eb)

```py
print(b'あいう')
# SyntaxError: bytes can only contain ASCII literal characters
```

他の文字はUnicode Literalでエスケープする必要がある。  
[#Unicode Literal](#unicode-literal)

エスケープしたいときは`ascii()`関数が恐らく便利。  
以下の例では`ascii()`が最初と最後につけるクォーテーションをSliceによって除去しているが、もっといいやり方があるかもしれない。

以下の通り、`bytes()`に渡すと`\`がエスケープされて`\\`になる。  
bytes型の仕様と思われる。

```py
print(bytes(ascii('えんでぃ')[1:-1], 'utf-8'))
# b'\\u3048\\u3093\\u3067\\u3043'
```

このエスケープを元に戻しつつ通常の文字列として表示するには、コツがいる。  
具体的には、`decode('unicode_escape')`でデコードすることで、エスケープシーケンスを再認識させる必要がある。

単にbytes型をデコードして文字列に戻しただけでは、`\u3048\u3093\u3067\u3043`という長い文字列が得られるだけである。  
リテラルで`'\u3048\u3093\u3067\u3043'`を指定したときは、エスケープシーケンスを解釈してcode pointに対応するUnicode文字列 (4字) を文字列オブジェクトの値として格納してくれるが、これは`\u3048\u3093\u3067\u3043`という24字の文字列とは別物である。

詳細は、以下のURLが参考になる。

* [bytes.decode()](https://docs.python.org/3/library/stdtypes.html#bytes.decode)
* [codecs - #Text Encodings](https://docs.python.org/3/library/codecs.html#text-encodings)
* [stack overflow - How to un-escape a backslash-escaped string?](https://stackoverflow.com/a/57192592/14387327)

```py
s1 = b'\\u3048\\u3093\\u3067\\u3043'.decode('utf-8')
s2 = b'\\u3048\\u3093\\u3067\\u3043'.decode('unicode_escape')

print(s1)
# \u3048\u3093\u3067\u3043

print(s2)
# えんでぃ
```

#### bytes() constructor

`bytes()`クラス呼び出しによってもbytesインスタンスを生成できる。  
[bytes()](https://docs.python.org/3/library/stdtypes.html#bytes)

`bytes()`には整数やイテラブルを渡すことができる。  
整数を渡すと、`0`を格納したUnicode code pointを指定した文字数だけ生成する。  
イテラブルを渡すと、イテラブルに含む整数をbytesオブジェクトに変換する。

```py
print(bytes(10))
# b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'

print(bytes(range(5)))
# b'\x00\x01\x02\x03\x04'
```

または、strオブジェクトを渡すこともできる。  
strオブジェクトを渡す場合は、第二引数のencodingの指定も必要となる。  
基本的にはPythonのデフォルトエンコーディング方式である`utf-8`を指定する。  
他の指定可能なエンコーディング方式は、以下を参照。  
[Standard Encodings](https://docs.python.org/3/library/codecs.html#standard-encodings)

また、strオブジェクトを渡す場合はUnicode code pointが0〜127のものしか指定できない (bytesリテラルの制限)。  
この書き方はあまりしない気がする。  
`b'xxx'`のリテラル表記や、[str.encode()](https://docs.python.org/3/library/stdtypes.html#str.encode)で十分事足りるためである。

```py
print(bytes('aaa', 'utf-8'))
# b'aaa'

print(b'aaa')
# b'aaa'

s = 'aaa'

print(s.encode('utf-8'))
# b'aaa'
```

### Unicode Literal

文字列リテラルでUnicodeを表記するには、主に3通りの表記がある。

* `\N{name}`: Unicodeの名前
* `\u0000`: 4桁の16進数 (ちょうど4桁である必要がある)
* `\U00000000`: 8桁の16進数 (ちょうど8桁である必要がある。桁数が多いのですべてのUnicodeを表現できる)

2番目、または3番目のパターンをよく使うイメージがある。

1番目の名前を使ったUnicode表記は正直扱いが難しい。  
名前は以下の資料から調べた。

* [Unicode® Character Name Index](https://www.unicode.org/charts/charindex.html)
  * Greek small letters → [Greek and Coptic](https://www.unicode.org/charts/PDF/U0370.pdf)
* 名前は大文字表記が正式だが、小文字が混ざっていても良い

```py
print('\N{GREEK SMALL LETTER BETA}')
# β

print('\u0041')
# A

print('\u3042')
# あ

print('\U00003042')
# あ
```

エスケープシーケンスに分類されるが、以下のユニコード表記も可能。  
[String and Bytes literals](https://docs.python.org/3/reference/lexical_analysis.html#string-and-bytes-literals)

* `\x00`: 2桁の16進数によるユニコード表記 (ちょうど2桁である必要がある)
* `\000`: 1〜3桁の8進数によるユニコード表記

```py
print('\x41')
# A

print('\101')
# A
```

#### Unicode Literal関連の関数

Unicode Literalと文字列を相互に変換する関数が3種類ある。

`ord()`と`chr()`は1文字にしか使えないが、`ascii()`は複数文字に使える。  
`ord()`と`chr()`はそれぞれ逆向きの変換をする関係にある。  
`ascii()`の逆向き変換を行う際は、上述の通り`decode('unicode_escape')`を使うのが良さそう。

各関数の詳細を以下に説明する。

`ord()`は、引数として渡した文字列のUnicode code point (Unicode符号点) を返す。  
引数には一文字しか渡せない。  
返される値は16進数表記なので注意。  
例えば、`'A'`のcode pointは`0x41`である。  
[ord()](https://docs.python.org/3/library/functions.html#ord)

```py
print(ord('A'))
# 41

print(ord('あ'))
# 12354

print(hex(ord('A')))
# 0x41

print(hex(ord('あ')))
# 0x3042
```

`chr()`は引数として渡した整数値のUnicode code pointに対応する文字列を返す。  
`ord()`の逆。  
[chr()](https://docs.python.org/3/library/functions.html#chr)

```py
print(chr(0x41))
# A
```

`ascii()`は、`repr()`のようなstring representationを返す。  
文字列型なので、文字列型リテラルに近いように`"'xxx'"`とクォーテーションで囲まれた文字列を返す。  
ASCII (code pointが0〜127のもの) に含まれない文字列は、`\uxxxx`や`\Uxxxxxxxx`でエスケープされる。

以下の例では半角スペース、`=`、`apple`などはASCIIに含まれるのでそのままだが、日本語の文字列はすべてエスケープされている。

```py
print(ascii('りんご = apple'))
# '\u308a\u3093\u3054 = apple'
```

`ascii()`は複数文字も受け取れるので、`ord()`とは違ってループすることなく処理できるのが便利である。  
しかしstring representationであることが邪魔になることもあるので、そんなときはsliceで先頭と末尾の`'`を削ってしまえば良い。

```py
print(ascii('りんご = apple')[1:-1])
# \u308a\u3093\u3054 = apple
```

### 組み合わせ

`rf`、`fr`、`rb`、`br`という組み合わせも存在する。  
他の組み合わせは今のところ無い。

## 文字列の連結

[The Python Language Reference - Lexical analysis - #String literal concatenation](https://docs.python.org/3/reference/lexical_analysis.html#string-literal-concatenation)

文字列を足し算で連結できることは既に触れた。

```py
print('Hello' + 'World')
# HelloWorld
```

文字列リテラルを並べることでも連結できる。  
間に入ったホワイトスペースやコメントは無視される。

```py
print(
    'Hi. '   # Greeting
    "Let's get going!"  # A sentence with apostrophes

    """
    Where to go?
    I don't know.
    """
)
# Hi. Let's get going!
#     Where to go?
#     I don't know.
```
