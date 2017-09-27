
Usando `setInterval`:

`` `js run
function printNumbers (de, para) {
Deixe a corrente = de;

deixe timerId = setInterval (function () {
alerta (atual);
se (atual == para) {
clearInterval (timerId);
}
atual ++;
}, 1000);
}

// uso:
números de impressão (5, 10);
`` `

Usando o `setTimeout 'recursivo:


`` `js run
function printNumbers (de, para) {
Deixe a corrente = de;

setTimeout (function go () {
alerta (atual);
se (atual <para) {
setTimeout (ir, 1000);
}
atual ++;
}, 1000);
}

// uso:
números de impressão (5, 10);
`` `

Observe que em ambas as soluções, há um atraso inicial antes da primeira saída. Às vezes, precisamos adicionar uma linha para fazer a primeira saída imediatamente, isso é fácil de fazer.

