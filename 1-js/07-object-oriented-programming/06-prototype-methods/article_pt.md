
# Métodos para protótipos

Neste capítulo, cobrimos métodos adicionais para trabalhar com um protótipo.

Existem também outras formas de obter / definir um protótipo, além dos que já conhecemos:

- [Object.create (proto [, descriptors])] (mdn: js / Object / create) - cria um objeto vazio com proto` dado como `[[Prototype]]` e descritores de propriedade opcionais.
- [Object.getPrototypeOf (obj)] (mdn: js / Object.getPrototypeOf) - retorna o `[[Prototype]]` de `obj`.
- [Object.setPrototypeOf (obj, proto)] (mdn: js / Object.setPrototypeOf) - define o `[[Protótipo]]` de `obj` para` proto`.

[cortar]

Por exemplo:

`` `js run
deixe animal = {
come: true
};

// crie um novo objeto com o animal como um protótipo
*! *
deixe coelho = Object.create (animal);
* /! *

alerta (rabbit.eats); // verdade
*! *
alerta (Object.getPrototypeOf (coelho) === animal); // obtenha o protótipo do coelho
* /! *

*! *
Object.setPrototypeOf (coelho, {}); // altera o protótipo de coelho para {}
* /! *
`` `

`Object.create` possui um segundo argumento opcional: descritores de propriedade. Podemos fornecer propriedades adicionais para o novo objeto, assim:

`` `js run
deixe animal = {
come: true
};

deixe coelho = Object.create (animal, {
saltos: {
valor: verdadeiro
}
});

alerta (coelho.jumps); // verdade
`` `

Os descritores estão no mesmo formato descrito no capítulo <info: property-descriptors>.

Podemos usar `Object.create` para executar uma clonagem de objetos mais poderosa do que copiar propriedades em` for..in`:

`` `js
// clone raso superficial totalmente idêntico de obj
Deixe clone = Object.create (Object.getPrototypeOf (obj), Object.getOwnPropertyDescriptors (obj));
`` `

Esta chamada faz uma cópia verdadeiramente exata de `obj`, incluindo todas as propriedades: enumeráveis ​​e não enumeráveis, propriedades de dados e setters / getters - tudo, e com o direito [[Protótipo]]`.

## Breve história

Se contamos todas as formas de gerenciar `[[Protótipo]]`, há muito! Muitas maneiras de fazer o mesmo!

Por quê?

Isso é por razões históricas.

- A propriedade "protótipo" de uma função de construtor funciona desde tempos muito antigos.
- Mais tarde no ano de 2012: `Object.create` apareceu no padrão. Permitiu criar objetos com o protótipo dado, mas não permitiu obter / configurá-lo. Assim, os navegadores implementaram o acessório `__proto__` não padrão que permitiu obter / definir um protótipo a qualquer momento.
- Mais tarde, no ano de 2015: `Object.setPrototypeOf` e` Object.getPrototypeOf` foram adicionados ao padrão. O `__proto__` foi implementado de facto em todos os lugares, por isso fez o caminho para o Anexo B do padrão, que é opcional para ambientes que não são do navegador.

A partir de agora, temos todas essas formas à nossa disposição.

Tecnicamente, podemos obter / configurar `[[Protótipo]]` a qualquer momento. Mas geralmente, nós apenas configuramos uma vez no tempo de criação do objeto e, em seguida, não modificamos: `rabbit` herda de` animal`, e isso não vai mudar. E os motores JavaScript são altamente otimizados para isso. Alterar um protótipo "on-the-fly" com `Object.setPrototypeOf` ou` obj .__ proto __ = `é uma operação muito lenta. Mas é possível.

## Objetos "muito simples"

Como sabemos, objetos podem ser usados ​​como arrays associativos para armazenar pares chave / valor.

... Mas se tentarmos armazenar as chaves * * fornecidas pelo usuário nele (por exemplo, um dicionário inserido pelo usuário), podemos ver uma falha interessante: todas as teclas funcionam bem, exceto `" __proto __ "`.

Confira o exemplo:

`` `js run
deixe obj = {};

Deixe key = prompt ("O que é a chave?", "__proto__");
obj [chave] = "algum valor";

alerta (obj [chave]); // [Objeto Objeto], não "algum valor"!
`` `

