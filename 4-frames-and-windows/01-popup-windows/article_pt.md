# Popups e métodos de janela

Uma janela pop-up é um dos métodos mais antigos para mostrar documento adicional ao usuário.

Basicamente, você apenas corre:
`` `js
window.open ('http://javascript.info/')
```

... E abrirá uma nova janela com determinado URL. A maioria dos navegadores modernos está configurada para abrir novas guias em vez de janelas separadas.

[cortar]

## Popup bloqueando

Popups existem de tempos muito antigos. A idéia inicial foi mostrar outro conteúdo sem fechar a janela principal. A partir de agora, existem outras maneiras de fazer isso: o JavaScript pode enviar solicitações para o servidor, então as janelas pop-up raramente são usadas. Mas às vezes eles ainda são úteis.

No passado, os sites malignos abusavam muito de popups. Uma página ruim poderia abrir toneladas de janelas pop-up com anúncios. Então, agora, a maioria dos navegadores tenta bloquear pop-ups e proteger o usuário.

** A maioria dos navegadores bloqueia as popups se elas são chamadas fora dos manipuladores de eventos acionados pelo usuário como 'onclick`. **

Se você pensa sobre isso, isso é um pouco complicado. Se o código estiver diretamente em um manipulador `onclick`, então é fácil. Mas o que o popup abre no `setTimeout`?

Experimente este código:

`` `js run
// abrir depois de 3 segundos
setTimeout (() => window.open ('http://google.com'), 3000);
```

O pop-up é aberto no Chrome, mas fica bloqueado no Firefox.

... E isso funciona no Firefox também:

`` `js run
// abrir depois de 1 segundo
setTimeout (() => window.open ('http://google.com'), 1000);
```

A diferença é que o Firefox trata um tempo limite de 2000ms ou menos são aceitáveis, mas depois disso - remove a "confiança", assumindo que agora está "fora da ação do usuário". Então, o primeiro está bloqueado e o segundo não está.

## Uso moderno

A partir de agora, temos muitos métodos para carregar e mostrar dados na página com JavaScript. Mas ainda há situações em que um pop-up funciona melhor.

Por exemplo, muitas lojas usam chats online para consultar pessoas. Um visitante clica no botão, ele é executado `window.open` e abre o popup com o bate-papo.

Por que um popup é bom aqui, por que não na página?

1. Um pop-up é uma janela separada com seu próprio ambiente de JavaScript independente. Portanto, um serviço de bate-papo não precisa se integrar aos scripts do site principal da loja.
2. Um pop-up é muito simples de anexar a um site, pouco a nenhum sobrecarga. É apenas um pequeno botão, sem scripts adicionais.
3. Um pop-up pode persistir mesmo que o usuário tenha deixado a página. Por exemplo, uma consulta aconselha o usuário a visitar a página de um novo "Super-Cooler" goodie. O usuário vai lá na janela principal sem sair do bate-papo.

## window.open

A sintaxe para abrir um pop-up é: `window.open (url, name, params)`:

url
: Um URL para carregar na nova janela.

nome
: Um nome da nova janela. Cada janela tem um `window.name`, e aqui podemos especificar qual janela usar para o popup. Se já existe uma janela com esse nome - o URL fornecido abre nela, caso contrário, uma nova janela é aberta.

Params
: A seqüência de configuração para a nova janela. Ele contém configurações, delimitadas por uma vírgula. Não deve haver espaços em params, por exemplo: `width: 200, height = 100`.

Configurações para `params`:

