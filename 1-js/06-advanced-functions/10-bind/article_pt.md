libs:
- Lodsh

---

# Ligação de função

Ao usar `setTimeout` com métodos de objeto ou métodos de objeto passando, há um problema conhecido:" perdendo isso ".

De repente, "isso" simplesmente pára de funcionar corretamente. A situação é típica para desenvolvedores novatos, mas também acontece com os experientes.

[cortar]

## Perder "este"

Nós já sabemos que em JavaScript é fácil perder `this`. Uma vez que um método é passado em algum lugar separadamente do objeto - `this` é perdido.

Veja como isso pode acontecer com `setTimeout`:

`` `js run
Deixe o usuário = {
primeiro nome: "John",
SayHi () {
alerta ('Olá, $ {this.firstName}! `);
}
};

*! *
setTimeout (user.sayHi, 1000); // Olá, indefinido!
* /! *
`` `

Como podemos ver, a saída mostra não "John" como `this.firstName`, mas` undefined`!

Isso porque o `setTimeout` obteve a função` user.sayHi`, separadamente do objeto. A última linha pode ser reescrita como:

`` `js
Deixe f = user.sayHi;
setTimeout (f, 1000); // contexto do usuário perdido
`` `

O método `setTimeout` no navegador é um pouco especial: ele define` this = window` para a chamada de função (para Node.JS, `this` torna-se o objeto do temporizador, mas na verdade não importa aqui). Então, para `this.firstName`, ele tenta obter` window.firstName`, que não existe. Em outros casos semelhantes, como veremos, geralmente "isso" simplesmente se torna "indefinido".

A tarefa é bastante típica - queremos passar um método de objeto em outro lugar (aqui - para o agendador) onde será chamado. Como garantir que ele seja chamado no contexto certo?

## Solução 1: um invólucro

A solução mais simples é usar uma função de envolvimento:

`` `js run
Deixe o usuário = {
primeiro nome: "John",
SayHi () {
alerta ('Olá, $ {this.firstName}! `);
}
};

*! *
setTimeout (function () {
user.sayHi (); // Olá john!
}, 1000);
* /! *
`` `

Agora funciona, porque recebe `usuário 'do ambiente lexical externo e, em seguida, chama o método normalmente.

O mesmo, mas mais curto:

`` `js
setTimeout (() => user.sayHi (), 1000); // Olá john!
`` `

Parece bem, mas uma pequena vulnerabilidade aparece em nossa estrutura de código.

E se antes do "setTimeout" desencadear (há um segundo atraso!), O "usuário" muda o valor? Então, de repente, isso chamará o objeto errado!


`` `js run
Deixe o usuário = {
primeiro nome: "John",
SayHi () {
alerta ('Olá, $ {this.firstName}! `);
}
};

setTimeout (() => user.sayHi (), 1000);

// ... dentro de 1 segundo
user = {sayHi () {alert ("Outro usuário em setTimeout!"); }};

// Outro usuário no setTimeout?!?
`` `

A próxima solução garante que tal coisa não acontecerá.

## Solução 2: ligar

As funções fornecem um método incorporado [bind] (mdn: js / Function / bind) que permite consertar `this`.

A sintaxe básica é:

`` `js
// uma sintaxe mais complexa será pouco depois
deixe boundFunc = func.bind (contexto);
`` ``

O resultado de `func.bind (context)` é uma função especial, como "objeto exótico", que é chamada como função e passa transparentemente a chamada para a configuração `func` this = context`.

Em outras palavras, chamar `boundFunc` é como` func` com fixo `this`.

Por exemplo, aqui `funcUser` passa uma chamada para` func` com `this = user`:

`` `js run
Deixe o usuário = {
primeiro nome: "John"
};

function func () {
alerta (this.firstName);
}

*! *
deixe funcUser = func.bind (user);
funcUser (); // John
* /! *
`` `

Aqui `func.bind (user)` como uma "variante vinculada" de `func`, com fixed` this = user`.

Todos os argumentos são passados ​​para o `func` original como está", por exemplo:

`` `js run
Deixe o usuário = {
primeiro nome: "John"
};

função func (frase) {
alerta (frase + ',' + this.firstName);
}

// liga isto ao usuário
deixe funcUser = func.bind (user);

*! *
funcUser ("Hello"); // Olá, John (argumento "Olá" é aprovado e isto = usuário)
* /! *
`` `

Agora vamos tentar com um método de objeto:


`` `js run
Deixe o usuário = {
primeiro nome: "John",
SayHi () {
alerta ('Olá, $ {this.firstName}! `);
}
};

*! *
deixe sayHi = user.sayHi.bind (user); // (*)
* /! *

diga oi(); // Olá john!

setTimeout (sayHi, 1000); // Olá john!
`` `

Na linha `(*)` tomamos o método `user.sayHi` e ligamos a` usuário`. O `sayHi` é uma função" vinculada ", que pode ser chamada sozinha ou passada para` setTimeout` - não importa, o contexto estará certo.

Aqui podemos ver que os argumentos são passados ​​"tal como está", apenas `this` é corrigido por` bind`:

`` `js run
Deixe o usuário = {
primeiro nome: "John",
diga (frase) {
alerta (`$ {frase}, $ {this.firstName}!`);
}
};

let say = user.say.bind (user);

diga olá"); // Olá, John (o argumento "Olá" é aprovado para dizer)
diga tchau"); // Bye, John ("Bye" é aprovado para dizer)
`` `

`` `` cabeçalho inteligente = "Método de conveniência:` bindAll` "
Se um objeto tem muitos métodos e planejamos passá-lo ativamente, então poderemos vinculá-los todos em um loop:

`` `js
para (deixe a chave no usuário) {
se (tipo de usuário [chave] == 'função') {
usuário [chave] = usuário [chave] .bind (usuário);
}
}
`` `

As bibliotecas de JavaScript também fornecem funções para ligação de massa conveniente, e. [_.bindAll (obj)] (http://lodash.com/docs#bindAll) em lodash.
`` ``

## Resumo

Método `func.bind (context, ... args)` retorna uma "variante vinculada" da função `func` que corrige o contexto` this` e os primeiros argumentos se for dado.

Normalmente, aplicamos `bind` para consertar` this` em um método de objeto, para que possamos passá-lo em algum lugar. Por exemplo, para `setTimeout`. Há mais motivos para "ligar" no desenvolvimento moderno, nós os encontraremos mais tarde.
