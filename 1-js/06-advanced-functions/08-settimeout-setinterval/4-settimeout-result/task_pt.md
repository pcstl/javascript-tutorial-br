importância: 5

---

# O que SetTimeout mostrará?

No código abaixo, há uma chamada `setTimeout` agendada, então um cálculo pesado é executado, que leva mais de 100ms para terminar.

Quando a função programada será executada?

1. Após o loop.
2. Antes do loop.
3. No início do loop.


Qual o alerta que vai mostrar?

`` `js
deixe i = 0;

setTimeout (() => alerta (i), 100); //?

// assume que o tempo para executar esta função é> 100ms
para (vamos j = 0; j <100000000; j ++) {
i ++;
}
`` `
