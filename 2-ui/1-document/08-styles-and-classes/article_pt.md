# Estilos e aulas

Antes de chegar a formas de JavaScript de lidar com estilos e aulas - aqui está uma regra importante. Espero que seja óbvio o suficiente, mas ainda temos que mencionar isso.

Geralmente, existem duas maneiras de modelar um elemento:

1. Crie uma classe em CSS e adicione-a: `<div class =" ... ">`
2. Escreva propriedades diretamente no `style`:` <div style = "..."> `.

[cortar]

CSS é sempre a maneira preferida - não só para HTML, mas também em JavaScript.

Nós só devemos manipular a propriedade `style` se classes" não podem lidar com isso ".

Por exemplo, `style` é aceitável se calculamos as coordenadas de um elemento de forma dinâmica e desejamos configurá-los de JavaScript, assim:

`` `js
Deixe top = / * cálculos complexos * /;
deixe a esquerda = / * cálculos complexos * /;
elem.style.left = left; // p.ex. '123px'
elem.style.top = top; // p.ex. '456px'
`` `

Para outros casos, como fazer o texto vermelho, adicionando um ícone de fundo - descreva isso em CSS e depois aplique a classe. Isso é mais flexível e fácil de suportar.

## className e classList

Alterar uma classe é uma das ações mais frequentemente em scripts.

Na época antiga, havia uma limitação em JavaScript: uma palavra reservada como "classe" não poderia ser uma propriedade de objeto. Essa limitação não existe agora, mas naquela época era impossível ter uma propriedade "classe", como `elem.class`.

Então, para as classes, a propriedade de aparência semelhante `" className "foi introduzida: o` elem.className` corresponde ao atributo `" class "`.

Por exemplo:

`` `html run
<body class = "main page">
<script>

</ script>
</ body>
`` `

Se designarmos algo para `elem.className`, ele substitui todas as cadeias de classes. Às vezes é o que precisamos, mas muitas vezes queremos adicionar / remover uma única classe.

Há outra propriedade para isso: `elem.classList`.

O `elem.classList` é um objeto especial com métodos para adicionar / remover / alternar classes.

Por exemplo:

`` `html run
<body class = "main page">
<script>
*! *
// adicione uma aula
document.body.classList.add ('artigo');
* /! *


</ script>
</ body>
`` `

Então, podemos operar ambos na cadeia de classe completa usando `className` ou em classes individuais usando` classList`. O que escolhemos depende de nossas necessidades.

Métodos de `classList`:

- `elem.classList.add / remove (" class ")` - adiciona / remove a classe.
- `elem.classList.toggle (" class ")` - se a classe existe, então o remove, caso contrário, adiciona.
- `elem.classList.contains (" class ")` - retorna `true / false`, verifica a classe dada.

Além disso, `classList` é iterável, então podemos listar todas as classes como esta:

`` `html run
<body class = "main page">
<script>
para (deixe o nome de document.body.classList) {
alerta (nome); // principal e, em seguida, página
}
</ script>
</ body>
`` `

## Estilo do elemento

A propriedade `elem.style` é um objeto que corresponde ao que está escrito no atributo` `style ''. Definir `elem.style.width =" 100px "` funciona como se tivéssemos no atributo `style =" width: 100px "`.

Para a propriedade de várias palavras, o camelCase é usado:

`` `js no-embellecer
background-color => element.style.backgroundColor
z-index => elem.style.zIndex
width-left-width => elem.style.borderLeftWidth
`` `

Por exemplo:

`` `js run
document.body.style.backgroundColor = prompt ('cor de fundo?', 'verde');
`` `

`` `` cabeçalho inteligente = "propriedades prefixadas"
Propriedades do prefixo do navegador como `-moz-border-radius`,` -webkit-border-radius` também seguem a mesma regra, por exemplo:

`` `js
button.style.MozBorderRadius = '5px';
button.style.WebkitBorderRadius = '5px';
`` `

Ou seja: um dash `" - "` se torna uma maiúscula.
`` ``

## Repor a propriedade de estilo

Às vezes, queremos atribuir uma propriedade de estilo e depois removê-la.

Por exemplo, para ocultar um elemento, podemos configurar `elem.style.display =" none "`.

Então, talvez possamos remover o `style.display` como se não estivesse definido. Em vez de `delete elem.style.display` devemos atribuir uma linha vazia para ele:` elem.style.display = "" `.

`` `js run
// se executarmos este código, o <body> "pisca"
document.body.style.display = "none"; // ocultar

