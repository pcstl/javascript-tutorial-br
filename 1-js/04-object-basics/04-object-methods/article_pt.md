# Métodos de objeto, "este"

Os objetos geralmente são criados para representar entidades do mundo real, como usuários, pedidos e assim por diante:

`` `js
Deixe o usuário = {
Nome: "John",
idade: 30
};
`` `

E, no mundo real, um usuário pode * agir *: selecione algo do carrinho de compras, login, logout etc.

As ações são representadas em JavaScript por funções em propriedades.

[cortar]

## Exemplos de métodos

Para começar, vamos ensinar o `usuário 'a dizer olá:

`` `js run
Deixe o usuário = {
Nome: "John",
idade: 30
};

*! *
user.sayHi = function () {

};
* /! *

user.sayHi (); // Olá!
`` `

Aqui, acabamos de usar uma expressão de função para criar a função e atribuí-la à propriedade `user.sayHi` do objeto.

Então podemos chamá-lo. O usuário agora pode falar!

Uma função que é propriedade de um objeto é chamada de * método *.

Então, aqui temos um método `sayHi` do objeto` usuário`.

Claro, podemos usar uma função pré-declarada como um método, como este:

`` `js run
Deixe o usuário = {
// ...
};

*! *
// primeiro, declare
função sayHi () {

};

// então adicione como um método
user.sayHi = sayHi;
* /! *

user.sayHi (); // Olá!
`` `

`` `cabeçalho inteligente =" programação orientada a objetos "
Quando escrevemos nosso código usando objetos para representar entidades, isso é chamado de [programação orientada a objetos] (https://en.wikipedia.org/wiki/Object-oriented_programming), em resumo: "OOP".

OOP é uma grande coisa, uma ciência interessante própria. Como escolher as entidades certas? Como organizar a interação entre eles? Essa é a arquitetura, e há ótimos livros sobre esse tema, como "Padrões de Design: Elementos de Software Orientado a Objetos Reutilizáveis" por E.Gamma, R.Helm, R.Johnson, J.Vissides ou "Análise e Design Orientado a Objetos com Aplicações "por G.Booch, e mais. Arranjaremos a superfície desse tópico mais adiante no capítulo <info: programação orientada a objetos>.
`` `
### Método abreviado

Existe uma sintaxe mais curta para métodos em um objeto literal:

`` `js
// estes objetos fazem o mesmo

Deixe o usuário = {
sayHi: function () {

}
};

// uma abreviatura de métodos parece melhor, né?
Deixe o usuário = {
*! *
sayHi () {// idêntico a "sayHi: function ()"
* /! *

}
};
`` `

Conforme demonstrado, podemos omitir a função "` e apenas escrever `sayHi ()`.

Para dizer a verdade, as notações não são totalmente idênticas. Há diferenças sutis relacionadas à herança de objetos (para serem abordadas mais tarde), mas, por enquanto, não importam. Em quase todos os casos, a sintaxe mais curta é preferida.

## "this" em métodos

É comum que um método de objeto precise acessar as informações armazenadas no objeto para fazer seu trabalho.

Por exemplo, o código dentro de `user.sayHi ()` pode precisar do nome do `usuário '.

** Para acessar o objeto, um método pode usar a palavra-chave `this`. **

O valor de `this` é o objeto" antes do ponto ", aquele usado para chamar o método.

Por exemplo:

`` `js run
Deixe o usuário = {
Nome: "John",
idade: 30,

SayHi () {
*! *
alerta (this.name);
* /! *
}

};

user.sayHi (); // John
`` `

Aqui durante a execução do `user.sayHi ()`, o valor de `this` será 'usuário'.

Tecnicamente, também é possível acessar o objeto sem `this`, referenciando-o através da variável externa:

`` `js
Deixe o usuário = {
Nome: "John",
idade: 30,

SayHi () {
*! *
alerta (user.name); // "usuário" em vez de "isso"
* /! *
}

};
`` `

... Mas esse código não é confiável. Se decidimos copiar `user` para outra variável, por exemplo, `admin = user` e substituir` user` por outra coisa, então ele irá acessar o objeto errado.

Isso é demonstrado abaixo:

`` `js run
Deixe o usuário = {
Nome: "John",
idade: 30,

SayHi () {
*! *
alerta (user.name); // leva a um erro
* /! *
}

};


Deixe admin = user;
usuário = nulo; // sobrescreva para tornar as coisas óbvias

admin.sayHi (); // Whoops! dentro de sayHi (), o nome antigo é usado! erro!
`` `

Se usássemos `this.name` em vez de` user.name` dentro do `alert`, então o código funcionaria.

## "isto" não está vinculado

Em JavaScript, a palavra-chave "this" se comporta ao contrário da maioria das outras linguagens de programação. Primeiro, ele pode ser usado em qualquer função.

Não há erro de sintaxe no código como esse:

`` `js
função sayHi () {
alerta (*! * este * /! *. nome);
}
`` `

O valor de `this` é avaliado durante o tempo de execução. E pode ser qualquer coisa.

Por exemplo, a mesma função pode ter "isto" diferente quando chamado de objetos diferentes:

`` `js run
deixe o usuário = {nome: "João"};
Deixe admin = {name: "Admin"};

função sayHi () {
alerta (this.name);
}

*! *
// usa as mesmas funções em dois objetos
user.f = sayHi;
admin.f = sayHi;
* /! *

// estas chamadas têm diferente isso
// "isto" dentro da função é o objeto "antes do ponto"
user.f (); // John (este == usuário)
admin.f (); // Admin (this == admin)

admin ['f'] (); // Admin (ponto ou colchetes acessam o método - não importa)
`` `

Na verdade, podemos chamar a função sem um objeto:

`` `js run
função sayHi () {
alerta (isto);
}

diga oi(); // Indefinido
`` `

Neste caso, `this` é` undefined` no modo estrito. Se tentarmos acessar `this.name`, haverá um erro.

Em modo não-estrito (se um se esquecer de "usar rigoroso"), o valor de `este" será o * objeto global * (`janela" em um navegador, chegamos a ele mais tarde). Este é um comportamento histórico que "usa" correções "estritas".

