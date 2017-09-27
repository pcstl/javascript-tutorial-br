# Coordenadas

Para mover os elementos, devemos estar familiarizados com as coordenadas.

A maioria dos métodos de JavaScript lidam com um dos dois sistemas de coordenadas:

1. Relativo à janela (ou a outra viewport) superior / esquerda.
2. Relativo ao documento superior / esquerdo.

É importante entender a diferença e qual é o tipo de lugar.

[cortar]

## Coordenadas da janela: getBoundingClientRect

As coordenadas da janela começam no canto superior esquerdo da janela.

O método `elem.getBoundingClientRect ()` retorna as coordenadas da janela para `elem` como um objeto com propriedades:

- `top` - Y-coordenada para a borda do elemento superior,
- `left` - coordenada X para a borda do elemento esquerdo,
- `right` - coordenada X para o lado direito do elemento,
- `bottom` - Y-coordenada para a borda inferior do elemento.

Como isso:

! [] (coords.png)


As coordenadas da janela não levam em consideração a parte do documento, eles são calculados a partir da janela canto esquerdo-superior.

Em outras palavras, quando rolarmos a página, o elemento vai para cima ou para baixo *, as coordenadas da janela podem mudar *. Isso é muito importante.

`` `online
Clique no botão para ver as coordenadas da janela:

<input id = "brTest" type = "button" value = "Mostrar button.getBoundingClientRect () para este botão" onclick = 'showRect (this)' />

<script>
function showRect (elem) {
Deixe r = elem.getBoundingClientRect ();
alerta ("{top:" + r.top + ", esquerda:" + r.left + ", direita:" + r.right + ", inferior:" + r.bottom + "}");
}
</ script>

Se você rolar a página, a posição do botão muda, e as coordenadas da janela também.
`` `

Além disso:

- As coordenadas podem ser frações decimais. Isso é normal, internamente o navegador usa-os para cálculos. Não temos que arredondá-los ao configurar "style.position.left / top", o navegador está bem com as frações.
- As coordenadas podem ser negativas. Por exemplo, se a página estiver deslocada e a parte superior do `elem` estiver agora acima da janela, então` elem.getBoundingClientRect (). Top` é negativo.
- Alguns navegadores (como o Chrome) também adicionam ao resultado `getBoundingClientRect` propriedades` width` e `height`. Podemos obtê-los também por subtração: `height = bottom-top`,` width = right-left`.

`` `warn header =" As coordenadas direita / inferior são diferentes das propriedades CSS "
Se compararmos as coordenadas das janelas com o posicionamento CSS, então há semelhanças óbvias com `position: fixed` - também a posição relativa à viewport.

Mas no CSS, a propriedade `direita 'significa a distância da borda direita e a parte inferior da borda inferior.

Se olharmos para a imagem abaixo, podemos ver isso em JavaScript, não é assim. Todas as coordenadas da janela são contadas a partir do canto superior esquerdo, incluindo estas.
`` `

## elementFromPoint (x, y) [#elementFromPoint]

A chamada para `document.elementFromPoint (x, y)` retorna o elemento mais aninhado nas coordenadas da janela `(x, y)`.

A sintaxe é:

`` `js
deixe elem = document.elementFromPoint (x, y);
`` `

Por exemplo, o código abaixo destaca e exibe a tag do elemento que está agora no meio da janela:

`` `js run
Deixe centerX = document.documentElement.clientWidth / 2;
deixe centerY = document.documentElement.clientHeight / 2;

deixe elem = document.elementFromPoint (centerX, centerY);

elem.style.background = "red";
alert (elem.tagName);
`` `

Como ele usa coordenadas de janela, o elemento pode ser diferente dependendo da posição de rolagem atual.

`` `` cabeçalho de aviso = "Para as coordenadas fora da janela o` elementFromPoint` retorna `null`"
O método `document.elementFromPoint (x, y)` só funciona se `(x, y)` estiverem dentro da área visível.

Se qualquer uma das coordenadas é negativa ou excede a largura / altura da janela, então ela retorna 'nulo'.

Na maioria dos casos, esse comportamento não é um problema, mas devemos ter isso em mente.

Aqui está um erro típico que pode ocorrer se não verificarmos isso:

`` `js
deixe elem = document.elementFromPoint (x, y);
// se as coordenadas estiverem fora da janela, então elem = nulo
*! *
elem.style.background = ''; // Erro!
* /! *
`` `
`` ``

## Usando para posição: fixo

Na maioria das vezes, precisamos de coordenadas para posicionar algo. Em CSS, para posicionar um elemento relativo à viewport usamos `position: fixed` junto com` left / top` (ou `right / bottom`).

Podemos usar `getBoundingClientRect` para obter as coordenadas de um elemento e depois mostrar algo próximo.

Por exemplo, a função `createMessageUnder (elem, html)` abaixo mostra a mensagem em `elem`:

`` `js
deixe elem = document.getElementById ("coords-show-mark");

