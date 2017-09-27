# Ações padrão do navegador

Muitos eventos levam automaticamente a ações do navegador.

Por exemplo:

- Um clique em um link - inicia a sua URL.
- Um clique no botão enviar dentro de um formulário - inicia sua submissão ao servidor.
- Pressionando um botão do mouse sobre um texto e movendo-o - seleciona o texto.

Se lidar com um evento em JavaScript, muitas vezes não queremos ações do navegador. Felizmente, isso pode ser prevenido.

[cortar]

# Prevenção de ações do navegador

Existem duas maneiras de dizer ao navegador que não queremos agir:

- O caminho principal é usar o objeto `event`. Existe um método `event.preventDefault ()`.
- Se o manipulador é atribuído usando `on <event>` (não por `addEventListener`), então podemos simplesmente retornar` false` dele.

No exemplo abaixo, um clique para links não leva a alteração de URL:

`` `html autorun height = 60 não-embelezar
<a href="/" onclick="return false"> clique aqui </a>
ou
<a href="/" onclick="event.preventDefault()"> aqui </a>
`` `

`` `warn header =" Não é necessário retornar `true`"
O valor retornado por um manipulador de eventos geralmente é ignorado.

A única exceção - é `return false` de um manipulador atribuído usando` on <event> `.

Em todos os outros casos, o retorno não é necessário e não é processado de qualquer forma.
`` `

### Exemplo: o menu

Considere um menu do site, como este:

`` `html
<ul id = "menu" class = "menu">
<li> <a href="/html"> HTML </a> </ li>
<li> <a href="/javascript"> JavaScript </a> </ li>
<li> <a href="/css"> CSS </a> </ li>
</ Ul>
`` `

Veja como parece com algum CSS:

[iframe height = 70 src = "menu" link editar]

Os itens do menu são links `<a>`, e não botões. Existem vários benefícios, por exemplo:

- Muitas pessoas gostam de usar "clique direito" - "abrir em uma nova janela". Se usarmos `<button>` ou `<span>`, isso não funciona.
- Os motores de busca seguem os <a href="..."> links durante a indexação.

Então usamos `<a>` na marcação. Mas, normalmente, pretendemos lidar com cliques em JavaScript. Portanto, devemos evitar a ação padrão do navegador.

Como aqui:

`` `js
menu.onclick = função (evento) {
se (event.target.nodeName! = 'A') retornar;

Deixe href = event.target.getAttribute ('href');
alerta (href); // ... pode ser carregado a partir do servidor, geração de UI, etc.

*! *
retornar falso; // impede a ação do navegador (não vá para o URL)
* /! *
};
`` `

Se omitimos `return false`, então, após o nosso código ser executado, o navegador fará sua" ação padrão "- seguindo o URL em` href`.

Por sinal, o uso da delegação de eventos aqui torna nosso menu flexível. Podemos adicionar listas aninhadas e modelá-las usando CSS para "deslizar para baixo".


## Prevenir outros eventos

Certos eventos fluem um para o outro. Se impedir o primeiro evento, não haverá nenhum segundo.

Por exemplo, `mousedown` em um campo` <input> `leva à focagem nele e ao evento` focus`. Se impedimos o evento `mousedown`, não há foco.

Tente clicar no primeiro `<input>` abaixo - o evento `focus` acontece. Isso é normal.

Mas se você clicar no segundo, não há foco.

`` `html execute o autorun
<input value = "Focus works" onfocus = "this.value = ''">
<input *! * onmousedown = "return false" * /! * onfocus = "this.value = ''" value = "Click me">
`` `

Isso ocorre porque a ação do navegador é cancelada no `mousedown`. A focagem ainda é possível se usarmos outra maneira de entrar na entrada. Por exemplo, a tecla `chave: Tab` para mudar da 1ª entrada para a 2ª. Mas não com o clique do mouse mais.


## event.defaultPrevented

A propriedade `event.defaultPrevented` é` true` se a ação padrão foi impedida e `false` caso contrário.

Existe um caso de uso interessante para isso.

Você lembra no capítulo <info: bubbling-and-captureuring> falamos sobre `event.stopPropagation ()` e por que parar de borbulhar é ruim?

Às vezes, podemos usar `event.defaultPrevented` em vez disso.

Vamos ver um exemplo prático onde parar os olhares borbulhantes necessários, mas na verdade podemos fazer bem sem ele.

Por padrão, o navegador no evento `contextmenu` (clique com o botão direito do mouse) mostra um menu de contexto com opções padrão. Podemos preveni-lo e mostrar o nosso próprio, assim:

`` `html autorun height = 50 não-beautify run
<botão> Clique com o botão direito do mouse no menu de contexto do navegador </ button>


Clique com o botão direito do mouse no nosso menu de contexto
</ button>
`` `

Agora, digamos que queremos implementar nosso próprio menu de contexto, com nossas opções. E dentro do documento, podemos ter outros elementos com seus próprios menus de contexto:

`` `html autorun height = 80 não-beautify run
<p> Clique com o botão direito do mouse no menu de contexto do documento </ p>
<button id = "elem"> Clique com o botão direito do mouse no menu de contexto do botão </ button>

<script>
elem.oncontextmenu = function (event) {
event.preventDefault ();
alerta ("menu de contexto do botão");
};

document.oncontextmenu = function (event) {
event.preventDefault ();
alerta ("menu de contexto do documento");
};
</ script>
`` `

