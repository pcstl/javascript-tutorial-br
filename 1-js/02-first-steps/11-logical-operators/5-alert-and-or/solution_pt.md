A resposta: `3`.

`` `js run
alerta (null || 2 && 3 || 4);
`` `

A prioridade de AND `&&` é maior que `||`, então ele é executado primeiro.

O resultado de `2 && 3 = 3`, então a expressão se torna:

`` `
nulo || 3 || 4
`` `

Agora, o resultado se o primeiro valor verdadeiro: `3`.

