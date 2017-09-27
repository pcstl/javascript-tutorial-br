
# Bandeiras de propriedades e descritores

Como sabemos, objetos podem armazenar propriedades.

Até agora, uma propriedade era um par simples de "valor-chave" para nós. Mas uma propriedade de objeto é realmente uma coisa mais complexa e ajustável.

[cortar]

## Bandeiras de propriedades

As propriedades dos objetos, além de ** `value` **, possuem três atributos especiais (os chamados" flags "):

- ** `writable` ** - if` true`, pode ser alterado, caso contrário, é somente leitura.
- ** `enumerable` ** - if` true`, então listado em loops, caso contrário, não está listado.
- ** `configurable` ** - if` true`, a propriedade pode ser excluída e esses atributos podem ser modificados, caso contrário, não.

Ainda não os vimos, porque geralmente eles não aparecem. Quando criamos uma propriedade "a maneira usual", todos são "verdadeiros". Mas também podemos alterá-los a qualquer momento.

Primeiro, vejamos como obter essas bandeiras.

O método [Object.getOwnPropertyDescriptor] (mdn: js / Object / getOwnPropertyDescriptor) permite consultar as informações * full * sobre uma propriedade.

A sintaxe é:
`` `js
Deixe descriptor = Object.getOwnPropertyDescriptor (obj, propertyName);
`` `

`obj`
: O objeto para obter informações.

`propertyName`
: O nome da propriedade.

O valor retornado é um chamado objeto "descritor de propriedades": ele contém o valor e todos os sinalizadores.

Por exemplo:

`` `js run
Deixe o usuário = {
Nome: "John"
};

Deixe descriptor = Object.getOwnPropertyDescriptor (usuário, 'nome');

alerta (JSON.stringify (descriptor, null, 2));
/ * descritor de propriedade:
{
"valor": "João",
"writable": true,
"enumerable": true
"configurável": verdadeiro
}
* /
`` `

Para alterar os sinalizadores, podemos usar [Object.defineProperty] (mdn: js / Object / defineProperty).

A sintaxe é:

`` `js
Object.defineProperty (obj, propertyName, descriptor)
`` `

`obj`,` propertyName`
: O objeto e a propriedade para trabalhar.

`descritor`
: Descritor de propriedade para se candidatar.

Se a propriedade existir, `defineProperty` atualiza suas bandeiras. Caso contrário, ele cria a propriedade com o valor e sinalizadores dados; nesse caso, se um sinalizador não for fornecido, é assumido como "falso".

Por exemplo, aqui uma propriedade `name` é criada com todas as bandeiras de falsy:

`` `js run
Deixe o usuário = {};

*! *
Object.defineProperty (user, "name", {
valor: "John"
});
* /! *

Deixe descriptor = Object.getOwnPropertyDescriptor (usuário, 'nome');

alerta (JSON.stringify (descriptor, null, 2));
/ *
{
"valor": "João",
*! *
"writable": falso,
"enumerable": falso,
"configurável": falso
* /! *
}
* /
`` `

Compare com "normalmente criado" `user.name` acima: agora todas as bandeiras são falsas. Se não é o que desejamos, é melhor configurá-los para `true` em` descriptor`.

Agora, vejamos os efeitos das bandeiras pelo exemplo.

## Somente leitura

Vamos fazer 'user.name` somente leitura alterando o sinalizador `writable`:

`` `js run
Deixe o usuário = {
Nome: "John"
};

Object.defineProperty (user, "name", {
*! *
gravável: falso
* /! *
});

*! *
user.name = "Pete"; // Erro: Não é possível atribuir a "nome" de propriedade de leitura ...
* /! *
`` `

Agora, ninguém pode mudar o nome do nosso usuário, a menos que ele aplique seu próprio `defineProperty` para substituir o nosso.

Aqui está a mesma operação, mas para o caso em que uma propriedade não existe:

`` `js run
Deixe o usuário = {};

Object.defineProperty (user, "name", {
*! *
valor: "Pete",
// para novas propriedades precisam listar explicitamente o que é verdade
enumerable: true
configurável: verdadeiro
* /! *
});

alerta (user.name); // Pete
user.name = "Alice"; // Erro
`` `


## Não enumerável

Agora, vamos adicionar um `` toString 'personalizado para `usuário'.

Normalmente, um built-in `toString` para objetos não é enumerável, ele não aparece em` for..in`. Mas se adicionarmos `toString` do nosso, então, por padrão, ele aparece em` for..in`, como este:

`` `js run
Deixe o usuário = {
Nome: "John",
para sequenciar() {
devolva this.name;
}
};

// Por padrão, ambas as propriedades estão listadas:
para (permitir chave no usuário) alerta (chave); // nome, toString
`` `

Se não gostamos, podemos definir `enumerable: false`. Então, ele não aparecerá no loop `for..in`, assim como o incorporado:

