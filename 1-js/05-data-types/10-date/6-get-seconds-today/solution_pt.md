Para obter o número de segundos, podemos gerar uma data usando o dia e a hora atuais 00:00:00, depois subtrai-lo de "agora".

A diferença é o número de milissegundos desde o início do dia, que devemos dividir por 1000 para obter segundos:

`` `js run
função getSecondsToday () {
Deixe agora = nova data ();

// crie um objeto usando o dia atual / mês / ano
Deixe hoje = nova data (now.getFullYear (), now.getMonth (), now.getDate ());

Deixe diff = now - today; // diferença de ms
retornar Math.round (diff / 1000); // faça segundos
}

alerta (getSecondsToday ());
`` `

Uma solução alternativa seria obter horas / minutos / segundos e convertê-los em segundos:

`` `js run
função getSecondsToday () {
let d = new Date ();
retornar d.getHours () * 3600 + d.getMinutes () * 60 + d.getSeconds ();
};
`` `
