importância: 5

---

# Esquisito exemplo de

Por que `instanceof` abaixo retorna` true`? Podemos ver facilmente que `a` não é criado por` B () `.

`` `js run
função A () {}
função B () {}

A.prototype = B.prototype = {};

Deixe a = novo A ();

*! *
alerta (uma instância de B); // verdade
* /! *
`` `
