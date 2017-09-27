importância: 5

---

# Decorador espião

Crie um decorador `spy (func)` que devolva um wrapper que salva todas as chamadas para funcionar em sua propriedade `calls`.

Cada chamada é guardada como uma matriz de argumentos.

Por exemplo:

`` `js
trabalho de função (a, b) {
alerta (a + b); // trabalho é uma função arbitrária ou método
}

*! *
trabalho = espião (trabalho);
* /! *

trabalho (1, 2); // 3
trabalho (4, 5); // 9

para (deixe args of work.calls) {
alerta ('call:' + args.join ()); // "chamada: 1,2", "chamada: 4,5"
}
`` `

P.S. Esse decorador às vezes é útil para testes de unidade, é uma forma avançada `sinon.spy` na biblioteca [Sinon.JS] (http://sinonjs.org/).
