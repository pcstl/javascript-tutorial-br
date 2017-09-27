# Herança prototípica

Na programação, muitas vezes queremos tomar algo e estendê-lo.

Por exemplo, temos um objeto de "usuário" com suas propriedades e métodos, e quer fazer `admin` e` guest` como variantes ligeiramente modificadas. Gostaríamos de reutilizar o que temos no "usuário", não copiar / reimplementar seus métodos, basta criar um novo objeto em cima dele.

* Herança prototípica * é um recurso de linguagem que ajuda nisso.

[cortar]

## [[Protótipo]]

Em JavaScript, os objetos têm uma propriedade oculta especial `[[Protótipo]]` (como indicado na especificação), que é "nulo" ou faz referência a outro objeto. Esse objeto é chamado de "um protótipo":

! [protótipo] (object-prototype-empty.png)

O `[[Protótipo]] tem um significado" mágico ". Quando queremos ler uma propriedade de `object`, e falta, o JavaScript o tira automaticamente do protótipo. Na programação, tal coisa é chamada de "herança prototípica". Muitas funcionalidades de linguagem e técnicas de programação são baseadas nela.

A propriedade `[[Protótipo]] é interna e oculta, mas há muitas maneiras de configurá-lo.

Um deles é usar `__proto__`, assim:

`` `js run
deixe animal = {
come: true
};
deixe coelho = {
saltos: verdadeiro
};

*! *
coelho .__ proto__ = animal;
* /! *
`` `

Observe que `__proto__` é * não o mesmo * como '[[Protótipo]]`. Isso é um getter / setter para isso. Falaremos sobre outras formas de configurá-lo mais tarde, mas por enquanto `__proto__` fará bem.

Se procuramos uma propriedade em `rabbit`, e está faltando, o JavaScript o leva automaticamente de` animal`.

Por exemplo:

`` `js run
deixe animal = {
come: true
};
deixe coelho = {
saltos: verdadeiro
};

*! *
coelho .__ proto__ = animal; // (*)
* /! *

// podemos encontrar ambas as propriedades em coelho agora:
*! *
alerta (rabbit.eats); // verdade (**)
* /! *
alerta (coelho.jumps); // verdade
`` `

Aqui, a linha `(*)` define `animal` como um protótipo de` coelho '.

Então, quando `alert` tenta ler a propriedade` rabbit.eats`` (**) `, não está em` rabbit`, então o JavaScript segue a referência `[[Prototype]]` e acha em `animal` (look de baixo para cima):

! [] (proto-animal-rabbit.png)

Aqui podemos dizer que "animal" é o protótipo de "coelho" ou "coelho" herdeiros prototípicos de "animais".

Então, se `animal` tem muitas propriedades e métodos úteis, então eles se tornam automaticamente disponíveis no 'coelho'. Tais propriedades são chamadas de "herdadas".

Se temos um método em `animal`, pode ser chamado 'coelho':

`` `js run
deixe animal = {
come: true
*! *
andar() {
alerta ("caminhada de animais");
}
* /! *
};

deixe coelho = {
saltos: verdade
__proto__: animal
};

// caminhada é tirada do protótipo
*! *
rabbit.walk (); // Caminhada de animais
* /! *
`` `

O método é retirado automaticamente do protótipo, como este:

! [] (proto-animal-rabbit-walk.png)

A cadeia do protótipo pode ser mais longa:


`` `js run
deixe animal = {
come: true
andar() {
alerta ("caminhada de animais");
}
};

deixe coelho = {
saltos: verdade
__proto__: animal
};

Deixe LongEar = {
EarLength: 10,
__proto__: coelho
}

// caminhada é tirada da cadeia de protótipo
longEar.walk (); // Caminhada de animais
alerta (longEar.jumps); // verdadeiro (de coelho)
`` `

! [] (proto-animal-rabbit-chain.png)

Na verdade, existem apenas duas limitações:

1. As referências não podem entrar em círculos. O JavaScript lançará um erro se tentarmos atribuir `__proto__` em um círculo.
2. O valor de `__proto__` pode ser um objeto ou 'nulo'. Todos os outros valores (como primitivas) são ignorados.

Também pode ser óbvio, mas ainda: pode haver apenas um `[[Protótipo]]`. Um objeto não pode herdar de outros dois.

## Read / write rules

O protótipo é usado apenas para ler propriedades.

Para propriedades de dados (não getters / setters), as operações de gravação / exclusão funcionam diretamente com o objeto.

No exemplo abaixo, atribuímos seu próprio método `walk` para` rabbit`:

`` `js run
deixe animal = {
come: true
andar() {
/ * este método não será usado por coelho * /
}
};

