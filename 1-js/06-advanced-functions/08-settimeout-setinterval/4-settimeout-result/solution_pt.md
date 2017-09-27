
Qualquer `setTimeout` será executado somente após o código atual ter terminado.

O `i` será o último:` 100000000`.

`` `js run
deixe i = 0;

setTimeout (() => alerta (i), 100); // 100000000

// assume que o tempo para executar esta função é> 100ms
para (vamos j = 0; j <100000000; j ++) {
i ++;
}
`` `
