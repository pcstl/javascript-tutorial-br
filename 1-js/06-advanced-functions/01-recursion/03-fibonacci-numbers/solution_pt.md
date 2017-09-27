A primeira solução que podemos tentar aqui é a recursiva.

Os números de Fibonacci são recursivos por definição:

`` `js run
função fib (n) {
retornar n <= 1? n: fib (n - 1) + fib (n - 2);
}

alerta (fib (3)); // 2
alerta (fib (7)); // 13
// fib (77); // será extremamente lento!
`` `

... Mas para grandes valores de `n` é muito lento. Por exemplo, `fib (77)` pode desligar o motor durante algum tempo comendo todos os recursos da CPU.

Isso porque a função faz muitas subcalls. Os mesmos valores são reavaliados novamente e novamente.

Por exemplo, vejamos um cálculo de `fib (5)`:

`` `js no-embellecer
...
fib (5) = fib (4) + fib (3)
fib (4) = fib (3) + fib (2)
...
`` `

Aqui podemos ver que o valor de `fib (3)` é necessário para ambos 'fib (5) `e' fib (4)`. Então, `fib (3)` será chamado e avaliado duas vezes completamente de forma independente.

Aqui está a árvore de recursão completa:

! [árvore de recursão de fibonacci] (fibonacci-recursion-tree.png)

Podemos notar claramente que `fib (3)` é avaliado duas vezes e `fib (2)` é avaliado três vezes. A quantidade total de cálculos cresce muito mais rápido do que `n`, tornando-o enorme mesmo para` n = 77`.

Podemos otimizar isso lembrando valores já avaliados: se um valor de dizer `fib (3)` é calculado uma vez, então podemos reutilizá-lo em cálculos futuros.

Outra variante seria abandonar a recursão e usar um algoritmo baseado em loop totalmente diferente.

Em vez de ir de `n para valores inferiores, podemos fazer um loop que começa a partir de` 1 e `2, então fica` fib (3) `como sua soma, depois` fib (4) `como a soma de dois valores anteriores, depois `fib (5)` e sobe para cima, até chegar ao valor necessário. Em cada passo, precisamos apenas lembrar dois valores anteriores.

Aqui estão as etapas do novo algoritmo em detalhes.

O começo:

`` `js
// a = fib (1), b = fib (2), esses valores são, por definição, 1
deixe a = 1, b = 1;

// obtenha c = fib (3) como sua soma
deixe c = a + b;

/ * agora temos fib (1), fib (2), fib (3)
a b c
1, 1, 2
* /
`` `

Agora queremos obter `fib (4) = fib (2) + fib (3)`.

Vamos mudar as variáveis: `a, b` obterá` fib (2), fib (3) `e` c` obterá a soma:

`` `js no-embellecer
a = b; // agora a = fib (2)
b = c; // agora b = fib (3)
c = a + b; // c = fib (4)

/ * agora temos a seqüência:
a b c
1, 1, 2, 3
* /
`` `

O próximo passo dá outro número de seqüência:

`` `js no-embellecer
a = b; // agora a = fib (3)
b = c; // agora b = fib (4)
c = a + b; // c = fib (5)

/ * agora a seqüência é (um número mais):
a b c
1, 1, 2, 3, 5
* /
`` `

... E assim por diante até obtermos o valor necessário. Isso é muito mais rápido do que a recursão e não envolve cálculos duplicados.

O código completo:

`` `js run
função fib (n) {
deixe a = 1;
Deixe B = 1;
para (vamos i = 3; i <= n; i ++) {
deixe c = a + b;
a = b;
b = c;
}
retornar b;
}

alerta (fib (3)); // 2
alerta (fib (7)); // 13
alerta (fib (77)); // 5527939700884757
`` `

O loop começa com `i = 3`, porque o primeiro e o segundo valores de seqüência são codificados em variáveis` a = 1`, `b = 1`.

A abordagem é chamada de [programação dinâmica de baixo para cima] (https://en.wikipedia.org/wiki/Dynamic_programming).