Observe que geralmente uma chamada de uma função que usa `isso sem um objeto não é normal, mas sim um erro de programação. Se uma função tem `isso ', então geralmente é chamado a ser chamado no contexto de um objeto.

`` `cabeçalho inteligente =" As conseqüências de não consolidado `isso"
Se você vem de outra linguagem de programação, provavelmente você está acostumado com a idéia de um "bound` this` ", onde os métodos definidos em um objeto sempre têm` this` fazendo referência a esse objeto.

Em JavaScript, `this` é" livre ", seu valor é avaliado em tempo de chamada e não depende de onde o método foi declarado, mas sim de qual é o objeto" antes do ponto ".

O conceito de tempo de execução avaliado 'this` tem vantagens e desvantagens. Por um lado, uma função pode ser reutilizada para objetos diferentes. Por outro lado, uma maior flexibilidade abre um lugar para erros.

Aqui, nossa posição não é julgar se esta decisão de design de idioma é boa ou ruim. Nós entenderemos como trabalhar com ele, como obter benefícios e evitar problemas.
`` `

## Internals: Tipo de Referência

`` `warn header =" recurso de idioma detalhado "
Esta seção aborda um tópico avançado, para entender melhor alguns casos de borda.

Se quiser continuar mais rápido, pode ser ignorado ou adiado.
`` `

Uma chamada de método intrincada pode perder `this`, por exemplo:

`` `js run
Deixe o usuário = {
Nome: "John",
oi () {alert (this.name); },
bye () {alert ("Bye"); }
};

user.hi (); // John (a chamada simples funciona)

*! *
// agora vamos chamar user.hi ou user.bye dependendo do nome
(user.name == "John"? user.hi: user.bye) (); // Erro!
* /! *
`` `

Na última linha, há um operador ternário que escolhe o `user.hi` ou` user.bye`. Nesse caso, o resultado é `user.hi`.

O método é imediatamente chamado com parênteses `()`. Mas não funciona direito!

Você pode ver que a chamada resulta em um erro, porque o valor de `" este "dentro da chamada se torna" indefinido ".

