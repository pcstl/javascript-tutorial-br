importância: 5

---

# Crie uma árvore do objeto

Escreva uma função `createTree` que cria uma lista de" ul / li "aninhada do objeto aninhado.

Por exemplo:

`` `js
Deixe dados = {
"Peixe": {
"truta": {},
"salmão": {}
},

"Árvore": {
"Enorme": {
"sequoia": {},
"carvalho": {}
},
"Floração": {
"redbud": {},
"magnólia": {}
}
}
};
`` `

A sintaxe:

`` `js
deixe container = document.getElementById ('container');
*! *
createTree (container, data); // cria a árvore no recipiente
* /! *
`` `

O resultado (árvore) deve ser assim:

[iframe border = 1 src = "build-tree-dom"]

Escolha uma das duas maneiras de resolver esta tarefa:

1. Crie o HTML para a árvore e, em seguida, atribua a `container.innerHTML`.
2. Crie nós de árvore e adicione com métodos DOM.

Seria ótimo se você pudesse fazer os dois.

P.S. A árvore não deve ter elementos "extras" como vazios `<ul> </ ul>` para as folhas.
