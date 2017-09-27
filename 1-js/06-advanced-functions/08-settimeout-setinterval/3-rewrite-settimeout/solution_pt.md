

`` `js run
deixe i = 0;

deixe start = Date.now ();

Deixe timer = setInterval (count, 0);

contagem de função () {

para (let j = 0; j <1000000; j ++) {
i ++;
}

se (i == 1000000000) {
alerta ("Concluído em" + (Date.now () - start) + 'ms');
cancelInterval (timer);
}

}
`` `

