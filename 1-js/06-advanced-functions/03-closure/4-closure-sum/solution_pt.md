Para que os segundos suportes funcionem, os primeiros devem retornar uma função.

Como isso:

`` `js run
soma de função (a) {

função de retorno (b) {
Retornar a + b; // tira "a" do ambiente lexical externo
};

}

alerta (soma (1) (2)); // 3
alerta (soma (5) (- 1)); // 4
`` `

