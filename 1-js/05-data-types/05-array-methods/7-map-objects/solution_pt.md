
`` `js run no-embellecer
let john = {nome: "John", sobrenome: "Smith", id: 1};
let pete = {nome: "Pete", sobrenome: "Hunt", id: 2};
deixe mary = {nome: "Mary", sobrenome: "Chave", id: 3};

Deixe os usuários = [john, pete, mary];

*! *
Deixe usuáriosMapados = users.map (user => ({
FullName: `$ {user.name} $ {user.sempleame}`,
id: user.id
}));
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

Observe que, para as funções de seta, precisamos usar suportes adicionais.

Não podemos escrever assim:
`` `js
Deixe usersMapped = users.map (user => *! * {* /! *
FullName: `$ {user.name} $ {user.sempleame}`,
id: user.id
});
`` `

Como nos lembramos, existem duas funções de seta: sem corpo `value => expr` e com corpo` value => {...} `.

Aqui, o JavaScript trataria `{` como o início do corpo da função, e não o início do objeto. A solução alternativa consiste em envolvê-los nos suportes "normais":

`` `js
Deixe usersMapped = users.map (user => *! * ({* /! *
FullName: `$ {user.name} $ {user.sempleame}`,
id: user.id
}));
`` `

Agora, bem.


