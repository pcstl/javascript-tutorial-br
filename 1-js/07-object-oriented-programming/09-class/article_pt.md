
# Aulas

A construção "classe" permite definir classes baseadas em protótipos com uma sintaxe limpa e de aparência agradável.

[cortar]

## A sintaxe "classe"

A sintaxe `class` é versátil, primeiro começamos com um exemplo simples.

Aqui está uma classe baseada em protótipo 'Usuário':

`` `js run
função Usuário (nome) {
this.name = name;
}

User.prototype.sayHi = function () {
alerta (this.name);
}

Deixe usuário = novo Usuário ("João");
user.sayHi ();
`` `

... E isso é o mesmo usando a sintaxe `class`:

`` `js run
Classe Usuário {

construtor (nome) {
this.name = name;
}

SayHi () {
alerta (this.name);
}

}

Deixe usuário = novo Usuário ("João");
user.sayHi ();
`` `

É fácil ver que os dois exemplos são parecidos. Basta observar que os métodos em uma classe não têm uma vírgula entre eles. Os desenvolvedores de avisos às vezes esquecem e colocam uma vírgula entre os métodos da classe e as coisas não funcionam. Esse não é um objeto literal, mas uma sintaxe de classe.

Então, o que exatamente faz a "classe"? Podemos pensar que define uma nova entidade de nível de idioma, mas isso seria errado.

O 'Usuário da classe' {...} `aqui realmente faz duas coisas:

1. Declara uma variável `Usuário 'que faz referência à função denominada` "construtor" `.
2. Coloca os métodos `User.prototype` listados na definição. Aqui inclui `sayHi` e o` construtor '.

Aqui está o código para cavar na classe e ver isso:

`` `js run
Classe Usuário {
construtor (nome) {this.name = name; }
sayHi () {alert (this.name); }
}

*! *
// prova: o usuário é a função "construtor"
* /! *
alerta (Usuário == User.prototype.constructor); // verdade

*! *
// prova: existem dois métodos em seu "protótipo"
* /! *
alerta (Object.getOwnPropertyNames (User.prototype)); // construtor, sayHi
`` `

Aqui está a ilustração do que `classe User` cria:

!] [class-user.png]



Então, `class` é uma sintaxe especial para definir um construtor junto com seus métodos de protótipo.

...Mas não só isso. Existem pequenos ajustes aqui e ali:

Construtores requerem "novo"
: Ao contrário de uma função normal, um "construtor de classe" não pode ser chamado sem `new`:

`` `js run
Classe Usuário {
construtor () {}
}

alerta (tipo de Usuário); // função
Do utilizador(); // Erro: construtor de classe O usuário não pode ser invocado sem 'novo'
`` `

Saída de string diferente
: Se o emitimos como "alerta (Usuário)", alguns motores mostram a "Usuário da classe ...", enquanto outros mostram a função "Usuário ..." `.

Por favor, não se confunda: a representação de seqüência de caracteres pode variar, mas ainda é uma função, não existe uma entidade separada de "classe" em linguagem JavaScript.

Os métodos de classe não são enumeráveis
: Uma definição de classe define o símbolo `enumerable` para` false` para todos os métodos no `` protótipo ''. Isso é bom, porque se nós forinvolvendo um objeto, geralmente não queremos seus métodos de classe.

As classes possuem um "construtor" padrão () {} `
: Se não houver `construtor 'na construção` class`, então uma função vazia é gerada, da mesma forma que se tivéssemos escrito `constructor () {}`.

Classes sempre `uso estrito '
: Todo o código dentro da construção da classe é automaticamente no modo estrito.

### Getters / setters

As aulas também podem incluir getters / setters. Aqui está um exemplo com `user.name` implementado usando-os:

`` `js run
Classe Usuário {

construtor (nome) {
// invoca o setter
this.name = name;
}

*! *
obter nome () {
* /! *
devolva this._name;
}

*! *
nome do conjunto (valor) {
* /! *
se (value.length <4) {
alerta ("Nome muito curto");
Retorna;
}
this._name = value;
}

}

Deixe usuário = novo Usuário ("João");
alerta (user.name); // John

usuário = novo Usuário (""); // Nome muito curto.
`` `

Internamente, getters e setters também são criados no protótipo 'Usuário', assim:

`` `js
Object.defineProperty (User.prototype, {
nome: {
obter() {
devolva this._name
},
nome do conjunto) {
// ...
}
}
});
`` `

### Somente métodos

Ao contrário dos literais de objeto, nenhuma atribuição `property: value` é permitida dentro de` class`. Pode haver apenas métodos e getters / setters. Existe algum trabalho na especificação para levantar essa limitação, mas ainda não está lá.

Se realmente precisamos colocar um valor sem função no protótipo, podemos alterar o `protótipo 'manualmente, assim:

`` `js run
Usuário de classe {}

User.prototype.test = 5;

alerta (novo teste de usuário ().); // 5
`` `

Então, tecnicamente isso é possível, mas devemos saber por que estamos fazendo isso. Tais propriedades serão compartilhadas entre todos os objetos da classe.

