A resposta: o primeiro e o terceiro serão executados.

Detalhes:

`` `js run
// Corre.
// O resultado de -1 || 0 = -1, truthy
se (-1 || 0) alert ('first');

// Não é executado
// -1 && 0 = 0, falsidade
se (-1 && 0) alerta ('segundo');

// Executa
// O operador && tem uma precedência maior do que ||
// então -1 && 1 executa primeiro, dando-nos a cadeia:
// nulo || -1 && 1 -> null || 1 -> 1
se (nulo || -1 && 1) alerta ('terceiro');
`` `