- Posição:
- `left / top` (numeric) - coordenadas da janela canto superior esquerdo na tela. Há uma limitação: uma nova janela não pode ser posicionada fora da tela.
- `width / height` (numeric) - largura e altura de uma nova janela. Existe um limite de largura / altura mínima, por isso é impossível criar uma janela invisível.
- Características da janela:
- `menubar` (sim / não) - mostra ou oculta o menu do navegador na nova janela.
- `toolbar` (sim / no) - mostra ou oculta a barra de navegação do navegador (de volta, encaminhar, recarregar etc.) na nova janela.
- `location` (sim / não) - mostra ou oculta o campo URL na nova janela. O FF eo IE não permitem ocultá-lo por padrão.
- `status` (sim / não) - mostra ou oculta a barra de status. Mais uma vez, a maioria dos navegadores o forçam a mostrar.
- `redimensionável '(sim / não) - permite desativar o redimensionamento para a nova janela. Não recomendado.
- `scrollbars` (sim / no) - permite desativar as barras de rolagem para a nova janela. Não recomendado.


Há também uma série de recursos menos específicos compatíveis com o navegador, que normalmente não são usados. Verifique <a href="https://developer.mozilla.org/pt/DOM/window.open"> window.open no MDN </a> para obter exemplos.

## Exemplo: uma janela minimalista

Vamos abrir uma janela com um conjunto mínimo de recursos apenas para ver qual deles o navegador permite desabilitar:

`` `js run
Deixe params = `scrollbars = não, redimensionável = não, status = não, localização = não, barra de ferramentas = não, menu = não,
largura = 0, altura = 0, esquerda = -1000, superior = -1000`;

aberto ('/', 'teste', params);
```

Aqui, a maioria das "características da janela" são desabilitadas e a janela é posicionada fora da tela. Execute e veja o que realmente acontece. A maioria dos navegadores "conserta" coisas estranhas como zero `width / height` e offscreen` left / top`. Por exemplo, o Chrome abre tal janela com largura / altura total, de modo que ocupe a tela cheia.

Vamos adicionar opções de posicionamento normais e coordenadas razoáveis ​​de `width`,` height`, `left`,` top`:

`` `js run
Deixe params = `scrollbars = não, redimensionável = não, status = não, localização = não, barra de ferramentas = não, menu = não,
largura = 600, altura = 300, esquerda = 100, superior = 100`;

aberto ('/', 'teste', params);
```

A maioria dos navegadores mostra o exemplo acima conforme necessário.

Regras para configurações omitidas:

- Se não houver nenhum terceiro argumento na chamada `aberta ', ou estiver vazio, os parâmetros de janela padrão serão usados.
- Se houver uma série de params, mas alguns recursos de sim / não são omitidos, os recursos omitidos são desabilitados, se o navegador permitir isso. Então, se você especificar params, verifique se você define explicitamente todos os recursos necessários para sim.
- Se não houver `left / top` em params, o navegador tenta abrir uma nova janela perto da última janela aberta.
- Se não houver `width / height`, a nova janela será do mesmo tamanho que a última aberta.

## Acessando um popup

A chamada `aberta 'retorna uma referência para a nova janela. Ele pode ser usado para manipular suas propriedades, alterar a localização e ainda mais.

No exemplo abaixo, o conteúdo da nova janela é modificado após o carregamento.

`` `js run
deixe NewWindow = aberto ('/', 'exemplo', 'largura = 300, altura = 300')
newWindow.focus ();

newWindow.onload = function () {
Deixe html = `<div style =" font-size: 30px "> Seja bem-vindo! </ div>`;
*!*
newWindow.document.body.insertAdjacentHTML ('afterbegin', html);
*/!*
};
```

Por favor, note que o conteúdo externo do `documento 'só é acessível para o Windows da mesma origem (o mesmo protocolo: // domínio: porta).

Para o Windows com URLs de outros sites, podemos alterar o local atribuindo `newWindow.location = ...`, mas não podemos ler a localização ou acessar o conteúdo. Isso é para a segurança do usuário, de modo que uma página malvada não pode abrir um popup com `http: // gmail.com` e ler os dados. Vamos falar mais sobre isso mais tarde.

## Acessando a janela do abridor

Um pop-up também pode acessar a janela "abridor". Um JavaScript nele pode usar `window.opener` para acessar a janela que abriu. É 'nulo' para todos os Windows, exceto popups.

