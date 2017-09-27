Precisamos "mapear" todos os valores do intervalo 0..1 em valores de `min` para` max`.

Isso pode ser feito em duas etapas:

1. Se multiplicarmos um número aleatório de 0..1 por `max-min`, então o intervalo de possíveis valores aumenta` 0..1` para `0..max-min`.
2. Agora, se adicionarmos `min`, o intervalo possível se torna de` min` para `max`.

A função:

`` `js run
função aleatória (min, max) {
Retornar min + Math.random () * (max - min);
}

alerta (aleatório (1, 5));
alerta (aleatório (1, 5));
alerta (aleatório (1, 5));
`` `

