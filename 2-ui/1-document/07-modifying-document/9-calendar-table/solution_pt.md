Vamos criar a tabela como uma string: `" <table> ... </ table> "`, e depois atribuí-lo a `innerHTML`.

O algoritmo:

1. Crie o cabeçalho da tabela com `<th>` e nomes do dia da semana.
1. Crie o objeto de data `d = new Date (year, month-1)`. Esse é o primeiro dia do `mês '(tendo em conta que meses no JavaScript começam a partir de` 0`, não `1`).
2. Primeiras células até o primeiro dia do mês `d.getDay ()` pode estar vazio. Vamos preenchê-los com `<td> </ td>`.
3. Aumente o dia em `d`:` d.setDate (d.getDate () + 1) `. Se `d.getMonth ()` ainda não estiver no próximo mês, adicione a nova célula `<td>` ao calendário. Se for um domingo, então adicione uma nova linha <code> "& lt; / tr & gt; & lt; tr & gt;" </ code>.
4. Se o mês tiver terminado, mas a linha da tabela ainda não está cheia, adicione `` td `` vazio nela, para torná-la quadrada.
