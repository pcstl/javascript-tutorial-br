O método `date.getDay ()` retorna o número do dia da semana, a partir de domingo.

Vamos fazer uma série de dias da semana, para que possamos obter o nome do dia adequado pelo seu número:

`` `js run
função getWeekDay (data) {
Deixe dias = ['SU', 'MO', 'TU', 'WE', 'TH', 'FR', 'SA'];

dias de retorno [date.getDay ()];
}

let date = new Date (2014, 0, 3); // 3 de janeiro de 2014
alerta (getWeekDay (data)); // FR
`` `
