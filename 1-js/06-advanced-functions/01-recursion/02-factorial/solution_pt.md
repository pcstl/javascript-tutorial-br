Por definição, um fatorial é `n!` Pode ser escrito como 'n * (n-1)! `.

Em outras palavras, o resultado de `factorial (n)` pode ser calculado como 'n` multiplicado pelo resultado de `factorial (n-1)`. E a chamada para `n-1` pode descer de forma recursiva mais baixa, e menor, até` 1`.

`` `js run
função fatorial (n) {
retorno (n! = 1)? n * fatorial (n - 1): 1;
}

alerta (fatorial (5)); // 120
`` `

A base da recursão é o valor `1`. Nós também podemos fazer `0 'a base aqui, não importa muito, mas dá mais um passo recursivo:

`` `js run
função fatorial (n) {
retornar n? n * fatorial (n - 1): 1;
}

alerta (fatorial (5)); // 120
`` `