Então, tanto a janela principal como o popup têm uma referência entre si. Eles podem se modificar livremente assumindo que eles vêm da mesma origem. Se isso não for assim, então ainda há meios para se comunicar, para serem abordados no próximo capítulo <info: cross-window-communication>.

## Fechar um popup

Se não precisarmos mais de um popup, podemos chamar `newWindow.close ()` nela.

Tecnicamente, o método `close ()` está disponível para qualquer `janela`, mas` window.close () `é ignorado pela maioria dos navegadores se` window` não for criada com `window.open ()`.

O `newWindow.closed` é` true` se a janela estiver fechada. Isso é útil para verificar se o popup (ou a janela principal) ainda está aberto ou não. Um usuário pode fechá-lo, e nosso código deve levar essa possibilidade em consideração.

Este código carrega e fecha a janela:

`` `js run
deixe NewWindow = aberto ('/', 'exemplo', 'largura = 300, altura = 300')
newWindow.onload = function () {
newWindow.close ();
alerta (newWindow.closed); // verdade
};
```

## Foco / borrão em um popup

Teoricamente, existem `window.focus ()` e `window.blur ()` métodos para focar / focar em uma janela. Também há eventos `focus / blur` que permitem focar uma janela e pegar o momento em que o visitante mudar para outro local.

No passado, as páginas malignas abusavam desses. Por exemplo, veja este código:

`` `js run
window.onblur = () => window.focus ();
```

Quando um usuário tenta mudar para fora da janela (`borrão '), ele o traz de volta ao foco. A intenção é "bloquear" o usuário dentro da `janela '.

Portanto, existem limitações que proíbem o código como esse. Existem muitas limitações para proteger o usuário de anúncios e páginas de males. Eles dependem do navegador.

Por exemplo, um navegador móvel geralmente ignora essa chamada completamente. A focagem também não funciona quando um pop-up é aberto em uma guia separada em vez de uma nova janela.

Ainda assim, existem algumas coisas que podem ser feitas.

Por exemplo:

- Quando abrimos um pop-up, pode ser uma boa idéia executar um `newWindow.focus ()` nele. Por favor, para algumas combinações de sistema operacional / navegador, ele garante que o usuário esteja na nova janela agora.
- Se queremos rastrear quando um visitante realmente usa o nosso aplicativo web, podemos acompanhar `window.onfocus / onblur`. Isso nos permite suspender / retomar atividades na página, animações, etc. Mas observe que o evento "borrão" significa que o visitante mudou da janela, mas ele ainda pode observá-lo. A janela está em segundo plano, mas ainda pode ser visível.

## Resumo

- Um popup pode ser aberto pela chamada `open (url, name, params)`. Ele retorna a referência para a janela recém-aberta.
- Por padrão, os navegadores bloqueiam as chamadas `abertas 'do código fora das ações do usuário. Normalmente, uma notificação aparece, para que um usuário possa permitir.
- O pop-up pode acessar a janela do abridor usando a propriedade `window.opener`, então os dois estão conectados.
- Se a janela principal e o pop-up vêm da mesma origem, eles podem ler e modificar livremente. Caso contrário, eles podem mudar a localização um do outro e se comunicar usando mensagens (para serem cobertas).
- Para fechar o pop-up: use a chamada `close ()`. Além disso, o usuário pode fechá-los (como qualquer outro Windows). O `window.closed` é` true` depois disso.
- Métodos `focus ()` e `blur ()` permitem focar / focar uma janela. As vezes.
- Eventos `focus` e` blur` permitem acompanhar a mudança de entrada e saída da janela. Mas observe que uma janela ainda pode ser visível mesmo no estado de fundo, depois de "borrão".

Além disso, se abrimos um pop-up, uma boa prática é notificar o usuário sobre isso. Um ícone com a janela de abertura pode ajudar o visitante a sobreviver à mudança de foco e manter as duas janelas em mente.
