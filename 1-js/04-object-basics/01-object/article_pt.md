
# Objetos

Como sabemos do capítulo <info: types>, existem sete tipos de idioma em JavaScript. Seis deles são chamados de "primitivos", porque seus valores contêm apenas uma única coisa (seja ela uma string ou um número ou qualquer outro).

Em contrapartida, os objetos são usados ​​para armazenar coleções com chaves de vários dados e entidades mais complexas. Em JavaScript, os objetos penetram quase todos os aspectos do idioma. Então devemos compreendê-los antes de ir em profundidade em qualquer outro lugar.

[cortar]

Um objeto pode ser criado com parênteses de figura `{...}` com uma lista opcional de * propriedades *. Uma propriedade é um par de "chave: valor", onde `chave` é uma string (também chamada de" nome da propriedade ") e o" valor "pode ​​ser qualquer coisa.

Podemos imaginar um objeto como um gabinete com arquivos assinados. Cada pedaço de dados é armazenado em seu arquivo pela chave. É fácil encontrar um arquivo pelo nome ou adicionar / remover um arquivo.

!] (object.png)

Um objeto vazio ("gabinete vazio") pode ser criado usando uma das duas sintaxes:

`` `js
deixe o usuário = novo Objeto (); // sintaxe do "construtor do objeto"
Deixe o usuário = {}; // sintaxe "objeto literal"
`` `

! [] (object-user-empty.png)

Normalmente, os parênteses de figura `{...}` são usados. Essa declaração é chamada de * objeto literal *.

## Literais e propriedades

Podemos colocar imediatamente algumas propriedades em `{...}` como pares "chave: valor":

`` `js
Deixe o usuário = {// um objeto
nome: "John", // por chave "nome" valor da loja "John"
idade: 30 // por valor de loja "idade" chave 30
};
`` `

Uma propriedade possui uma chave (também conhecida como "nome" ou "identificador") antes do ponto ``: `` e um valor à direita disso.

No objeto `usuário`, existem duas propriedades:

1. A primeira propriedade tem o nome `` name '' e o valor `` John ''.
2. O segundo tem o nome `` age '' e o valor `30`.

O objeto resultante do "usuário" pode ser imaginado como um gabinete com dois arquivos assinados com o nome "nome" e "idade".

! [objeto do usuário] (object-user.png)

Podemos adicionar, remover e ler arquivos a partir dele a qualquer momento.

Os valores de propriedade são acessíveis usando a notação de pontos:

`` `js
// Obter campos do objeto:
alerta (user.name); // John
alerta (user.age); // 30
`` `

O valor pode ser de qualquer tipo. Vamos adicionar um booleano:

`` `js
user.isAdmin = true;
`` `

! [objeto do usuário 2] (objeto-usuário-isadmin.png)

Para remover uma propriedade, podemos usar o operador `delete`:

`` `js
exclua user.age;
`` `

! [objeto do usuário 3] (object-user-delete.png)

Nós também podemos usar nomes de propriedade de multipalavras, mas eles devem ser citados:

`` `js
Deixe o usuário = {
Nome: "John",
idade: 30,
"gosta de pássaros": verdadeiro // o nome da propriedade de multiword deve ser citado
};
`` `

! [] (object-user-props.png)


`` `` cabeçalho inteligente = "rótulo da vírgula"
A última propriedade na lista pode terminar com uma vírgula:
`` `js
Deixe o usuário = {
Nome: "John",
idade: 30 *! *, * /! *
}
`` `
Isso é chamado de "vinda" ou "vinda". Facilita adicionar / remover / mover as propriedades, porque todas as linhas se tornam iguais.
`` ``

## Colchetes

Para propriedades de várias palavras, o acesso por pontos não funciona:

`` `js run
// isso daria um erro de sintaxe
user.likes birds = true
`` `

Isso porque o ponto requer a chave para ser um identificador de variável válido. Isto é: sem espaços e outras limitações.

Há uma "notação de suporte quadrado" alternativa que funciona com qualquer string:


`` `js run
Deixe o usuário = {};

// set
usuário ["gosta de pássaros"] = verdadeiro;

// obter
alerta (usuário ["gosta de pássaros"]); // verdade

// apagar
excluir usuário ["gosta de pássaros"];
`` `

Agora tudo está bem. Observe que a corda dentro dos colchetes é corretamente citada (qualquer tipo de cotação fará).

