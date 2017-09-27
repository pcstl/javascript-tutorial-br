Claro, funciona, não há problema.

O `const` protege apenas a própria variável de mudar.

Em outras palavras, `user` armazena uma referência ao objeto. E não pode ser alterado. Mas o conteúdo do objeto pode.

`` `js run
const user = {
Nome: "John"
};

*! *
// trabalho
user.name = "Pete";
* /! *

// erro
usuário = 123;
`` `
