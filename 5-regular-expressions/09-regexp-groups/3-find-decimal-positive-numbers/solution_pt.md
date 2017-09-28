
Um número inteiro é `padrão: \ d +`.

Uma parte decimal é: `padrão: \. \ D +`.

Como a parte decimal é opcional, coloquemos em parênteses com o padrão quantificador: '?' `.

Finalmente, temos o regexp: `pattern: \ d + (\. \ D +)?`:

`` `js run
Deixe reg = /\d+(\.\d+)?/g;

Deixe str = "1.5 0 12. 123.4.";

alerta (str.match (re)); // 1.5, 0, 12, 123.4
```
