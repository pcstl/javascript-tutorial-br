Vamos fazer um loop sobre `<li>`:

`` `js
para (deixe li de document.querySelector ('li')) {
...
}
`` `

No final, precisamos obter o texto dentro de cada `li`. Podemos lê-lo diretamente do primeiro nó filho, ou seja, o nó de texto:

`` `js
para (deixe li de document.querySelector ('li')) {
Deixe title = li.firstChild.data;

// title é o texto em <li> antes de outros nós
}
`` `

Então podemos obter o número de descendentes `li.getElementsByTagName ('li')`.
