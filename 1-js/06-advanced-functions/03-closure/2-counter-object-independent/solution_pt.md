
Certamente, ele vai funcionar bem.

Ambas as funções aninhadas são criadas dentro do mesmo ambiente Lexical externo, de modo que compartilham o acesso à mesma variável `count`:

`` `js run
função Counter () {
Deixe contagem = 0;

this.up = function () {
retornar contagem de ++;
};

this.down = function () {
retornar - quantidade;
};
}

Let counter = new Counter ();

alerta (counter.up ()); // 1
alerta (counter.up ()); // 2
alerta (counter.down ()); // 1
`` `
