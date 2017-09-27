
# Padrões de classe

`` `quote author =" Wikipedia "
Na programação orientada a objetos, um * class * é um modelo de código de programa extensível para criar objetos, fornecendo valores iniciais para o estado (variáveis ​​de membros) e implementações de comportamento (funções ou métodos de membros).
`` `

Existe uma construção de sintaxe especial e uma palavra-chave `class` em JavaScript. Mas antes de estudá-lo, devemos considerar que o termo "classe" vem da teoria da programação orientada a objetos. A definição é citada acima, e é independente de linguagem.

Em JavaScript, existem vários padrões de programação bem conhecidos para fazer classes mesmo sem usar a palavra-chave `classe`. E aqui vamos falar sobre eles primeiro.

A construção da "classe" será descrita no próximo capítulo, mas em JavaScript é um "açúcar de sintaxe" e uma extensão de um dos padrões que estudaremos aqui.

[cortar]


## Padrão de classe funcional

A função de construtor abaixo pode ser considerada uma "classe" de acordo com a definição:

`` `js run
função Usuário (nome) {
this.sayHi = function () {
alerta (nome);
};
}

Deixe usuário = novo Usuário ("João");
user.sayHi (); // John
`` `

Ele segue todas as partes da definição:

1. É um "programa-código-modelo" para criar objetos (chamável com `new`).
2. Ele fornece valores iniciais para o estado (`nome 'dos ​​parâmetros).
3. Fornece métodos (`sayHi`).

Isso é chamado de * padrão de classe funcional *.

No padrão de classe funcional, as variáveis ​​locais e as funções aninhadas dentro do `Usuário ', que não são atribuídas a` isso', são visíveis a partir do interior, mas não são acessadas pelo código externo.

Então podemos adicionar facilmente funções e variáveis ​​internas, como `calcAge ()` aqui:

`` `js run
função Usuário (nome, aniversário) {

*! *
// apenas visível de outros métodos dentro do usuário
function calcAge () {
retornar nova data (). getFullYear () - birthday.getFullYear ();
}
* /! *

this.sayHi = function () {
alerta (nome + ', idade:' + calcAge ());
};
}

deixe usuário = novo Usuário ("João", nova Data (2000,0,1));
user.sayHi (); // John
`` `

Neste código variáveis ​​`name`,` birthday` e a função `calcAge ()` são internas, * private * para o objeto. Eles são visíveis somente por dentro.

Por outro lado, `sayHi` é o método externo * * público *. O código externo que cria "usuário" pode acessá-lo.

Desta forma, podemos esconder detalhes internos de implementação e métodos auxiliares do código externo. Somente o que é atribuído a `this` torna-se visível fora.

## Padrão de classe de fábrica

Podemos criar uma aula sem usar `new`.

Como isso:

`` `js run
função Usuário (nome, aniversário) {
// apenas visível de outros métodos dentro do usuário
function calcAge () {
retornar nova data (). getFullYear () - birthday.getFullYear ();
}

Retorna {
SayHi () {
alerta (nome + ', idade:' + calcAge ());
}
};
}

*! *
Deixe usuário = Usuário ("João", nova Data (2000,0,1));
* /! *
user.sayHi (); // John
`` `

Como podemos ver, a função `Usuário 'retorna um objeto com propriedades públicas e métodos. O único benefício deste método é que podemos omitir `new`: escreva` allow user = User (...) `em vez de` let user = new User (...) `. Em outros aspectos, é quase o mesmo que o padrão funcional.

## Classes baseadas em protótipos

As classes baseadas em protótipos são as mais importantes e geralmente as melhores. Os padrões de classe funcional e funcional raramente são usados ​​na prática.

Logo você verá o porquê.

Aqui está a mesma classe reescrita usando protótipos:

`` `js run
função Usuário (nome, aniversário) {
*! *
this._name = name;
this._birthday = aniversário;
* /! *
}

*! *
User.prototype._calcAge = function () {
* /! *
retornar nova data (). getFullYear () - this._birthday.getFullYear ();
};

User.prototype.sayHi = function () {
alerta (this._name + ', idade:' + this._calcAge ());
};

deixe usuário = novo Usuário ("João", nova Data (2000,0,1));
user.sayHi (); // John
`` `

A estrutura do código:

- O construtor `Usuário 'inicializa apenas o estado do objeto atual.
- Métodos são adicionados a `User.prototype`.

Como podemos ver, os métodos são léxicamente não dentro da "função Usuário", eles não compartilham um ambiente léxico comum. Se declararmos variáveis ​​dentro da "função Usuário", elas não serão visíveis para os métodos.

Portanto, existe um acordo amplamente conhecido de que as propriedades internas e os métodos são precedidos por um sublinhado "_". Como `_name` ou` _calcAge () `. Tecnicamente, isso é apenas um acordo, o código externo ainda pode acessá-los. Mas a maioria dos desenvolvedores reconhece o significado de "_" e tente não tocar em propriedades e métodos prefixados no código externo.

Aqui estão as vantagens sobre o padrão funcional:

- No padrão funcional, cada objeto possui sua própria cópia de cada método. Nós atribuímos uma cópia separada de `this.sayHi = function () {...}` e outros métodos no construtor.
- No padrão prototípico, todos os métodos estão em `User.prototype` que é compartilhado entre todos os objetos do usuário. Um objeto em si apenas armazena os dados.

Portanto, o padrão prototípico é mais eficiente em termos de memória.

...Mas não só isso. Os protótipos nos permitem configurar a herança de uma maneira realmente eficiente. Objetos de JavaScript incorporados usam protótipos. Também existe uma construção de sintaxe especial: "classe", que oferece uma sintaxe de aparência agradável para eles. E há mais, então vamos continuar com eles.

## Herança baseada em protótipo para aulas

Digamos que temos duas classes baseadas em protótipos.

`Rabbit`:

`` `js
função Coelho (nome) {
this.name = name;
}

Rabbit.prototype.jump = function () {
alerta (this.name + 'jumps!');
};

deixe coelho = coelho novo ("meu coelho");
`` `

! [] (coelho-animal-independente-1.png)

... E `Animal`:

`` `js
função Animal (nome) {
this.name = name;
}

Animal.prototype.eat = function () {
alerta (this.name + 'come' ');
};

Deixe animal = novo Animal ("Meu animal");
`` `

! [] (coelho-animal-independente-2.png)

Agora, eles são totalmente independentes.

Mas gostaríamos que o "Coelho" estendesse o "Animal". Em outras palavras, os coelhos devem basear-se em animais, ter acesso a métodos de "Animal" e estendê-los com seus próprios métodos.

O que significa na linguagem dos protótipos?

Agora os métodos para objetos `rabbit` estão em` Rabbit.prototype`. Nós gostaríamos que `rabbit` usasse` Animal.prototype` como um "fallback", se o método não for encontrado em `Rabbit.prototype`.

Então, a cadeia de protótipo deve ser `Rabbit` ->` Rabbit.prototype` -> `Animal.prototype`.

Como isso:

! [] (class-inheritance-rabbit-animal.png)

O código para implementar isso:

`` `js run
// O mesmo animal que antes
função Animal (nome) {
this.name = name;
}

// Todos os animais podem comer, certo?
Animal.prototype.eat = function () {
alerta (this.name + 'come' ');
};

// O mesmo coelho que antes
função Coelho (nome) {
this.name = name;
}

Rabbit.prototype.jump = function () {
alerta (this.name + 'jumps!');
};

*! *
// configura a cadeia de herança
Rabbit.prototype .__ proto__ = Animal.prototype; // (*)
* /! *

deixe coelho = coelho novo ("coelho branco");
*! *
rabbit.eat (); // coelhos também podem comer
* /! *
coelho.jump ();
`` `

A linha `(*)` configura a cadeia de protótipo. Então, o `rabbit` procura primeiro métodos em` Rabbit.prototype`, então `Animal.prototype`. E, em seguida, apenas para completar, vamos mencionar que, se o método não for encontrado em `Animal.prototype`, a busca continua no` Object.prototype`, porque `Animal.prototype` é um objeto comum regular, por isso herda de isto.

Então, aqui está a imagem completa:

!] [] (classe-herança-coelho-animal-2.png)

## Resumo

O termo "classe" vem da programação orientada a objetos. Em JavaScript, geralmente significa o padrão de classe funcional ou o padrão prototípico. O padrão prototípico é mais poderoso e eficiente em termos de memória, por isso recomenda-se aderir a ele.

De acordo com o padrão prototípico:
1. Os métodos são armazenados em `Class.prototype`.
2. Os protótipos herdam um do outro.

No próximo capítulo, estudaremos a palavra-chave e a construção de "classe". Ele permite escrever classes prototípicas mais curtas e oferece alguns benefícios adicionais.
