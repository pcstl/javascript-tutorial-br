
# Re-resolver uma promessa?


Qual é o resultado do código abaixo?

`` `js
deixe prometer = nova Promessa (função (resolver, rejeitar) {
resolve(1);

setTimeout (() => resolve (2), 1000);
});

promessa. (alerta);
```
