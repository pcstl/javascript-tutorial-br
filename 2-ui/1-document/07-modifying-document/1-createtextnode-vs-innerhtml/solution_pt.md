Resposta: ** 1 e 3 **.

Ambos os comandos resultam na adição do `texto` como texto" no `elem`.

Aqui está um exemplo:

`` `html run height = 80
<div id = "elem1"> </ div>
<div id = "elem2"> </ div>
<div id = "elem3"> </ div>
<script>
Deixe texto = '<b> texto </ b>';

elem1.append (document.createTextNode (texto));
elem2.textContent = text;
elem3.innerHTML = texto;
</ script>
`` `
