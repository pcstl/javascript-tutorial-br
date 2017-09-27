importância: 1

---

# Por que "aaa" permanece?

Execute o exemplo. Por que `table.remove ()` não exclui o texto `" aaa "`?

`` `html height = 100 run
<table id = "table">
aaa
<tr>
<td> Teste </ td>
</ tr>
</ table>

<script>
alerta (tabela); // a tabela, como deveria ser

table.remove ();
// por que ainda existe um documento no documento?
</ script>
`` `