`` `js run
Deixe o usuário = {
Nome: "John",
para sequenciar() {
devolva this.name;
}
};

Object.defineProperty (usuário, "toString", {
*! *
enumerável: falso
* /! *
});

*! *
// Agora o nosso toString desaparece:
* /! *
para (permitir chave no usuário) alerta (chave); // nome
`` `

Propriedades não enumeráveis ​​também são excluídas de `Object.keys`:

`` `js
alerta (Object.keys (usuário)); // nome
`` `

## Não configurável

O sinalizador configurável (`configurable: false`) às vezes é predefinido para objetos e propriedades incorporados.

Uma propriedade não configurável não pode ser excluída ou alterada com `defineProperty`.

Por exemplo, `Math.PI` é somente leitura, não enumerável e não configurável:

`` `js run
Deixe descriptor = Object.getOwnPropertyDescriptor (Matemática, 'PI');

alerta (JSON.stringify (descriptor, null, 2));
/ *
{
"valor": 3.141592653589793,
"writable": falso,
"enumerable": falso,
"configurável": falso
}
* /
`` `
Assim, um programador não consegue alterar o valor de `Math.PI` ou substituí-lo.

`` `js run
Math.PI = 3; // Erro

// delete Math.PI também não funcionará
`` `

Fazer uma propriedade não configurável é uma via unidirecional. Não podemos alterá-lo de volta, porque `defineProperty` não funciona em propriedades não configuráveis.

Aqui estamos fazendo `user.name` uma constante" para sempre selada ":

`` `js run
Deixe o usuário = {};

Object.defineProperty (user, "name", {
valor: "John",
gravável: falso
configurável: falso
});

*! *
// não será capaz de alterar user.name ou seus sinalizadores
// tudo isso não funcionará:
// user.name = "Pete"
// delete user.name
// defineProperty (user, "name", ...)
Object.defineProperty (user, "name", {writable: true}); // Erro
* /! *
`` `

`` `smart header =" Erros aparecem apenas em uso estrito "
No modo não-estrito, não ocorrem erros ao escrever para propriedades somente de leitura e tal. Mas a operação ainda não terá sucesso. As ações de violação de bandeira são ignoradas silenciosamente em termos não-estritos.
`` `

## Object.defineProperties

Existe um método [Object.defineProperties (obj, descritores)] (mdn: js / Object / defineProperties) que permite definir várias propriedades ao mesmo tempo.

A sintaxe é:

`` `js
Object.defineProperties (obj, {
prop1: descriptor1,
prop2: descriptor2
// ...
});
`` `

Por exemplo:

`` `js
Object.defineProperties (usuário, {
nome: {valor: "João", gravável: falso},
sobrenome: {valor: "Smith", gravável: falso},
// ...
});
`` `

Então, podemos configurar muitas propriedades de uma só vez.

## Object.getOwnPropertyDescriptors

Para obter todos os descritores de propriedade ao mesmo tempo, podemos usar o método [Object.getOwnPropertyDescriptors (obj)] (mdn: js / Object / getOwnPropertyDescriptors).

Juntamente com `Object.defineProperties`, ele pode ser usado como uma maneira de clonar um objeto com" flags-aware ":

`` `js
Deixe clone = Object.defineProperties ({}, Object.getOwnPropertyDescriptors (obj));
`` `

Normalmente, quando clonamos um objeto, usamos uma tarefa para copiar propriedades, como esta:

`` `js
para (deixe a chave no usuário) {
clone [chave] = usuário [chave]
}
`` `

... Mas isso não copia bandeiras. Então, se queremos um clone "melhor", então "Object.defineProperties" é o preferido.

Outra diferença é que `for..in 'ignora propriedades simbólicas, mas` Object.getOwnPropertyDescriptors` retorna * all * descritores de propriedades, incluindo simbólicos.

## Selando um objeto globalmente

Os descritores de propriedade funcionam ao nível das propriedades individuais.

Existem também métodos que limitam o acesso ao * objeto inteiro *:

[Object.preventExtensions (obj)] (mdn: js / Object / preventExtensions)
: Proíbe adicionar propriedades ao objeto.

[Object.seal (obj)] (mdn: js / Object / seal)
: Proíbe adicionar / remover propriedades, define para todas as propriedades existentes `configurable: false`.

[Object.freeze (obj)] (mdn: js / Object / freeze)
: Proíbe adicionar / remover / alterar propriedades, define para todas as propriedades existentes `configurável: falso, gravável: falso '.

E também há testes para eles:

[Object.isExtensible (obj)] (mdn: js / Object / isExtensible)
: Retorna `false` se adicionar propriedades for proibido, caso contrário` true`.

[Object.isSealed (obj)] (mdn: js / Object / isSealed)
: Retorna `true` se a adição / remoção de propriedades for proibida, e todas as propriedades existentes possuem` configurable: false`.

[Object.isFrozen (obj)] (mdn: js / Object / isFrozen)
: Retorna `true` se a adição / remoção / alteração de propriedades estiver proibida, e todas as propriedades atuais são` configuráveis: false, writable: false`.

Esses métodos raramente são usados ​​na prática.