Os colchetes também fornecem uma maneira de obter o nome da propriedade como resultado de qualquer expressão - em oposição a uma string literal - como de uma variável da seguinte maneira:

`` `js
Deixe Key = "gosta de pássaros";

// mesmo que usuário ["gosta de pássaros"] = verdadeiro;
usuário [chave] = verdadeiro;
`` `

Aqui, a variável `chave` pode ser calculada em tempo de execução ou depende da entrada do usuário. E então usamos isso para acessar a propriedade. Isso nos dá uma grande flexibilidade. A notação de pontos não pode ser usada de forma semelhante.

Por exemplo:

`` `js run
Deixe o usuário = {
Nome: "John",
idade: 30
};

Deixe key = prompt ("O que você quer saber sobre o usuário?", "name");

// acesso por variável
alerta (usuário [chave]); // John (se entrar "nome")
`` `


### Propriedades calculadas

Podemos usar colchetes em um objeto literal. Isso é chamado de * propriedades calculadas *.

Por exemplo:

`` `js run
deixe fruit = prompt ("Qual fruta para comprar?", "apple");

deixe bag = {
*! *
[fruta]: 5, // o nome da propriedade é retirado da fruta variável
* /! *
};

alerta (bag.apple); // 5 se fruta = "maçã"
`` `

O significado de uma propriedade calculada é simples: `[fruit]` significa que o nome da propriedade deve ser retirado de `fruit`.

Então, se um visitante entra `` apple '', 'bag` se tornará `{apple: 5}`.

Essencialmente, isso funciona da mesma forma que:
`` `js run
deixe fruit = prompt ("Qual fruta para comprar?", "apple");
deixe bag = {};

// pegue o nome da propriedade da variável do fruto
bolsa [fruta] = 5;
`` `

... Mas parece melhor.

Podemos usar expressões mais complexas dentro de colchetes:

`` `js
deixe fruta = 'maçã';
deixe bag = {
[frutas + 'Computadores']: 5 // bag.appleComputers = 5
};
`` `

Os colchetes são muito mais poderosos do que a notação de pontos. Eles permitem qualquer nome de propriedade e variáveis. Mas eles também são mais pesados ​​para escrever.

Assim, na maioria das vezes, quando nomes de propriedade são conhecidos e simples, o ponto é usado. E se precisamos de algo mais complexo, então nós alternamos para colchetes.



`` `` cabeçalho inteligente = "As palavras reservadas são permitidas como nomes de propriedade"
Uma variável não pode ter um nome igual a uma das palavras reservadas ao idioma, como "para", "deixar", "retornar" etc.

Mas, para uma propriedade de objeto, não existe essa restrição. Qualquer nome está bem:

`` `js run
deixe obj = {
para: 1,
Deixe: 2,
retorno: 3
}

alerta (obj.for + obj.let + obj.return); // 6
`` `

Basicamente, qualquer nome é permitido, mas há um especial: `` __proto __ '' que recebe tratamento especial por motivos históricos. Por exemplo, não podemos configurá-lo para um valor não-objeto:

`` `js run
deixe obj = {};
obj .__ proto__ = 5;
alerta (obj .__ proto__); // [object Object Object], não funcionou como pretendido
`` `

Como vemos do código, a atribuição para um primitivo `5 'é ignorada.

Isso pode ser uma fonte de erros e até mesmo de vulnerabilidades se tenguemos a intenção de armazenar pares de valores-chave arbitrários em um objeto e permitir que um visitante especifique as chaves. Nesse caso, o visitante pode escolher "__proto__" como a chave e a lógica de atribuição será arruinada (como mostrado acima).

Há outra estrutura de dados [Mapa] (info: map-set-weakmap-weakset), que aprenderemos no capítulo <info: map-set-weakmap-weakset>, que suporta chaves arbitrárias. Também há uma maneira de fazer objetos tratar `__proto__` como uma propriedade regular, mas primeiro precisamos saber mais sobre objetos para compreendê-lo.
`` ``


## Taquigrafia do valor da propriedade

Em código real, geralmente usamos variáveis ​​existentes como valores para nomes de propriedade.

Por exemplo:

`` `js run
função makeUser (nome, idade) {
Retorna {
nome: nome,
idade: idade
// ... outras propriedades
};
}

