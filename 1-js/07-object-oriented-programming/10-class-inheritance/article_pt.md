
# Classe de herança, super

As aulas podem se estender. Existe uma sintaxe agradável, tecnicamente baseada na herança prototípica.

Para herdar de outra classe, devemos especificar `" extends "` e a classe pai antes dos brackets `{..}`.

[cortar]

Aqui `Rabbit` herda de` Animal`:

`` `js run
classe Animal {

construtor (nome) {
this.peed = 0;
this.name = name;
}

executar (velocidade) {
esta velocidade + = velocidade;
alerta (`$ {this.name} é executado com a velocidade $ {this.speed} .`);
}

Pare() {
this.peed = 0;
alerta (`$ {this.name} parado.`);
}

}

*! *
// Herdar de Animal
Classe Rabbit extends Animal {
ocultar() {
alerta (`$ {this.name} esconde!`);
}
}
* /! *

deixe coelho = coelho novo ("coelho branco");

rabbit.run (5); // White Rabbit corre com velocidade 5.
rabbit.hide (); // Coelho branco se esconde!
`` `

A palavra-chave `extends` realmente adiciona uma referência` [[Prototype]] `de` Rabbit.prototype` para Animal.prototype`, assim como você espera, e como já vimos antes.

! [] (animal-rabbit-extendeds.png)

Então, agora `rabbit` tem acesso tanto aos seus próprios métodos quanto aos métodos de` Animal`.

`` `` smart header = "Qualquer expressão é permitida após` extends` "
A sintaxe de classe permite especificar não apenas uma classe, mas qualquer expressão após `se estende '.

Por exemplo, uma chamada de função que gera a classe pai:

`` `js run
função f (frase) {
classe de retorno {
sayHi () {alert (frase)}
}
}

*! *
classe O usuário estende f ("Olá") {}
* /! *

novo usuário (). sayHi (); // Olá
`` `
Aqui `class User` herda do resultado de` f ("Hello") `.

Isso pode ser útil para padrões de programação avançados quando usamos funções para gerar classes dependendo de muitas condições e podem herdar delas.
`` ``

## Substituindo um método

Agora vamos avançar e substituir um método. A partir de agora, `Rabbit` herda o método` stop` que define `this.speed = 0` de` Animal`.

Se especificarmos o nosso próprio `stop` em` Rabbit`, então ele será usado em vez disso:

`` `js
Classe Rabbit extends Animal {
Pare() {
// ... isso será usado para rabbit.stop ()
}
}
`` `


... Mas, geralmente, não queremos substituir totalmente um método pai, mas sim construir em cima dele, ajustar ou ampliar sua funcionalidade. Nós fazemos algo em nosso método, mas ligue para o método pai antes / depois dele ou no processo.

As classes fornecem uma palavra-chave "super" para isso.

- `super.method (...)` para chamar um método pai.
- `super (...)` para chamar um construtor pai (somente dentro do nosso construtor).

Por exemplo, deixe nosso auto-couro de coelho quando parado:

`` `js run
classe Animal {

construtor (nome) {
this.peed = 0;
this.name = name;
}

executar (velocidade) {
esta velocidade + = velocidade;
alerta (`$ {this.name} é executado com a velocidade $ {this.speed} .`);
}

Pare() {
this.peed = 0;
alerta (`$ {this.name} parado.`);
}

}

Classe Rabbit extends Animal {
ocultar() {
alerta (`$ {this.name} esconde!`);
}

*! *
Pare() {
super.stop (); // chamada parada dos pais
this.hide (); // e então esconda
}
* /! *
}

deixe coelho = coelho novo ("coelho branco");

rabbit.run (5); // White Rabbit corre com velocidade 5.
coelho.stop (); // White Rabbit parou. Coelhos de coelho branco!
`` `

Agora `Rabbit` tem o método` stop` que chama o pai `super.stop ()` no processo.

`` `` cabeçalho inteligente = "as funções de seta não têm` super` "
Como mencionado no capítulo <info: arrow-functions>, as funções de seta não possuem `super`.

Se tiver acesso, é retirado da função externa. Por exemplo:
`` `js
Classe Rabbit extends Animal {
Pare() {
setTimeout (() => super.stop (), 1000); // liga para pai após 1sec
}
}
`` `

O `super` na função de seta é o mesmo que em` stop () `, então funciona como pretendido. Se especificarmos uma função "regular" aqui, haveria um erro:

`` `js
// super inesperado
setTimeout (function () {super.stop ()}, 1000);
`` `
`` ``


## Substituindo o construtor

Com os construtores, as coisas são um pouco complicadas.

