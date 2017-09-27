# Funções de seta revisadas

Vamos rever as funções de seta.

[cortar]

As funções de seta não são apenas uma "taquigrafia" para escrever coisas pequenas.

O JavaScript está cheio de situações em que precisamos escrever uma pequena função, que é executada em outro lugar.

Por exemplo:

- `arr.forEach (func)` - `func` é executado por` forEach` para cada item da matriz.
- `setTimeout (func)` - `func` é executado pelo programador interno.
- ...há mais.

É no espírito de JavaScript criar uma função e passá-la em algum lugar.

E em tais funções geralmente não queremos deixar o contexto atual.

# Seta funções não têm "isto"

Como lembramos do capítulo <info: object-methods>, as funções de seta não possuem `this`. Se "acessível", é tirado do lado de fora.

Por exemplo, podemos usá-lo para iterar dentro de um método de objeto:

`` `js run
deixe group = {
título: "nosso grupo"
estudantes: ["John", "Pete", "Alice"],

showList() {
*! *
this.students.forEach (
student => alert (this.title + ':' + student)
);
* /! *
}
};

group.showList ();
`` `

Aqui em `forEach`, a função de seta é usada, então` this.title` é exatamente o mesmo que no método externo `showList`. Isso é: `group.title`.

Se usássemos uma função "regular", haveria um erro:

`` `js run
deixe group = {
título: "nosso grupo"
estudantes: ["John", "Pete", "Alice"],

showList() {
*! *
this.students.forEach (function (student) {
// Erro: Não é possível ler propriedade 'título' de indefinido
alerta (this.title + ':' + student)
});
* /! *
}
};

group.showList ();
`` `

O erro ocorre porque `forEach` executa funções com` this = undefined` por padrão, então a tentativa de acessar `undefined.title` é feita.

Isso não afeta as funções de seta, porque eles simplesmente não têm `this`.

`` `warn header =" As funções de seta não podem ser executadas com `new`"
Não ter "isto", naturalmente, significa outra limitação: as funções de seta não podem ser usadas como construtores. Não podem ser chamados com `new`.
`` `

`` `cabeçalho inteligente =" Funções de seta VS bind "
Há uma diferença sutil entre uma função de seta `=>` e uma função regular chamada com `.bind (this)`:

- `.bind (this)` cria uma "versão vinculada" da função.
- A seta `=>` não cria nenhuma ligação. A função simplesmente não tem `this`. A pesquisa de `this` é feita exatamente da mesma maneira que uma busca variável regular: no ambiente lexical externo.
`` `

# # As setas não têm "argumentos"

As funções de seta também não possuem uma variável de "argumentos".

Isso é ótimo para os decoradores, quando precisamos encaminhar uma chamada com o `this` atual e` argumentos '.

Por exemplo, `adiar (f, ms)` obtém uma função e retorna um invólucro ao redor que atrasa a chamada por milissegundos de ms ms:

`` `js run
função deferimento (f, ms) {
função de retorno () {
setTimeout (() => f.apply (this, arguments), ms)
};
}

função sayHi (who) {

}

deixe sayHiDeferred = adiar (sayHi, 2000);
sayHiDeferred ("John"); // Olá, John depois de 2 segundos
`` `

O mesmo sem uma função de seta parece:

`` `js
função deferimento (f, ms) {
função de retorno (... args) {
deixe ctx = isso;
setTimeout (function () {
retornar f.apply (ctx, args);
}, Senhora);
};
}
`` `

Aqui, tivemos que criar variáveis ​​adicionais `args` e` ctx` para que a função dentro de `setTimeout` pudesse levá-las.

## Resumo

Funções de seta:

- Não tenha `isto '.
- Não tem `argumentos '.
- Não pode ser chamado com `new`.
- (Eles também não têm `super`, mas não o estudamos. Será no capítulo <info: class-herança>).

Isso é porque eles são destinados a pequenos pedaços de código que não têm seu próprio "contexto", mas sim funcionam no atual. E eles realmente brilham nesse caso de uso.
