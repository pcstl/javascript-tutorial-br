importância: 5

---

# Números de Fibonacci

A seqüência de [números Fibonacci] (https://en.wikipedia.org/wiki/Fibonacci_number) tem a fórmula <code> F <sub> n </ sub> = F <sub> n-1 </ sub> + F <sub> n-2 </ sub> </ code>. Em outras palavras, o próximo número é uma soma dos dois precedentes.

Os dois primeiros são `1`, então` 2 (1 + 1) `, depois` 3 (1 + 2) `,` 5 (2 + 3) `e assim por diante:` 1, 1, 2, 3, 5 8, 13, 21 ... `.

Os números de Fibonacci estão relacionados ao [Relação de Ouro] (https://en.wikipedia.org/wiki/Golden_ratio) e muitos fenômenos naturais ao nosso redor.

Escreva uma função `fib (n)` que retorna o número de Fibonacci `n-th`.

Um exemplo de trabalho:

`` `js
função fib (n) {/ * seu código * /}

alerta (fib (3)); // 2
alerta (fib (7)); // 13
alerta (fib (77)); // 5527939700884757
`` `

P.S. A função deve ser rápida. A chamada para `fib (77)` não deve demorar mais do que uma fração de segundo.
