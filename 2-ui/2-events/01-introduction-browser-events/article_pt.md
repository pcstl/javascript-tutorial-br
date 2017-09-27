# Introdução aos eventos do navegador

* Um evento * é um sinal de que algo aconteceu. Todos os nós DOM geram tais sinais (mas os eventos não estão limitados a DOM).

[cortar]

Aqui está uma lista dos eventos DOM mais úteis, apenas para dar uma olhada em:

** Eventos de mouse: **
- `clique" - quando o mouse clica em um elemento (os dispositivos da tela sensível ao toque o geram em uma torneira).
- `contextmenu` - quando o mouse clica com o botão direito do mouse em um elemento.
- `mouseover` /` mouseout` - quando o cursor do mouse aparece / deixa um elemento.
- `mousedown` /` mouseup` - quando o botão do mouse é pressionado / liberado por um elemento.
- `mousemove` - quando o mouse é movido.

** Eventos do elemento do formulário: **
- `submit` - quando o visitante envia um` <form> `.
- `focus` - quando o visitante se concentra em um elemento, e. em um `<input>`.

** Eventos de teclado: **
- `keydown` e` keyup` - quando o visitante pressiona e depois solta o botão.

** Eventos de documentos **
- `DOMContentLoaded` - quando o HTML é carregado e processado, o DOM é totalmente construído.

** Eventos CSS: **
- `transitionend` - quando uma animação CSS termina.

Há muitos outros eventos. Entraremos em mais detalhes de eventos específicos nos próximos capítulos.

## Manipuladores de eventos

Para reagir nos eventos, podemos atribuir um * manipulador * - uma função que é executada no caso de um evento.

Handlers é uma maneira de executar código JavaScript em caso de ações do usuário.

Existem várias maneiras de atribuir um manipulador. Vamos vê-los, começando pelo mais simples.

### HTML-attribute

Um manipulador pode ser configurado em HTML com um atributo chamado `on <event>`.

Por exemplo, para atribuir um manipulador de "clique" para uma "entrada", podemos usar `onclick`, como aqui:

`` `html run

`` `

No clique do mouse, o código dentro do `onclick` é executado.



Um atributo HTML não é um lugar conveniente para escrever muitos códigos, então é melhor criar uma função JavaScript e chamá-lo para lá.

Aqui, um clique executa a função `countRabbits ()`:

`` `html autorun height = 50
<script>
função countRabbits () {
para (vamos i = 1; i <= 3; i ++) {
alerta ("número do coelho" + i);
}
}
</ script>

<input type = "button" *! * onclick = "countRabbits ()" * /! * value = "Count rabbits!">
`` `

Como sabemos, os nomes dos atributos HTML não são sensíveis a maiúsculas e minúsculas, então `ONCLICK` funciona bem como` onClick` e `onCLICK` ... Mas geralmente os atributos são em minúsculas:` onclick`.

### propriedade DOM

Podemos atribuir um manipulador usando uma propriedade DOM `em <evento>`.

Por exemplo, `elem.onclick`:

`` `html autorun
<input id = "elem" type = "button" value = "Click me">
<script>
*! *
element.onclick = function () {
alerta ('Obrigado');
};
* /! *
</ script>
`` `

Se o manipulador for atribuído usando um atributo HTML, o navegador o lê, cria uma nova função a partir do conteúdo do atributo e a grava na propriedade DOM.

Então, dessa forma, é o mesmo que o anterior.

** O manipulador está sempre na propriedade DOM: o atributo HTML é apenas uma das maneiras de inicializá-lo. **

Estas duas peças de código funcionam da mesma forma:

1. Somente HTML:

`` `html autorun height = 50

`` `
2. HTML + JS:

`` `html autorun height = 50
<input type = "button" id = "button" value = "Button">
<script>
*! *
button.onclick = function () {
alerta ('Clique!');
};
* /! *
</ script>
`` `

** Como há apenas uma propriedade `onclick`, não podemos atribuir mais de um manipulador de eventos. **

No exemplo abaixo, adicionar um manipulador com JavaScript substitui o manipulador existente:

`` `html run height = 50 autorun

<script>
*! *
elem.onclick = function () {// sobrescreve o manipulador existente
alerta ('Depois'); // somente isso será mostrado
};
* /! *
</ script>
`` `

Por sinal, podemos atribuir uma função existente como manipulador diretamente:

`` `js
function sayThanks () {
alerta ('Obrigado!');
}

element.onclick = sayThanks;
`` `

Para remover um manipulador - atribua `elem.onclick = null`.

## Acessando o elemento: isso

O valor de `this` dentro de um manipulador é o elemento. Aquele que tem o manipulador nele.

No código abaixo, `botão 'mostra o seu conteúdo usando` this.innerHTML`:

`` `html height = 50 autorun

`` `

## Possíveis erros

Se você está começando a trabalhar com evento - por favor, note algumas sutilezas.

** A função deve ser atribuída como `sayThanks`, não` sayThanks () `. **

`` `js
// certo
button.onclick = sayThanks;

// errado
button.onclick = sayThanks ();
`` `

