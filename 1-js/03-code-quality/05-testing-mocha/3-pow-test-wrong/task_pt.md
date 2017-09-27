importância: 5

---

# O que há de errado no teste?

O que há de errado no teste de `pow` abaixo?

`` `js
("Eleva x para a potência n", função () {
deixe x = 5;

Deixe resultado = x;
assert.equal (pow (x, 1), resultado);

resultado * = x;
assert.equal (pow (x, 2), resultado);

resultado * = x;
assert.equal (pow (x, 3), resultado);
});
`` `

P.S. Sintaticamente, o teste está correto e passa.
