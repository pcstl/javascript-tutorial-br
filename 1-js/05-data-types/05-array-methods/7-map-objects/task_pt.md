importância: 5

---

# Mapear objetos

Você tem uma série de objetos de "usuário", cada um tem `nome`,` sobrenome 'e `id`.

Escreva o código para criar outra matriz a partir dele, de objetos com `id` e` fullName`, onde `fullName` é gerado a partir de` name` e `surname`.

Por exemplo:

`` `js no-embellecer
let john = {nome: "John", sobrenome: "Smith", id: 1};
let pete = {nome: "Pete", sobrenome: "Hunt", id: 2};
deixe mary = {nome: "Mary", sobrenome: "Chave", id: 3};

Deixe os usuários = [john, pete, mary];

*! *
deixe os usuários ser usados ​​= / * ... seu código ... * /
* /! *

/ *
usersMapped = [
{nome completo: "John Smith", id: 1},
{nome completo: "Pete Hunt", id: 2},
{nome completo: "Mary Key", id: 3}
]
* /

alerta (usersMapped [0] .id) // 1
alerta (usersMapped [0] .fullName) // John Smith
`` `

Então, na verdade, você precisa mapear uma série de objetos para outro. Tente usar `=>` aqui. Há uma pequena captura.