deixe coelho = {
__proto__: animal
}

*! *
rabbit.walk = function () {
alerta ("Rabbit! Bounce-bounce!");
};
* /! *

rabbit.walk (); // Coelho! Bounce-bounce!
`` `

De agora em diante, a chamada `rabbit.walk ()` encontra o método imediatamente no objeto e o executa, sem usar o protótipo:

! [] (proto-animal-rabbit-walk-2.png)

Para getters / setters - se lemos / escrevemos uma propriedade, eles são pesquisados ​​no protótipo e invocados.

Por exemplo, confira a propriedade `admin.fullName` no código abaixo:

`` `js run
Deixe o usuário = {
Nome: "John",
sobrenome: "Smith"

set fullName (value) {
[this.name, this.sendame] = value.split ("");
},

Get FullName () {
retornar `$ {this.name} $ {this.sempleame}`;
}
};

Deixe admin = {
__proto__: usuário,
isAdmin: true
};

alerta (admin.fullName); // John Smith (*)

// setter triggers!
admin.fullName = "Alice Cooper"; // (**)
`` `

Aqui na linha `(*)` a propriedade `admin.full Name` tem um getter no protótipo` usuário`, então é chamado. E na linha `(**)` a propriedade possui um setter no protótipo, então é chamado.

## O valor de "this"

Uma pergunta interessante pode surgir no exemplo acima: qual é o valor de `this` dentro de` set fullName (value) `? Onde as propriedades `this.name` e` this.sempleame` estão escritas: `user` ou` admin`?

A resposta é simples: "isto" não é afetado por protótipos.

** Não importa onde o método é encontrado: em um objeto ou seu protótipo. Em uma chamada de método, `this` é sempre o objeto antes do ponto. **

Assim, o setter realmente usa `admin` como` this`, e não `usuário`.

Isso é realmente uma coisa super-importante, porque podemos ter um grande objeto com muitos métodos e herdar dele. Então podemos executar seus métodos em objetos herdados e eles modificarão o estado desses objetos, e não o grande.

Por exemplo, aqui `animal` representa um" armazenamento de métodos ", e` coelho 'faz uso disso.

A chamada `rabbit.sleep ()` define `this.isSleeping` no objeto` rabbit`:

`` `js run
// animal tem métodos
deixe animal = {
andar() {
se (! this.isSleeping) {
alerta ("eu ando");
}
},
dormir() {
This isisleing = true;
}
};

deixe coelho = {
Nome: "White Rabbit",
__proto__: animal
};

// coelho modificado. Sono
Rabbit.sleep ();

alerta (rabbit.isSleeping); // verdade
alerta (animal.isSleeping); // indefinido (sem tal propriedade no protótipo)
`` `

A imagem resultante:

! [] (proto-animal-rabbit-walk-3.png)

Se tivéssemos outros objetos como `bird`,` serpente ', etc. herdando de `animal`, eles também teriam acesso a métodos de' animal '. Mas `this` em cada método seria o objeto correspondente, avaliado no tempo de chamada (antes do ponto), não` animal`. Então, quando escrevemos dados em `this`, ele é armazenado nesses objetos.

Como resultado, os métodos são compartilhados, mas o estado do objeto não é.

## Resumo

- Em JavaScript, todos os objetos têm uma propriedade [[Protótipo]] `escondida que é qualquer outro objeto ou" nulo ".
- Podemos usar `obj .__ proto__` para acessá-lo (também há outras formas, para serem cobertos em breve).
- O objeto referenciado por `[[Protótipo]] é chamado de" protótipo ".
- Se quisermos ler uma propriedade de `obj` ou chamar um método, e não existe, o JavaScript tenta encontrá-lo no protótipo. As operações de gravação / supressão funcionam diretamente no objeto, eles não usam o protótipo (a menos que a propriedade seja realmente um setter).
- Se chamamos `obj.method ()`, e o `método 'é tirado do protótipo,' this` ainda faz referência a` obj`. Portanto, os métodos sempre funcionam com o objeto atual, mesmo que sejam herdados.