function createMessageUnder (elem, html) {
// criar elemento de mensagem
Deixe message = document.createElement ('div');
// melhor usar uma classe css para o estilo aqui
message.style.cssText = "position: fixed; color: red";

*! *
// atribua coordenadas, não esqueça "px"!
let coords = elem.getBoundingClientRect ();

message.style.left = coords.left + "px";
message.style.top = coords.bottom + "px";
* /! *

message.innerHTML = html;

mensagem de retorno;
}

// Uso:
// adicione-o por 5 segundos no documento
Deixe message = createMessageUnder (elem, 'Hello, world!');
document.body.append (mensagem);
setTimeout (() => message.remove (), 5000);
`` `

`` `online
Clique no botão para executá-lo:

<button id = "coords-show-mark"> Botão com id = "coords-show-mark", a mensagem aparecerá sob ele </ button>
`` `

O código pode ser modificado para mostrar a mensagem à esquerda, direita, abaixo, aplicar animações CSS para "desaparecer" e assim por diante. Isso é fácil, pois temos todas as coordenadas e tamanhos do elemento.

Mas observe os detalhes importantes: quando a página está rolando, a mensagem flui para longe do botão.

O motivo é óbvio: o elemento mensagem depende da "posição: fixa", por isso permanece no mesmo local da janela enquanto a página se desloca.

Para mudar isso, precisamos usar coordenadas baseadas em documentos e `position: absolute`.

## Coordenadas do documento

As coordenadas relativas ao documento começam a partir do canto superior esquerdo do documento, e não a janela.

Em CSS, as coordenadas da janela correspondem a `position: fixed`, enquanto as coordenadas do documento são semelhantes a` position: absolute` em cima.

Podemos usar `position: absolute` e` top / left` para colocar algo em um determinado local do documento, para que ele permaneça lá durante uma página de rolagem. Mas precisamos primeiro das coordenadas certas.

Para maior clareza, chamaremos as coordenadas da janela `(clientX, clientY)` e as coordenadas do documento `(pageX, pageY)`.

Quando a página não está rolando, as coordenadas da janela e as coordenadas do documento são realmente as mesmas. Os pontos zero coincidem também:

! [] (document-window-coordinates-zero.png)

E se o rolarmos, então `(clientX, clientY)` mudam, porque são relativos à janela, mas `(pageX, pageY)` permanecem os mesmos.

Aqui está a mesma página após o pergaminho vertical:

! [] (document-window-coordinates-scroll.png)

- `clientY` do cabeçalho` 'Do artigo apresentado hoje' 'tornou-se' 0 ', porque o elemento está agora no topo da janela.
- `clientX` não mudou, pois não nos deslocamos horizontalmente.
- as coordenadas `pageX` e` pageY` do elemento ainda são iguais, porque são relativas ao documento.

## Obtendo coordenadas do documento [#getCoords]

Não existe um método padrão para obter as coordenadas do documento de um elemento. Mas é fácil escrevê-lo.

Os dois sistemas de coordenadas estão conectados pela fórmula:
- `pageY` =` clientY` + altura da parte vertical deslocada do documento.
- `pageX` =` clientX` + largura da parte horizontal deslocada do documento.

A função `getCoords (elem)` tomará as coordenadas da janela de `elem.getBoundingClientRect ()` e adicionará a rolagem atual para eles:

`` `js
// obter coordenadas do documento do elemento
função getCoords (elemento) {
Deixe box = elem.getBoundingClientRect ();

Retorna {
topo: box.top + pageYOffset,
esquerda: box.left + pageXOffset
};
}
`` `

## Resumo

Qualquer ponto na página tem coordenadas:

1. Relativo à janela - `elem.getBoundingClientRect ()`.
2. Relativo ao documento - `elem.getBoundingClientRect ()` mais o deslocamento da página atual.

As coordenadas da janela são ótimas para usar com `position: fixed`, e as coordenadas do documento funcionam bem com` position: absolute`.

Ambos os sistemas de coordenadas têm seus "pro" e "contra", há momentos em que precisamos um ou outro, assim como o CSS `` `` `absolute` e` fixed`.
