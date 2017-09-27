importância: 5

---

# Classificar por campo

Temos uma série de objetos a serem ordenados:

`` `js
deixe os usuários = [
{nome: "John", idade: 20, sobrenome: "Johnson"},
{nome: "Pete", idade: 18, sobrenome: "Peterson"},
{nome: "Ann", idade: 19, sobrenome: "Hathaway"}
];
`` `

A maneira usual de fazer isso seria:

`` `js
// pelo nome (Ann, John, Pete)
users.sort ((a, b) => a.name> b.name? 1: -1);

// por idade (Pete, Ann, John)
users.sort ((a, b) => a.age> b.age? 1: -1);
`` `

Podemos torná-lo ainda menos detalhado, assim?

`` `js
users.sort (por campo ('nome'));
users.sort (por campo ('idade'));
`` `

Então, em vez de escrever uma função, basta colocar `byField (fieldName)`.

Escreva a função `byField` que pode ser usada para isso.
