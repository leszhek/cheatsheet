## Как установить tensorflow

Ну, во-первых, устанавливать будем старенькую версию 1.14, а во-вторых, будем отучать ее ругаться.

1. Версия 1.14 не работает с python выше 3.7.11
2. Зависима от numpy (нужна версия не выше 1.16.4)
3. На моем macbook m1 сильно ругается на инструкции процессора - лечится установкой nomkl

устанавливал все в такой последовательности:

```
$ conda create -n tensor1 python=3.7.11 numpy=1.16.4 tensorflow=1.14 nomkl pandas scikit-learn matplotlib

$ conda activate tensor1

$conda install -c conda-forge notebook
```