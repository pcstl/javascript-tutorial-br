Para combinar com precisão a funcionalidade de `switch`, o` if` deve usar uma comparação rigorosa `'==='`.

Para algumas cadeias, um simples `'=='` também funciona.

`` `js no-embellecer
se (browser == 'Edge') {
alerta ("You've the Edge!");
} else if (browser == 'Chrome'
|| navegador == 'Firefox'
|| navegador == 'Safari'
|| navegador == 'Opera') {
alerta ('Ok, nós também apoiamos esses navegadores');
} outro {
alerta ('Esperamos que esta página pareça bem!');
}
`` `

Observe: a construção `browser == 'Chrome' || browser == 'Firefox' ... `é dividido em múltiplas linhas para melhor legibilidade.

Mas a construção `switch` ainda é mais limpa e mais descritiva.
