
1. Para que tudo funcione * de qualquer forma *, o resultado da "soma" deve ser função.
2. Essa função deve manter na memória o valor atual entre as chamadas.
3. De acordo com a tarefa, a função deve se tornar o número quando usado em `==`. As funções são objetos, então a conversão acontece conforme descrito no capítulo <info: object-toprimitive>, e podemos fornecer nosso próprio método que retorna o número.

Agora, o código:

`` `js run
soma de função (a) {

Deixe CurrentSum = a;

função f (b) {
currentSum + = b;
retornar f;
}

f.toString = function () {
retornar atualSum;
};

retornar f;
}

alerta (soma (1) (2)); // 3
alerta (soma (5) (- 1) (2)); // 6
alerta (soma (6) (- 1) (- 2) (- 3)); // 0
alerta (soma (0) (1) (2) (3) (4) (5)); // 15
`` `

Observe que a função `sum` realmente funciona apenas uma vez. Retorna a função `f`.

Então, em cada chamada subsequente, `f` adiciona seu parâmetro à soma` currentSum` e retorna-se.

** Não há recursão na última linha de `f`. **

Aqui está a aparência da recursão:

`` `js
função f (b) {
currentSum + = b;
regresso de(); // <- chamada recursiva
}
`` `

E, no nosso caso, acabamos de retornar a função, sem chamá-la:

`` `js
função f (b) {
currentSum + = b;
retornar f; // <- não se chama, se retorna
}
`` `

Este `f` será usado na próxima chamada, novamente retornará, tantas vezes quanto necessário. Então, quando usado como um número ou uma string - o `toString` retorna o` currentSum`. Poderíamos também usar `Symbol.toPrimitive` ou` valueOf` aqui para a conversão.
