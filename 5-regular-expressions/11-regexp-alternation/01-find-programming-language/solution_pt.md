
A primeira idéia pode ser listar os idiomas com `|` in-between.

Mas isso não funciona direito:

`` `js run
Deixe reg = / Java | JavaScript | PHP | C | C \ + \ + / g;

Deixe str = "Java, JavaScript, PHP, C, C ++";

alerta (str.match (reg)); // Java, Java, PHP, C, C
```

O mecanismo de expressão regular procura por alterações um a um. Isto é: primeiro ele verifica se temos "match: Java", caso contrário, procura por "match: JavaScript" e assim por diante.

Como resultado, `match: JavaScript` nunca pode ser encontrado, apenas porque` match: Java` é verificado primeiro.

O mesmo com `match: C` e` match: C ++ `.

Existem duas soluções para esse problema:

1. Mude a ordem para verificar primeiro a partida mais longa: `padrão: JavaScript | Java | C \ + \ + | C | PHP`.
2. Merge variantes com a mesma estrela: `padrão: Javascript)? | C (\ + \ +)? | PHP`.

Em ação:

`` `js run
Deixe reg = / Java (Script)? | C (\ + \ +)? | PHP / g;

Deixe str = "Java, JavaScript, PHP, C, C ++";

alerta (str.match (reg)); // Java, JavaScript, PHP, C, C ++
```
