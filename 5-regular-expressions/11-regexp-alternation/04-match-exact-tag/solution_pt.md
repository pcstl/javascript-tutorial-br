
O início do padrão é óbvio: `padrão: <estilo`.

... Mas, então, não podemos simplesmente escrever `pattern: <style. *?>`, Porque `match: <styler>` o combinaria.

Precisamos de um espaço após `match: <style` e, em seguida, opcionalmente outra coisa ou o ending` match:> `.

Na linguagem regex: `pattern: <styles. *?>)`.

Em ação:

`` `js run
Deixe reg = /<style(>|\\s.??>)/g;

alerta ('<style> <styler> <style test = "...">'. match (reg)); // <style>, <style test = "...">
```
