Aqui está a linha com o erro:

`` `js
Rabbit.prototype = Animal.prototype;
`` `

Aqui `Rabbit.prototype` e` Animal.prototype` tornam-se o mesmo objeto. Então, os métodos de ambas as classes se misturam nesse objeto.

Como resultado, `Rabbit.prototype.walk` substitui` Animal.prototype.walk`, então todos os animais começam a saltar:

`` `js run
função Animal (nome) {
this.name = name;
}

Animal.prototype.walk = function () {
alerta (this.name + 'walkks');
};

função Coelho (nome) {
this.name = name;
}

*! *
Rabbit.prototype = Animal.prototype;
* /! *

Rabbit.prototype.walk = function () {
alerta (this.name + "bounces!");
};

*! *
deixe animal = novo animal ("porco");
animal.walk (); // ganha porco!
* /! *
`` `

A variante correta seria:

`` `js
Rabbit.prototype .__ proto__ = Animal.prototype;
// ou assim:
Rabbit.prototype = Object.create (Animal.prototype);
`` `

Isso faz com que os protótipos se separem, cada um deles armazena métodos da classe correspondente, mas o `Rabbit.prototype` herda do` Animal.prototype`.
