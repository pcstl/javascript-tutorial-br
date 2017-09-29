# Ciclo de vida da página: DOMContentLoaded, load, beforeunload, unload

O ciclo de vida de uma página HTML tem três eventos importantes:

- `DOMContentLoaded` - o HTML totalmente carregado do navegador e a árvore DOM é construída, mas recursos externos, como imagens` <img> `e folhas de estilo, podem ainda não estar carregados.
- `load` - o navegador carregou todos os recursos (imagens, estilos, etc.).
- `beforeunload / unload` - quando o usuário está saindo da página.

Cada evento pode ser útil:

- Evento `DOMContentLoaded` - O DOM está pronto, de modo que o manipulador pode procurar os nós DOM, inicializar a interface.
- evento `load` - recursos adicionais são carregados, podemos obter tamanhos de imagem (se não for especificado em HTML / CSS), etc.
- evento 'beforeunload / unload' - o usuário está saindo: podemos verificar se o usuário salvou as mudanças e perguntar-lhe se ele realmente quer sair.

Vamos explorar os detalhes desses eventos.

[cortar]

## DOMContentLoaded

O evento `DOMContentLoaded` acontece no objeto 'documento'.

Devemos usar `addEventListener` para pegá-lo:

`` `js
document.addEventListener ("DOMContentLoaded", pronto);
`` `

Por exemplo:

`` `html run height = 200 refresh
<script>
função pronta () {
alerta ('DOM está pronto');

// a imagem ainda não foi carregada (a menos que tenha sido armazenada em cache), então o tamanho é 0x0
alerta (`Tamanho da imagem: $ {img.offsetWidth} x $ {img.offsetHeight}`);
}

*! *
document.addEventListener ("DOMContentLoaded", pronto);
* /! *
</ script>

<img id = "img" src = "https://en.js.cx/clipart/train.gif?speed=1&cache=0">
`` `

No exemplo, o manipulador `DOMContentLoaded` é executado quando o documento está carregado, não aguarda a carga da página. Então, 'alerta' mostra zero tamanhos.

Na primeira vista, o evento `DOMContentLoaded` é muito simples. A árvore DOM está pronta - aqui está o evento. Mas há poucas peculiaridades.

### DOMContentLoaded e scripts

Quando o navegador inicialmente carrega o HTML e aparece em um `<script> ... </ script>` no texto, não pode continuar a construir o DOM. Ele deve executar o script agora. Portanto, `DOMContentLoaded` só pode acontecer depois que todos esses scripts são executados.

Os scripts externos (com `src`) também colocam o edifício DOM para pausar enquanto o script está sendo carregado e executado. Então, `DOMContentLoaded` espera scripts externos também.

A única exceção são scripts externos com atributos `async` e` defer``. Dizem ao navegador que continue processando sem aguardar os scripts. Assim, o usuário pode ver a página antes que os scripts terminem de carregar, bons para o desempenho.

`` `smart header =" Uma palavra sobre `async` e` defer``
Os atributos `async` e` defer` funcionam apenas para scripts externos. Eles são ignorados se não houver `src`.

Ambos dizem ao navegador que pode continuar trabalhando com a página e carregar o script "em segundo plano" e, em seguida, executar o script quando ele é carregado. Portanto, o script não bloqueia a criação de DOM e a renderização de página.

Existem duas diferenças entre eles.

| | `async` | `adiar
| --------- | --------- | --------- |
| Ordem | Scripts com `async` executam * na ordem de carregamento primeiro *. A sua encomenda de documentos não importa - o que carrega primeiro corre primeiro. | Os scripts com "adiar" sempre executam * na ordem do documento * (conforme eles estão no documento). |
| `DOMContentLoaded` | Os scripts com `async` podem ser carregados e executados enquanto o documento ainda não foi totalmente baixado. Isso acontece se os scripts são pequenos ou armazenados em cache, e o documento é suficientemente longo. | Scripts com `defer` executar depois que o documento é carregado e analisado (eles esperam, se necessário), logo antes de` DOMContentLoaded`. |

Então `async` é usado para scripts totalmente independentes.

`` `

### DOMContentLoaded e estilos