Até agora, `Rabbit` não tinha seu próprio` construtor '.

De acordo com a [especificação] (https://tc39.github.io/ecma262/#sec-runtime-semantics-classdefinitionevaluation), se uma classe estender outra classe e não tiver um 'construtor', o seguinte `construtor 'é gerado :

`` `js
Classe Rabbit extends Animal {
// gerado para estender as aulas sem construtores próprios
*! *
construtor (... args) {
super (... args);
}
* /! *
}
`` `

Como podemos ver, ele basicamente chama o "construtor" pai passando todos os argumentos. Isso acontece se não escrevemos um construtor nosso.

Agora vamos adicionar um construtor personalizado para `Rabbit`. Ele especificará o `earLength` além do` name`:

`` `js run
classe Animal {
construtor (nome) {
this.peed = 0;
this.name = name;
}
// ...
}

Classe Rabbit extends Animal {

*! *
construtor (nome, earLength) {
this.peed = 0;
this.name = name;
this.earLength = earLength;
}
* /! *

// ...
}

*! *
// Não funciona!
deixe coelho = coelho novo ("coelho branco", 10); // Erro: isso não está definido.
* /! *
`` `

Ups! Temos um erro. Agora não podemos criar coelhos. O que deu errado?

A resposta curta é: os construtores nas classes de herança devem chamar `super (...)`, e (!) Fazê-lo antes de usar `this`.

...Mas por que? Oque esta acontecendo aqui? Na verdade, o requisito parece estranho.

Claro, há uma explicação. Vamos entrar em detalhes, então você realmente entenderia o que está acontecendo.

Em JavaScript, há uma distinção entre uma "função de construtor de uma classe de herança" e todas as demais. Em uma classe de herança, a função de construtor correspondente é rotulada com uma propriedade interna especial `[[ConstructorKind]]:" derivado "`.

A diferença é:

- Quando um construtor normal é executado, ele cria um objeto vazio como "isso" e continua com ele.
- Mas quando um construtor derivado é executado, não o faz. Ele espera que o construtor pai faça esse trabalho.

Então, se estamos fazendo o nosso próprio construtor, devemos chamar `super`, pois, de outro modo, o objeto com essa referência a ele não será criado. E teremos um erro.

Para `Rabbit` para trabalhar, precisamos chamar` super () `antes de usar` this`, como aqui:

`` `js run
classe Animal {

construtor (nome) {
this.peed = 0;
this.name = name;
}

// ...
}

Classe Rabbit extends Animal {

construtor (nome, earLength) {
*! *
super (nome);
* /! *
this.earLength = earLength;
}

// ...
}

*! *
// agora está bem
deixe coelho = coelho novo ("coelho branco", 10);
alerta (rabbit.name); // Coelho branco
alerta (comprimento do coelho); // 10
* /! *
`` `


## Super: internals, [[HomeObject]]

Vamos ficar um pouco mais profundos sob o capô do `super`. Veremos algumas coisas interessantes, por sinal.

Em primeiro lugar, de tudo o que aprendemos até agora, é impossível que o `super` funcione.

Sim, de fato, vamos nos perguntar, como isso poderia funcionar tecnicamente? Quando um método de objeto é executado, ele obtém o objeto atual como `this`. Se chamamos `super.method ()` então, como recuperar o `método`? Naturalmente, precisamos tomar o `método 'do protótipo do objeto atual. Como, tecnicamente, nós (ou um mecanismo de JavaScript) podemos fazê-lo?

Talvez possamos obter o método de `[[Protótipo]]` de `isso ', como` este .__ proto __. Method`? Infelizmente, isso não funciona.

Vamos tentar fazê-lo. Sem classes, usando objetos simples por uma questão de simplicidade.

Aqui, `rabbit.eat ()` deve chamar o método `animal.eat ()` do objeto pai:

`` `js run
deixe animal = {
Nome: "Animal",
comer () {
alerta (this.name + "come");
}
};

deixe coelho = {
__proto__: animal,
Nome: "Coelho",
comer () {
*! *
// é assim que super.eat () poderia funcionar presumivelmente
este. proto __. eat.call (this); // (*)
* /! *
}
};

rabbit.eat (); // Coelho come.
`` `

Na linha `(*)` tomamos `eat` do protótipo (` animal`) e chamamos-o no contexto do objeto atual. Observe que `.call (this)` é importante aqui, porque um simples `this .__ proto __. Eat ()` executaria o pai `eat` no contexto do protótipo, e não o objeto atual.

E no código acima, ele funciona de acordo com o objetivo: temos o "alerta" correto.

Agora vamos adicionar mais um objeto à cadeia. Veremos como as coisas se quebram:

