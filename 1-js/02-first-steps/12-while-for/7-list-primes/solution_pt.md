Existem muitos algoritmos para esta tarefa.

Vamos usar um loop aninhado:

`` `js
Para cada i no intervalo {
verifique se eu tenho um divisor de 1 ..
se sim => o valor não é um primário
se não => o valor é um primo, mostre-o
}
`` `

O código que usa um rótulo:

`` `js run
deixe n = 10;

nextPrime:
para (vamos i = 2; i <= n; i ++) {// para cada eu ...

para (deixe j = 2; j <i; j ++) {// procure um divisor ..
se (i% j == 0) continue nextPrime; // não é um primário, vá em seguida i
}

alerta (i); // um primeiro
}
`` `

Há muito espaço para opiá-lo. Por exemplo, poderíamos procurar os divisores de `2 'para a raiz quadrada de` i`. Mas, de qualquer forma, se queremos ser realmente eficientes para grandes intervalos, precisamos mudar a abordagem e confiar em matemática avançada e algoritmos complexos como [Quadratic sieve] (https://en.wikipedia.org/wiki/Quadratic_sieve), [Geral peneira de número de campo] (https://en.wikipedia.org/wiki/General_number_field_sieve) etc.
