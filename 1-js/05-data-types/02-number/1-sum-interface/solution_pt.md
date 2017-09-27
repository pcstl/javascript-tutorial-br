

`` `js run demo
Deixe a = + prompt ("O primeiro número?", "");
Deixe b = + prompt ("O segundo número?", "");

alerta (a + b);
`` `

Observe o unary plus `+` before `prompt`. Ele converte imediatamente o valor em um número.

Caso contrário, `a` e` b` seriam uma string, sua soma seria sua concatenação, isto é: `" 1 "+" 2 "=" 12 "`.
