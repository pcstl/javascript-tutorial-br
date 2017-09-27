Podemos usar essa abordagem se tivermos certeza de que a propriedade `" construtor "possui o valor correto.

Por exemplo, se não tocarmos o "protótipo" padrão, esse código funciona com certeza:

`` `js run
função Usuário (nome) {
this.name = name;
}

Deixe usuário = novo Usuário ('João');
Deixe user2 = new user.constructor ('Pete');

alerta (user2.name); // Pete (trabalhou!)
`` `

Funcionou, porque `User.prototype.constructor == Usuário '.

... Mas se alguém, por assim dizer, sobrescreva `User.prototype` e esquece de recriar` `constructor" `, então falharia.

Por exemplo:

`` `js run
função Usuário (nome) {
this.name = name;
}
*! *
User.prototype = {}; // (*)
* /! *

Deixe usuário = novo Usuário ('João');
Deixe user2 = new user.constructor ('Pete');

alerta (user2.name); // Indefinido
`` `

Por que `user2.name` é` undefined`?

Veja como `new user.constructor ('Pete')` funciona:

1. Primeiro, ele procura o `construtor 'no` usuário'. Nada.
2. Então segue a cadeia do protótipo. O protótipo do `usuário 'é` User.prototype`, e também não tem nada.
3. O valor de `User.prototype` é um objeto simples` {} `, seu protótipo é` Object.prototype`. E há `Object.prototype.constructor == Object`. Então é usado.

No final, temos "deixar user2 = novo Objeto ('Pete')`. O construtor incorporado 'Objeto' ignora os argumentos, ele sempre cria um objeto vazio - é o que temos no `usuário2 'depois de tudo.
