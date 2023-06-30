# 数値リテラル (応用編)

## 数値リテラルの分類

[The Python Language Reference - Lexical analysis - #Numeric literals](https://docs.python.org/3/reference/lexical_analysis.html#numeric-literals)

数値リテラルは、以下の3種類のみ。

* 整数
* 小数
* 虚数

(以下は豆知識)

例えば複素数は、`実数 + 虚数`という2つのリテラルと1つの演算子の組み合わせで表現される。

またリテラルには`+`や`-`などの記号はつかない。  
`-1`は、`-`という演算子と`1`というリテラルの組み合わせ。

## literalの表記

### int literalの表記

[The Python Language Reference - Lexical analysis - #Integer literals](https://docs.python.org/3/reference/lexical_analysis.html#integer-literals)

Integer literalは、数字と数字の間に`_`を挿入しても意味は変わらない。  
3桁ごとに入れると見やすくなる...かも？

```py
print(1_000_000)
# 1000000

print(1_2_3)
# 123
```

2進数、8進数、16進数表記。  
アルファベット部分は`B`, `O`, `X`のように大文字でも同じ意味になる。

```py
print(0b1010)
# 10

print(0o73)
# 59

print(0xa5)
# 165

print(0xF)
# 15
```

### float literalの表記

* [The Python Language Reference - Lexical analysis - #Floating point literals](https://docs.python.org/3/reference/lexical_analysis.html#floating-point-literals)

数字にピリオドがつくとfloatになる。  
`3.0`だけでなく、`3.`のようにピリオドで終わっても良い。

```py
print(5.0)
# 5.0

print(5.)
# 5.0
```

`6.02e23`のように、10のN乗を表現できる。  
`e`の代わりに`E`を使っても同じ。

このようなexponential表記をすると必ずfloat型になるので注意。

`e`の後には負の数が来ても良い。

```py
print(6.02e23)
# 6.02e+23

print(314e-2)
# 3.14

print(5e2)
# 500.0
```