Uma alternativa "in-class" é usar um getter:

`` `js run
Classe Usuário {
obtenha teste () {
retornar 5;
}
}

alerta (novo teste de usuário ().); // 5
`` `

Do código externo, o uso é o mesmo. Mas a variante getter é um pouco mais lenta.

## Expressão de classe

Assim como as funções, as classes podem ser definidas dentro de outra expressão, transmitidas, devolvidas etc.

Aqui está uma função de retorno de classe ("classe fábrica"):

`` `js run
function makeClass (frase) {
*! *
// declarar uma classe e devolvê-la
classe de retorno {
SayHi () {
alerta (frase);
};
};
* /! *
}

deixe User = makeClass ("Hello");

novo usuário (). sayHi (); // Olá
`` `

Isso é bastante normal se lembrarmos que a "classe" é apenas uma forma especial de uma definição de função com protótipo.

E, como Expressões de Função Nomeada, essas classes também podem ter um nome, que é visível somente dentro dessa classe:

`` `js run
// "Named Class Expression" (infelizmente, sem esse termo, mas é o que está acontecendo)
Deixe User = class *! * MyClass * /! * {
SayHi () {
alerta (MyClass); // MyClass é visível apenas dentro da classe
}
};

novo usuário (). sayHi (); // funciona, mostra a definição do MyClass

alerta (MyClass); // erro, MyClass não é visível fora da classe
`` `

## Métodos estáticos

Também podemos atribuir métodos à função de classe, não ao seu "protótipo". Tais métodos são chamados * static *.

Um exemplo:

`` `js run
Classe Usuário {
*! *
static staticMethod () {
* /! *
alerta (este == Usuário);
}
}

User.staticMethod (); // verdade
`` `

Isso realmente faz o mesmo que atribuí-lo como uma propriedade de função:

`` `js
função Usuário () {}

User.staticMethod = function () {
alerta (este == Usuário);
};
`` `

O valor de `this` dentro de` User.staticMethod () `é o construtor de classe` Usuário 'em si (a regra "objeto antes do ponto").

Geralmente, os métodos estáticos são usados ​​para implementar funções que pertencem à classe, mas não para nenhum objeto particular da mesma.

Por exemplo, temos objetos "Artigo" e precisamos de uma função para compará-los. A escolha natural seria "Article.compare", assim:

`` `js run
classe Artigo {
construtor (título, data) {
this.title = title;
this.date = date;
}

*! *
comparação estática (articleS, articlesS) {
retornar articleA.date - articleB.date;
}
* /! *
}

// uso
deixe artigos = [
novo artigo ("Mind", nova data (2016, 1, 1)),
novo artigo ("Corpo", nova data (2016, 0, 1)),
novo artigo ("JavaScript", nova data (2016, 11, 1))
];

*! *
articles.sort (Article.compare);
* /! *

alerta (artigos [0] .title); // Corpo
`` `

Aqui "Article.compare" está "sobre" os artigos, como forma de compará-los. Não é um método de um artigo, mas sim de toda a classe.

Outro exemplo seria o chamado método de "fábrica". Imagine, precisamos de algumas maneiras de criar um artigo:

1. Criar por parâmetros determinados (`title`,` date` etc).
2. Crie um artigo vazio com a data de hoje.
3. ...

A primeira maneira pode ser implementada pelo construtor. E para o segundo, podemos fazer um método estático da classe.

Como `Article.createTodays ()` aqui:

`` `js run
classe Artigo {
construtor (título, data) {
this.title = title;
this.date = date;
}

*! *
static createTodays () {
// lembre-se, este = artigo
devolva novo ("Digest de hoje", nova Data ());
}
* /! *
}

Deixe article = Article.createTodays ();

alerta (article.title); // digestão de hoje
`` `

Agora, cada vez que precisamos criar um resumo de hoje, podemos chamar `Article.createTodays ()`. Mais uma vez, esse não é um método de um artigo, mas um método de toda a classe.

Os métodos estáticos também são usados ​​em classes relacionadas ao banco de dados para pesquisar / salvar / remover entradas do banco de dados, como este:

`` `js
// assumindo que o artigo é uma classe especial para gerenciar artigos
// método estático para remover o artigo:
Article.remove ({id: 12345});
`` `

## Resumo

A sintaxe da classe básica parece assim:

`` `js
classe MyClass {
construtor (...) {
// ...
}
method1 (...) {}
method2 (...) {}
obter algo (...) {}
definir algo (...) {}
static staticMethod (...) {}
// ...
}
`` `

O valor de 'MyClass` é uma função fornecida como `construtor'. Se não houver `construtor ', então uma função vazia.

Em qualquer caso, os métodos listados na declaração de classe se tornam membros do seu "protótipo", com exceção de métodos estáticos que são escritos na própria função e chamáveis ​​como 'MyClass.staticMethod () `. Os métodos estáticos são usados ​​quando precisamos de uma função vinculada a uma classe, mas não a qualquer objeto dessa classe.

No próximo capítulo, aprenderemos mais sobre as aulas, incluindo a herança.
