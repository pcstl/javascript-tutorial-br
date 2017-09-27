importância: 5

---

# Descobriu o decorador

O resultado de `debounce (f, ms)` decorador deve ser um invólucro que passa a chamada para `f` no máximo uma vez por milissegundos.

Em outras palavras, quando chamamos de função "devolvida", ela garante que todos os outros futuros nos milliseconds mais próximos serão ignorados.

Por exemplo:

`` `js no-embellecer
deixe f = debounce (alerta, 1000);

f (1); // é executado imediatamente
f (2); // ignorado

setTimeout (() => f (3), 100); // ignorado (apenas 100 ms passaram)
setTimeout (() => f (4), 1100); // corre
setTimeout (() => f (5), 1500); // ignorado (menos de 1000 ms da última execução)
`` `

Na prática, `debounce` é útil para funções que recuperam / atualizam algo quando sabemos que nada de novo pode ser feito em um curto período de tempo, então é melhor não desperdiçar recursos.