setTimeout (() => document.body.style.display = "", 1000); // de volta ao normal
`` `

Se configurarmos `display` para uma string vazia, o navegador aplica as classes CSS e seus estilos internos normalmente, como se não existisse nenhuma propriedade` style`.

`` `` cabeçalho inteligente = "reescrita completa com` style.cssText` "
Normalmente, usamos `style. *` Para atribuir propriedades de estilo individuais. Não podemos definir o estilo completo como `div.style =" cor: vermelho; largura: 100px "`, porque `div.style` é um objeto e é somente leitura.

Para definir o estilo completo como uma string, existe uma propriedade especial `style.cssText`:

`` `html run
<div id = "div"> Botão </ div>

<script>
// podemos definir bandeiras de estilo especiais como "importantes" aqui
div.style.cssText = `color: red! important;
cor de fundo: amarelo;
largura: 100px;
texto-alinhamento: centro;
`;

alerta (div.style.cssText);
</ script>
`` `

Nós raramente usamos isso, porque essa tarefa remove todos os estilos existentes: ele não adiciona, mas os substitui. Pode ocasionalmente excluir algo necessário. Mas ainda pode ser feito para novos elementos quando sabemos que não eliminamos algo importante.

O mesmo pode ser realizado definindo um atributo: `div.setAttribute ('style', 'color: red ...')`.
`` ``

# Mente as unidades

As unidades CSS devem ser fornecidas com valores de estilo.

Por exemplo, não devemos configurar `elem.style.top` para` 10`, mas sim `10px`. Caso contrário, não funcionaria:

`` `html run height = 100
<corpo>
<script>
*! *
// não funciona!
document.body.style.margin = 20;

* /! *

// agora adicione a unidade CSS (px) - e funciona
document.body.style.margin = '20px';




</ script>
</ body>
`` `

Observe como o navegador "descompacta" a propriedade `style.margin` nas últimas linhas e infere` style.marginLeft` e `style.marginTop` (e outras margens parciais) dele.

## Estilos calculados: getComputedStyle

Modificar um estilo é fácil. Mas como * ler *?

Por exemplo, queremos saber o tamanho, as margens, a cor de um elemento. Como fazer isso?

** A propriedade `style` opera apenas no valor do atributo` "style" `, sem qualquer cascata CSS. **

Portanto, não podemos ler nada que venha de classes CSS usando `elem.style`.

Por exemplo, aqui `style` não vê a margem:

`` `html run height = 60 não-embelezar
<head>
<style> corpo {cor: vermelho; margem: 5px} </ style>
</ head>
<corpo>

O texto vermelho
<script>
*! *


* /! *
</ script>
</ body>
`` `

... Mas e se precisarmos, digamos, aumentar a margem em 20px? Queremos o valor atual para o início.

Existe outro método para isso: `getComputedStyle`.

A sintaxe é:

`` `js
GetComputedStyle (elemento [, pseudo])
`` `

elemento
: Elemento para ler o valor para.

pseudo
: Um pseudo-elemento, se necessário, por exemplo `:: before`. Uma string vazia ou nenhum argumento significa o próprio elemento.

O resultado é um objeto com propriedades de estilo, como `elem.style`, mas agora em relação a todas as classes de CSS.

Por exemplo:

`` `html run height = 100
<head>
<style> corpo {cor: vermelho; margem: 5px} </ style>
</ head>
<corpo>

