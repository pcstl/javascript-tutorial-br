Para obter o tempo de `data` até agora - vamos subtrair as datas.

`` `js run
formato de funçãoDate (data) {
Deixe diff = new Date () - date; // a diferença em milissegundos

se (diff <1000) {// menos de 1 segundo
regresse "agora mesmo";
}

deixe sec = Math.floor (diff / 1000); // converter diff para segundos

se (seg <60) {
retornar seg + 'seg. atrás';
}

deixe min = Math.floor (diff / 60000); // converter diff para minutos
se (min <60) {
Retornar min + 'min. atrás';
}

// formatar a data
// adicionar zeros à esquerda para um dígito dia / mês / horas / minutos
Deixe d = data;
d = [
'0' + d.getDate (),
'0' + (d.getMonth () + 1),
'' + d.getFullYear (),
'0' + d.getHours (),
'0' + d.getMinutes ()
] .map (component => component.slice (-2)); // pegue os últimos 2 dígitos de cada componente

// junte os componentes à data
retornar d.slice (0, 3). juntar ('.') + '' + d.slice (3) .join (':');
}

alerta (formatDate (nova data (nova data - 1))); // "agora mesmo"

alerta (formatDate (nova data (nova data - 30 * 1000))); // "30 seg. Ago"

alerta (formatDate (nova data (nova data - 5 * 60 * 1000))); // "5 min. Ago"

// data de ontem como 31.12.2016, 20:00
alerta (formatDate (nova data (nova data - 86400 * 1000)));
`` `
