
# Object.keys, valores, entradas

Vamos afastar-nos das estruturas de dados individuais e falar sobre as iterações sobre elas.

No capítulo anterior, vimos os métodos `map.keys ()`, `map.values ​​()`, `map.entries ()`.

Esses métodos são genéricos, existe um acordo comum para usá-los para estruturas de dados. Se alguma vez criarmos uma estrutura de dados própria, devemos implementá-los também.

Eles são suportados por:

- `Map`
- `Set`
- `Array` (exceto` arr.values ​​() `)

Objetos simples também suportam métodos semelhantes, mas a sintaxe é um pouco diferente.

## Object.keys, valores, entradas

Para objetos simples, os seguintes métodos estão disponíveis:

- [Object.keys (obj)] (mdn: js / Object / keys) - retorna uma matriz de chaves.
- [Object.values ​​(obj)] (mdn: js / Object / values) - retorna uma matriz de valores.
- [Object.entries (obj)] (mdn: js / Object / entries) - retorna uma matriz de `[key, value]` pairs.

... Mas observe as distinções (em comparação com o mapa, por exemplo):

| | Mapa | Objeto |
| ------------- | ------------------ | -------------- |
| Sintaxe de chamada | `map.keys ()` | `Object.keys (obj)`, mas não `obj.keys ()` |
| Retorna | iterável | matriz "real" |

A primeira diferença é que devemos chamar `Object.keys (obj)`, e não `obj.keys ()`.

Por quê? O principal motivo é a flexibilidade. Lembre-se, os objetos são uma base de todas as estruturas complexas em JavaScript. Então, podemos ter um objeto nosso como `order` que implementa seu próprio método` order.values ​​() `. E ainda podemos chamar `Object.values ​​(order)` nele.

A segunda diferença é que os métodos `Object. *` Retornam objetos de matriz "reais", não apenas um iterável. Isso é principalmente por razões históricas.

Por exemplo:

`` `js
Deixe o usuário = {
Nome: "John",
idade: 30
};
`` `

- `Object.keys (usuário) = [nome, idade]`
- `Object.values ​​(user) = [" John ", 30]`
- `Object.entries (user) = [[" name "," John "], [" age ", 30]]`

Aqui está um exemplo de usar `Object.values` para ignorar valores de propriedade:

`` `js run
Deixe o usuário = {
Nome: "John",
idade: 30
};

// loop over values
para (deixe o valor de Object.values ​​(usuário)) {
alerta (valor); // John, então 30
}
`` `

## Object.keys / values ​​/ entries ignoram propriedades simbólicas

Assim como um "for..in" loop, esses métodos ignoram propriedades que usam `Symbol (...)` como chaves.

Normalmente isso é conveniente. Mas se queremos chaves simbólicas também, existe um método separado [Object.getOwnPropertySymbols] (mdn: js / Object / getOwnPropertySymbols) que retorna uma matriz de apenas chaves simbólicas. Além disso, o método [Reflect.ownKeys (obj)] (mdn: js / Reflect / ownKeys) retorna as teclas * all *.