Se nós adicionarmos colchetes, então `sayThanks ()` - será o * resultado * da execução da função, então `onclick` no último código torna-se 'indefinido' (a função não retorna nada). Isso não funcionará.

... Mas na marcação precisamos dos colchetes:

`` `html
<tipo de entrada = "botão" id = "botão" onclick = "sayThanks ()">
`` `

A diferença é fácil de explicar. Quando o navegador lê o atributo, ele cria uma função de manipulador com o corpo de seu conteúdo.

Então, o último exemplo é o mesmo que:
`` `js
button.onclick = function () {
*! *
sayThanks (); // o conteúdo do atributo
* /! *
};
`` `

** Use funções, não strings. **



** Não use `setAttribute` para manipuladores. **

Essa chamada não funcionará:

`` `js run no-embellecer
// um clique em <body> irá gerar erros,
// porque os atributos são sempre strings, a função se torna uma string

`` `

** O caso DOM-property é importante. **

Atribua um manipulador para `element.onclick`, não` elem.ONCLICK`, porque as propriedades do DOM são sensíveis a maiúsculas e minúsculas.

## addEventListener

O problema fundamental das formas acima mencionadas de atribuição de manipuladores - não podemos atribuir vários manipuladores a um evento.

Por exemplo, uma parte do nosso código quer destacar um botão no clique e outro quer mostrar uma mensagem.

Gostaríamos de atribuir dois manipuladores de eventos para isso. Mas uma nova propriedade DOM substituirá o existente:

`` `js no-embellecer

// ...

`` `

Os desenvolvedores padrão da Web entenderam isso há muito tempo e sugeriram uma maneira alternativa de gerenciar manipuladores usando métodos especiais `addEventListener` e` removeEventListener`. Eles são livres de tal problema.

A sintaxe para adicionar um manipulador:

`` `js
element.addEventListener (evento, manipulador [, fase]);
`` `

`evento '
: Nome do evento, p.ex. `" clique em "".

`handler`
: A função do manipulador.

`fase '
: Um argumento opcional, a "fase" para que o manipulador funcione. Para ser coberto mais tarde. Normalmente, não usamos isso.

Para remover o manipulador, use `removeEventListener`:


`` `js
// exatamente os mesmos argumentos que addEventListener
element.removeEventListener (evento, manipulador [, fase]);
`` `

`` `` cabeçalho de aviso = "A remoção requer a mesma função"
Para remover um manipulador, devemos passar exatamente na mesma função que foi atribuída.

Isso não funciona:

`` `js no-embellecer
elem.addEventListener ("clique", () => alerta ('Obrigado!'));
// ....
elem.removeEventListener ("clique", () => alerta ('Obrigado!'));
`` `

O manipulador não será removido, porque `removeEventListener` obtém outra função - com o mesmo código, mas isso não importa.

Aqui está o caminho certo:

`` `js
manipulador de função () {
alerta ('Obrigado!');
}

input.addEventListener ("clique", manipulador);
// ....
input.removeEventListener ("clique", manipulador);
`` `

Observe - se não armazenarmos a função em uma variável, não podemos removê-la. Não há como "ler" manipuladores atribuídos por `addEventListener`.
`` ``

Várias chamadas para `addEventListener` permitem adicionar vários manipuladores, como este:

`` `html run no-embellecer
<input id = "elem" type = "botão" value = "Clique-me" />

<script>
manipulador de função1 () {
alerta ('Obrigado!');
};

function handler2 () {
alerta ('Obrigado novamente!');
}

*! *

elem.addEventListener ("clique", handler1); // Obrigado!
elem.addEventListener ("clique", handler2); // Obrigado novamente!
* /! *
</ script>
`` `

Como podemos ver no exemplo acima, podemos configurar os manipuladores * ambos * usando uma propriedade DOM e `addEventListener`. Mas, geralmente, usamos apenas uma dessas maneiras.

`` `` cabeçalho de aviso = "Para alguns manipuladores de eventos só funcionam com` addEventListener` "
Existem eventos que não podem ser atribuídos através de uma propriedade DOM. Deve usar `addEventListener`.

Por exemplo, o evento `transitionend` (animação CSS finalizada) é assim.

Experimente o código abaixo. Na maioria dos navegadores, o segundo manipulador funciona, não o primeiro.

`` `html run
<style>
entrada {
transição: largura 1s;
largura: 100px;
}

.Largo {
largura: 300px;
}
</ style>

<input type = "button" id = "elem" onclick = "this.classList.toggle ('wide')" value = "Click me">

<script>
element.transitionend = function () {
alerta ("propriedade DOM"); // não funciona
};

*! *
elem.addEventListener ("transitionend", function () {
alerta ("addEventListener"); // aparece quando a animação termina
});
* /! *
</ script>
`` `
`` ``

## Objeto de evento

Para lidar adequadamente com um evento, gostaríamos de saber mais sobre o que aconteceu. Não apenas um "clique" ou uma "tecla pressionada", mas quais eram as coordenadas do ponteiro? Qual tecla foi pressionada? E assim por diante.

Quando um evento acontece, o navegador cria um * objeto de evento *, coloca detalhes nele e passa como um argumento para o manipulador.

Aqui está um exemplo de obter coordenadas de mouse do objeto de evento:

`` `html run
<input type = "button" value = "Click me" id = "elem">

