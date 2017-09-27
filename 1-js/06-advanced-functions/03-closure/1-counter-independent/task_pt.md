importância: 5

---

# Os contadores são independentes?

Aqui fazemos dois contadores: `counter` e` counter2` usando a mesma função `makeCounter`.

Eles são independentes? O que o segundo contador vai mostrar? `0,1` ou` 2,3` ou outra coisa?

`` `js
função makeCounter () {
Deixe contagem = 0;

função de retorno () {
contagem de retorno ++;
};
}

let counter = makeCounter ();
Deixe counter2 = makeCounter ();

alerta (contador ()); // 0
alerta (contador ()); // 1

*! *
alerta (counter2 ()); //?
alerta (counter2 ()); //?
* /! *
`` `

