
`` `js run demo
função pow (x, n) {
Deixe resultado = x;

para (vamos i = 1; i <n; i ++) {
resultado * = x;
}

resultado de retorno;
}

Deixe x = prompt ("x?", '');
deixe n = prompt ("n?", '');

se (n <= 1) {
alerta ('Power $ {n} não é suportado,
use um número inteiro maior que 0`);
} outro {
alerta (pow (x, n));
}
`` `

