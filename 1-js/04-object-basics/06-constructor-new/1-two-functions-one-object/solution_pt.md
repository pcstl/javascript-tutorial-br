Sim é possivel.

Se uma função retorna um objeto, o "novo" retorna ao invés de "isso".

Então, pode, por exemplo, retornar o mesmo objeto definido externamente 'obj`:

`` `js run no-embellecer
deixe obj = {};

função A () {return obj; }
função B () {return obj; }

alerta (novo A () == novo B ()); // verdade
`` `
