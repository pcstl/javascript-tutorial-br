# Drag'n'Drop com eventos de mouse

Drag'n'Drop é uma ótima solução de interface. Tirar algo, arrastar e soltar é uma maneira clara e simples de fazer muitas coisas, desde copiar e mover (ver gerentes de arquivos) até a ordenação (cair no carrinho).

No padrão HTML moderno existe uma [seção sobre Drag Events] (https://html.spec.whatwg.org/multipage/interaction.html#dnd).

Eles são interessantes, porque permitem resolver tarefas simples com facilidade e também permitem manipular o arrasto de arquivos "externos" no navegador. Então, podemos levar um arquivo no gerenciador de arquivos do SO e soltá-lo na janela do navegador. Então o JavaScript ganha acesso ao seu conteúdo.

Mas os eventos nativos do Drag têm também limitações. Por exemplo, podemos limitar o arrastar por uma determinada área. Também não podemos torná-lo "horizontal" ou "vertical" apenas. Existem outras tarefas de drag'n'drop que não podem ser implementadas usando essa API.

Então, aqui, veremos como implementar Arrastar e Soltar usando eventos do mouse. Não tão difícil também.

Algoritmo ## Drag'n'Drop

O algoritmo básico Drag'n'Drop se parece com isto:

1. Pegue `mousedown` em um elemento arrastável.
2. Prepare o elemento para mover-se (talvez crie uma cópia dele ou seja o que for).
3. Em seguida, em `mousemove` mova-o mudando` left / top` e `position: absolute`.
4. No `mouseup` (botão de liberação) - execute todas as ações relacionadas a um Drag'n'Drop concluído.

Estes são os conceitos básicos. Podemos estendê-lo, por exemplo, destacando os elementos descartáveis ​​(disponíveis para a queda) ao pairar sobre eles.

Aqui está o algoritmo para drag'n'drop de uma bola:

`` `js
ball.onmousedown = function (event) {// (1) iniciar o processo

// (2) prepare-se para mover: faça absoluto e no topo por índice z
ball.style.position = 'absolute';
ball.style.zIndex = 1000;
// mova-o para fora de qualquer pai atual diretamente no corpo
// para posicioná-lo em relação ao corpo
document.body.append (bola);
// ... e coloque essa bola absolutamente posicionada sob o cursor

agindo (event.pageX, event.pageY);

// centra a bola em coordenadas (pageX, pageY)
função de agir (pageX, pageY) {
ball.style.left = pageX - ball.offsetWidth / 2 + 'px';
ball.style.top = pageY - ball.offsetHeight / 2 + 'px';
}

função onMouseMove (evento) {
agindo (event.pageX, event.pageY);
}

// (3) move a bola no mousemove
document.addEventListener ('mousemove', onMouseMove);

// (4) solte a bola, remova os manipuladores desnecessários
ball.onmouseup = function () {
document.removeEventListener ('mousemove', onMouseMove);
ball.onmouseup = null;
};

};
`` `

Se executarmos o código, podemos notar algo estranho. No início do drag'n'drop, a bola "garfos": começamos a arrastar o "clone".

`` `online
Aqui está um exemplo em ação:

[iframe src = altura "bola" = 230]

Tente arrastar e soltar o mouse e você verá o comportamento estranho.
`` `

Isso porque o navegador possui seu próprio Drag'n'Drop para imagens e alguns outros elementos que são executados automaticamente e estão em conflito com os nossos.

Para desativá-lo:

`` `js
ball.ondragstart = function () {
retornar falso;
};
`` `

Agora tudo ficará bem.

`` `online
Em ação:

[iframe src = "ball2" height = 230]
`` `

Outro aspecto importante - rastreamos 'mousemove` no' documento ', não em' bola '. Desde a primeira vista, pode parecer que o mouse está sempre sobre a bola, e podemos colocar `mousemove` nela.

Mas, como nos lembramos, `mousemove` desencadeia frequentemente, mas não para cada pixel. Então, depois do movimento rápido, o cursor pode pular da bola em algum lugar no meio do documento (ou mesmo fora da janela).

Então, devemos ouvir no documento para pegá-lo.

## Correto posicionamento

Nos exemplos acima, a bola sempre está centrada no ponteiro:

`` `js
ball.style.left = pageX - ball.offsetWidth / 2 + 'px';
ball.style.top = pageY - ball.offsetHeight / 2 + 'px';
`` `

Nada mal, mas há um efeito colateral. Para iniciar o arrastar e soltar, podemos "mousedown" em qualquer lugar da bola. Se o fizer na borda, então a bola de repente "salta" para se tornar centrada.

Seria melhor se mantivéssemos o deslocamento inicial do elemento relativo ao ponteiro.

Por exemplo, se começarmos a arrastar pela borda da bola, então o cursor deve permanecer sobre a borda enquanto arrasta.

! [] (ball_shift.png)

1. Quando um visitante pressiona o botão (`mousedown`) - podemos lembrar a distância do cursor para o canto superior esquerdo da bola nas variáveis` shiftX / shiftY`. Devemos manter essa distância enquanto arrastávamos.

Para obter essas mudanças, podemos subtrair as coordenadas:

`` `js
// onmousedown
Deixe shiftX = event.clientX - ball.getBoundingClientRect (). left;
Deixe shiftY = event.clientY - ball.getBoundingClientRect (). top;
`` `

Observe que não há nenhum método para obter coordenadas relativas ao documento em JavaScript, então usamos as coordenadas da janela aqui.

2. Então, ao arrastar, posicionamos a bola no mesmo turno em relação ao ponteiro, assim:

`` `js
// onmousemove
// bola tem posição: absoluta
ball.style.left = event.pageX - *! * shiftX * /! * + 'px';
ball.style.top = event.pageY - *! * shiftY * /! * + 'px';
`` `

O código final com melhor posicionamento:

`` `js
ball.onmousedown = function (event) {

*! *
Deixe shiftX = event.clientX - ball.getBoundingClientRect (). left;
Deixe shiftY = event.clientY - ball.getBoundingClientRect (). top;
* /! *

ball.style.position = 'absolute';
ball.style.zIndex = 1000;
document.body.append (bola);

agindo (event.pageX, event.pageY);

// centra a bola em coordenadas (pageX, pageY)
função de agir (pageX, pageY) {
ball.style.left = pageX - *! * shiftX * /! * + 'px';
ball.style.top = pageY - *! * shiftY * /! * + 'px';
}

função onMouseMove (evento) {
agindo (event.pageX, event.pageY);
}

// (3) move a bola no mousemove
document.addEventListener ('mousemove', onMouseMove);

// (4) solte a bola, remova os manipuladores desnecessários
ball.onmouseup = function () {
document.removeEventListener ('mousemove', onMouseMove);
ball.onmouseup = null;
};

};

ball.ondragstart = function () {
retornar falso;
};
`` `

`` `online
Em ação (dentro de `<iframe>`):

[iframe src = "ball3" height = 230]
`` `

A diferença é especialmente notável se arrastarmos a bola pelo canto inferior direito. No exemplo anterior, a bola "pula" sob o ponteiro. Agora, ele segue fluentemente o cursor da posição atual.

## Detectando droppables

Nos exemplos anteriores, a bola poderia ser descartada apenas em "lugar" para ficar. Na vida real geralmente tomamos um elemento e o deixamos cair em outro. Por exemplo, um arquivo em uma pasta, ou um usuário em um lixo ou o que quer que seja.

Resumidamente, nós levamos um elemento "arrastável" e largamos-o no elemento "droppable".

Precisamos conhecer o alvo descartável no final do Drag'n'Drop - para fazer a ação correspondente e, de preferência, durante o processo de arrastar, para destacar.

A solução é muito interessante e apenas um pouco complicada, então vamos cobri-la aqui.

Qual a primeira ideia? Provavelmente para colocar manipuladores 'onmouseover / mouseup' em potenciais devaneios e detectar quando o ponteiro do mouse aparece sobre eles. E então sabemos que estamos arrastando / soltando esse elemento.

Mas isso não funciona.

O problema é que, enquanto estamos arrastando, o elemento arrastável está sempre acima de outros elementos. E os eventos do mouse só acontecem no elemento superior, e não nos que estão abaixo.

Por exemplo, abaixo são dois elementos `<div>`, vermelho em cima do azul. Não há como pegar um evento no azul, porque o vermelho está no topo:

`` `html executar autorun height = 60
<style>
div {
largura: 50px;
altura: 50px;
posição: absoluta;
topo: 0;
}
</ style>


`` `

O mesmo com um elemento arrastável. A bola está sempre em cima de outros elementos, então os eventos acontecem nisso. Independentemente dos manipuladores que definimos em elementos inferiores, eles não funcionarão.

É por isso que a idéia inicial de colocar os manipuladores sobre potenciais descartes não funciona na prática. Eles não correrão.

Então o que fazer?

Existe um método chamado `document.elementFromPoint (clientX, clientY)`. Retorna o elemento mais aninhado em determinadas coordenadas relativas à janela (ou "nulo" se as coordenadas estiverem fora da janela).

Então, em qualquer um dos nossos manipuladores de eventos de mouse, podemos detectar o potencial que pode ser descartado sob o ponteiro como este:

`` `js
// em um manipulador de eventos de mouse
ball.hidden = true; // (*)
deixe elemBelow = document.elementFromPoint (event.clientX, event.clientY);
ball.hidden = false;
// elemBelow é o elemento abaixo da bola. Se é droppable, podemos lidar com isso.
`` `

Observe: precisamos esconder a bola antes da chamada `(*)`. Caso contrário, geralmente teremos uma bola nessas coordenadas, pois é o elemento superior sob o ponteiro: `elemBelow = bola '.

Podemos usar esse código para verificar o que estamos "voando sobre" a qualquer momento. E lidar com a queda quando isso acontecer.

Um código extenso de `onMouseMove` para encontrar elementos" droppable ":

`` `js
Deixe CurrentDroppable = null; // potencial droppable que estamos voando agora mesmo

função onMouseMove (evento) {
agindo (event.pageX, event.pageY);

ball.hidden = true;
deixe elemBelow = document.elementFromPoint (event.clientX, event.clientY);
ball.hidden = false;

// eventos mousemove podem desencadear da janela (quando a bola é arrastada para fora da tela)
// se clientX / clientY estiverem fora da janela, então elementfromPoint retorna nulo
se (! elementBelow) retornar;

// potenciais possíveis são rotulados com a classe "droppable" (pode ser outra lógica)
deixe droppableBelow = elementBelow.closest ('.droppable');

se (currentDroppable! = droppableBelow) {// se houver alguma alteração
// estamos voando para dentro ou para fora ...
// nota: ambos os valores podem ser nulos
// currentDroppable = null se não estivéssemos excedidos (por exemplo, em um espaço vazio)
// droppableBelow = nulo se não estivermos excedendo agora, durante este evento

se (currentDroppable) {
// a lógica para processar "voando para fora" do droppable (remover destaque)
LeaveDroppable (currentDroppable);
}
currentDroppable = droppableBelow;
se (currentDroppable) {
// a lógica para processar "voando" da droppable
enterDroppable (currentDroppable);
}
}
}
`` `

No exemplo abaixo, quando a bola é arrastada sobre o portão de futebol, o portão é destacado.

[codetabs height = 250 src = "ball4"]

Agora, temos o atual "drop target" na variável `currentDroppable` durante todo o processo e pode usá-lo para destacar ou qualquer outro material.

## Resumo

Consideramos um algoritmo básico 'Drag'n'Drop`.

Os principais componentes:

1. Fluxo de eventos: `ball.mousedown` ->` document.mousemove` -> `ball.mouseup` (cancelar o" ondragstart "nativo).
2. No arrastar arranque - lembre-se da mudança inicial do ponteiro em relação ao elemento: `shiftX / shiftY` e mantenha-o durante o arrastar.
3. Detectar elementos droppable sob o ponteiro usando `document.elementFromPoint`.

Podemos estabelecer muito nesta base.

- No `mouseup`, podemos finalizar a queda: alterar dados, mover elementos ao redor.
- Podemos destacar os elementos sobre os quais estamos voando.
- Podemos limitar o arrastar por uma determinada área ou direção.
- Podemos usar a delegação de eventos para `mousedown / up`. Um manipulador de eventos de grande área que verifica `event.target` pode gerenciar Drag'n'Drop para centenas de elementos.
- E assim por diante.

Existem estruturas que criam arquitetura sobre ele: `DragZone`,` Droppable`, 'Draggable` e outras classes. A maioria deles faz as coisas semelhantes descritas acima, então deve ser fácil de entender agora. Ou roote o nosso próprio, porque você já sabe como lidar com o processo, e pode ser mais flexível do que adaptar outra coisa.
