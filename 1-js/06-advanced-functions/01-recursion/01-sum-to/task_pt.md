importância: 5

---

# Soma todos os números até o dado

Escreva uma função `sumTo (n)` que calcula a soma dos números `1 + 2 + ... + n`.

Por exemplo:

`` `js no-embellecer
Sumter (1) = 1
Sumter (2) = 2 + 1 = 3
Sumter (3) = 3 + 2 + 1 = 6
Sumter (4) = 4 + 3 + 2 + 1 = 10
...
Sumter (100) = 100 + 99 + ... + 2 + 1 = 5050
`` `

Faça 3 variantes de solução:

1. Usando um loop for.
2. Usando uma recursão, cause `sumTo (n) = n + sumTo (n-1)` para `n> 1`.
3. Usando a fórmula [progressão aritmética] (https://en.wikipedia.org/wiki/Arithmetic_progression).

Um exemplo do resultado:

`` `js
função sumTo (n) {/ * ... seu código ... * /}

alerta (Sumter (100)); // 5050
`` `

P.S. Qual solução é a mais rápida? O mais lento? Por quê?

P.P.S. Podemos usar a recursão para contar `sumTo (100000)`?
