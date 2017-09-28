Um número positivo com uma parte decimal opcional é (por tarefa anterior): `padrão: \ d + (\. \ D +)?`.

Vamos adicionar um opcional `-` no início:

`` `js run
deixe reg = /-?\d+(\.\d+)?/g;

Deixe str = "-1.5 0 2 -123.4.";

alerta (str.match (reg)); // -1.5, 0, 2, -123.4
```
