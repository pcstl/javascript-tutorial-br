
# Tipo de símbolo

Por especificação, as chaves de propriedades do objeto podem ser de tipo de string ou de tipo de símbolo. Não são números, não booleanos, apenas strings ou símbolos, esses dois tipos.

Até agora, só vimos cordas. Agora vejamos as vantagens que os símbolos podem nos dar.

[cortar]

## Símbolos

O valor "Símbolo" representa um identificador exclusivo.

Um valor desse tipo pode ser criado usando `Symbol ()`:

`` `js
// id é um novo símbolo
deixe sim = símbolo ();
`` `

Podemos também dar um símbolo a uma descrição (também chamada de um nome de símbolo), principalmente útil para fins de depuração:

`` `js
// id é um símbolo com a descrição "id"
deixe-o = símbolo ("id");
`` `

Os símbolos são garantidos como únicos. Mesmo que criemos muitos símbolos com a mesma descrição, eles são valores diferentes. A descrição é apenas um rótulo que não afeta nada.

Por exemplo, aqui estão dois símbolos com a mesma descrição - eles não são iguais:

`` `js run
deixe id1 = símbolo ("id");
let id2 = Symbol ("id");

*! *
alerta (id1 == id2); // falso
* /! *
`` `

Se você está familiarizado com Ruby ou outro idioma que também tenha algum tipo de "símbolos" - não seja equivocado. Os símbolos de JavaScript são diferentes.

`` `` warn header = "Os símbolos não se auto-convertem para uma string"
A maioria dos valores em JavaScript suporta conversão implícita em uma string. Por exemplo, podemos "alertar" praticamente qualquer valor, e isso funcionará. Os símbolos são especiais. Eles não se auto-convertem.

Por exemplo, esse "alerta" mostrará um erro:

`` `js run
deixe-o = símbolo ("id");
*! *
alerta (id); // TypeError: Não é possível converter um valor Symbol para uma string
* /! *
`` `

Se realmente queremos mostrar um símbolo, precisamos chamar `.toString ()`, como aqui:
`` `js run
deixe-o = símbolo ("id");
*! *
alerta (id.toString ()); // Símbolo (ID), agora funciona
* /! *
`` `

Isso é um "protetor de linguagem" contra o desastre, porque as cordas e os símbolos são fundamentalmente diferentes e, ocasionalmente, não podem converter um para o outro.
`` ``

## propriedades "escondidas"

Os símbolos nos permitem criar propriedades "escondidas" de um objeto, que nenhuma outra parte do código ocasionalmente pode acessar ou sobrescrever.

Por exemplo, se quisermos armazenar um "identificador" para o objeto `usuário`, podemos usar um símbolo como uma chave para ele:

`` `js run
deixe o usuário = {nome: "João"};
deixe-o = símbolo ("id");

User [id] = "ID Value";
alerta (user [id]); // podemos acessar os dados usando o símbolo como a chave
`` `

Qual é o benefício ao usar `Symbol (" id ")` sobre uma string `" id "`?

Vamos dar um exemplo um pouco mais para ver isso.

Imagine que outro script quer ter sua própria propriedade "id" dentro do `usuário ', para seus próprios fins. Essa pode ser outra biblioteca de JavaScript, então os scripts são completamente inconscientes uns dos outros.

Então esse script pode criar o seu próprio 'Símbolo ("id") `, como este:

`` `js
// ...
deixe-o = símbolo ("id");

usuário [id] = "Seu valor de identificação";
`` `

Não haverá conflito, porque os símbolos são sempre diferentes, mesmo que eles tenham o mesmo nome.

Agora note que se usássemos uma string `" id "` em vez de um símbolo para o mesmo propósito, então * * seria * um conflito:

`` `js run
deixe o usuário = {nome: "João"};

// nosso script usa propriedade "id"
user.id = "ID Value";

// ... se mais tarde outro script usa "id" para seus propósitos ...

user.id = "O seu valor id"
// estrondo! substituído! não queria prejudicar o colega, mas fez isso!
`` `