deixe user = makeUser ("John", 30);
alerta (user.name); // John
`` `

No exemplo acima, as propriedades têm os mesmos nomes que as variáveis. O caso de uso de fazer uma propriedade de uma variável é tão comum, que existe uma taquigrafia de valor de propriedade especial * para torná-la mais curta.

Em vez de `name: name`, podemos simplesmente escrever` name`, assim:

`` `js
função makeUser (nome, idade) {
*! *
Retorna {
nome, // mesmo que nome: nome
idade // mesmo que idade: idade
// ...
};
* /! *
}
`` `

Podemos usar propriedades normais e shorthands no mesmo objeto:

`` `js
Deixe o usuário = {
nome, // mesmo que nome: nome
idade: 30
};
`` `

## verificação de existência

Um recurso de objetos notável é que é possível acessar qualquer propriedade. Não haverá nenhum erro se a propriedade não existir! Acessar uma propriedade não existente simplesmente retorna `undefined`. Ele fornece uma maneira muito comum de testar se a propriedade existe - para obtê-lo e comparar vs indefinido:

`` `js run
Deixe o usuário = {};

alerta (user.noSuchProperty === indefinido); // verdadeiro significa "nenhuma propriedade desse tipo"
`` `

Existe também um operador especial `" em "` para verificar a existência de uma propriedade.

A sintaxe é:
`` `js
"chave" no objeto
`` `

Por exemplo:

`` `js run
deixe o usuário = {nome: "João", idade: 30};

alerta ("idade" no usuário); // true, user.age existe
alerta ("blabla" no usuário); // falso, user.blabla não existe
`` `

Por favor, note que no lado esquerdo do `in` deve haver um * nome da propriedade *. Isso geralmente é uma string citada.

Se omitimos aspas, isso significaria uma variável contendo o nome real a ser testado. Por exemplo:

`` `js run
Deixe o usuário = {idade: 30};

Deixe a chave = "idade";
alerta (*! * key * /! * no usuário); // true, leva o nome da chave e verifica essa propriedade
`` `

`` `` cabeçalho inteligente = "Usando \" em \ "para propriedades que armazenam` indefinido` "
Normalmente, a comparação rigorosa `" === indefinido "` checa funciona bem. Mas há um caso especial quando ele falha, mas `" in "` funciona corretamente.

É quando existe uma propriedade de objeto, mas armazena `indefinido ':

`` `js run
deixe obj = {
teste: indefinido
};

alerta (obj.test); // é indefinido, então - nenhuma propriedade desse tipo?


`` `


No código acima, a propriedade `obj.test` existe tecnicamente. Então, o operador `in` funciona corretamente.

Situações como esta acontecem muito raramente, porque o `indefinido 'geralmente não é atribuído. Nós usamos principalmente null para valores "desconhecidos" ou "vazios". Então, o operador `in` é um convidado exótico no código.
`` ``


## O loop "for..in"

Para caminhar sobre todas as chaves de um objeto, existe uma forma especial do loop: `for..in`. Esta é uma coisa completamente diferente da construção `for (;;)` que estudamos antes.

A sintaxe:

`` `js
para (chave no objeto) {
// executa o corpo para cada chave entre as propriedades do objeto
}
`` `

Por exemplo, vamos exibir todas as propriedades do `usuário ':

`` `js run
Deixe o usuário = {
Nome: "John",
idade: 30,
isAdmin: true
};

para (deixe a chave no usuário) {
// chaves
alerta (chave); // nome, idade, isAdmin
// valores para as chaves
alerta (usuário [chave]); // John, 30, verdadeiro
}
`` `

Observe que todas as construções "para" nos permitem declarar a variável de loop dentro do loop, como `let key` aqui.

Além disso, poderíamos usar outro nome de variável aqui em vez de `chave`. Por exemplo, `" for (let prop in obj) "também é amplamente utilizado.


### Ordenado como um objeto

Os objetos são pedidos? Em outras palavras, se encurralarmos um objeto, obtemos todas as propriedades na mesma ordem em que são adicionados nela? Podemos confiar nisso?

A resposta curta é: "ordenados de forma especial": as propriedades inteiras são classificadas, outras aparecem na ordem de criação. Os detalhes seguem.

Como exemplo, consideremos um objeto com os códigos do telefone:

`` `js run
Deixe códigos = {
"49": "Alemanha",
"41": "Suíça",
"44": "Grã-Bretanha",
// ..,
"1": "EUA"
};

