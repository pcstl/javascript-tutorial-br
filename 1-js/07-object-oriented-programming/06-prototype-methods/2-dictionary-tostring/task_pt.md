importância: 5

---

# Adicionar aString to the dictionary

Existe um objeto `dictionary`, criado como` Object.create (null) `, para armazenar quaisquer pares` key / value`.

Adicione o método `dictionary.toString ()` a ele, que deve retornar uma lista delimitada por vírgulas de chaves. O `toString` não deve aparecer no` for..in` sobre o objeto.

Veja como deve funcionar:

`` `js
Let dictionary = Object.create (null);

*! *
// seu código para adicionar o método dictionary.toString
* /! *

// adicione alguns dados
dictionary.apple = "Apple";
dicionário .__ proto__ = "teste"; // __proto__ é uma chave de propriedade regular aqui

// apenas maçã e __proto__ estão no laço
para (deixe a chave no dicionário) {
alerta (chave); // "maçã", então "__proto__"
}

// seu toString em ação
alerta (dicionário); // "apple, __ proto__"
`` `