### Símbolos em um literal

Se quisermos usar um símbolo em um objeto literal, precisamos de colchetes.

Como isso:

`` `js
deixe-o = símbolo ("id");

Deixe o usuário = {
Nome: "John",
*! *
[id]: 123 // não apenas "id: 123"
* /! *
};
`` `
Isso porque precisamos do valor da variável `id` como a chave, não a string" id ".

### Símbolos são ignorados por ...

Propriedades simbólicas não participam do loop `for..in`.

Por exemplo:

`` `js run
deixe-o = símbolo ("id");
Deixe o usuário = {
Nome: "John",
idade: 30,
[ID]: 123
};

*! *
para (permitir chave no usuário) alerta (chave); // nome, idade (sem símbolos)
* /! *

// o acesso direto pelo símbolo funciona
alerta ("Direto:" + usuário [id]);
`` `

Isso faz parte do conceito geral de "esconder". Se outro script ou uma biblioteca percorrer nosso objeto, ele não acessará inesperadamente uma propriedade simbólica.

Em contraste, [Object.assign] (mdn: js / Object / assign) copia as propriedades da string e do símbolo:

`` `js run
deixe-o = símbolo ("id");
Deixe o usuário = {
[ID]: 123
};

Deixe clone = Object.assign ({}, usuário);

alerta (clone [id]); // 123
`` `

Não há paradoxo aqui. Isso é por design. A ideia é que, quando clonamos um objeto ou mesclamos objetos, geralmente queremos que as propriedades * all * sejam copiadas (incluindo símbolos como `id`).

`` `` cabeçalho inteligente = "As chaves de propriedades de outros tipos são coagidas a strings"
Nós podemos usar apenas strings ou símbolos como chaves em objetos. Outros tipos são convertidos em strings.

Por exemplo, um número `0` torna-se uma string` "0" `quando usado como uma chave de propriedade:

`` `js run
deixe obj = {
0: "teste" // mesmo que "0": "teste"
};

// ambos os alertas acessam a mesma propriedade (o número 0 é convertido na string "0")
alerta (obj ["0"]); // teste
alerta (obj [0]); // teste (mesma propriedade)
`` `
`` ``

## Símbolos globais

Como já vimos, geralmente todos os símbolos são diferentes, mesmo que eles tenham os mesmos nomes. Mas às vezes queremos que os mesmos símbolos citados sejam as mesmas entidades.

Por exemplo, diferentes partes do nosso aplicativo desejam acessar o símbolo `` id '' que significa exatamente a mesma propriedade.

Para conseguir isso, existe um * registro de símbolo global *. Podemos criar símbolos nele e acessá-los mais tarde, e garante que os acessos repetidos pelo mesmo nome retornem exatamente o mesmo símbolo.

Para criar ou ler um símbolo no registro, use `Symbol.for (key)`.

Essa chamada verifica o registro global e, se houver um símbolo descrito como `chave`, retorna-o, caso contrário, cria um novo símbolo` Símbolo (chave) `e o armazena no registro pela" chave "fornecida.

Por exemplo:

`` `js run
// lido do registro global
Deixe id = Symbol.for ("id"); // se o símbolo não existisse, ele é criado

// lê-lo novamente
Deixe idAgain = Symbol.for ("id");

// o mesmo símbolo
alerta (id === idAgain); // verdade
`` `

Símbolos dentro do registro são chamados * símbolos globais *. Se quisermos um símbolo de todo o aplicativo, acessível em todos os lugares no código - é por isso que eles são.

`` `cabeçalho inteligente =" Isso parece Ruby "
Em algumas linguagens de programação, como o Ruby, há um único símbolo por nome.

Em JavaScript, como podemos ver, é certo para símbolos globais.
`` `

### Symbol.key For

