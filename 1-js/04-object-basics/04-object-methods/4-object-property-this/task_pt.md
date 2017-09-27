importância: 5

---

# Usando "this" em objeto literal

Aqui, a função `makeUser` retorna um objeto.

Qual é o resultado do acesso ao `ref`? Por quê?

`` `js
function makeUser () {
Retorna {
Nome: "John",
ref: this
};
};

deixe user = makeUser ();

alerta (user.ref.name); // Qual é o resultado?
`` `

