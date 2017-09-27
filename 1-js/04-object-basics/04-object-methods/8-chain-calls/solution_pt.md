A solução é retornar o objeto em si de cada chamada.

`` `js run
deixa ladder = {
passo 0,
acima() {
this.step ++;
*! *
devolva isso;
* /! *
},
baixa() {
this.step--;
*! *
devolva isso;
* /! *
},
showStep () {
alerta (this.step);
*! *
devolva isso;
* /! *
}
}

ladder.up (). up (). down (). up (). down (). showStep (); // 1
`` `

Também podemos escrever uma única chamada por linha. Para cadeias longas, é mais legível:

`` `js
escada
.acima()
.acima()
.baixa()
.acima()
.baixa()
.showStep (); // 1
`` `