Para símbolos globais, não apenas `Symbol.for (key)` retorna um símbolo por nome, mas há uma chamada inversa: `Symbol.keyFor (sym)`, que faz o reverso: retorna um nome por um símbolo global.

Por exemplo:

`` `js run
Deixe sym = Symbol.for ("name");
deixe sym2 = Symbol.for ("id");

// pegue o nome do símbolo
alerta (Symbol.key For (símbolo)); // nome
alerta (Symbol.key For (sym2)); // identidade
`` `

O `Symbol.keyFor` internamente usa o registro de símbolos globais para procurar a chave do símbolo. Portanto, não funciona para símbolos não globais. Se o símbolo não for global, não será capaz de encontrá-lo e retornar 'indefinido'.

Por exemplo:

`` `js run
alerta (Symbol.key For (Symbol.for ("name"))); // nome, símbolo global

alerta (Symbol.key For (símbolo ("name2"))); // indefinido, o argumento não é um símbolo global
`` `

## Símbolos do sistema

Existem muitos símbolos de "sistema" que o JavaScript usa internamente e podemos usá-los para afinar vários aspectos de nossos objetos.

Eles estão listados na especificação na tabela [Símbolos conhecidos] (https://tc39.github.io/ecma262/#sec-well-known-symbols):

- `Symbol.hasInstance`
- `Symbol.isConcatSpreadable`
- `Symbol.iterator`
- `Symbol.toPrimitive`
- ...e assim por diante.

Por exemplo, `Symbol.toPrimitive` nos permite descrever o objeto para a conversão primitiva. Veremos seu uso em breve.

Outros símbolos também se tornarão familiares quando estudarmos os recursos de linguagem correspondentes.

## Resumo

`Symbol` é um tipo primitivo para identificadores exclusivos.

Os símbolos são criados com a chamada `Symbol ()` com uma descrição opcional.

Os símbolos são sempre valores diferentes, mesmo que tenham o mesmo nome. Se quisermos que os símbolos do mesmo nome sejam iguais, então devemos usar o registro global: `Symbol.for (key)` retorna (cria, se necessário) um símbolo global com `key` como o nome. Várias chamadas de `Symbol.for` retornam exatamente o mesmo símbolo.

Os símbolos têm dois casos de uso principais:

1. Propriedades do objeto "oculto".
Se quisermos adicionar uma propriedade a um objeto que "pertence" a outro script ou a uma biblioteca, podemos criar um símbolo e usá-lo como uma chave de propriedade. Uma propriedade simbólica não aparece em `for..in`, então não será ocasionalmente listado. Também não será acessado diretamente, porque outro script não possui nosso símbolo, portanto, não intervirá ocasionalmente em suas ações.

Então, podemos "esconder" ocultar algo em objetos que precisamos, mas outros não devem ver, usando propriedades simbólicas.

2. Existem muitos símbolos de sistema usados ​​pelo JavaScript que são acessíveis como `Symbol. *`. Podemos usá-los para alterar alguns comportamentos internos. Por exemplo, mais tarde, no tutorial, usaremos `Symbol.iterator` para [iterables] (info: iterable),` Symbol.toPrimitive` para configurar [conversão de objeto a primitivo] (info: objeto-toprimitive) e assim em.

Tecnicamente, os símbolos não estão 100% ocultos. Existe um método interno [Object.getOwnPropertySymbols (obj)] (mdn: js / Object / getOwnPropertySymbols) que nos permite obter todos os símbolos. Também há um método chamado [Reflect.ownKeys (obj)] (mdn: js / Reflect / ownKeys) que retorna * todas * chaves de um objeto, incluindo os simbólicos. Então eles não estão realmente escondidos. Mas a maioria das bibliotecas, métodos integrados e construções de sintaxe aderem a um acordo comum que eles são. E aquele que expressamente chama os métodos acima mencionados provavelmente entende bem o que está fazendo.
