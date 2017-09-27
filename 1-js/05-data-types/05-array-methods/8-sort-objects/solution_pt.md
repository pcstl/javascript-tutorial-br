`` `js run no-embellecer
função classByName (arr) {
arr.sort ((a, b) => a.name> b.name);
}

deixe john = {nome: "John", idade: 25};
let pete = {name: "Pete", idade: 30};
Deixe mary = {nome: "Mary", idade: 28};

deixe arr = [john, pete, mary];

SortByName (arr);

// agora ordenado é: [john, mary, pete]
alerta (arr [1] .name) // Mary
`` `