As folhas de estilo externo não afetam DOM e, portanto, `DOMContentLoaded` não espera por elas.

Mas há uma armadilha: se tivermos um script após o estilo, esse script deve aguardar a execução da folha de estilos:

`` `html
<link type = "text / css" rel = "stylesheet" href = "style.css">
<script>
// o script não é executado até que a folha de estilos seja carregada
alerta (getComputedStyle (document.body) .marginTop);
</ script>
`` `

A razão é que o script pode querer obter coordenadas e outras propriedades dependentes do estilo de elementos, como no exemplo acima. Naturalmente, tem que aguardar os estilos para serem carregados.

Como `DOMContentLoaded` aguarda scripts, ele agora espera estilos antes deles também.

### Enchimento automático do navegador incorporado

Formulários de preenchimento automático Firefox, Chrome e Opera no `DOMContentLoaded`.

Por exemplo, se a página tiver um formulário com login e senha, e o navegador lembrou os valores, então em `DOMContentLoaded` pode tentar preenchê-los automaticamente (se aprovado pelo usuário).

Então, se `DOMContentLoaded` for adiado por scripts de carregamento longo, o preenchimento automático também o aguarda. Você provavelmente viu isso em alguns sites (se você usar o preenchimento automático do navegador) - os campos de login / senha não são preenchidos automaticamente, mas há um atraso até a página carregar completamente. Essa é realmente a demora até o evento `DOMContentLoaded`.

Um dos menores benefícios no uso de `async` e` defer` para scripts externos - eles não bloqueiam `DOMContentLoaded` e não demoram o preenchimento automático do navegador.

## window.onload [# window-onload]

O evento `load` no objeto` window` dispara quando toda a página é carregada, incluindo estilos, imagens e outros recursos.

O exemplo abaixo mostra corretamente os tamanhos de imagem, porque `window.onload` aguarda todas as imagens:

`` `html run height = 200 refresh
<script>
window.onload = function () {
alerta ('Página carregada');

// imagem é carregada neste momento
alerta (`Tamanho da imagem: $ {img.offsetWidth} x $ {img.offsetHeight}`);
};
</ script>

<img id = "img" src = "https://en.js.cx/clipart/train.gif?speed=1&cache=0">
`` `

## window.onunload

Quando um visitante deixa a página, o evento 'descarregar' dispara na `janela '. Nós podemos fazer algo lá que não envolva um atraso, como o fechamento de janelas popup relacionadas. Mas não podemos cancelar a transição para outra página.

Para isso devemos usar outro evento - 'onbeforeunload`.

## window.onbeforeunload [# window.onbeforeunload]

Se um visitante iniciou a saída da página ou tenta fechar a janela, o manipulador 'onbeforeunload` pode solicitar uma confirmação adicional.

Ele precisa retornar a string com a pergunta. O navegador irá mostrar isso.

Por exemplo:

`` `js
window.onbeforeunload = function () {
retornar "Existem mudanças não salvas. Deixe agora?";
};
`` `

`` `online
Clique no botão em `<iframe>` abaixo para configurar o manipulador e, em seguida, clique no link para vê-lo em ação:

[iframe src = "window-onbeforeunload" border = "1" height = "80" link edita]
`` `

`` `warn header =" Alguns navegadores ignoram o texto e mostram sua própria mensagem em vez disso "
Alguns navegadores como o Chrome e o Firefox ignoram a string e mostra sua própria mensagem. Isso é por pura segurança, para proteger o usuário de mensagens potencialmente enganosas e hackeadas.
`` `

## readyState

O que acontece se configurarmos o manipulador `DOMContentLoaded` após o carregamento do documento?

Naturalmente, ele nunca corre.

Há casos em que não temos certeza se o documento está pronto ou não, por exemplo, um script externo com atributo de "assíncrono" e é executado de forma assíncrona. Dependendo da rede, ele pode ser carregado e executado antes que o documento seja concluído ou depois disso, não podemos ter certeza. Portanto, devemos saber o estado atual do documento.

A propriedade `document.readyState` nos fornece informações sobre isso. Existem 3 valores possíveis:

