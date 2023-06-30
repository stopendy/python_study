# BNF, EBNF

[@IT - BNF記法入門（1）](https://atmarkit.itmedia.co.jp/fxml/ddd/ddd004/ddd004-bnf.html)
[BNFとEBNFの復習](http://www.sakurai.comp.ae.keio.ac.jp/classes/DENDAI/2010/03BNFScanner.pdf)

前半は肩慣らしの一般論。  
楽に読めば良い。
暗記する必要はない。

**Python的に本当に重要なのは[#PythonのModified BNF](#PythonのModified-BNF)以降。**

## BNFとは

Buckus Naur Form (バッカス・ナウア・フォーム) の略。  
バッカス、ピーターという2人の人名から取った名前。

構文を記述するために使われている。  
今どきはBNFを拡張したEBNF (Extended BNF) が主流。  
EBNFとは全く形の異なるABNFというのもあるが、必要にならない限りはあまり触れない。

各種BNFについて特徴をまとめた。

* BNF
  * “Revised Report on the Algorithmic Language ALGOL 60”
  * BNFはあまり使われていなさそう
* EBNF
  * “ISO/IEC 14977:1996 Information technology -- Syntactic metalanguage -- Extended BNF”
  * EBNFには方言が多い
* ABNF
  * [RFC2234 “Augmented BNF for Syntax Specifications: ABNF”](https://www.ietf.org/rfc/rfc2234.txt)
  * EBNFとかなり異なる
  * 国際化も考慮されていない

**以降はEBNFに焦点を当てる。**

## EBNFの重要性

Pythonの公式ドキュメントでは、EBNFとPEPを混ぜた表現で構文が説明される ([参考](https://docs.python.org/3/reference/grammar.html))。

上記リンクをまともに理解する必要はないはずだが、それ以外にもちょくちょくEBNFが登場する...。

Python公式に限らず、jmespathなど他のライブラリでもたまにEBNFが登場するので、最低限の読み方はなんとなく知っておいて損はないはず。

## EBNFの読み方

* `A ::= B`は、「AとはBである」と読む
* `'a' | 'b'`は、「'a'または'b'」と読む

### 正の整数を表すEBNFを書いてみる

@ITの記事より。

前提として、以下を定める。

* 「数字」とは、0〜9のこと
* 「非ゼロ数字」とは1〜9のこと

```
数字 ::= '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
非ゼロ数字 ::= '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
```

「1桁の数字」、「2桁の数字」、「3桁の数字」を規定してみる。

```
1桁の正の整数 ::= 非ゼロ数字
2桁の正の整数 ::= 非ゼロ数字 数字
3桁の正の整数 ::= 非ゼロ数字 数字 数字
```

無限に続くと困るので、繰り返し構文を使う。  
`*`は「0回以上の繰り返し」を表す (正規表現と似てる)。

```
正整数 ::= 非ゼロ数字 数字*
```

正の整数には、先頭に`+`記号をつけてもつけなくても良いとする。  
`?`は、「直前のものがあってもなくても良い」を表す (正規表現と似てる)。

以下の2行は同じ意味になる。

```sh
# 正の整数 ::= '+' 非ゼロ数字 数字* | 非ゼロ数字 数字*
正整数 ::= '+'? 非ゼロ数字 数字*
```

### 整数を定義してみる

```
ゼロ ::= '0'
負整数 ::= '-' 非ゼロ数字 数字*
整数 ::= 正の整数 | ゼロ | 負の整数
```

## その他の記号

* `+`は、「1回以上の繰り返し」
* `#xN`は、文字の16進数ユニコード表記 (例: `#x20`)
* `-`は、範囲指定を表す記号
  * `[0-9]`: 0から9まで
  * `'a-e'`: aからeまで
  * `#x20-#xD7FF`: ユニコードの範囲
* 演算子の`-`は除外を表す
  * `整数 - '0'`は、'0'を除く全ての整数という意味

## PythonのModified BNF

[The Python Language Reference - Notation](https://docs.python.org/3/reference/introduction.html#notation)

* `::=`、`|`、`*`、`+`は上述と同じ
  * `::=`: (左辺)は(右辺)である
  * `|`: OR。(左辺)と(右辺)のどちらかが入る
  * `*`: 直前の文字列の0回以上の繰り返し
  * `+`: 直前の文字列の1回以上の繰り返し
* `[ ]`: 0回、または1回の繰り返し
* `( )`: グルーピングに使う
* `'a-z'`相当の表記は、Pythonでは`"a"..."z"`
* 複数行に渡る長大なルールは、行頭を`|`にして継続する

### Lexical Definition

超ベース部分の定義。  
例えば、以下のような内容。

"name" とは文字と`_`の集合である。  
文字とは`a`から`z`のことである。

```
name      ::=  lc_letter (lc_letter | "_")*
lc_letter ::=  "a"..."z"
```

Lexical Definitionは、[Lexical Analysis](https://docs.python.org/3/reference/lexical_analysis.html)のページのみで使われる。

Lexical DefinitionにおけるEBNFの各種シンボルは、通常通り「直前の1文字」を修飾する。

### Syntactic Definition

Lexical Definition以外の部分は全てSyntactic Definition。  
Lexical Definitionで定義した「トークン」 (キーワードのこと) を使って構文を説明する。

**Lexical Definitionとの最大の違いは、「キーワードを最小単位とする」ところ。**  
例えば、Syntactic Definitionの`expression*`は、「`expression`を0回以上繰り返す」という意味。  
「`n`を0回以上繰り返す」ではない。

上述の[Lexical Analysis](https://docs.python.org/3/reference/lexical_analysis.html)より後の全てのページは、Syntactic Definitionが使用されている。

## Lexical Analysis

[The Python Language Reference - Lexical Analysis](https://docs.python.org/3/reference/lexical_analysis.html)をかいつまんで理解してみる。

### Lexical AnalyzerとParser

Pythonの構文解析は2つのコンポーネントによって行われる。

まずLexical Analyzerによって、Pythonのソースコード文字列がTokenという塊ごとに認識される。  
そして、ParserがToken単位で構文解析して処理内容を認識する。

ここではLexical Analyzerに焦点を当てる。

### Tokenとは

Python的に意味のある文字列。  
分類するとこれだけある。

| トークン名             | 意味                                     |
| ---------------------- | ---------------------------------------- |
| NEWLINE                | Logical Lineの終点                       |
| INDENT                 | インデントを一段階増やす                 |
| DEDENT                 | インデントを一段階減らす                 |
| identifiers<br>(names) | オブジェクト名？先頭が数字以外の文字列。 |
| keywords               | 予約された名前。`def`や`False`など       |
| literals               |                                          |
| operators              |                                          |
| delimiters             |                                          |

トークンの切れ目が曖昧な場合は、長いトークン文字列を優先して解釈する。

### 特殊なクラス名

[Reserved classes of identifiers](https://docs.python.org/3/reference/lexical_analysis.html#reserved-classes-of-identifiers)より。

| クラス名のパターン | 意味                                       |
| ------------------ | ------------------------------------------ |
| `_*`               | `from module import *`でインポートされない |
| `__*__`            | System-defined names.<br>インタープリタが自動生成する名前 (※1)       |
| `__*`              | Class-private names.<br>名前の重複を避けるために使われる記法 (※2) |

<span style="font-size: 0.8em">(※1) 人間が手動で宣言すべきではない。現状存在するものは[Special method names](https://docs.python.org/3/reference/datamodel.html#specialnames)を参照</span>

<span style="font-size: 0.8em">(※2) クラス定義内のスコープのみで利用される。クラスが呼ばれた時、`_CLASS__*`に改名される。詳細は[Identifiers (Names)](https://docs.python.org/3/reference/expressions.html#atom-identifiers)の"private name mangling"を参照</span>

### 雑多な話

* Physical Linesは`LF`, `CR LF`, `CR`などによって分かれる
* 1つ、または複数のPhysical LinesがLogical Linesを作る
  * 行末に`\`を持つPhysical Lineは、Logical Lineの継続を意味する
  * `()`、`[]`、`{}`の間に改行文字が入った場合も、Logical Lineを継続する
