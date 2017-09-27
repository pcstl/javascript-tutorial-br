Vamos criar uma data usando o próximo mês, mas passamos zero como o dia:
`` `js run
função getLastDayOfMonth (ano, mês) {
deixar data = nova Data (ano, mês + 1, 0);
date.getDate de retorno ();
}

alerta (getLastDayOfMonth (2012, 0)); // 31
alerta (getLastDayOfMonth (2012, 1)); // 29
alerta (getLastDayOfMonth (2013, 1)); // 28
`` `

Normalmente, as datas começam a partir de 1, mas, tecnicamente, podemos passar qualquer número, a data se autoajará. Então, quando passamos 0, isso significa "um dia antes do primeiro dia do mês", ou seja: "o último dia do mês anterior".
