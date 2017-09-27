# Tamanho do elemento e rolagem

Existem muitas propriedades de JavaScript que permitem ler informações sobre a largura do elemento, a altura e outros recursos de geometria.

Muitas vezes, precisamos deles quando movemos ou posicionamos elementos em JavaScript, para calcular corretamente as coordenadas.

[cortar]


## Elemento de exemplo

Como um elemento de amostra para demonstrar propriedades, usaremos o que está abaixo:

`` `html não embelezam
<div id = "exemplo">
...Texto...
</ div>
<style>
#exemplo {
largura: 300px;
altura: 200px;
borda: 25px sólido # E8C48F;
preenchimento: 20px;
transbordamento: auto;
}
</ style>
`` `

Tem a borda, preenchimento e rolagem. O conjunto completo de recursos. Não há margens, pois não são parte do próprio elemento e não existem propriedades especiais para eles.

O elemento parece assim:

!] [] (metric-css.png)

Você pode [abrir o documento na caixa de areia] (sandbox: métrica).

`` `smart header =" Mind the scrollbar "
A imagem acima demonstra o caso mais complexo quando o elemento possui uma barra de rolagem. Alguns navegadores (nem todos) reservam o espaço para isso tirando o conteúdo.

Assim, sem barra de rolagem, a largura do conteúdo seria `300px`, mas se a barra de rolagem for" 16px "(a largura pode variar entre dispositivos e navegadores), então apenas` 300-16 = 284px` permanece, e devemos levá-lo em consideração . É por isso que exemplos deste capítulo assumem que há uma barra de rolagem. Se não há barra de rolagem, as coisas são um pouco mais simples.
`` `

`` `smart header =" O `padding-bottom` pode ser preenchido com o texto"
Normalmente, os estofos são mostrados vazios em ilustrações, mas se há muito texto no elemento e ele transborda, os navegadores mostram o texto "transbordando" no `padding-bottom`, para que você possa ver isso em exemplos. Mas o preenchimento ainda existe, a menos que especificado de outra forma.
`` `

## Geometria

As propriedades dos elementos que fornecem largura, altura e outras geometrias são sempre números. Eles são assumidos em pixels.

Aqui está a imagem geral:

! [] (metric-all.png)

Eles são muitas propriedades, é difícil encaixá-las todas na única imagem, mas seus valores são simples e fáceis de entender.

Vamos começar a explorá-los de fora do elemento.

## offsetParent, offsetLeft / Top

Essas propriedades raramente são necessárias, mas ainda são as propriedades de geometria "mais externas", então vamos começar com elas.

O `offsetParent` é o antepassado mais próximo que é:

1. CSS posicionado (`position` é` absolute`, `relative` ou` fixed`),
2. ou `<td>`, `<th>`, `<table>`,
2. ou `<body>`.

Na maioria dos casos práticos, podemos usar `offsetParent` para obter o antepassado mais próximo do CSS. E `offsetLeft / offsetTop` fornecem coordenadas x / y em relação ao seu canto esquerdo-superior.

