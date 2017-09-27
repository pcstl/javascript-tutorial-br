
A primeira chamada tem `this == rabbit`, as outras têm 'this` igual a` Rabbit.prototype`, porque é realmente o objeto antes do ponto.

Então, apenas a primeira chamada mostra `Rabbit`, outros mostram` undefined`:

`` `js run
função Coelho (nome) {
this.name = name;
}
Rabbit.prototype.sayHi = function () {
alerta (this.name);
}

Deixe o coelho = Coelho novo ("Coelho");

coelho.sayHi (); // Coelho
Rabbit.prototype.sayHi (); // Indefinido
Object.getPrototypeOf (coelho) .sayHi (); // Indefinido
coelho .__ proto __. sayHi (); // Indefinido
`` `