*! *
para (deixe código em códigos) {
alerta (código); // 1, 41, 44, 49
}
* /! *
`` `

O objeto pode ser usado para sugerir uma lista de opções para o usuário. Se estamos fazendo um site principalmente para a audiência alemã, provavelmente queremos que o '49' seja o primeiro.

Mas se corremos o código, vemos uma imagem totalmente diferente:

- EUA (1) vai primeiro
- então Suíça (41) e assim por diante.

Os códigos do telefone vão na ordem ordenada ascendente, porque eles são inteiros. Então, vemos `1, 41, 44, 49`.

`` `` cabeçalho inteligente = "propriedades inteiras? O que é isso?"
O termo "propriedade inteira" aqui significa uma string que pode ser convertida em-e-de um inteiro sem uma alteração.

Então, "49" é um nome de propriedade de número inteiro, porque quando é transformado em um número inteiro e vice-versa, ele ainda é o mesmo. Mas "+49" e "1.2" não são:

`` `js run
// Math.trunc é uma função interna que remove a parte decimal
alerta (String (Math.trunc (Number ("49")))); // "49", mesmo, propriedade inteira
alerta (String (Math.trunc (Número ("+ 49")))); // "49", não mesmo ⇒ não propriedade inteira
alerta (String (Math.trunc (Número ("1.2")))); // "1", não o mesmo ⇒ não propriedade inteira
`` `
`` ``

... Por outro lado, se as teclas não são inteiras, elas estão listadas na ordem de criação, por exemplo:

`` `js run
Deixe o usuário = {
Nome: "John",
sobrenome: "Smith"
};
user.age = 25; // adicione mais um

*! *
// propriedades não inteiras estão listadas na ordem de criação
* /! *
para (permitir suporte no usuário) {
alerta (suporte); // nome, sobrenome, idade
}
`` `

Portanto, para corrigir o problema com os códigos do telefone, podemos "enganar" ao fazer os códigos não inteiros. Adicionando um sinal de mais `" + "` antes de cada código é suficiente.

Como isso:

`` `js run
Deixe códigos = {
"+49": "Alemanha",
"+41": "Suíça",
"+44": "Grã-Bretanha",
// ..,
"+1": "EUA"
};

para (deixe código em códigos) {
alerta (+ código); // 49, 41, 44, 1
}
`` `

Agora funciona como pretendido.

## Copiando por referência

Uma das diferenças fundamentais de objetos vs primitivas é que eles são armazenados e copiados "por referência".

Valores primitivos: strings, números, booleanos - são atribuídos / copiados "como um valor inteiro".

Por exemplo:

`` `js
Deixe a mensagem = "Olá!";
let phrase = message;
`` `

Como resultado, temos duas variáveis ​​independentes, cada uma está armazenando a seqüência de caracteres `" Olá! "`.

!] [] (variable-copy-value.png)

Os objetos não são assim.

** Uma variável armazena não o próprio objeto, mas é "endereço na memória", ou seja, "uma referência". **

Aqui está a foto para o objeto:

`` `js
Deixe o usuário = {
Nome: "John"
};
`` `

! [] (variable-contains-reference.png)

Aqui, o objeto é armazenado em algum lugar na memória. E a variável `usuário` tem uma" referência ".

** Quando uma variável de objeto é copiada - a referência é copiada, o objeto não está duplicado. **

Se nós imaginarmos um objeto como um armário, então uma variável é uma chave para ele. Copiar uma variável duplica a chave, mas não o próprio gabinete.

Por exemplo:

`` `js no-embellecer
deixe o usuário = {nome: "João"};

Deixe admin = user; // copie a referência
`` `

Agora, temos duas variáveis, cada uma com referência ao mesmo objeto:

!] [] (variable-copy-reference.png)

Podemos usar qualquer variável para acessar o gabinete e modificar seus conteúdos:

`` `js run
Deixe o usuário = {nome: 'João'};

Deixe admin = user;

*! *
admin.name = 'Pete'; // alterado pela referência "admin"
* /! *

alerta (*! * user.name * /! *); // 'Pete', as mudanças são vistas a partir da referência do "usuário"
`` `

O exemplo acima demonstra que existe apenas um objeto. Como se tivéssemos um gabinete com duas chaves e usássemos um deles (`admin`) para entrar nisso. Então, se usarmos mais tarde a outra chave (`usuário '), veríamos mudanças.

