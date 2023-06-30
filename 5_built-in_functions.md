# 組み込み関数

[The Python Standard Library - Built-in Functions](https://docs.python.org/3/library/functions.html)

Built-in Functionsとは、`import`なしで使える関数のこと。[^1]

## print()

引数に渡されたオブジェクトを標準出力に出力する。

```py
print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)
```

## input()

[@IT - Python Tutorial - もう少し難しいHello Worldプログラム](https://atmarkit.itmedia.co.jp/ait/articles/1904/05/news021_2.html#threelinehello)

標準入力から受け取った文字列を返す。  
`print()`関数は、複数の引数を受け取るとスペース区切りで連結する。  
→ [The Python Standard Library - Built-in Functions - #print()](https://docs.python.org/3/library/functions.html#print)

```py
name = input('Input your name: ')
print('Hello', name)
# Input your name: endy
# Hello endy

num = input('Input a whole number to add to 3: ')
print('3 +', num, '=', 3 + int(num))
# Input a whole number to add to 3: 2
# 3 + 2 = 5
```

[^1]: [The Python Standard Library - Introduction](https://docs.python.org/3/library/intro.html#:~:text=The%20library,statement)
