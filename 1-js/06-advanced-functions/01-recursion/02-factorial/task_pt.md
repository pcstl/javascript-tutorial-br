importância: 4

---

# Calcula o fatorial

O [fatorial] (https://en.wikipedia.org/wiki/Factorial) de um número natural é um número multiplicado por "número menos um" `, então por` "número menos dois" `, e assim por diante até `1`. O fatorial de `n 'é denotado como' n! '

Podemos escrever uma definição de fatorial como esta:

`` `js
n! = n * (n - 1) * (n - 2) * ... * 1
`` `

Valores de factorials para diferentes `n`:

`` `js
1! = 1
2! = 2 * 1 = 2
3! = 3 * 2 * 1 = 6
4! = 4 * 3 * 2 * 1 = 24
5! = 5 * 4 * 3 * 2 * 1 = 120
`` `

A tarefa é escrever uma função `factorial (n)` que calcula `n!` Usando chamadas recursivas.

`` `js
alerta (fatorial (5)); // 120
`` `

P.S. Dica: `n!` Pode ser escrito como 'n * (n-1)! `Por exemplo:` 3! = 3 * 2! = 3 * 2 * 1! = 6`
