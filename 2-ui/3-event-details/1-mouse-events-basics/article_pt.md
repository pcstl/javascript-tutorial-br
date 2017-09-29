# Fundamentos dos eventos do mouse

Os eventos do mouse vêm não só de "manipuladores de mouse", mas também são emulados em dispositivos sensíveis, tornando-os compatíveis.

Neste capítulo, entraremos em mais detalhes sobre eventos do mouse e suas propriedades.

[cortar]

## Tipos de eventos do mouse

Podemos dividir os eventos do mouse em duas categorias: "simples" e "complexas"

### Eventos simples

Os eventos simples mais utilizados são:

`mousedown / mouseup`
: O botão do mouse é clicado / liberado em um elemento.

`mouseover / mouseout`
: O ponteiro do mouse vem / sai de um elemento.

`mousemove`
: Todo movimento do mouse sobre um elemento desencadeia esse evento.

... Existem vários outros tipos de eventos também, vamos abordá-los mais tarde.

### Eventos complexos

`clique"
: Dispara após `mousedown` e depois` mouseup` sobre o mesmo elemento se o botão esquerdo do mouse foi usado.

`contextmenu`
: Dispara após `mousedown` se o botão direito do mouse fosse usado.

`dblclick`
: Dispara após um duplo clique sobre um elemento.

Eventos complexos são feitos de simples, então, em teoria, poderíamos viver sem eles. Mas eles existem, e isso é bom, porque eles são convenientes.

### Ordem de eventos

Uma ação pode desencadear vários eventos.

Por exemplo, um clique primeiro dispara `mousedown`, quando o botão é pressionado, depois` mouseup` e `click` quando é lançado.

Nos casos em que uma única ação inicia vários eventos, seu pedido é corrigido. Ou seja, os manipuladores são chamados na ordem `mousedown` ->` mouseup` -> `click`. Os eventos são tratados na mesma seqüência: `onmouseup` termina antes que o` onclick` seja executado.

`` `online
Clique no botão abaixo e você verá os eventos. Tente fazer duplo clique também.

No teste, abaixo, todos os eventos do mouse são registrados, e se houver mais de 1 segundo de atraso entre eles, eles são separados por uma régua horizontal.

Também podemos ver a propriedade `which` que permite detectar o botão do mouse.

<input onmousedown = "return logMouse (evento)" onmouseup = "return logMouse (evento)" onclick = "return logMouse (evento)" oncontextmenu = "return logMouse (evento)" ondblclick = "return logMouse (evento)" value = " Clique-me com o botão direito ou esquerdo do mouse "tipo =" botão "> <input onclick =" logClear ('test') "value =" Clear "type =" button "> <form id =" testform "name =" testform "> <textarea style =" font-size: 12px; height: 150px; width: 360px; "> </ textarea> </ form>
`` `

## Obtendo o botão: qual

Os eventos relacionados ao clique sempre têm a propriedade `which` que permite obter o botão.

Não é usado para eventos `clique e` contextomenu`, porque o primeiro ocorre apenas no clique esquerdo e o último - apenas no botão direito do mouse.

Mas se rastreamos `mousedown` e` mouseup`, então precisamos disso, porque esses eventos desencadeiam qualquer botão, então `which` permite distinguir entre" right-mousedown "e" left-mousedown ".

Existem os três valores possíveis:

- `event.which == 1` - o botão esquerdo
- `event.which == 2` - o botão do meio
- `event.which == 3` - o botão direito

O botão do meio é um pouco exótico no momento e é muito raramente usado.

## Modificadores: shift, alt, ctrl e meta

Todos os eventos do mouse incluem a informação sobre as teclas modificadoras pressionadas.

As propriedades são:

