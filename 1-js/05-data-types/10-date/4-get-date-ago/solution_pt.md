A idéia é simples: subtrair determinado número de dias de `data`:

`` `js
função getDateAgo (data, dias) {
date.setDate (date.getDate () - days);
date.getDate de retorno ();
}
`` `

... Mas a função não deve mudar `data`. Isso é importante, porque o código externo que nos dá a data não espera que ele mude.

Para implementá-lo, vamos clonar a data, assim:

`` `js run
função getDateAgo (data, dias) {
Deixe dateCopy = new Date (date);

dateCopy.setDate (date.getDate () - days);
data de retornoCopy.getDate ();
}

Deixe data = nova data (2015, 0, 2);

alerta (getDateAgo (data, 1)); // 1, (1 de janeiro de 2015)
alerta (getDateAgo (data, 2)); // 31, (31 de dezembro de 2014)
alerta (getDateAgo (data, 365)); // 2, (2 de janeiro de 2014)
`` `
