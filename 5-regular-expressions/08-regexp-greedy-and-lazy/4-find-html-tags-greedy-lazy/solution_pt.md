
A solução é `padrão: <[^ <>] +>`.

`` `js run
deixe reg = / <[^ <>] +> / g;

Deixe str = '<> <a href="/"> <input type = "radio" verificado> <b>';

alerta (str.match (reg)); // '<a href="/">', '<input type = "radio" marcado>', '<b>'
```