Aqui, se o usuário digitar `__proto__`, a atribuição é ignorada!

Isso não deve nos surpreender. A propriedade `__proto__` é especial: deve ser um objeto ou 'nulo', uma seqüência de caracteres não pode se tornar um protótipo.

Mas não pretendemos implementar esse comportamento, certo? Queremos armazenar pares chave / valor, e a chave chamada `` __proto __ '' não foi guardada corretamente. Então é um bug. Aqui, as consequências não são terríveis. Mas, em outros casos, o protótipo pode realmente ser alterado, de modo que a execução pode dar errado de maneiras totalmente inesperadas.

O que é pior - geralmente os desenvolvedores não pensam em tal possibilidade. Isso faz com que esses erros sejam difíceis de perceber e até mesmo transformá-los em vulnerabilidades, especialmente quando o JavaScript é usado no lado do servidor.

Tal coisa acontece apenas com `__proto__`. Todas as outras propriedades são "atribuíveis" normalmente.

Como evadir o problema?

Primeiro, podemos mudar para usar o `Map`, então tudo está bem.

Mas `Object` também pode nos servir bem aqui, porque os criadores de linguagem refletiram esse problema há muito tempo.

O `__proto__` não é uma propriedade de um objeto, mas uma propriedade de acesso de` Object.prototype`:

! [] (object-prototype-2.png)

Então, se `obj .__ proto__` for lido ou atribuído, o getter / setter correspondente é chamado de seu protótipo, e ele obtém / define` [[Prototype]] `.

Como foi dito no início: `__proto__` é uma maneira de acessar` [[Prototype]] `, não é` [[Prototype]] `.

Agora, se quisermos usar um objeto como uma matriz associativa, podemos fazê-lo com um pequeno truque:

`` `js run
*! *
deixe obj = Object.create (null);
* /! *

Deixe key = prompt ("O que é a chave?", "__proto__");
obj [chave] = "algum valor";

alerta (obj [chave]); // "algum valor"
`` `

`Object.create (null)` cria um objeto vazio sem um protótipo (`[[Prototype]]` is `null`):

! [] (object-prototype-null.png)

Então, não há getter / setter herdado para `__proto__`. Agora ele é processado como uma propriedade de dados regulares, então o exemplo acima funciona corretamente.

Podemos chamar esse objeto "muito simples" ou "objetos de dicionário puro", porque eles são ainda mais simples do que o objeto comum regular `{...}`.

Uma desvantagem é que tais objetos não possuem nenhum método de objeto incorporado, p. `toString`:

`` `js run
*! *
deixe obj = Object.create (null);
* /! *

alerta (obj); // Erro (não toString)
`` `

... Mas isso geralmente é bom para arrays associativos.

Observe que a maioria dos métodos relacionados a objetos são `Object.something (...)`, como `Object.keys (obj)` - eles não estão no protótipo, então eles continuarão trabalhando em tais objetos:


`` `js run
deixe chineseDictionary = Object.create (null);
chineseDictionary.hello = "ni hao";
Escola chinesa. Graduação = "adeus";

alerta (Object.keys (chinêsDictionary)); // Olá Tchau
`` `

## Obtendo todas as propriedades

Há muitas maneiras de obter chaves / valores de um objeto.

Nós já conhecemos estes:

- [Object.keys (obj)] (mdn: js / Object / keys) / [Object.values ​​(obj)] (mdn: js / Object / values) / [Object.entries (obj)] (mdn: js / Objeto / entradas) - retorna uma matriz de nomes de propriedade de cadeia própria enumeráveis ​​/ pares de valores / chave-valor. Esses métodos apenas listam * propriedades enumeráveis ​​* e aqueles que possuem * strings como chaves *.

Se queremos propriedades simbólicas:

- [Object.getOwnPropertySymbols (obj)] (mdn: js / Object / getOwnPropertySymbols) - retorna uma matriz de todos os nomes de propriedade simbólica própria.

Se queremos propriedades não enumeráveis:

- [Object.getOwnPropertyNames (obj)] (mdn: js / Object / getOwnPropertyNames) - retorna uma matriz de todos os nomes de propriedades de seqüência própria.

Se quisermos * todas * propriedades:

- [Reflect.ownKeys (obj)] (mdn: js / Reflect / ownKeys) - retorna uma matriz de todos os nomes de propriedade própria.

