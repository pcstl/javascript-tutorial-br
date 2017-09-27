importância: 5

---

# Um erro na herança

Encontre um erro na herança prototípica abaixo.

O que está errado? Quais são as consequências?

`` `js
função Animal (nome) {
this.name = name;
}

Animal.prototype.walk = function () {
alerta (this.name + 'walkks');
};

função Coelho (nome) {
this.name = name;
}

Rabbit.prototype = Animal.prototype;

Rabbit.prototype.walk = function () {
alerta (this.name + "bounces!");
};
`` `