<script>
elem.onclick = function (*! * event * /! *) {
// mostra o tipo de evento, o elemento e as coordenadas do clique
alerta (event.type + "at" + event.currentTarget);
alerta ("Coordenadas:" + event.clientX + ":" + event.clientY);
};
</ script>
`` `

Algumas propriedades do objeto `event`:

`event.type`
: Tipo de evento, aqui é `" clique "`.

`event.currentTarget`
: Elemento que manipulou o evento. Isso é exatamente o mesmo que `this`, a menos que você vincule` this` a outra coisa, e então `event.currentTarget` se torna útil.

`event.clientX / event.clientY`
: Coordenadas relativas à janela do cursor, para eventos do mouse.

Existem mais propriedades. Eles dependem do tipo de evento, então vamos estudá-los mais tarde quando chegar a eventos diferentes em detalhes.

`` `` smart header = "O objeto de evento também está acessível a partir de HTML"
Se atribuirmos um manipulador em HTML, também podemos usar o objeto `event`, como este:

`` `html autorun height = 60
<input type = "button" onclick = "*! * alert (event.type) * /! *" value = "Tipo de evento">
`` `

Isso é possível porque quando o navegador lê o atributo, ele cria um manipulador como este: `function (event) {alert (event.type)}`. Isto é: seu primeiro argumento é chamado de "evento" e o corpo é retirado do atributo.
`` ``


## Manipuladores de objetos: handleEvent

Podemos atribuir um objeto como um manipulador de eventos usando `addEventListener`. Quando ocorre um evento, o método `handleEvent` é chamado com ele.

Por exemplo:


`` `html run
<button id = "elem"> Clique-me </ button>

<script>
element.addEventListener ('clique', {
handleEvent (evento) {
alerta (event.type + "at" + event.currentTarget);
}
});
</ script>
`` `

Em outras palavras, quando `addEventListener` recebe um objeto como manipulador, ele chama` object.handleEvent (event) `no caso de um evento.

Podemos também usar uma classe para isso:


`` `html run
<button id = "elem"> Clique-me </ button>

<script>
classe Menu {
handleEvent (evento) {
switch (event.type) {
caso 'mousedown':
elem.innerHTML = "Botão do mouse pressionado";
pausa;
caso 'mouseup':
elem.innerHTML + = "... e lançado.";
pausa;
}
}
}

*! *
deixe o menu = novo menu ();
elem.addEventListener ('mousedown', menu);
elem.addEventListener ('mouseup', menu);
* /! *
</ script>
`` `

Aqui o mesmo objeto lida com ambos os eventos. Observe que precisamos configurar explicitamente os eventos para ouvir usando `addEventListener`. O objeto `menu` só recebe` mousedown` e `mouseup` aqui, nem outros tipos de eventos.

O método `handleEvent` não precisa fazer todo o trabalho sozinho. Pode, em vez disso, chamar outros métodos específicos de eventos, assim:

`` `html run
<button id = "elem"> Clique-me </ button>

<script>
classe Menu {
handleEvent (evento) {
// mousedown -> onMousedown
deixe method = 'on' + event.type [0] .toUpperCase () + event.type.slice (1);
este [método] (evento);
}

onMousedown () {
elem.innerHTML = "Botão do mouse pressionado";
}

onMouseup () {
elem.innerHTML + = "... e lançado.";
}
}

deixe o menu = novo menu ();
elem.addEventListener ('mousedown', menu);
elem.addEventListener ('mouseup', menu);
</ script>
`` `

Agora, os manipuladores de eventos estão claramente separados, que podem ser mais fáceis de suportar.

## Resumo

Existem três maneiras de atribuir manipuladores de eventos:

1. Atributo HTML: `onclick =" ... "`.
2. Propriedade DOM: `elem.onclick = function`.
3. Métodos: `elem.addEventListener (evento, manipulador [, fase])` para adicionar, `removeEventListener` para remover.

Os atributos HTML são usados ​​com moderação, porque o JavaScript no meio de uma tag HTML parece um pouco estranho e estranho. Também não é possível escrever muitos códigos lá.

As propriedades do DOM são aprovadas, mas não podemos atribuir mais de um manipulador do evento específico. Em muitos casos, essa limitação não é urgente.

O último caminho é o mais flexível, mas também é o mais longo para escrever. Existem poucos eventos que só funcionam com ele, por exemplo `transtionend` e` DOMContentLoaded` (para serem cobertos). Também `addEventListener` suporta objetos como manipuladores de eventos. Nesse caso, o método `handleEvent` é chamado no caso do evento.

Não importa como você atribua o manipulador - ele recebe um objeto de evento como o primeiro argumento. Esse objeto contém os detalhes sobre o que aconteceu.

Aprenderemos mais sobre eventos em geral e sobre diferentes tipos de eventos nos próximos capítulos.