- `shiftKey`
- `altKey`
- `ctrlKey`
- `metaKey` (` chave: Cmd para Mac)

Por exemplo, o botão abaixo só funciona na tecla ': Alt + Shift` + clique:

`` `html autorun height = 60
<botão id = "botão"> Alt + Shift + Clique em mim! </ button>

<script>
button.onclick = function (event) {
*! *
se (event.altKey && event.shiftKey) {
* /! *
alerta ('Hooray!');
}
};
</ script>
`` `

`` `warn header =" Atenção: no Mac geralmente é "Cmd" em vez de `Ctrl`"
No Windows e no Linux existem teclas modificadoras `key: Alt`,` key: Shift` e `key: Ctrl`. No Mac, há mais uma coisa: `chave: Cmd, corresponde à propriedade` metaKey`.

Na maioria dos casos, quando o Windows / Linux usa `key: Ctrl`, as pessoas Mac usam` key: Cmd`. Então, onde um usuário do Windows pressiona `chave: Ctrl + Enter` ou` chave: Ctrl + A`, um usuário de Mac pressionaria `chave: Cmd + Enter` ou` chave: Cmd + A`, e assim por diante, a maioria dos aplicativos usa `chave: Cmd` em vez de` key: Ctrl`.

Então, se quisermos suportar combinações como 'chave: Ctrl` + clique, então, para o Mac, faz sentido usar `chave: Cmd + clique. Isso é mais confortável para usuários de Mac.

Mesmo que desejemos forçar os usuários do Mac a 'chave: Ctrl` + clique - isso é meio difícil. O problema é: um clique esquerdo com `chave: Ctrl` é interpretado como * com o botão direito do mouse * no Mac, e gera o evento` contextmenu`, e não o `clique 'como Windows / Linux.

Então, se queremos que os usuários de todos os sistemas operacionais se sintam confortáveis, então, juntamente com `ctrlKey`, devemos usar` metaKey`.

Para o código JS, significa que devemos verificar `if (event.ctrlKey || event.metaKey)`.
`` `

`` `warn header =" Existem também dispositivos móveis "
As combinações de teclado são boas como uma adição ao fluxo de trabalho. Então, se o visitante tiver um
teclado - funciona. E se seu dispositivo não o possui - então há outra maneira de fazer o mesmo.
`` `

## Coordenadas: clientX / Y, pageX / Y

Todos os eventos do mouse possuem coordenadas em dois sabores:

1. Window-relative: `clientX` e` clientY`.
2. Document-relative: `pageX` e` pageY`.

Por exemplo, se tivermos uma janela do tamanho 500x500 e o mouse estiver no canto superior esquerdo, então `clientX` e` clientY` são `0`. E se o mouse estiver no centro, então `clientX` e` clientY` são `250`, independentemente do lugar no documento que seja. Eles são semelhantes a `position: fixed`.

`` `` online
Mova o mouse sobre o campo de entrada para ver `clientX / clientY` (está no` iframe`, então as coordenadas são relativas a `iframe`):

`` `html autorun height = 50
<input onmousemove = "this.value = event.clientX + ':' + event.clientY" value = "Mouse over me">
`` `
`` ``

As coordenadas relativas do documento são contadas a partir do canto superior esquerdo do documento, e não a janela.
As coordenadas `pageX`,` pageY` são semelhantes a `position: absolute` no nível do documento.

Você pode ler mais sobre as coordenadas no capítulo <info: coordenadas>.

## Nenhuma seleção em mousedown

Os cliques do mouse têm um efeito colateral que pode ser perturbador. Um duplo clique seleciona o texto.

Se quisermos lidar com eventos de clique, a seleção "extra" não parece boa.

Por exemplo, um duplo clique no texto abaixo seleciona-o além do nosso manipulador:

`` `html autorun height = 50

`` `

Existe uma maneira CSS de interromper a seleção: a propriedade `user-select` de [CSS UI Draft] (https://www.w3.org/TR/css-ui-4/).

A maioria dos navegadores o suporta com prefixos:

`` `html autorun height = 50
<style>
b {
-webkit-user-select: none;
-moz-user-select: none;
-ms-user-select: none;
usuário-selecionar: nenhum;
}
</ style>

Antes...

Não selecionável
</ b>
...Depois de
`` `

Agora, se você clicar duas vezes em "Unselectable", ele não será selecionado. Parece que funciona.

... Mas há um problema potencial! O texto tornou-se verdadeiramente desmarcível. Mesmo que um usuário comece a seleção de "Antes" e termine com "Depois", a seleção ignora a parte "Unselectable". Queremos realmente tornar nosso texto inseguro?

Na maioria das vezes, nós não. Um usuário pode ter motivos válidos para selecionar o texto, para copiar ou outras necessidades. Isso pode ser perturbador se não permitimos que ele o faça. Então, a solução não é tão boa.

O que queremos é evitar a seleção em um duplo clique, é isso.

Uma seleção de texto é a ação padrão do navegador no evento `mousedown`. Então, a solução alternativa seria lidar com `mousedown` e evitá-lo, assim:

`` `html autorun height = 50
Antes...

Clique duas vezes em mim
</ b>
...Depois de
`` `

Agora, o elemento em negrito não está selecionado em dois cliques.

Por outro lado, o texto dentro dele ainda é selecionável. A seleção deve começar não no próprio texto, mas antes ou depois dele. Normalmente, está bem.

`` `` cabeçalho inteligente = "Cancelar a seleção"
Em vez de * impedir * a seleção, podemos cancelá-la "post-factum" no manipulador de eventos.

Veja como:

`` `html autorun height = 50
Antes...
<b ondblclick = "*! * getSelection (). removeAllRanges () * /! *">
Clique duas vezes em mim
</ b>
...Depois de
`` `

Se você clicar duas vezes no elemento em negrito, então a seleção será exibida e, em seguida, será removida imediatamente. Isso não parece ser bom.
`` ``

`` `` cabeçalho inteligente = "Prevenção de cópia"
Se quisermos desabilitar a seleção para proteger o nosso conteúdo de copiar, podemos usar outro evento: `oncopy`.

`` `html autorun height = 80 não-embelezar

Querido usuário,
A cópia está proibida para você.
Se você conhece JS ou HTML, então você pode obter tudo da fonte da página.
</ div>
`` `
Se você tentar copiar um texto no `<div>`, isso não funcionará, porque a ação padrão 'oncopy` é impedida.

Certamente, isso não pode impedir o usuário de abrir HTML-source, mas nem todos sabem como fazê-lo.
`` ``

## Resumo

Os eventos do mouse possuem as seguintes propriedades:

- Botão: `que`.
- Teclas modificadoras (`true` se pressionadas):` altKey`, `ctrlKey`,` shiftKey` e `metaKey` (Mac).
- Se você deseja lidar com `key: Ctrl`, então não se esqueça dos usuários do Mac, eles usam a tecla` `` Cmd` ", então é melhor verificar` if (e.metaKey || e.ctrlKey) `.

- Coordenadas relativas à janela: `clientX / clientY`.
- Coordenadas relativas ao documento: `pageX / clientX`.

Nas tarefas abaixo, também é importante lidar com a seleção como um efeito colateral indesejado dos cliques.

Há várias maneiras, por exemplo:
1. CSS-property `user-select: none` (com prefixos do navegador) desativa-o completamente.
2. Cancelar o post-factum de seleção usando `getSelection (). RemoveAllRanges ()`.
3. Manusear `mousedown` e evitar a ação padrão (geralmente a melhor).