- "carregar" - o documento está carregando.
- `` interativo '' - o documento foi totalmente lido.
- `" complete "- o documento foi totalmente lido e todos os recursos (como imagens) também são carregados.

Então, podemos verificar `document.readyState` e configurar um manipulador ou executar o código imediatamente se estiver pronto.

Como isso:

`` `js
função de trabalho () {/*...*/}

se (document.readyState == 'carregar') {
document.addEventListener ('DOMContentLoaded', trabalho);
} outro {
trabalhos();
}
`` `

Há um evento `readystatechange 'que dispara quando o estado muda, para que possamos imprimir todos esses estados como este:

`` `js run
// Estado atual
console.log (document.readyState);

// imprime alterações de estado
document.addEventListener ('readystatechange', () => console.log (document.readyState));
`` `

O evento `readystatechange` é uma mecânica alternativa de rastreamento do estado de carregamento do documento, que apareceu há muito tempo. Hoje em dia, raramente é usado, mas vamos cobri-lo por completo.

Qual é o lugar de `readystatechange 'entre outros eventos?

Para ver o timing, aqui está um documento com `<iframe>`, `<img>` e manipuladores que logam eventos:

`` `html
<script>
log de função (texto) {/ * saída do tempo e mensagem * /}
log ('initial readyState:' + document.readyState);

document.addEventListener ('readystatechange', () => log ('readyState:' + document.readyState));
document.addEventListener ('DOMContentLoaded', () => log ('DOMContentLoaded'));

window.onload = () => log ('window onload');
</ script>

<iframe src = "iframe.html" onload = "log ('iframe onload')"> </ iframe>

<img src = "http://pt.js.cx/clipart/train.gif" id = "img">
<script>
img.onload = () => log ('img onload');
</ script>
`` `

O exemplo de trabalho é [na caixa de areia] (sandbox: readystate).

A saída típica:
1. [1] readyState inicial: carregando
2. [2] readyState: interativo
3. [2] DOMContentLoaded
4. [3] iframe onload
5. [4] readyState: complete
6. [4] img onload
7. [4] janela onload

Os números entre colchetes indicam o tempo aproximado de quando acontece. O tempo real é um pouco maior, mas os eventos rotulados com o mesmo dígito ocorrem aproximadamente ao mesmo tempo (+ - alguns ms).

- `document.readyState` torna-se` interativo` imediatamente antes de `DOMContentLoaded`. Esses dois eventos realmente significam o mesmo.
- `document.readyState` torna-se` completo` quando todos os recursos (`iframe` e` img`) são carregados. Aqui podemos ver que acontece em aproximadamente o mesmo tempo que `img.onload` (` img` é o último recurso) e `window.onload`. Alternar para o estado `complete` significa o mesmo que` window.onload`. A diferença é que `window.onload` sempre funciona após todos os outros manipuladores` load`.


## Resumo

Eventos do ciclo de vida da página:

- O evento `DOMContentLoaded` desencadeia no` documento` quando o DOM está pronto. Podemos aplicar JavaScript aos elementos nesta fase.
- Todos os scripts são executados, exceto aqueles que são externos com `async` ou` adiar
- Imagens e outros recursos ainda podem continuar carregando.
- O evento 'load` no' window` dispara quando a página e todos os recursos são carregados. Nós raramente usamos isso, porque geralmente não há necessidade de aguardar por tanto tempo.
- evento 'beforeunload` no `window` dispara quando o usuário deseja sair da página. Se retornar uma string, o navegador mostra uma questão de saber se o usuário realmente quer sair ou não.
- "descarregar" evento no `janela 'dispara quando o usuário está finalmente saindo, no manipulador só podemos fazer coisas simples que não envolvem atrasos ou perguntando a um usuário. Por causa dessa limitação, raramente é usada.
- `document.readyState` é o estado atual do documento, as alterações podem ser rastreadas no evento` readystatechange`:
- `carregar '- o documento está carregando.
- `interativo '- o documento é analisado, acontece aproximadamente na mesma hora que` DOMContentLoaded`, mas antes dele.
- `complete` - o documento e os recursos são carregados, acontece aproximadamente na mesma hora que` window.onload`, mas antes dele.