O problema é que, quando clicamos no `elemento ', recebemos dois menus: o nível do botão e (o evento borbulha) o menu do nível do documento.

Como corrigi-lo? Uma das soluções é pensar como: "Nós lidarmos com o evento no manipulador de botão, vamos parar" e usar `event.stopPropagation ()`:

`` `html autorun height = 80 não-beautify run
<p> Clique com o botão direito do mouse no menu do documento </ p>
<button id = "elem"> Clique com o botão direito do mouse no menu do botão (corrigido com event.stopPropagation) </ button>

<script>
elem.oncontextmenu = function (event) {
event.preventDefault ();
*! *
event.stopPropagation ();
* /! *
alerta ("menu de contexto do botão");
};

document.oncontextmenu = function (event) {
event.preventDefault ();
alerta ("menu de contexto do documento");
};
</ script>
`` `

Agora, o menu de nível de botão funciona como previsto. Mas o preço é alto. Nós negamos para sempre o acesso a informações sobre cliques diretos para qualquer código externo, incluindo contadores que coletam estatísticas e assim por diante. Isso é bastante imprudente.

Uma solução alternativa seria verificar o manipulador 'documento' se a ação padrão fosse impedida? Se assim for, então o evento foi tratado, e não precisamos reagir nisso.


`` `html autorun height = 80 não-beautify run
<p> Clique com o botão direito do mouse no menu do documento (corrigido com event.defaultPrevented) </ p>
<button id = "elem"> Clique com o botão direito do mouse no menu do botão </ button>

<script>
elem.oncontextmenu = function (event) {
event.preventDefault ();
alerta ("menu de contexto do botão");
};

document.oncontextmenu = function (event) {
*! *
se (event.defaultPrevented) retornar;
* /! *

event.preventDefault ();
alerta ("menu de contexto do documento");
};
</ script>
`` `

Agora, tudo também funciona corretamente. Se temos elementos aninhados, e cada um deles tem um menu de contexto próprio, isso também funcionará. Certifique-se de verificar para `event.defaultPrevented` em cada manipulador` contextmenu`.

`` `smart header =" event.stopPropagation () e event.preventDefault () "
Como podemos ver claramente, `event.stopPropagation ()` e `event.preventDefault ()` (também conhecido como `return false`) são duas coisas diferentes. Eles não estão relacionados entre si.
`` `

`` `cabeçalho inteligente =" arquitetura de menus de contexto aninhados "
Existem também formas alternativas de implementar menus de contexto aninhados. Um deles é ter um objeto global especial com um método que manipule `document.oncontextmenu`, e também métodos que permitem armazenar vários manipuladores de" nível inferior "nela.

O objeto pegará qualquer clique com o botão direito do mouse, olhe através dos manipuladores armazenados e execute o apropriado.

Mas, em seguida, cada pedaço de código que deseja um menu de contexto deve conhecer esse objeto e usar sua ajuda em vez do próprio manipulador `contextmenu`.
`` `

## Resumo

Existem várias ações padrão do navegador:

- `mousedown` - inicia a seleção (mova o mouse para selecionar).
- `clicar 'em` <input type = "checkbox"> `- verifica / desmarca a` entrada'.
- `submit` - clicar em um` <input type = "submit"> `ou clicar em 'key: Enter` dentro de um campo de formulário faz com que este evento aconteça, e o navegador envia o formulário depois dele.
- `wheel` - rolar um evento da roda do mouse tem a rolagem como a ação padrão.
- `keydown` - pressionar uma tecla pode levar a adicionar um caractere a um campo ou a outras ações.
- `contextmenu` - o evento acontece com um clique direito, a ação é para mostrar o menu de contexto do navegador.
- ...há mais...

Todas as ações padrão podem ser prevenidas se quisermos lidar com o evento exclusivamente por JavaScript.

Para evitar uma ação padrão - use `event.preventDefault ()` ou `return false`. O segundo método funciona apenas para manipuladores atribuídos com `on <event>`.

Se a ação padrão foi impedida, o valor de `event.defaultPrevented` torna-se` true`, caso contrário, é `false`.

`` `warn header =" Stay semantic, do not abuse "
Tecnicamente, ao impedir ações padrão e adicionar JavaScript, podemos personalizar o comportamento de qualquer elemento. Por exemplo, podemos fazer um link `<a>` funcionar como um botão, e um botão `<botão>` se comportam como um link (redirecionar para outro URL ou assim).

Mas geralmente devemos manter o significado semântico dos elementos HTML. Por exemplo, `<a>` deve preformar a navegação, não um botão.

Além de ser "apenas uma coisa boa", isso torna seu HTML melhor em termos de acessibilidade.

Além disso, se considerarmos o exemplo com `<a>`, então note: um navegador permite abrir esses links em uma nova janela (clicando com o botão direito do mouse neles e outros meios). E as pessoas gostam disso. Mas se formos um botão comportam-se como um link usando o JavaScript e até parecem um link usando o CSS, os recursos de navegador "<a>` ainda não funcionarão para isso.
`` `
