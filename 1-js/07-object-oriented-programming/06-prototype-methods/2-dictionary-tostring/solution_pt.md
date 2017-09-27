
O método pode levar todas as chaves enumeráveis ​​usando `Object.keys` e exibir sua lista.

Para tornar `toString` não enumerável, vamos defini-lo usando um descritor de propriedade. A sintaxe de `Object.create` permite fornecer um objeto com descritores de propriedade como o segundo argumento.

`` `js run
*! *
let dictionary = Object.create (null, {
toString: {// define a propriedade toString
value () {// o valor é uma função
retornar Object.keys (this) .join ();
}
}
});
* /! *

dictionary.apple = "Apple";
dicionário .__ proto__ = "teste";

// apple e __proto__ estão no laço
para (deixe a chave no dicionário) {
alerta (chave); // "maçã", então "__proto__"
}

// lista separada por vírgulas de propriedades por toString
alerta (dicionário); // "apple, __ proto__"
`` `

Quando criamos uma propriedade usando um descritor, suas bandeiras são "falsas" por padrão. Então, no código acima, `dictionary.toString` não é enumerável.
