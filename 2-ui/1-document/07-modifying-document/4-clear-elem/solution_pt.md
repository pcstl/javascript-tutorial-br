
Primeiro, vejamos como * não * para fazê-lo:

`` `js
função clara (elem) {
para (vamos i = 0; i <elem.childNodes.length; i ++) {
elem.childNodes [i] .remove ();
}
}
`` `

Isso não funcionará, porque a chamada para `remove ()` desloca a coleção `elem.childNodes`, então os elementos começam a partir do índice` 0` sempre. Mas `i` aumenta, e alguns elementos serão ignorados.

O loop `for..of` também faz o mesmo.

A variante certa pode ser:

`` `js
função clara (elem) {
enquanto (elem.firstChild) {
elem.firstChild.remove ();
}
}
`` `

E também há uma maneira mais simples de fazer o mesmo:

`` `js
função clara (elem) {
elem.innerHTML = '';
}
`` `
