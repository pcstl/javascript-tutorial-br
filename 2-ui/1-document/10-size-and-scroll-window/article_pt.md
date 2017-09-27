# Tamanho das janelas e rolagem

Como descobrir a largura da janela do navegador? Como obter a altura total do documento, incluindo a parte rolada? Como rolar a página usando JavaScript?

Do ponto de vista do DOM, o elemento do documento raiz é `document.documentElement`. Esse elemento corresponde a `<html>` e possui propriedades de geometria descritas no [capítulo anterior] (info: size-and-scroll). Para alguns casos, podemos usá-lo, mas existem métodos e peculiaridades adicionais importantes o bastante para considerar.

[cortar]

## Largura / altura da janela

As propriedades `clientWidth / clientHeight` de` document.documentElement` são exatamente o que queremos aqui:

! [] (document-client-width-height.png)

`` `online
Por exemplo, esse botão mostra a altura da sua janela:


`` `

`` `'' '' '' '' '' '' '' '' '' '' '' '' '' '' '' '' '' '' '' ''
Os navegadores também oferecem suporte às propriedades `window.innerWidth / innerHeight`. Parecem com o que queremos. Então, qual é a diferença?

Se houver uma barra de rolagem que ocupe algum espaço, `clientWidth / clientHeight` fornece a largura / altura dentro dela. Em outras palavras, eles retornam largura / altura da parte visível do documento, disponível para o conteúdo.

E `window.innerWidth / innerHeight` ignoram a barra de rolagem.

Se há uma barra de rolagem e ocupa algum espaço, essas duas linhas mostram valores diferentes:
`` `js run
alerta (window.innerWidth); // largura da janela completa
alerta (document.documentElement.clientWidth); // largura da janela menos a barra de rolagem
`` `

Na maioria dos casos, precisamos da largura da janela * disponível *: para desenhar ou posicionar algo. Isto é: dentro de barras de rolagem se houver algum. Então, devemos usar `documentElement.clientHeight / Width`.
`` ``

`` `warn header =" `DOCTYPE` é importante"
Observe: as propriedades de geometria de nível superior podem funcionar um pouco diferente quando não há `<! DOCTYPE HTML>` em HTML. São possíveis coisas estranhas.

No HTML moderno, sempre devemos escrever `DOCTYPE`. Geralmente, essa não é uma questão de JavaScript, mas aqui também afeta o JavaScript.
`` `

## Largura / altura do documento

Teoricamente, como o elemento do documento raiz é `documentElement.clientWidth / Height`, e inclui todo o conteúdo, poderíamos medir o tamanho completo como` documentElement.scrollWidth / scrollHeight`.

Essas propriedades funcionam bem para elementos comuns. Mas, para toda a página, essas propriedades não funcionam como pretendido. No Chrome / Safari / Opera se não houver rolo, então `documentElement.scrollHeight` pode ser ainda menor do que` documentElement.clientHeight`! Para elementos regulares que é um absurdo.

Para ter um tamanho de janela cheio confiável, devemos ter o máximo dessas propriedades:

`` `js run
Deixe scrollHeight = Math.max (
document.body.scrollHeight, document.documentElement.scrollHeight,
document.body.offsetHeight, document.documentElement.offsetHeight,
document.body.clientHeight, document.documentElement.clientHeight
);

alerta ('Largura total do documento, com parte desfiada:' + scrollHeight);
`` `

Por quê? Melhor não perguntar. Essas inconsistências vêm dos tempos antigos, não uma lógica "inteligente".

## Obtenha o deslocamento atual [# page-scroll]

Elementos regulares têm seu estado atual de rolagem em `elem.scrollLeft / scrollTop`.

O que há com a página? A maioria dos navegadores fornece `documentElement.scrollLeft / Top` para o deslocamento do documento, mas o Chrome / Safari / Opera tem erros (como [157855] (https://code.google.com/p/chromium/issues/detail?id=157855 ), [106133] (https://bugs.webkit.org/show_bug.cgi?id=106133)) e devemos usar `document.body` em vez de` document.documentElement` lá.

Por sorte, não precisamos lembrar essas peculiaridades, por causa das propriedades especiais `window.pageXOffset / pageYOffset`:

`` `js run
alerta ('Percurso atual a partir do topo:' + window.pageYOffset);
alerta ('Rolo atual a partir da esquerda:' + window.pageXOffset);
`` `

Essas propriedades são somente leitura.

## Deslocando: scrollTo, scrollBy, scrollIntoView [# window-scroll]

`` `warn
Para deslocar a página de JavaScript, o DOM deve ser totalmente construído.

