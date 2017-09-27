** Resposta: um erro. **

Tente:
`` `js run
function makeUser () {
Retorna {
Nome: "John",
ref: this
};
};

deixe user = makeUser ();

alerta (user.ref.name); // Erro: Não é possível ler a propriedade 'nome' de indefinido
`` `

Isso porque as regras que definem `this` não olham os literais do objeto.

Aqui, o valor de `this` dentro de` makeUser () `é` undefined`, porque é chamado como uma função, não como um método.

E o objeto literal em si não tem efeito sobre isso. O valor de `this` é um para toda a função, blocos de código e literais de objeto não o afetam.

Então `ref: this` realmente leva atual` this` da função.

Aqui está o caso oposto:

`` `js run
function makeUser () {
Retorna {
Nome: "John",
*! *
ref () {
devolva isso;
}
* /! *
};
};

deixe user = makeUser ();

alerta (user.ref (). nome); // John
`` `

Agora funciona, porque `user.ref ()` é um método. E o valor de `this` é definido para o objeto antes do ponto` .`.


