# Despachando eventos personalizados

Nós não podemos apenas atribuir manipuladores, mas também gerar eventos de JavaScript.

Eventos personalizados podem ser usados ​​para criar "componentes gráficos". Por exemplo, um elemento raiz do menu pode desencadear eventos informando o que acontece com o menu: `open` (menu aberto),` select` (um item está selecionado) e assim por diante.

Também podemos gerar eventos internos como "clique", "mousedown" etc., que podem ser bons para testar.

[cortar]

## Event constructor

Os eventos formam uma hierarquia, assim como classes de elementos DOM. A raiz é a classe incorporada [Evento] (http://www.w3.org/TR/dom/#event).

Podemos criar objetos de "Evento" como este:

`` `js
let event = new Event (tipo de evento [, options]);
`` `

Argumentos:

- * tipo de evento * - pode ser qualquer string, como "clicar" ou nosso próprio como "hey-ho!" `.
- * opções * - o objeto com duas propriedades opcionais:
- `bubbles: true / false` - if` true`, então o evento borbulha.
- `cancelable: true / false` - se` true`, então a "ação padrão" pode ser impedida. Mais tarde, veremos o que isso significa para eventos personalizados.

Por padrão, ambos são falsos: `{bubbles: false, cancelable: false}`.

## dispatchEvent

Após a criação de um objeto de evento, devemos "executá-lo" em um elemento usando a chamada `elem.dispatchEvent (evento)`.

Então, os manipuladores reagem nele como se fosse um evento interno regular. Se o evento foi criado com a bandeira 'bubbles`, então ele borbulha.

No exemplo abaixo, o evento 'clique' é iniciado em JavaScript. O manipulador funciona da mesma maneira que se o botão foi clicado:

`` `html run no-embellecer


<script>
deixe evento = novo Evento ("clique");
elem.dispatchEvent (evento);
</ script>
`` `

`` `smart header =" event.isTrusted "
Existe uma maneira de dizer um evento de usuário "real" a partir de um script gerado.

A propriedade `event.isTrusted` é` true` para eventos que provêm de ações de usuário real e `false` para eventos gerados por script.
`` `

# Exemplo de borbulhamento

Podemos criar um evento borbulhante com o nome `" hello "` e pegá-lo no `documento '.

Tudo o que precisamos é configurar `bubbles` para` true`:

`` `html run no-embellecer
<h1 id = "elem"> Olá do script! </ h1>

<script>
// pegue no documento ...
document.addEventListener ("hello", função (evento) {// (1)

});

// ... despachar no elemento!
let event = new Event ("hello", {bubbles: true}); // (2)
elem.dispatchEvent (evento);
</ script>
`` `

Notas:

1. Devemos usar `addEventListener` para nossos eventos personalizados, porque` on <event> `só existe para eventos internos,` document.onhello` não funciona.
2. Deve definir `bubbles: true`, caso contrário, o evento não se expandirá.

A mecânica de borbulhar é a mesma para os eventos embutidos (`clique") e personalizados (`hello`). Há também etapas de captura e borbulhamento.

## MouseEvent, KeyboardEvent e outros

Aqui está uma pequena lista de classes para Eventos de UI da [Especificação do Evento de UI] (https://www.w3.org/TR/uievents):

- `UIEvent`
- `FocusEvent`
- `MouseEvent`
- `WheelEvent`
- `KeyboardEvent`
- ...

Devemos usá-los em vez de `new Event` se quisermos criar esses eventos. Por exemplo, `novo MouseEvent (" clique ")`.

O construtor correto permite especificar propriedades padrão para esse tipo de evento.

Como `clientX / clientY` para um evento de mouse:

`` `js run
deixe event = novo MouseEvent ("clique", {
bolhas: verdadeiro
cancelável: verdadeiro
clientX: 100,
clientY: 100
});

*! *
alerta (event.clientX); // 100
* /! *
`` `

Por favor, note: o construtor genérico do `Event` não permite isso.

Vamos tentar:

`` `js run
let event = new Event ("clique", {
bolhas: verdadeiro, // apenas bolhas e canceláveis
cancelável: true, // trabalho no construtor de eventos
clientX: 100,
clientY: 100
});

*! *
alerta (event.clientX); // indefinido, a propriedade desconhecida é ignorada!
* /! *
`` `

Tecnicamente, podemos contornar isso ao atribuir diretamente `event.clientX = 100` após a criação. Então é uma questão de conveniência e seguindo as regras. Os eventos gerados pelo navegador sempre têm o tipo certo.

A lista completa de propriedades para diferentes eventos de UI está na especificação, por exemplo [MouseEvent] (https://www.w3.org/TR/uievents/#mouseevent).

## Eventos personalizados

Para os nossos próprios eventos personalizados, como `" hello ", devemos usar` new CustomEvent`. Tecnicamente [CustomEvent] (https://dom.spec.whatwg.org/#customevent) é o mesmo que `Event`, com uma exceção.

No segundo argumento (objeto) podemos adicionar uma propriedade adicional `detalhe 'para qualquer informação personalizada que queremos passar com o evento.

Por exemplo:

`` `html run refresh
<h1 id = "elem"> Olá para John! </ h1>

<script>
// detalhes adicionais vêm com o evento para o manipulador
elem.addEventListener ("hello", função (evento) {
alerta (*! * event.detail.name * /! *);
});

elem.dispatchEvent (novo CustomEvent ("hello", {
*! *
detalhe: {nome: "John"}
* /! *
});
</ script>
`` `

A propriedade `detail` pode ter quaisquer dados. Tecnicamente, poderíamos viver sem, porque podemos atribuir quaisquer propriedades em um objeto regular do `novo evento 'após sua criação. Mas `CustomEvent` fornece o campo especial` detalhe 'para que evite conflitos com outras propriedades do evento.

A classe de evento diz algo sobre "que tipo de evento" é, e se o evento é personalizado, então devemos usar o `CustomEvent 'apenas para ser claro sobre o que é.

## event.preventDefault ()

Podemos chamar `event.preventDefault ()` em um evento gerado por script se o sinalizador 'cancelable: true` for especificado.

Claro, se o evento tiver um nome não padrão, não é conhecido pelo navegador, e não há "ação padrão do navegador" para ele.

Mas o código gerador de eventos pode planejar algumas ações após `dispatchEvent`.

A chamada de `event.preventDefault ()` é uma maneira para o manipulador enviar um sinal de que essas ações não devem ser executadas.

Nesse caso, a chamada para `elem.dispatchEvent (event)` retorna `false`. E o código gerador de eventos sabe que o processamento não deve continuar.

Por exemplo, no exemplo abaixo, há uma função `hide ()`. Ele gera o evento `" hide "no elemento` # rabbit`, notificando todas as partes interessadas que o coelho vai se esconder.

Um manipulador definido por `rabbit.addEventListener ('hide', ...) 'aprenderá sobre isso e, se quiser, pode impedir essa ação chamando` event.preventDefault () `. Então o coelho não se esconde:

`` `html run refresh
<pre id = "coelho">
| \ / |
\ | _ | /
/. . \
(Isto é,
{>o<}
</ pre>

<script>
// hide () será chamado automaticamente em 2 segundos
function hide () {
deixe event = new CustomEvent ("hide", {
cancelável: true // sem essa bandeira preventDefault não funciona
});
se (! rabbit.dispatchEvent (evento)) {
alerta ("a ação foi impedida por um manipulador");
} outro {
coelho.hidden = true;
}
}

rabbit.addEventListener ('hide', function (event) {
se (confirmar ("Call preventDefault?")) {
event.preventDefault ();
}
});

// ocultar em 2 segundos
setTimeout (ocultar, 2000);

</ script>
`` `


## Eventos-in-events são síncronos

Normalmente, os eventos são processados ​​de forma assíncrona. Isto é: se o navegador estiver processando `onclick` e no processo ocorre um novo evento, aguarda até que o processamento 'onclick` esteja concluído.

A exceção é quando um evento é iniciado a partir de outro.

Em seguida, o controle salta para o manipulador de eventos aninhados, e depois ele volta.

Por exemplo, aqui o evento 'menu-aberto' aninhado é processado de forma síncrona, durante o `onclick`:

`` `html run
<botão id = "menu"> Menu (clique em mim) </ button>

<script>
// 1 -> aninhado -> 2
menu.onclick = function () {


// alerta ("aninhado")
menu.dispatchEvent (novo CustomEvent ("menu-open", {
bolhas: verdadeira
}));


};

document.addEventListener ('menu-open', () => alerta ('aninhado'))
</ script>
`` `

Por favor, note que o evento "menu-aberto" do evento aninhado é exibido e é tratado no documento. A propagação do evento aninhado está totalmente concluída antes que o processamento volte ao código externo (`onclick`).

Não se trata apenas de `dispatchEvent`, existem outros casos. O JavaScript em um manipulador de eventos pode chamar métodos que levam a outros eventos - eles também são processados ​​de forma síncrona.

Se não gostarmos, podemos colocar o `dispatchEvent` (ou outra chamada desencadeada de eventos) no final de 'onclick` ou, se for inconveniente, envolva-o em` setTimeout (..., 0) `:

`` `html run
<botão id = "menu"> Menu (clique em mim) </ button>

<script>
// 1 -> 2 -> aninhado
menu.onclick = function () {



setTimeout (() => menu.dispatchEvent (novo CustomEvent ("menu-open", {
bolhas: verdadeira
})), 0);


};

document.addEventListener ('menu-open', () => alerta ('aninhado'))
</ script>
`` `

## Resumo

Para gerar um evento, primeiro precisamos criar um objeto de evento.

O genérico `Event (name, options)` construtor aceita um nome de evento arbitrário e o objeto `options` com duas propriedades:
- `bubbles: true` se o evento for bolha.
- `cancelable: true` é o` event.preventDefault () `deve funcionar.

Outros construtores de eventos nativos como `MouseEvent`, KeyboardEvent e assim por diante aceitam propriedades específicas desse tipo de evento. Por exemplo, `clientX` para eventos de mouse.

Para eventos personalizados, devemos usar o construtor `CustomEvent`. Tem uma opção adicional chamada `detalhe ', devemos atribuir os dados específicos do evento a ele. Em seguida, todos os manipuladores podem acessá-lo como `event.detail`.

Apesar da possibilidade técnica de gerar eventos do navegador como "clique" ou "keydown", devemos usar com o grande cuidado.

Não devemos gerar eventos do navegador, pois é uma maneira hackeada de executar manipuladores. Essa é uma arquitetura ruim na maioria das vezes.

Os eventos nativos podem ser gerados:

- Como um hack sujo para tornar as bibliotecas de terceiros funcionando da maneira necessária, se não fornecem outros meios de interação.
- Para testes automatizados, "clique no botão" no script e veja se a interface reage corretamente.

Os eventos personalizados com nossos próprios nomes geralmente são gerados para fins arquitetônicos, para sinalizar o que acontece dentro de nossos menus, controles deslizantes, carrosséis etc.