<script>
deixe computedStyle = getComputedStyle (document.body);

// agora podemos ler a margem e a cor dele

alerta (calculadoStyle.marginTop); // 5px
alerta (computedStyle.color); // rgb (255, 0, 0)
</ script>

</ body>
`` `

`` `cabeçalho inteligente =" valores calculados e resolvidos "
Existem dois conceitos em [CSS] (https://drafts.csswg.org/cssom/#resolved-values):

1. Um * valor de estilo calculado * é o valor depois que todas as regras CSS e herança CSS são aplicadas, como resultado da cascata CSS. Se pode parecer "height: 1em` ou` font-size: 125% `.
2. Um * valor de estilo * resolvido é o que finalmente foi aplicado ao elemento. Valores como `1em` ou` 125% `são relativos. O navegador leva o valor calculado e torna todas as unidades fixas e absolutas, por exemplo: `height: 20px` ou` font-size: 16px`. Para propriedades de geometria, os valores resolvidos podem ter um ponto flutuante, como "largura: 50.5px".

Há muito tempo, "getComputedStyle" foi criado para obter valores calculados, mas descobriu que os valores resolvidos são muito mais convenientes e o padrão mudou.

Então, hoje em dia `getComputedStyle` retorna o valor resolvido da propriedade.
`` `

`` ``'ar cabeçalho = "` getComputedStyle` requer o nome completo da propriedade "
Nós sempre devemos pedir a propriedade exata que queremos, como `paddingLeft` ou` marginTop` ou `borderTopWidth`. Caso contrário, o resultado correto não é garantido.

Por exemplo, se houver propriedades `paddingLeft / paddingTop`, então o que devemos obter para` getComputedStyle (elem) .padding`? Nada, ou talvez um valor "gerado" de blocos conhecidos? Não há uma regra padrão aqui.

Existem outras inconsistências. Por exemplo, alguns navegadores (Chrome) mostram `10px` no documento abaixo, e alguns deles (Firefox) - não:

`` `html run
<style>
corpo {
margem: 10px;
}
</ style>
<script>
deixe style = getComputedStyle (document.body);
alerta (style.margin); // string vazia no Firefox
</ script>
`` `
`` ``

Os estilos de links `` `smart header =" \ "Visitado \" estão ocultos! "
Os links visitados podem ser coloridos usando a pseudo-classe C: CSS visitada.

Mas `getComputedStyle` não dá acesso a essa cor, porque, de outra forma, uma página arbitrária poderia descobrir se o usuário visitou um link criando-o na página e verificando os estilos.

JavaScript pode não ver os estilos aplicados por `: visitado '. E também, há uma limitação no CSS que proíbe a aplicação de estilos de mudança de geometria em `: visitado '. Isso é para garantir que não há nenhuma maneira secundária para uma página doente para testar se um link foi visitado e, portanto, quebrar a privacidade.
`` `

## Resumo

Para gerenciar classes, existem duas propriedades DOM:

- `className` - o valor da string, bom para gerenciar todo o conjunto de classes.
- `classList` - o objeto com métodos` add / remove / toggle / contains`, bom para classes individuais.

Para alterar os estilos:

- A propriedade `style` é um objeto com estilos CamelCased. Ler e escrever para ele tem o mesmo significado que modificar propriedades individuais no atributo `" style "`. Para ver como aplicar `importante` e outras coisas raras - existe uma lista de métodos em [MDN] (mdn: api / CSSStyleDeclaration).

- A propriedade `style.cssText` corresponde ao atributo` `style`` inteiro, a cadeia completa de estilos.

Para ler os estilos resolvidos (em relação a todas as classes, depois de todo o CSS é aplicado e os valores finais são calculados):

- O `getComputedStyle (elem [, pseudo]) retorna o objeto estilo-estilo com eles. Somente leitura.
