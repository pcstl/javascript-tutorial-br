importância: 5

---

# Trabalhando com protótipo

Aqui está o código que cria um par de objetos e os modifica.

Quais valores são mostrados no processo?

`` `js
deixe animal = {
saltos: nulo
};
deixe coelho = {
__proto__: animal,
saltos: verdadeiro
};

alerta (coelho.jumps); //? (1)

apague coelho.jumps;

alerta (coelho.jumps); //? (2)

delete animal.jumps;

alerta (coelho.jumps); //? (3)
`` `

Deve haver 3 respostas.
