
Isso é porque `map.keys ()` retorna uma iterável, mas não uma matriz.

Podemos convertê-lo em uma matriz usando `Array.from`:


`` `js run
Deixe map = new Map ();

map.set ("nome", "John");

*! *
Deixe keys = Array.from (map.keys ());
* /! *

keys.push ("mais");

alerta (chaves); // nome, mais
`` `
