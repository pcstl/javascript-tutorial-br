Internamente, a fração decimal "6.35" é um binário infinito. Como sempre em tais casos, ele é armazenado com uma perda de precisão.

Vamos ver:

`` `js run
alerta (6.35.toFixed (20)); // 6.34999999999999964473
`` `

A perda de precisão pode causar aumento e diminuição de um número. Neste caso específico, o número se torna um pouco menos, é por isso que ele se reduziu.

E o que é para 1.35?

`` `js run
alerta (1.35.toFixed (20)); // 1.35000000000000008882
`` `

Aqui, a perda de precisão fez o número um pouco maior, então ele se acumulou.

** Como podemos solucionar o problema com `6.35` se quisermos que seja arredondado do jeito certo? **

Devemos aproximá-lo de um inteiro antes do arredondamento:

`` `js run
alerta ((6.35 * 10) .toFixed (20)); // 63.50000000000000000000
`` `

Observe que o `63.5` não possui nenhuma perda de precisão. Isso porque a parte decimal `0.5` é na verdade` 1 / 2`. As frações divididas por poderes de `2 são exatamente representadas no sistema binário, agora podemos arredondá-lo:


`` `js run
alerta (Math.round (6.35 * 10) / 10); // 6.35 -> 63.5 -> 64 (arredondado) -> 6.4
`` `

