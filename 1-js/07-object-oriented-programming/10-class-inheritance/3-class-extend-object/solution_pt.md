A resposta tem duas partes.

O primeiro, fácil, é que a classe de herança precisa chamar `super ()` no construtor. Caso contrário, "este" não será "definido".

Então, aqui está a correção:

`` `js run
Classe Rabbit extends Object {
construtor (nome) {
*! *
super(); // precisa ligar para o construtor pai quando herdar
* /! *
this.name = name;
}
}

deixe coelho = coelho novo ("Rab");

alerta (rabbit.hasOwnProperty ('name')); // verdade
`` `

Mas isso ainda não é tudo.

Mesmo após a correção, ainda há diferença importante na classe Rabbit extends Object "` versus `classe Rabbit`.

Como sabemos, a sintaxe "se estende" configura dois protótipos:

1. Entre o "protótipo" das funções do construtor (para métodos).
2. Entre as próprias funções do construtor (para métodos estáticos).

No nosso caso, para a classe Rabbit extends Object, isso significa:

`` `js run
Classe Rabbit extends Object {}

alerta (Rabbit.prototype .__ proto__ === Object.prototype); // (1) true
alerta (Coelho .__ proto__ === Objeto); // (2) true
`` `

Portanto, podemos acessar métodos estáticos de `Object` via` Rabbit`, como este:

`` `js run
Classe Rabbit extends Object {}

*! *
// normalmente chamamos Object.getOwnPropertyNames
alerta (Rabbit.getOwnPropertyNames ({a: 1, b: 2})); // a, b
* /! *
`` `

E se não usarmos `extends`, então` classe Rabbit` não recebe a segunda referência.

Compare com ele:

`` `js run
classe Rabbit {}

alerta (Rabbit.prototype .__ proto__ === Object.prototype); // (1) true
alerta (Coelho .__ proto__ === Objeto); // (2) falso (!)

*! *
// erro, sem essa função em Rabbit
alerta (Rabbit.getOwnPropertyNames ({a: 1, b: 2})); // Erro
* /! *
`` `

Para o simples "coelho da classe", a função `Rabbit` tem o mesmo protótipo

`` `js run
classe Rabbit {}

// em vez de (2) isso é correto para Rabbit (assim como qualquer função):
alerta (Coelho .__ proto__ === Function.prototype);
`` `

Aliás, o `Function.prototype` possui métodos de função" genéricos ", como` call`, `bind` etc. Eles estão, em última instância, disponíveis em ambos os casos, porque para o construtor 'Object` incorporado' Object .__ proto__ = == Function.prototype`.

Aqui está a foto:

! [] (rabbit-extends-object.png)

Então, para resumir, há duas diferenças:

| Coelho Classe | classe Rabbit extends Object |
| -------------- | ------------------------------ |
| - | precisa chamar `super ()` no construtor |
| `Rabbit .__ proto__ === Function.prototype` | `Rabbit .__ proto__ === Object` |