Isso funciona (método ponto metodo):
`` `js
user.hi ();
`` `

Isso não (método avaliado):
`` `js
(user.name == "John"? user.hi: user.bye) (); // Erro!
`` `

Por quê? Se queremos entender por que isso acontece, vamos sob o capô de como a chamada `obj.method ()` funciona.

Observando de perto, podemos notar duas operações na instrução obj.method () `:

1. Primeiro, o ponto `'.'` recupera a propriedade` obj.method`.
2. Então parênteses `()` executá-lo.

Então, como as informações sobre `this` passam da primeira parte para a segunda?

Se colocarmos essas operações em linhas separadas, então "isso" será perdido com certeza:

`` `js run
Deixe o usuário = {
Nome: "John",
oi () {alert (this.name); }
}

*! *
// dividir recebendo e chamando o método em duas linhas
deixe oi = user.hi;
Oi(); // Erro, porque isso não está definido
* /! *
`` `

Aqui, `hi = user.hi` coloca a função na variável e, na última linha, é completamente autônomo e, portanto, não existe` this`.

** Para que as chamadas `User.hi ()` funcionem, o JavaScript usa um truque - o ponto `'.' 'Não retorna uma função, mas um valor do especial [Tipo de Referência] (https: //tc39.github .io / ecma262 / # seg-reference-specification-type). **

O tipo de referência é um "tipo de especificação". Não podemos explicitamente usá-lo, mas é usado internamente pelo idioma.

O valor do tipo de referência é uma combinação de três valores `(base, nome, rigoroso)`, onde:

- `base` é o objeto.
- `nome` é a propriedade.
- `strict` é verdadeiro se` use strict` estiver em vigor.

O resultado de um acesso de propriedade `user.hi` não é uma função, mas um valor de Tipo de Referência. Para `user.hi` no modo estrito é:

`` `js
// Tipo de referência valor
(usuário, "oi", verdadeiro)
`` `

Quando os parênteses `()` são chamados no Tipo de Referência, eles recebem as informações completas sobre o objeto e seu método, e podem definir o direito 'this` (`= user` neste caso).

Qualquer outra operação como atribuição `hi = user.hi` descarta o tipo de referência como um todo, leva o valor de` user.hi` (uma função) e o passa. Então, qualquer operação adicional "perde" essa ".

Assim, como resultado, o valor de `this` só é passado do jeito certo se a função é chamada diretamente usando um ponto` obj.method () `ou colchetes 'obj [method] ()` sintaxe (eles fazem o o mesmo aqui).

# Seta funções não têm "isto"

As funções de seta são especiais: elas não têm o "próprio" "isso". Se nós fazemos referência a "isto" dessa função, é tirada da função "normal" externa.

Por exemplo, aqui `arrow ()` usa `this` do método` `usuário.sayHi ()` externo:

`` `js run
Deixe o usuário = {
primeiro nome: "Ilya",
SayHi () {
Deixe arrow = () => alert (this.firstName);
flecha();
}
};

user.sayHi (); // Ilya
`` `

Essa é uma característica especial das funções de seta, é útil quando na verdade não queremos ter um "isso" separado, mas sim retirá-lo do contexto externo. Mais adiante, no capítulo <info: arrow-functions> iremos mais profundamente às funções de seta.


## Resumo

- Funções que são armazenadas em propriedades do objeto são chamadas de "métodos".
- Métodos permitem que os objetos "atuem" como `object.doSomething ()`.
- Métodos podem fazer referência ao objeto como "isso".

O valor de `this` é definido em tempo de execução.
- Quando uma função é declarada, pode usar `this`, mas que` this` não tem valor até que a função seja chamada.
- Essa função pode ser copiada entre objetos.
- Quando uma função é chamada na sintaxe "método": `object.method ()`, o valor de `this` durante a chamada é` object`.

Observe que as funções de seta são especiais: elas não têm `isso '. Quando "isto" é acessado dentro de uma função de seta, ele é retirado do lado de fora.
