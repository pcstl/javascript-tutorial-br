importância: 5

---

# Contador de objeto

Aqui, um contador de objeto é feito com a ajuda da função do construtor.

será que vai dar certo? O que isso vai mostrar?

`` `js
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

alerta (counter.up ()); //?
alerta (counter.up ()); //?
alerta (counter.down ()); //?
`` `