### Comparação por referência

A igualdade `==` e a igualdade rigorosa '=== `operadores para objetos funcionam exatamente o mesmo.

** Dois objetos são iguais somente se forem o mesmo objeto. **

Por exemplo, duas variáveis ​​referenciam o mesmo objeto, elas são iguais:

`` `js run
deixe a = {};
deixe b = a; // copie a referência

alerta (a == b); // true, ambas as variáveis ​​fazem referência ao mesmo objeto
alerta (a === b); // verdade
`` `

E aqui dois objetos independentes não são iguais, mesmo que ambos estejam vazios:

`` `js run
deixe a = {};
deixe b = {}; // dois objetos independentes

alerta (a == b); // falso
`` `

Para comparações como `obj1> obj2` ou para uma comparação contra um primitivo` obj == 5 ', os objetos são convertidos em primitivas. Estudaremos como as conversões de objetos funcionam muito em breve, mas para dizer a verdade, tais comparações são necessárias muito raramente e geralmente são resultado de um erro de codificação.

### objeto Const

Um objeto declarado como `const` * pode * ser alterado.

Por exemplo:

`` `js run
const user = {
Nome: "John"
};

*! *
user.age = 25; // (*)
* /! *

alerta (user.age); // 25
`` `

Pode parecer que a linha `(*)` causaria um erro, mas não, não há problema. Isso porque `const` corrige o valor do` usuário 'em si. E aqui `usuário` armazena a referência ao mesmo objeto o tempo todo. A linha `(*)` vai * dentro * do objeto, ele não reatribui `usuário '.

O `const 'daria um erro se tentarmos configurar` user` para outra coisa, por exemplo:

`` `js run
const user = {
Nome: "John"
};

*! *
// Erro (não pode reatribuir usuário)
* /! *
usuário = {
Nome: "Pete"
};
`` `

... Mas e se quisermos fazer propriedades constantes do objeto? Então, `` user.age = 25` daria um erro. Isso também é possível. Vamos abordá-lo no capítulo <info: property-descriptors>.

## Clonagem e fusão, Object.assign

Então, copiar uma variável de objeto cria uma referência mais para o mesmo objeto.

Mas e se precisarmos duplicar um objeto? Crie uma cópia independente, um clone?

Isso também é possível, mas um pouco mais difícil, porque não existe um método interno para isso em JavaScript. Na verdade, raramente é necessário. Copiar por referência é bom na maioria das vezes.

Mas se realmente queremos isso, precisamos criar um novo objeto e replicar a estrutura do existente, iterando sobre suas propriedades e copiando-as no nível primitivo.

Como isso:

`` `js run
Deixe o usuário = {
Nome: "John",
idade: 30
};

*! *
Deixe clone = {}; // o novo objeto vazio

// vamos copiar todas as propriedades do usuário nela
para (deixe a chave no usuário) {
clone [key] = user [key];
}
* /! *

// agora o clone é um clone completamente independente
clone.name = "Pete"; // mudou os dados nele

alerta (user.name); // ainda John no objeto original
`` `

Também podemos usar o método [Object.assign] (mdn: js / Object / assign) para isso.

A sintaxe é:

`` `js
Object.assign (dest [, src1, src2, src3 ...])
`` `

- Argumentos `dest`, e` src1, ..., srcN` (podem ser tantos quanto necessário) são objetos.
- Copia as propriedades de todos os objetos `src1, ..., srcN` em` dest`. Em outras palavras, as propriedades de todos os argumentos a partir do 2º são copiadas para o 1º. Então ele retorna `dest`.

Por exemplo, podemos usá-lo para mesclar vários objetos em um:
`` `js
deixe o usuário = {nome: "João"};

Deixe permissions1 = {canView: true};
Deixe permissions2 = {canEdit: true};

*! *
// copia todas as propriedades de permissions1 e permissions2 para o usuário
Object.assign (user, permissions1, permissions2);
* /! *

// now user = {name: "John", canView: true, canEdit: true}
`` `

