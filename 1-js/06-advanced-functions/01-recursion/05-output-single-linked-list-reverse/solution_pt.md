# Usando uma recursão

A lógica recursiva é um pouco complicada aqui.

Nós precisamos emitir primeiro o resto da lista e * então * exibir o atual:

`` `js run
deixe list = {
valor: 1,
Próximo: {
valor: 2,
Próximo: {
valor: 3,
Próximo: {
valor: 4,
seguinte: nulo
}
}
}
};

function printReverseList (lista) {

se (list.next) {
printReverseList (list.next);
}

alerta (lista.valor);
}

printReverseList (lista);
`` `

# Usando um loop

A variante do loop também é um pouco mais complicada do que a saída direta.

Não há como obter o último valor na nossa "lista". Nós também não podemos "voltar".

Então, o que podemos fazer é primeiro passar pelos itens na ordem direta e rememorá-los em uma matriz e, em seguida, emitir o que lembramos na ordem inversa:

`` `js run
deixe list = {
valor: 1,
Próximo: {
valor: 2,
Próximo: {
valor: 3,
Próximo: {
valor: 4,
seguinte: nulo
}
}
}
};

function printReverseList (lista) {
deixe arr = [];
Deixe tmp = list;

enquanto (tmp) {
arr.push (tmp.value);
tmp = tmp.next;
}

para (deixe i = arr.length - 1; i> = 0; i--) {
alerta (arr [i]);
}
}

printReverseList (lista);
`` `

Observe que a solução recursiva realmente faz exatamente o mesmo: ela segue a lista, lembra os itens na cadeia de chamadas aninhadas (na pilha de contexto de execução) e, em seguida, as exibe.