No exemplo abaixo, o `` <div> `tem '<main>` como `offsetParent` e` offsetLeft / offsetTop` são deslocamentos do seu canto superior esquerdo (`180'):

`` `html run height = 10
<main style = "position: relative" id = "main">
<artigo>
<div id = "exemplo" style = "position: absolute; left: 180px; top: 180px"> ... </ div>
</ article>
</ main>
<script>
alerta (example.offsetParent.id); // a Principal
alerta (exemplo.offsetLeft); // 180 (nota: um número, não uma string "180px")
alerta (exemplo.offsetTop); // 180
</ script>
`` `

!] [] (metric-offset-parent.png)


Há várias ocasiões em que `offsetParent` é` null`:

1. Para elementos não apresentados (`display: none` ou não no documento).
2. Para `<body>` e `<html>`.
3. Para elementos com `position: fixed` neles.

## offsetWidth / Height

Agora vamos passar para o próprio elemento.

Essas duas propriedades são as mais simples. Eles fornecem a largura / altura "externa" do elemento. Ou, em outras palavras, seu tamanho completo, incluindo fronteiras.

!] [] (metric-offset-width-height.png)

Para o nosso elemento de amostra:

- `offsetWidth = 390` - a largura externa, pode ser calculada como largura CSS interna (` 300px`) mais cofres (`2 * 20px`) e bordas (` 2 * 25px`).
- `offsetHeight = 290` - a altura exterior.

`` `` cabeçalho inteligente = "Propriedades de geometria para elementos não mostrados são zero / nulo"
As propriedades de geometria são calculadas apenas para os elementos mostrados.

Se um elemento (ou qualquer um dos seus antepassados) tiver "exibir: nenhum" ou não estiver no documento, todas as propriedades de geometria são zero ou "nulo", dependendo do que é.

Por exemplo, `offsetParent` é` null`, e `offsetWidth`,` offsetHeight` são `0`.

Podemos usar isso para verificar se um elemento está escondido, como este:

`` `js
function isHidden (elemento) {
return! element.offsetWidth &&! element.offsetHeight;
}
`` `

Observe que tal `isHidden` retorna` true` para elementos que estão na tela, mas têm zero tamanhos (como um vazio `<div>`).
`` ``

## clientTop / Left

Dentro do elemento, temos as fronteiras.

Para os medir, existem propriedades `clientTop` e` clientLeft`.

No nosso exemplo:

- `clientLeft = 25` - largura da borda esquerda
- `clientTop = 25` - largura da borda superior

!] [] (metric-client-left-top.png)

... Mas, para ser preciso - não são bordas, mas coordenadas relativas do lado interno do lado externo.

Qual é a diferença?

Ele fica visível quando o documento é de direita para a esquerda (o sistema de operação está em idiomas árabes ou hebraicos). A barra de rolagem não está à direita, mas à esquerda, e depois `clientLeft` também inclui a largura da barra de rolagem.

Nesse caso, `clientLeft` no nosso exemplo não seria` 25`, mas com a largura da barra de rolagem `25 + 16 = 41`:

!] [] (metric-client-left-top-rtl.png)

## clientWidth / Height

Essas propriedades fornecem o tamanho da área dentro das bordas do elemento.

Eles incluem a largura do conteúdo juntamente com os paddings, mas sem a barra de rolagem:

!] [] (metric-client-width-height.png)

Na foto acima, consideremos primeiro `clientHeight`: é mais fácil de avaliar. Não existe uma barra de rolagem horizontal, então é exatamente a soma do que está dentro das bordas: CSS-height `200px` mais top e bottom paddings (` 2 * 20px`) total `240px`.

Agora, `clientWidth` - aqui a largura do conteúdo não é` 300px`, mas `284px`, porque` 16px` são ocupados pela barra de rolagem. Então, a soma é `284px` mais os estojos esquerdo e direito, total` 324px`.

** Se não houver nenhum paddings, então `clientWidth / Height` é exatamente a área de conteúdo, dentro das bordas e da barra de rolagem (se houver). **

! [] (metric-client-width-nopadding.png)

Então, quando não há preenchimento, podemos usar `clientWidth / clientHeight` para obter o tamanho da área de conteúdo.

## scrollWidth / Height

- Propriedades `clientWidth / clientHeight` apenas conta para a parte visível do elemento.
- Propriedades `scrollWidth / scrollHeight` também incluem a parte deslocada (oculta):

!] [] (metric-scroll-width-height.png)

Na foto acima:

- `scrollHeight = 723` - é a altura interna total da área de conteúdo, incluindo a parte rolada.
- `scrollWidth = 324` - é a largura interna total, aqui não temos scroll horizontal, então é igual a` clientWidth`.

Nós podemos usar essas propriedades para expandir o elemento de largura para sua largura / altura total.

Como isso:

`` `js
// expande o elemento para a altura total do conteúdo
element.style.height = element.scrollHeight + 'px';
`` `

`` `online
Clique no botão para expandir o elemento:

<div id = "element" style = "width: 300px; height: 200px; padding: 0; overflow: auto; border: 1px solid black;"> texto texto texto texto texto texto texto texto texto texto texto texto texto texto texto texto do texto texto texto texto texto texto texto texto texto texto texto texto texto texto texto texto texto texto texto texto text texto text texto do texto texto texto texto texto texto texto texto texto texto texto texto texto texto texto texto texto texto texto texto text texto text texto do texto texto texto texto texto texto texto texto texto text texto text texto texto texto texto texto texto texto texto </ div>

<button style = "padding: 0" onclick = "element.style.height = element.scrollHeight + 'px'"> element.style.height = element.scrollHeight + 'px' </ button>
`` `

## scrollLeft / scrollTop

Propriedades `scrollLeft / scrollTop` são a largura / altura da parte oculta e deslocada do elemento.

Na imagem abaixo, podemos ver `scrollHeight` e` scrollTop` para um bloco com um rolagem vertical.

!] [] (metric-scroll-top.png)

Em outras palavras, `scrollTop` é" quanto é rolado ".

`` `` cabeçalho inteligente = "` scrollLeft / scrollTop` pode ser modificado "
A maioria das propriedades de geometria que são somente leitura, mas `scrollLeft / scrollTop` pode ser alterada e o navegador irá rolar o elemento.

`` `online
Se você clicar no elemento abaixo, o código `elem.scrollTop + = 10 'será executado. Isso faz com que o conteúdo do elemento role `10px` abaixo.

<div onclick = "this.scrollTop + = 10" style = "cursor: ponteiro; borda: 1px sólido preto; largura: 100px; altura: 80px; transbordamento: auto"> Click to Me <br> 1 <br> 2 <br> 3 <br> 4 <br> 5 <br> 6 <br> 7 <br> 8 <br> 9 </ div>
`` `

Definir `scrollTop` para` 0` ou `Infinity` fará o elemento se deslocar para o topo / baixo, respectivamente.
`` ``

## Não tome largura / altura de CSS

Acabamos de abordar as propriedades da geometria dos elementos DOM. Normalmente são usados ​​para obter larguras, alturas e calcular distâncias.

Mas, como sabemos do capítulo <info: styles-and-classes>, podemos ler CSS-height e width usando `getComputedStyle`.

Então, por que não ler a largura de um elemento como este?

`` `js run
deixe elem = document.body;

alerta (getComputedStyle (elem) .width); // mostra largura CSS para elem
`` `

Por que devemos usar propriedades de geometria em vez disso? Existem dois motivos:

1. Primeiro, a largura / altura do CSS depende de outra propriedade: `box-sizing` que define" o que é "CSS largura e altura. Uma alteração no `box-sizing` para fins CSS pode quebrar esse JavaScript.
2. Em segundo lugar, CSS `width / height` pode ser` auto`, por exemplo, para um elemento inline:

`` `html run
<span id = "elem"> Olá! </ span>

<script>
*! *
alerta (getComputedStyle (elem) .width); // auto
* /! *
</ script>
`` `

Do ponto de vista do CSS, `width: auto` é perfeitamente normal, mas em JavaScript precisamos de um tamanho exato em` px` que podemos usar nos cálculos. Então, a largura do CSS é inútil.

E há mais um motivo: uma barra de rolagem. Às vezes, o código que funciona bem sem uma barra de rolagem começa a se incomodar, porque uma barra de rolagem tira o espaço do conteúdo em alguns navegadores. Portanto, a largura real disponível para o conteúdo é * menos * do que a largura do CSS. E `clientWidth / clientHeight 'leva isso em conta.

... Mas com `getComputedStyle (elem) .width` a situação é diferente. Alguns navegadores (por exemplo, Chrome) retornam a largura interna real, menos a barra de rolagem, e algumas delas (por exemplo, Firefox) - Largura CSS (ignore a barra de rolagem). Essas diferenças de cross-browser são a razão para não usar `getComputedStyle`, mas sim dependem de propriedades de geometria.

`` `online
Se o seu navegador reservar o espaço para uma barra de rolagem (a maioria dos navegadores para o Windows), você pode testá-lo abaixo.

[iframe src = "cssWidthScroll" link border = 1]

O elemento com texto tem "largura: 300 px" do CSS.

Em um desktop Windows OS, Firefox, Chrome, Edge tudo reserva o espaço para a barra de rolagem. Mas o Firefox mostra `300px`, enquanto o Chrome e o Edge mostram menos. Isso é porque o Firefox retorna a largura do CSS e outros navegadores retornam a largura "real".
`` `

Observe que a diferença descrita é apenas sobre a leitura de "getComputedStyle (...). Width" de JavaScript, visualmente tudo está correto.

## Resumo

Os elementos possuem as seguintes propriedades de geometria:

- `offsetParent` - é o ancestro posicionado mais próximo ou` td`, `th`,` table`, `body`.
- `offsetLeft / offsetTop` - coordenadas relativas ao limite superior esquerdo de` offsetParent`.
- `offsetWidth / offsetHeight` - largura / altura" externa "de um elemento, incluindo bordas.
- `clientLeft / clientTop` - a distância do canto externo superior esquerdo para o canto interno superior esquerdo-superior. Para o sistema operacional esquerdo para a direita, eles são sempre as larguras das bordas esquerda / superior. Para o sistema operacional de direita para a esquerda, a barra de rolagem vertical está à esquerda, então `clientLeft` inclui sua largura também.
- `clientWidth / clientHeight` - a largura / altura do conteúdo, incluindo os paddings, mas sem a barra de rolagem.
- `scrollWidth / scrollHeight` - a largura / altura do conteúdo, incluindo a parte rolada. Também inclui estofados, mas não a barra de rolagem.
- `scrollLeft / scrollTop` - largura / altura da parte deslocada do elemento, a partir do canto esquerdo-superior.

Todas as propriedades são de apenas leitura, exceto `scrollLeft / scrollTop`. Eles fazem o navegador rolar o elemento se for alterado.
