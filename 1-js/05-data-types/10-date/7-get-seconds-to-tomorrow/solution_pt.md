Para obter o número de milissegundos até amanhã, podemos de "amanhã 00:00:00" restar a data atual.

Primeiro, geramos esse "amanhã" e depois fazemos isso:

`` `js run
função getSecondsToTomorrow () {
Deixe agora = nova data ();

// data de amanhã
deixe amanhã = nova data (now.getFullYear (), now.getMonth (), *! * now.getDate () + 1 * /! *);

deixe diff = amanhã - agora; // diferença em ms
retornar Math.round (diff / 1000); // converter em segundos
}
`` `
