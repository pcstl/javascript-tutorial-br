A resposta: `1` e` 2`.

O primeiro manipulador dispara, porque não é removido por `removeEventListener`. Para remover o manipulador, precisamos passar exatamente a função que foi atribuída. E no código que uma nova função é passada, isso parece igual, mas ainda é outra função.

Para remover um objeto de função, precisamos armazenar uma referência a ele, assim:

`` `js
manipulador de função () {

}

button.addEventListener ("clique", manipulador);
button.removeEventListener ("clique", manipulador);
`` `

O manipulador `button.onclick` funciona de forma independente e além de` addEventListener`.
