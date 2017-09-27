A bola tem `posição: absoluta '. Isso significa que as coordenadas "esquerda / superior" são medidas a partir do elemento posicionado mais próximo, que é `# field` (porque tem` position: relative`).

As coordenadas começam a partir do canto interno esquerdo-superior do campo:

!] (campo.png)

A largura / altura interna do campo é `clientWidth / clientHeight`. Então, o centro de campo tem coordenadas `(clientWidth / 2, clientHeight / 2)`.

... Mas se definimos `ball.style.left / top` para esses valores, então não a bola como um todo, mas a borda superior esquerda da bola ficaria no centro:

`` `js
ball.style.left = Math.round (field.clientWidth / 2) + 'px';
ball.style.top = Math.round (field.clientHeight / 2) + 'px';
`` `

Veja como parece:

[iframe height = 180 src = "ball-half"]

Para alinhar o centro da bola com o centro do campo, devemos mover a bola para a metade da sua largura para a esquerda e para a metade da sua altura até a parte superior:

`` `js
ball.style.left = Math.round (field.clientWidth / 2 - ball.offsetWidth / 2) + 'px';
ball.style.top = Math.round (field.clientHeight / 2 - ball.offsetHeight / 2) + 'px';
`` `

** Atenção: a armadilha! **

O código não funcionará de forma confiável enquanto `<img>` não possui largura / altura:

`` `html
<img src = "ball.png" id = "bola">
`` `

Quando o navegador não conhece a largura / altura de uma imagem (de atributos de etiqueta ou CSS), então assume que eles sejam iguais a `0 até a imagem terminar de carregar.

Na vida real após o primeiro navegador de carga normalmente armazena a imagem em cache, e nas próximas cargas, ele terá o tamanho imediatamente.

Mas na primeira carga, o valor de `ball.offsetWidth` é` 0`. Isso leva a coordenadas erradas.

Devemos consertar isso adicionando `width / height` a` <img> `:

`` `html
<img src = "ball.png" *! * width = "40" height = "40" * /! * id = "ball">
`` `

... Ou forneça o tamanho em CSS:

`` `css
#ball {
largura: 40px;
altura: 40px;
}
`` `
