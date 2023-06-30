# 組み込み関数

[The Python Standard Library - Built-in Functions](https://docs.python.org/3/library/functions.html)

## abs(x)

* [abs()](https://docs.python.org/3/library/functions.html#abs)

数値オブジェクトの絶対値を返す。

```py
print(abs(-4))
# 4

print(abs(2))
# 2
```

## aiter(async_iterable)

* [aiter()](https://docs.python.org/3/library/functions.html#aiter)

Asynchronous iterableを渡すと、Asynchronous iteratorを返す。  
オブジェクト`x`を渡したとき、`x.__aiter__()`を実行するのと同等。

**★ここで一旦ストップ。Tutorialでiteratorまで復習したい。**