Se o objeto de recebimento (`usuário ') já tiver a mesma propriedade com nome, ele será substituído:

`` `js
deixe o usuário = {nome: "João"};

// sobrescreva o nome, adicione isAdmin
Object.assign (usuário, {nome: "Pete", isAdmin: true});

// agora usuário = {nome: "Pete", isAdmin: true}
`` `

Também podemos usar `Object.assign` para substituir o loop por simples clonagem:

`` `js
Deixe o usuário = {
Nome: "John",
idade: 30
};

*! *
Deixe clone = Object.assign ({}, usuário);
* /! *
`` `

Ele copia todas as propriedades do `usuário 'no objeto vazio e retorna. Na verdade, o mesmo que o loop, mas mais curto.

Até agora, assumimos que todas as propriedades do `usuário 'são primitivas. Mas as propriedades podem ser referências a outros objetos. O que fazer com eles?

Como isso:
`` `js run
Deixe o usuário = {
Nome: "John",
tamanhos: {
altura: 182,
largura: 50
}
};

alerta (user.sizes.height); // 182
`` `

Agora, não é suficiente copiar `clone.sizes = user.sizes`, porque o` user.sizes` é um objeto, ele será copiado por referência. Então `clone` e` user` vão compartilhar os mesmos tamanhos:

Como isso:
`` `js run
Deixe o usuário = {
Nome: "John",
tamanhos: {
altura: 182,
largura: 50
}
};

Deixe clone = Object.assign ({}, usuário);

alerta (user.sizes === clone.sizes); // verdadeiro, mesmo objeto

// usuário e clone compartilham tamanhos
user.sizes.width ++; // muda uma propriedade de um só lugar
alerta (clone.sizes.width); // 51, veja o resultado do outro
`` `

Para consertar isso, devemos usar o loop de clonagem que examina cada valor de `usuário [chave]` e, se for um objeto, replica também a estrutura. Isso é chamado de "clonagem profunda".

Existe um algoritmo padrão para a clonagem profunda que lida com o caso acima e casos mais complexos, denominados [algoritmo de clonagem estruturado] (https://w3c.github.io/html/infrastructure.html#internal-structured-cloning-algorithm). Para não reinventar a roda, podemos usar uma implementação funcional da biblioteca de JavaScript [lodash] (https://lodash.com), o método é chamado [_.cloneDeep (obj)] (https: // lodash.com/docs#cloneDeep).



## Resumo

Os objetos são arrays associativos com vários recursos especiais.

Eles armazenam propriedades (pares de valores-chave), onde:
- As chaves de propriedade devem ser strings ou símbolos (geralmente strings).
- Os valores podem ser de qualquer tipo.

Para acessar uma propriedade, podemos usar:
- A notação de ponto: `obj.property`.
- Notação de colchetes `obj [" propriedade "]`. Os suportes quadrados permitem tirar a chave de uma variável, como `obj [varWithKey]`.

Operadores adicionais:
- Para excluir uma propriedade: `delete obj.prop`.
- Para verificar se existe uma propriedade com a chave dada: `" chave "no obj`.
- Para iterar sobre um objeto: `para (deixe a tecla em obj)` loop.

Os objetos são atribuídos e copiados por referência. Em outras palavras, uma variável não armazena o "valor do objeto", mas uma "referência" (endereço na memória) para o valor. Então, copiando essa variável ou passando como um argumento de função copia essa referência, não o objeto. Todas as operações através de referências copiadas (como adicionar / remover propriedades) são realizadas no mesmo objeto.

Para fazer uma "cópia real" (um clone), podemos usar `Object.assign` ou [_.cloneDeep (obj)] (https://lodash.com/docs#cloneDeep).

O que estudamos neste capítulo é chamado de "objeto simples", ou apenas 'Objeto'.

Há muitos outros tipos de objetos em JavaScript:

- `Array` para armazenar cobranças de dados ordenadas,
- `Date` para armazenar as informações sobre a data e a hora,
- `Error` para armazenar as informações sobre um erro.
- ...E assim por diante.

Eles têm suas características especiais que estudaremos mais tarde. Às vezes, as pessoas dizem algo como "tipo de matriz" ou "Tipo de data", mas, formalmente, não são tipos próprios, mas pertencem a um único tipo de "objeto". E eles estendem-no de várias maneiras.

Objetos em JavaScript são muito poderosos. Aqui, acabamos de arranhar a superfície do tópico que é realmente enorme. Nós estaremos trabalhando com objetos e aprendendo mais sobre eles em outras partes do tutorial.