Esses métodos são um pouco diferentes sobre quais propriedades eles retornam, mas todos eles operam no próprio objeto. As propriedades do protótipo não estão listadas.

O loop `for..in 'é diferente: ele também supera as propriedades herdadas.

Por exemplo:

`` `js run
deixe animal = {
come: true
};

deixe coelho = {
saltos: verdade
__proto__: animal
};

*! *
// apenas possui chaves
alerta (Object.keys (coelho)); // salta
* /! *

*! *
// chaves herdadas também
para (let prop in rabbit) alert (prop); // salta, então come
* /! *
`` `

Se queremos distinguir propriedades herdadas, existe um método incorporado [obj.hasOwnProperty (chave)] (mdn: js / Object / hasOwnProperty): retorna `true` se` obj` tiver sua própria propriedade (não herdada) chamada `chave`.

Então podemos filtrar propriedades herdadas (ou fazer algo mais com elas):

`` `js run
deixe animal = {
come: true
};

deixe coelho = {
saltos: verdade
__proto__: animal
};

para (deixe o suporte em coelho) {
deixe isOwn = rabbit.hasOwnProperty (prop);
alerta (`$ {prop}: $ {isOwn}`); // salta: true, então come: false
}
`` `
Aqui temos a seguinte cadeia de herança: `rabbit`, then` animal`, then `Object.prototype` (porque` animal` é um objeto literal `{...}`, então é por padrão) e então 'null `acima dela:

! [] (rabbit-animal-object.png)

Note, há uma coisa divertida. Onde é que o método `rabbit.hasOwnProperty` vem? Olhando para a cadeia, podemos ver que o método é fornecido por `Object.prototype.hasOwnProperty`. Em outras palavras, é herdado.

... Mas por que `hasOwnProperty` não aparece no loop` for..in`, se ele enumera todas as propriedades herdadas? A resposta é simples: não é enumerável. Assim como todas as outras propriedades do `Object.prototype`. É por isso que eles não estão listados.

## Resumo

Aqui está uma breve lista de métodos que discutimos neste capítulo - como uma recapitulação:

- [Object.create (proto [, descriptors])] (mdn: js / Object / create) - cria um objeto vazio com `proto` dado como` [[Prototype]] `(pode ser 'nulo') e opcional descritores de propriedade.
- [Object.getPrototypeOf (obj)] (mdn: js / Object.getPrototypeOf) - retorna o `[[Prototype]]` de `obj` (mesmo que` __proto__` getter).
- [Object.setPrototypeOf (obj, proto)] (mdn: js / Object.setPrototypeOf) - define o `[[Protótipo]]` de `obj` para` proto` (mesmo que `__proto__` setter).
- [Object.keys (obj)] (mdn: js / Object / keys) / [Object.values ​​(obj)] (mdn: js / Object / values) / [Object.entries (obj)] (mdn: js / Objeto / entradas) - retorna uma matriz de nomes de propriedade de cadeia própria enumeráveis ​​/ pares de valores / chave-valor.
- [Object.getOwnPropertySymbols (obj)] (mdn: js / Object / getOwnPropertySymbols) - retorna uma matriz de todos os nomes de propriedade simbólica própria.
- [Object.getOwnPropertyNames (obj)] (mdn: js / Object / getOwnPropertyNames) - retorna uma matriz de todos os nomes de propriedades de seqüência própria.
- [Reflect.ownKeys (obj)] (mdn: js / Reflect / ownKeys) - retorna uma matriz de todos os nomes de propriedade própria.
- [obj.hasOwnProperty (chave)] (mdn: js / Object / hasOwnProperty): ele retorna `true` se` obj` tiver sua própria propriedade (não herdada) chamada `key`.

Nós também deixamos claro que `__proto__` é um getter / setter para` [[Prototype]] `e reside em` Object.prototype`, assim como outros métodos.

Podemos criar um objeto sem um protótipo por `Object.create (null)`. Tais objetos são usados ​​como "dicionários puros", eles não têm problemas com `` __proto __ '' como a chave.

Todos os métodos que retornam as propriedades do objeto (como `Object.keys` e outros) - retornam propriedades" próprias ". Se queremos herdados, podemos usar `for..in`.
