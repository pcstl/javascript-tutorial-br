
# Atrasar com uma promessa

A função integrada `setTimeout` usa retorno de chamada. Crie uma alternativa baseada em promessas.

A função `delay (ms)` deve retornar uma promessa. Essa promessa deve ser resolvida após `ms 'milissegundos, para que possamos adicionar` .then` a ele, assim:

`` `js
atraso de função (ms) {
// seu código
}

atrasar (3000) .then (() => alert ('runs after 3 seconds'));
```