Por exemplo, se tentarmos rolar a página do script em `<head>`, isso não funcionará.
`` `

Os elementos regulares podem ser rolados alterando `scrollTop / scrollLeft`.

Podemos fazer o mesmo para a página:
- Para todos os navegadores, exceto Chrome / Safari / Opera: modifique `document.documentElement.scrollTop / Left`.
- No Chrome / Safari / Opera: use `document.body.scrollTop / Left` em vez disso.

Isso deve funcionar, mas cheira a incompatibilidades entre navegadores. Não é bom. Felizmente, existe uma solução mais simples e mais universal: métodos especiais [window.scrollBy (x, y)] (mdn: api / Window / scrollBy) e [window.scrollTo (pageX, pageY)] (mdn: api / Window / scrollTo ).

- O método `scrollBy (x, y)` percorre a página em relação à sua posição atual. Por exemplo, `scrollBy (0,10)` rola a página `10px 'para baixo.

`` `online
O botão abaixo demonstra isso:

<button onclick = "window.scrollBy (0,10)"> window.scrollBy (0,10) </ button>
`` `
- O método `scrollTo (pageX, pageY)` percorre a página em relação ao canto superior esquerdo do documento. É como configurar `scrollLeft / scrollTop`.

Para se deslocar até o início, podemos usar `scrollTo (0,0)`.

`` `online
<button onclick = "window.scrollTo (0,0)"> window.scrollTo (0,0) </ button>
`` `

Esses métodos funcionam para todos os navegadores da mesma maneira.

## scrollIntoView

Para completude, vamos cobrir um método mais: [elem.scrollIntoView (superior)] (mdn: api / Element / scrollIntoView).

A chamada para `elem.scrollIntoView (top)` rola a página para tornar `elem` visível. Tem um argumento:

- se `top = true` (esse é o padrão), a página será roteada para que o` element` apareça no topo da janela. A borda superior do elemento está alinhada com a parte superior da janela.
- se `top = false`, a página rola para fazer o` element` aparecer na parte inferior. A borda inferior do elemento está alinhada com a parte inferior da janela.

`` `online
O botão abaixo percorre a página para se mostrar em cima da janela:

<button onclick = "this.scrollIntoView ()"> this.scrollIntoView () </ button>

E este botão percorre a página para mostrá-la na parte inferior:

<button onclick = "this.scrollIntoView (false)"> this.scrollIntoView (false) </ button>
`` `

## Proibir a rolagem

Às vezes, precisamos fazer o documento "insuperável". Por exemplo, quando precisamos cobri-lo com uma mensagem grande que exige atenção imediata, e queremos que o visitante interaja com essa mensagem e não com o documento.

Para tornar o documento irrefutável, basta configurar `document.body.style.overflow =" hidden "`. A página congelará em seu rolo atual.

`` `online
Tente:

<button onclick = "document.body.style.overflow = 'hidden'"> `document.body.style.overflow = 'hidden'` </ button>

<button onclick = "document.body.style.overflow = ''"> `document.body.style.overflow = ''` </ button>

O primeiro botão congela o pergaminho, o segundo retoma-o.
`` `

Podemos usar a mesma técnica para "congelar" o pergaminho de outros elementos, não apenas para `document.body`.

A desvantagem do método é que a barra de rolagem desaparece. Se ocupasse algum espaço, então esse espaço agora está livre e o conteúdo "salta" para preenchê-lo.

Isso parece um pouco estranho, mas pode ser trabalhado se compararmos o `clientWidth` antes e depois do congelamento, e se ele aumentasse (a barra de rolagem desapareceu), adicione` padding` a `document.body` no lugar da barra de rolagem, para mantenha a largura do conteúdo igual.

## Resumo

Geometria:

- Largura / altura da parte visível do documento (largura / altura da área de conteúdo): `document.documentElement.clientWidth / Height`
- Largura / altura de todo o documento, com a parte deslocada:

`` `js
Deixe scrollHeight = Math.max (
document.body.scrollHeight, document.documentElement.scrollHeight,
document.body.offsetHeight, document.documentElement.offsetHeight,
document.body.clientHeight, document.documentElement.clientHeight
);
`` `

Deslocamento:

- Leia o pergaminho atual: `window.pageYOffset / pageXOffset`.
- Alterar o pergaminho atual:

- `window.scrollTo (pageX, pageY)` - coordenadas absolutas,
- `window.scrollBy (x, y)` - rolar em relação ao local atual,
- `element.scrollIntoView (superior)` - role para tornar `elemento 'visível (alinhar com a parte superior / inferior da janela).
