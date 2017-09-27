

`` `js run
deixe os usuários = [
{nome: "John", idade: 20, sobrenome: "Johnson"},
{nome: "Pete", idade: 18, sobrenome: "Peterson"},
{nome: "Ann", idade: 19, sobrenome: "Hathaway"}
];

*! *
função por campo (campo) {
retornar (a, b) => a [campo]> b [campo]? 1: -1;
}
* /! *

users.sort (por campo ('nome'));
users.forEach (user => alert (user.name)); // Ann, John, Pete

users.sort (por campo ('idade'));
users.forEach (user => alert (user.name)); // Pete, Ann, John
`` `

