A resposta: `1`.

`` `js run
deixe i = 3;

enquanto eu) {
alerta (eu--);
}
`` `

Toda iteração de loop diminui `i` por` 1`. A verificação `while (i)` pára o loop quando `i = 0`.

Por isso, as etapas do loop formam a seguinte sequência ("loop desenrolado"):

`` `js
deixe i = 3;

alerta (eu--); // mostra 3, diminui para 2

alerta (i--) // mostra 2, diminui i para 1

alerta (i--) // mostra 1, diminui i para 0

// feito, enquanto (i) verificar pára o loop
`` `
