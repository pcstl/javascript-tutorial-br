A solução é curta, mas pode parecer um pouco complicada, então, forneço comentários extensivos:


`` `js
Deixe-se ordenadoRows = Array.from (table.rows)
.slice (1)
.sort ((rowA, rowB) => rowA.cells [0] .innerHTML> rowB.cells [0] .innerHTML? 1: -1);

table.tBodies [0] .append (... sortedRows);
`` `

1. Obter todos `<tr>`, como `table.querySelectorAll ('tr')`, então faça uma matriz deles, porque precisamos de métodos de matriz.
2. O primeiro TR (`table.rows [0]`) é realmente um cabeçalho da tabela, então nós tomamos o resto por `.slice (1)`.
3. Em seguida, classifique-os comparando pelo conteúdo do primeiro `<td>` (o campo de nome).
4. Agora insira os nós na ordem correta por `.append (... sortedRows)`.

As tabelas sempre têm um elemento <tbody> implícito, então precisamos levá-lo e inseri-lo: um simples `table.append (...)` falharia.

Por favor, note: não temos que removê-los, apenas "reinserir", eles deixam o antigo local automaticamente.
