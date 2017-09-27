# Mixins

Em JavaScript, podemos apenas herdar de um único objeto. Pode haver apenas um [[Protótipo]] para um objeto. E uma classe pode estender apenas uma outra classe.

Mas às vezes isso é limitado. Por exemplo, eu tenho uma classe `StreetSweeper` e uma classe` Bycicle`, e quero fazer um `StreetSweepingBycicle`.

Ou, falando sobre programação, temos uma classe `Renderer` que implementa modelos e uma classe` EventEmitter` que implementa o gerenciamento de eventos e quer fundir essas funcionalidades junto com uma classe `Page`, para criar uma página que pode usar modelos e emite eventos.

Há um conceito que pode ajudar aqui, chamado "mixins".

Conforme definido na Wikipédia, um [mixin] (https://en.wikipedia.org/wiki/Mixin) é uma classe que contém métodos para uso por outras classes sem ter que ser a classe pai dessas outras classes.

Em outras palavras, um * mixin * fornece métodos que implementam um determinado comportamento, mas não usamos isso sozinho, nós o usamos para adicionar o comportamento a outras classes.

## A superstição

A maneira mais simples de fazer um mixin em JavaScript é fazer um objeto com métodos úteis, de modo que possamos combiná-los facilmente em um protótipo de qualquer classe.

Por exemplo, aqui o mixin `sayHiMixin` é usado para adicionar algum" discurso "para` Usuário ':

`` `js run
*! *
// superstição
* /! *
deixe sayHiMixin = {
SayHi () {

},
diga tchau() {
alerta ("Bye" + this.name);
}
};

*! *
// uso:
* /! *
Classe Usuário {
construtor (nome) {
this.name = name;
}
}

// copie os métodos
Object.assign (User.prototype, sayHiMixin);

// agora o usuário pode dizer oi
Novo Usuário ("Dude"). sayHi (); // Oi cara!
`` `

Não há herança, mas um método simples de cópia. Então `Usuário 'pode ampliar uma outra classe e também incluir o mixin para" misturar "os métodos adicionais, como este:

`` `js
classe O usuário estende Pessoa {
// ...
}

Object.assign (User.prototype, sayHiMixin);
`` `

Mixins podem fazer uso da herança dentro de si mesmos.

Por exemplo, aqui `sayHiMixin` herda de` sayMixin`:

`` `js run
deixe sayMixin = {
diga (frase) {
alerta (frase);
}
};

deixe sayHiMixin = {
__proto__: sayMixin, // (ou podemos usar Object.create para configurar o protótipo aqui)

SayHi () {
*! *
// método de chamada por assinatura
* /! *
super.say ("Olá" + this.name);
},
diga tchau() {
super.say ("Bye" + this.name);
}
};

Classe Usuário {
construtor (nome) {
this.name = name;
}
}

// copie os métodos
Object.assign (User.prototype, sayHiMixin);

// agora o usuário pode dizer oi
Novo Usuário ("Dude"). sayHi (); // Hello Dude!
`` `

Observe que a chamada para o método pai `super.say ()` de `sayHiMixin` procura o método no protótipo desse mixin, não da classe.

[] (Superstição - herança.PNG)

Isso ocorre porque os métodos de `sayHiMixin` têm` [[HomeObject]] `configurados para ele. Então `super` realmente significa` sayHiMixin .__ proto__`, não `Usuário .__ proto__`.

## EventMixin

Agora vamos fazer uma mistura para a vida real.

A característica importante de muitos objetos é trabalhar com eventos.

Isto é: um objeto deve ter um método para "gerar um evento" quando algo importante acontecer, e outros objetos podem ser capazes de "ouvir" esses eventos.

Um evento deve ter um nome e, opcionalmente, agrupar alguns dados adicionais.

Por exemplo, um objeto `usuário` pode gerar um evento" login "` quando o visitante faz logon. E outro objeto `calendário 'pode querer receber esses eventos para carregar o calendário para a pessoa conectada.

Ou, um `menu` pode gerar o evento` "selecionar" quando um item de menu é selecionado e outros objetos podem querer obter essa informação e reagir nesse evento.

Eventos é uma maneira de "compartilhar informações" com quem quiser. Eles podem ser úteis em qualquer classe, então vamos fazer uma mistura para eles:

`` `js run
deixe eventMixin = {
/ **
* Assinar o evento, uso:
* menu.on ('select', função (item) {...}
* /
on (eventName, handler) {
se (! this._eventHandlers) this._eventHandlers = {};
se (! this._ventingHandlers [eventName]) {
this._eventHandlers [eventName] = [];
}
this._eventHandlers [eventName] .push (manipulador);
},

/ **
* Cancelar a assinatura, uso:
* menu.off ('selecionar', manipulador)
* /
off (eventName, handler) {
deixe manipuladores = this._eventHandlers && this._eventHandlers [eventName];
se (! handlers) retornarem;
para (deixe i = 0; i <handlers.length; i ++) {
se (manipuladores [i] == manipulador) {
handlers.splice (i--, 1);
}
}
},

/ **
* Gerar o evento e anexar os dados a ele
* this.trigger ('select', data1, data2);
* /
trigger (eventName, ... args) {
se (! this._eventHandlers ||! this._eventHandlers [eventName]) {
Retorna; // nenhum manipulador para esse nome de evento
}

// chamar os manipuladores
this._eventHandlers [eventName] .forEach (handler => handler.apply (this, args));
}
};
`` `

Existem três métodos aqui:

1. `.on (eventName, handler)` - atribui a função `handler` para executar quando ocorre o evento com esse nome. Os manipuladores são armazenados na propriedade `_eventHandlers`.
2. `.off (eventName, handler)` - remove a função da lista de manipuladores.
3. `.trigger (eventName, ... args)` - gera o evento: todos os manipuladores atribuídos são chamados e `args` são passados ​​como argumentos para eles.


Uso:

`` `js run
// Faça uma aula
classe Menu {
escolha (valor) {
this.trigger ("select", value);
}
}
// Adicione o mixin
Object.assign (Menu.prototype, eventMixin);

deixe o menu = novo menu ();

// chame o manipulador na seleção:
*! *
menu.on ("selecionar", valor => alerta ("Valor selecionado:" + valor));
* /! *

// aciona o evento => mostra Valor selecionado: 123
menu.choose ("123"); // valor selecionado
`` `

Agora, se tivermos o código interessado em reagir na seleção do usuário, podemos vinculá-lo com `menu.on (...)`.

E o `eventMixin` pode adicionar esse comportamento a tantas classes como gostaríamos, sem interferir com a cadeia de herança.

## Resumo

* Mixin * - é um termo de programação genérico orientado a objetos: uma classe que contém métodos para outras classes.

Algumas outras línguas, por exemplo, Python permite criar mixins usando múltiplas heranças. O JavaScript não suporta múltiplas heranças, mas as mixins podem ser implementadas copiando-as para o protótipo.

Podemos usar mixins como forma de aumentar uma classe por múltiplos comportamentos, como o manuseio de eventos, como vimos acima.

As misturas podem se tornar um ponto de conflito se ocasionalmente substituírem métodos de classe nativa. Então, geralmente, um deve pensar bem sobre a nomeação de um mixin, para minimizar tal possibilidade.
