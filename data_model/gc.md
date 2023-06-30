# Garbage Collection

[The Python Language Reference - Data Model](https://docs.python.org/3/reference/datamodel.html#objects-values-and-types#:~:text=Objects%20are%20never%20explicitly%20destroyed)

## Pythonのgcの実装

Pythonのgcが行われるタイミングは明確に決まっていない。

オブジェクトを参照できなくなった時、gcが行われることがある。  
gcは実装によって延期したり無効化したりできる。  
[gc](https://docs.python.org/3/library/gc.html#module-gc)モジュールによってこの挙動を制御することもできる。

CPythonの実装は変わることがありうる。  
したがって、オブジェクトのリソース開放はgcに依存してはならない。  
特にファイルのような外部リソースにアクセスする場合は、必ず`close()`のような操作で明示的に閉じる必要がある。

`close()`漏れを防ぐために、ファイルであれば`with`宣言や`try...finally`宣言を便利に活用するのも良い。
