# Cantos exteriores

Os cantos externos são basicamente o que obtemos de [elem.getBoundingClientRect ()] (https://developer.mozilla.org/en-US/docs/DOM/element.getBoundingClientRect).

Coordenadas do canto superior esquerdo `answer1` e do canto inferior direito` answer2`:

`` `js
let coords = elem.getBoundingClientRect ();

Deixe answer1 = [coords.left, coords.top];
Deixe answer2 = [coords.right, coords.bottom];
`` `

# Canto interno superior esquerdo

Isso difere do canto externo pela largura da borda. Uma maneira confiável de obter a distância é `clientLeft / clientTop`:

`` `js
Deixe answer3 = [coords.left + field.clientLeft, coords.top + field.clientTop];
`` `

# Canto interno inferior direito

No nosso caso, precisamos subtrair o tamanho da borda das coordenadas externas.

Podemos usar o modo CSS:

`` `js
Deixe answer4 = [
coords.right - parseInt (getComputedStyle (campo) .borderRightWidth),
coords.bottom - parseInt (getComputedStyle (campo) .borderBottomWidth)
];
`` `

Uma maneira alternativa seria adicionar `clientWidth / clientHeight` às coordenadas do canto superior esquerdo. Isso provavelmente é ainda melhor:

`` `js
Deixe answer4 = [
coords.left + element.clientLeft + element.clientWidth,
coords.top + element.clientTop + element.clientHeight
];
`` `
