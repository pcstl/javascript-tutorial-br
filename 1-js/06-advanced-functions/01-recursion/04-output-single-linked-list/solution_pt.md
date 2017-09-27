# Solução baseada em Loop

A variante baseada em loop da solução:

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

function printList (list) {
Deixe tmp = list;

enquanto (tmp) {
alerta (tmp.value);
tmp = tmp.next;
}

}

printList (lista);
`` `

Observe que usamos uma variável temporária `tmp` para percorrer a lista. Tecnicamente, poderíamos usar um parâmetro de função `list` em vez disso:

`` `js
function printList (list) {

enquanto (*! * list * /! *) {
alerta (lista.valor);
list = list.next;
}

}
`` `

... Mas isso não seria prudente. No futuro, talvez precisemos estender uma função, fazer outra coisa com a lista. Se mudarmos `list`, então perderemos essa habilidade.

Falando sobre nomes de variáveis ​​boas, `list` aqui é a própria lista. O primeiro elemento disso. E deve permanecer assim. Isso é claro e confiável.

Do outro lado, o papel de `tmp` é exclusivamente um percurso de lista, como` i` no loop `for`.

# Solução recursiva

A variante recursiva de `printList (list)` segue uma lógica simples: para emitir uma lista, devemos enviar o elemento atual `list`, então faça o mesmo para` list.next`:

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

function printList (list) {

alerta (lista.valor); // exibe o item atual

se (list.next) {
printList (list.next); // faça o mesmo pelo resto da lista
}

}

printList (lista);
`` `

Agora, o que é melhor?

Tecnicamente, o loop é mais efetivo. Essas duas variantes fazem o mesmo, mas o loop não gasta recursos para chamadas de função aninhadas.

Do outro lado, a variante recursiva é mais curta e às vezes mais fácil de entender.
