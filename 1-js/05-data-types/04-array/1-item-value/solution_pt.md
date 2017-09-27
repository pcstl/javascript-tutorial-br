O resultado é `4`:


`` `js run
Deixe frutas = ["Maçãs", "Pêra", "Laranja"];

deixe shoppingCart = frutas;

shoppingCart.push ("Banana");

*! *
alerta (fruits.length); // 4
* /! *
`` `

Isso porque os arrays são objetos. Então, ambos `shoppingCart` e` fruits` são as referências à mesma matriz.