`` `js run
deixe animal = {
Nome: "Animal",
comer () {
alerta (this.name + "come");
}
};

deixe coelho = {
__proto__: animal,
comer () {
// ... rejeitar o estilo do coelho e chamar o método dos pais (animal)
este. proto __. eat.call (this); // (*)
}
};

Deixe LongEar = {
__proto__: coelho
comer () {
// ... faça algo com orelhas compridas e conecte o método dos pais (coelho)
este. proto __. eat.call (this); // (**)
}
};

*! *
longEar.eat (); // Erro: o tamanho máximo da pilha de chamadas excedeu
* /! *
`` `

O código não funciona mais! Podemos ver o erro tentando chamar `longEar.eat ()`.

Pode não ser tão óbvio, mas se rastrear o chamado `longEar.eat ()`, então podemos ver o porquê. Em ambas as linhas `(*)` e `(**)` o valor de `this` é o objeto atual (` longEar`). Isso é essencial: todos os métodos de objeto obtêm o objeto atual como `this`, não um protótipo ou algo assim.

Então, em ambas as linhas `(*)` e `(**)` o valor de `this .__ proto__` é exatamente o mesmo:` rabbit`. Ambos chamam `rabbit.eat` sem subir a corrente no loop infinito.

Aqui está a imagem do que acontece:

! [] (this-super-loop.png)

1. Dentro de `longEar.eat ()`, a linha `(**)` chama `rabbit.eat` fornecendo isso com` this = longEar`.
`` `js
// inside longEar.eat () temos isto = longEar
este .__ proto __. eat.call (this) // (**)
// torna-se
LongEar .__ proto __. eat.call (este)
// isso é
rabbit.eat.call (this);
`` `
2. Então, na linha `(*)` de `rabbit.eat`, gostaríamos de passar a chamada ainda maior na cadeia, mas` this = longEar`, então `this .__ proto __. Eat` é novamente` Rabbit.eat`!

`` `js
// dentro de rabbit.eat () também temos isso = longEar
this .__ proto __. eat.call (this) // (*)
// torna-se
LongEar .__ proto __. eat.call (este)
// ou (novamente)
rabbit.eat.call (this);
`` `

3. ... Então `rabbit.eat` chama-se no loop infinito, porque não pode ascender mais.

O problema não pode ser resolvido usando `this` sozinho.

### `[[HomeObject]]`

Para fornecer a solução, o JavaScript adiciona mais uma propriedade interna especial para funções: `[[HomeObject]]`.

** Quando uma função é especificada como um método de classe ou objeto, a propriedade `[[HomeObject]]` torna-se esse objeto. **

Isso realmente viola a idéia de funções "não vinculadas", porque os métodos se lembram de seus objetos. E `[[HomeObject]]` não pode ser alterado, então esse limite é para sempre. Então, essa é uma mudança muito importante no idioma.

Mas esta mudança é segura. `[[HomeObject]]` é usado apenas para chamar métodos pai em `super`, para resolver o protótipo. Portanto, não quebra a compatibilidade.

Vamos ver como funciona para `super` - novamente, usando objetos simples:

`` `js run
deixe animal = {
Nome: "Animal",
eat () {// [[HomeObject]] == animal
alerta (this.name + "come");
}
};

deixe coelho = {
__proto__: animal,
Nome: "Coelho",
eat () {// [[HomeObject]] == coelho
super.eat ();
}
};

Deixe LongEar = {
__proto__: coelho
Nome: "Long Ear",
eat () {// [[HomeObject]] == longEar
super.eat ();
}
};

*! *
longEar.eat (); // Ear Ear long come.
* /! *
`` `

Todo método lembra seu objeto na propriedade interna `[[HomeObject]]`. Então `super` usa-o para resolver o protótipo original.

`[[HomeObject]]` é definido para métodos definidos tanto em classes como em objetos simples. Mas, para objetos, os métodos devem ser especificados exatamente da maneira especificada: como `method ()`, não como `" method: function () "`.

No exemplo abaixo, uma sintaxe sem método é usada para comparação. `[[HomeObject]]` propriedade não está definida e a herança não funciona:

`` `js run
deixe animal = {
eat: function () {// deve ser a sintaxe curta: eat () {...}
// ...
}
};

deixe coelho = {
__proto__: animal,
eat: function () {
super.eat ();
}
};

*! *
rabbit.eat (); // Erro ao ligar para o super (porque não há [[HomeObject]])
* /! *
`` `

## Métodos estáticos e herança

A sintaxe `class` suporta herança para propriedades estáticas também.

Por exemplo:

