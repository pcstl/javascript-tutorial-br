O comprimento máximo deve ser `maxlength`, então precisamos cortá-lo um pouco mais curto, para dar espaço para as reticências.

Observe que, na verdade, existe um único caractere unicode para uma reticência. Não são três pontos.

`` `js run
função truncada (str, maxlength) {
retorno (str.length> maxlength)?
str.slice (0, maxlength - 1) + '...': str;
}
`` `

