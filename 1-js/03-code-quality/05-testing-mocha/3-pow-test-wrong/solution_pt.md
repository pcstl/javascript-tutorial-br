O teste demonstra uma das tentações que um desenvolvedor atende ao escrever testes.

O que temos aqui é na verdade 3 testes, mas apresentado como uma única função com 3 afirmações.

Às vezes, é mais fácil escrever dessa maneira, mas se ocorrer um erro, é muito menos óbvio o que deu errado.

Se um erro ocorrer dentro de um fluxo de execução complexo, então teremos que descobrir os dados nesse ponto. Na verdade, devemos * depurar o teste *.

Seria muito melhor quebrar o teste em múltiplos blocos `it` com entradas e saídas claramente escritas.

Como isso:
`` `js
descreva ("Eleva x ao poder n", função () {
("5 no poder de 1 é igual a 5", função () {
assert.equal (pow (5, 1), 5);
});

("5 no poder de 2 é igual a 25", função () {
assert.equal (pow (5, 2), 25);
});

("5 no poder de 3 é igual a 125", função () {
assert.equal (pow (5, 3), 125);
});
});
`` `

Nós substituímos o `it` único por` `` `` `` `e um grupo de blocos` it`. Agora, se algo falhar, veríamos claramente quais eram os dados.

Também podemos isolar um único teste e executá-lo no modo autônomo, escrevendo `it.only` em vez de` it`:


`` `js
descreva ("Eleva x ao poder n", função () {
("5 no poder de 1 é igual a 5", função () {
assert.equal (pow (5, 1), 5);
});

*! *
// Mocha executará apenas esse bloco
apenas ("5 no poder de 2 é igual a 25", função () {
assert.equal (pow (5, 2), 25);
});
* /! *

("5 no poder de 3 é igual a 125", função () {
assert.equal (pow (5, 3), 125);
});
});
`` `
