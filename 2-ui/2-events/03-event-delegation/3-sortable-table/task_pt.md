importância: 4

---

# Tabela selecionável

Tornar a tabela classificável: os cliques nos elementos `<the>` devem ordená-lo pela coluna correspondente.

Cada `<th>` tem o tipo no atributo, assim:

`` `html
<table id = "grid">
<thead>
<tr>
*! *
<o tipo de dados = "número"> Idade </ th>
<th data-type = "string"> Nome </ th>
* /! *
</ tr>
</ thead>
<tbody>
<tr>
<Td> 5 </ td>
<td> John </ td>
</ tr>
<tr>
<Td> 10 </ td>
<td> Ann </ td>
</ tr>
...
</ tbody>
</ table>
`` `

No exemplo acima, a primeira coluna tem números e a segunda - strings. A função de classificação deve gerir o tipo de acordo com o tipo.

Somente os tipos `` string '' e `` number '' devem ser suportados.

O exemplo de trabalho:

[iframe border = 1 src = "solução" height = 190]

P.S. A tabela pode ser grande, com qualquer número de linhas e colunas.