`` `js run
classe Animal {

construtor (nome, velocidade) {
this.peed = speed;
this.name = name;
}

executar (velocidade = 0) {
esta velocidade + = velocidade;
alerta (`$ {this.name} é executado com a velocidade $ {this.speed} .`);
}

comparação estática (animalA, animalB) {
retorno da velocidade animal. velocidade animal.
}

}

// Herdar de Animal
Classe Rabbit extends Animal {
ocultar() {
alerta (`$ {this.name} esconde!`);
}
}

deixe coelhos = [
Coelho novo ("Coelho Branco", 10),
Coelho novo ("coelho preto", 5)
];

rabbits.sort (Rabbit.compare);

coelhos [0] .run (); // Black Rabbit corre com velocidade 5.
`` `

Agora podemos chamar `Rabbit.compare 'assumindo que o" Animal.compare "herdado será chamado.

Como funciona? Novamente, usando protótipos. Como você já deve ter adivinhado, estende também dá `Rabbit` o` [[Prototype]] `reference to` Animal`.


! [] (animal-rabbit-static.png)

Então, a função `Rabbit` agora herda da função` Animal`. E a função `Animal` normalmente possui` [[Protótipo]] `referenciando` Function.prototype`, porque não "estende" qualquer coisa.

Aqui, vamos verificar isso:

`` `js run
Classe Animal {}
classe Rabbit extends Animal {}

// para propriedades estáticas e métodos
alerta (Coelho .__ proto__ == Animal); // verdade

// e o próximo passo é Function.prototype
alerta (Animal .__ proto__ == Function.prototype); // verdade

// que é além da cadeia de protótipo "normal" para métodos de objeto
alerta (Rabbit.prototype .__ proto__ === Animal.prototype);
`` `

Desta forma, `Rabbit` tem acesso a todos os métodos estáticos de` Animal`.

### Nenhuma herança estática em built-ins

Observe que as classes internas não possuem uma referência estática `[[Protótipo]]`. Por exemplo, `Object` tem` Object.defineProperty`, `Object.keys` e assim por diante, mas` Array`, `Date` etc. não os herdam.

Aqui está a estrutura da imagem para `Date` e` Object`:

! [] (object-date-inheritance.png)

Observe que não existe nenhum link entre `Date` e` Object`. Ambos `Object` e` Data` existem de forma independente. `Date.prototype` herda de` Object.prototype`, mas isso é tudo.

Essa diferença existe por razões históricas: não houve pensamento sobre a sintaxe da classe e herdando métodos estáticos no início da linguagem JavaScript.

## Os Nativos são extensíveis

Classes integradas como Array, Map e outras são extensíveis também.

Por exemplo, aqui `PowerArray` herda do nativo` Array`:

`` `js run
// adicione mais um método para isso (pode fazer mais)
classe PowerArray extends Array {
está vazia() {
devolva this.length == 0;
}
}

Deixe arr = novo PowerArray (1, 2, 5, 10, 50);
alerta (arr.isEmpty ()); // falso

Deixe filterArr = arr.filter (item => item> = 10);
alerta (filterArr); // 10, 50
alerta (filterArr.isEmpty ()); // falso
`` `

Por favor, note uma coisa muito interessante. Métodos incorporados como `filter`,` map` e outros - retornam novos objetos de exatamente o tipo herdado. Eles dependem da propriedade `constructor` para fazê-lo.

No exemplo acima,
`` `js
arr.constructor === PowerArray
`` `

Então, quando `arr.filter ()` é chamado, ele cria internamente a nova matriz de resultados exatamente como `novo PowerArray`. E podemos continuar usando seus métodos mais abaixo da cadeia.

Ainda mais, podemos personalizar esse comportamento. O getter estático `Symbol.species`, se existir, retorna o construtor para usar em tais casos.

Por exemplo, aqui devido aos métodos incorporados `Symbol.species` como` map`, `filter` retornará arrays" normais ":

`` `js run
classe PowerArray extends Array {
está vazia() {
devolva this.length == 0;
}

*! *
// métodos incorporados usarão isso como o construtor
static get [Symbol.species] () {
retornar Array;
}
* /! *
}

Deixe arr = novo PowerArray (1, 2, 5, 10, 50);
alerta (arr.isEmpty ()); // falso

// filter cria nova matriz usando arr.constructor [Symbol.species] como construtor
Deixe filterArr = arr.filter (item => item> = 10);

*! *
// filterArr não é PowerArray, mas Array
* /! *
alerta (filterArr.isEmpty ()); // Erro: filterArr.isEmpty não é uma função
`` `

Podemos usá-lo em chaves mais avançadas para tirar a funcionalidade estendida dos valores resultantes se não for necessário. Ou, talvez, estendê-lo ainda mais.
