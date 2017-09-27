# Propriedades do nó: tipo, tag e conteúdo

Vamos dar uma visão mais aprofundada dos nós DOM.

Neste capítulo, veremos mais sobre o que são e suas propriedades mais utilizadas.

[cortar]

## Classes de nó DOM

Os nós DOM têm propriedades diferentes dependendo da sua classe. Por exemplo, um nó de elemento correspondente à tag `<a>` possui propriedades relacionadas ao link, e o que corresponde a `<input>` tem propriedades relacionadas à entrada e assim por diante. Os nós de texto não são iguais aos nós dos elementos. Mas também existem propriedades e métodos comuns entre todos eles, porque todas as classes de nós DOM formam uma hierarquia única.

Cada nó DOM pertence à classe incorporada correspondente.

A raiz da hierarquia é [EventTarget] (https://dom.spec.whatwg.org/#eventtarget), que é herdada por [Node] (http://dom.spec.whatwg.org/#interface-node ), e outros nós DOM herdam dela.

Aqui está a imagem, as explicações a seguir:

! [] (dom-class-hierarchy.png)

As aulas são:

- [EventTarget] (https://dom.spec.whatwg.org/#eventtarget) - é a classe "abstrata" da raiz. Os objetos dessa classe nunca são criados. Ele serve como base, de modo que todos os nós DOM suportam os chamados "eventos", nós os estudaremos mais tarde.
- [Node] (http://dom.spec.whatwg.org/#interface-node) - também é uma classe "abstrata", que serve como base para nós DOM. Ele fornece a funcionalidade da árvore central: `parentNode`,` nextSibling`, `childNodes` e assim por diante (eles são getters). Os objetos da classe `Node` nunca são criados. Mas há classes de nó de concreto que herdam dela, nomeadamente: `Texto 'para nós de texto,` Elemento' para nós de elementos e mais exóticos como 'Comentário' para nós de comentários.
- [Element] (http://dom.spec.whatwg.org/#interface-element) - é uma classe base para os elementos DOM. Fornece navegação em nível de elemento como `nextElementSibling`,` children` e métodos de pesquisa como `getElementsByTagName`,` querySelector`. No navegador, pode haver não apenas HTML, mas também documentos XML e SVG. A classe `Element` serve como base para classes mais específicas:` SVGElement`, `XMLElement` e` HTMLElement`.
- [HTMLElement] (https://html.spec.whatwg.org/multipage/dom.html#htmlelement) - é finalmente a classe básica para todos os elementos HTML. É herdado por vários elementos HTML:
- [HTMLInputElement] (https://html.spec.whatwg.org/multipage/forms.html#htmlinputelement) - a classe para elementos `<input>`,
- [HTMLBodyElement] (https://html.spec.whatwg.org/multipage/semantics.html#htmlbodyelement) - a classe para elementos `<body>`,
- [HTMLAnchorElement] (https://html.spec.whatwg.org/multipage/semantics.html#htmlanchorelement) - a classe para elementos `<a>`
- ... e assim por diante, cada tag tem sua própria classe que pode fornecer propriedades e métodos específicos.

Assim, o conjunto completo de propriedades e métodos de um dado nó vem como resultado da herança.

Por exemplo, consideremos o objeto DOM para um elemento `<input>`. Ele pertence à classe [HTMLInputElement] (https://html.spec.whatwg.org/multipage/forms.html#htmlinputelement). Obtém propriedades e métodos como uma superposição de:

- `HTMLInputElement` - esta classe fornece propriedades específicas de entrada e herda de ...
- `HTMLElement` - fornece métodos de elemento HTML comuns (e getters / setters) e herda de ...
- `Element` - fornece métodos de elementos genéricos e herda de ...
- `Node` - fornece propriedades de nó DOM comuns e herda de ...
- `EventTarget` - oferece suporte para eventos (a serem cobertos),
- ... e, finalmente, herda de "Objeto", então, métodos de "objeto puro", como 'hasOwnProperty` também estão disponíveis.

Para ver o nome da classe do nó DOM, podemos lembrar que um objeto geralmente possui a propriedade `constructor`. Referências ao construtor da classe e `constrtor.name` é o seu nome:

`` `js run
alerta (document.body.constructor.name); // HTMLBodyElement
`` `

... Ou podemos simplesmente 'toString` it:

`` `js run
alerta (document.body); // [objeto HTMLBodyElement]
`` `

Também podemos usar `instanceof` para verificar a herança:

`` `js run
alerta (document.body instanceof HTMLBodyElement); // verdade
alerta (document.body instanceof HTMLElement); // verdade
alerta (document.body instanceof Element); // verdade
alerta (document.body instanceof Node); // verdade
alerta (document.body instanceof EventTarget); // verdade
`` `

Como podemos ver, os nós DOM são objetos JavaScript regulares. Eles usam classes baseadas em protótipos para herança.

Isso também é fácil de ver, enviando um elemento com `console.dir (elem)` em um navegador. No console você pode ver `HTMLElement.prototype`,` Element.prototype` e assim por diante.

`` `smart header =" console.dir (elem) `versus` console.log (elem) ``
A maioria dos navegadores suporta dois comandos nas ferramentas do desenvolvedor: `console.log` e` console.dir`. Eles exibem seus argumentos para o console. Para os objetos JavaScript, esses comandos costumam fazer o mesmo.

Mas para elementos de DOM eles são diferentes:

- `console.log (elem)` mostra a árvore do elemento DOM.
- `console.dir (elem)` mostra o elemento como um objeto DOM, bom para explorar suas propriedades.

Experimente o documento "document.body".
`` `

`` `` cabeçalho inteligente = "IDL na especificação"
Nas classes de especificação são descritas usando não JavaScript, mas um especial [Idioma da descrição da interface] (https://en.wikipedia.org/wiki/Interface_description_language) (IDL), geralmente é fácil de entender.

Em IDL, todas as propriedades são precedidas por seus tipos. Por exemplo, `DOMString`,` boolean` e assim por diante.

Aqui está um trecho com comentários:

`` `js
// Definir HTMLInputElement
*! *
// O cólon ":" significa que HTMLInputElement herda do HTMLElement
* /! *
interface HTMLInputElement: HTMLElement {
// aqui vá propriedades e métodos dos elementos <input>

*! *
// "DOMString" significa que essas propriedades são strings
* /! *
O atributo DOMString aceita;
atributo DOMString alt;
atribuir autocompletar DOMString;
atribuir valor DOMString;

*! *
// propriedade booleana (true / false)
atribuir autofoco booleano;
* /! *
...
*! *
// agora o método: "vazio" significa que isso não retorna nenhum valor
* /! *
void select ();
...
}
`` `

Outras classes são um pouco semelhantes.
`` ``

## A propriedade "nodeType"

A propriedade `nodeType` fornece uma maneira antiga para obter o" tipo "de um nó DOM.

Tem um valor numérico:
- `elem.nodeType == 1` para nós de elemento,
- `elem.nodeType == 3` para nós de texto,
- `elem.nodeType == 9` para o objeto do documento,
- Existem poucos outros valores em [a especificação] (https://dom.spec.whatwg.org/#node).

Por exemplo:

`` `html run
<corpo>
<script>
deixe elem = document.body;

// vamos examinar o que é?
alerta (elem.nodeType); // 1 => elemento

// e o primeiro filho é ...
alerta (elem.firstChild.nodeType); // 3 => texto

// para o objeto do documento, o tipo é 9
alerta (document.nodeType); // 9
</ script>
</ body>
`` `

Nos scripts modernos, podemos usar `instanceof` e outros testes baseados em classe para ver o tipo de nó, mas às vezes` nodeType` pode ser mais simples. Só podemos ler `nodeType`, não mudá-lo.

## Tag: nodeName e tagName

Dado um nó DOM, podemos ler seu nome de tag de propriedades `nodeName` ou` tagName`:

Por exemplo:

`` `js run
alerta (document.body.nodeName); // CORPO
alerta (document.body.tagName); // CORPO
`` `

Existe alguma diferença entre tagName e nodeName?

Claro, a diferença se reflete em seus nomes, mas é de fato um pouco sutil.

- A propriedade `tagName` existe apenas para nós` Element`.
- O `nodeName` está definido para qualquer` Node`:
- para elementos significa o mesmo que `tagName`.
- para outros tipos de nó (texto, comentário, etc.), ele possui uma string com o tipo de nó.

Em outras palavras, `tagName` é suportado apenas por nós de elemento (como é originário da classe` Element`), enquanto `nodeName` pode dizer algo sobre outros tipos de nó.

Por exemplo, vamos comparar `tagName` e` nodeName` para o `document` e um nó de comentário:


`` `html run
<body> <! - comment ->

<script>
// para comentário
alerta (document.body.firstChild.tagName); // indefinido (não elemento)
alerta (document.body.firstChild.nodeName); // #comente

// para documento
alerta (document.tagName); // indefinido (não elemento)
alerta (document.nodeName); // #document
</ script>
</ body>
`` `

Se lidamos apenas com elementos, o `tagName` é a única coisa que devemos usar.


`` `smart header =" O nome da tag é sempre maiúsculo, exceto XHTML "
O navegador possui dois modos de processamento de documentos: HTML e XML. Normalmente, o modo HTML é usado para páginas da web. O modo XML é ativado quando o navegador recebe um documento XML com o cabeçalho: `Content-Type: application / xml + xhtml`.

No modo HTML `tagName / nodeName` é sempre maiúsculo: é` BODY` para '<body> `ou` <BoDy> `.

No modo XML, o caso é mantido "como está". Atualmente, o modo XML raramente é usado.
`` `


## innerHTML: o conteúdo

A propriedade [innerHTML] (https://w3c.github.io/DOM-Parsing/#widl-Element-innerHTML) permite obter o HTML dentro do elemento como uma string.

Também podemos modificá-lo. Portanto, é uma das maneiras mais poderosas de mudar a página.

O exemplo mostra o conteúdo do `document.body` e depois o substitui completamente:

`` `html run
<corpo>
<p> Um parágrafo </ p>
<div> A div </ div>

<script>
alerta (document.body.innerHTML); // leia o conteúdo atual
document.body.innerHTML = 'O novo CORPO!'; // substitua
</ script>

</ body>
`` `

Podemos tentar inserir um HTML inválido, o navegador irá corrigir nossos erros:

`` `html run
<corpo>

<script>
document.body.innerHTML = '<b> teste'; // esqueci de fechar a etiqueta
alerta (document.body.innerHTML); // <b> teste </ b> (fixo)
</ script>

</ body>
`` `

`` `cabeçalho inteligente =" Scripts não executam "
Se `innerHTML` insere uma tag` <script> `no documento - não é executado.

Torna-se parte do HTML, assim como um script que já foi executado.
`` `

### Cuidado: "innerHTML + =" faz uma substituição completa

Podemos acrescentar "mais HTML" usando `elem.innerHTML + =" algo "`.

Como isso:

`` `js
chatDiv.innerHTML + = "<div> Olá <img src = 'smile.gif' />! </ div>";
chatDiv.innerHTML + = "Como vai?";
`` `

Mas devemos ter muito cuidado em fazê-lo, porque o que está acontecendo é * não * uma adição, mas uma sobregravação completa.

Tecnicamente, essas duas linhas fazem o mesmo:

`` `js
elem.innerHTML + = "...";
// é uma maneira mais curta de escrever:
*! *
elem.innerHTML = element.innerHTML + "..."
* /! *
`` `

Em outras palavras, `innerHTML + =` faz isso:

1. O conteúdo antigo é removido.
2. O novo `innerHTML` é escrito em vez disso (uma concatenação do antigo e do novo).

** Como o conteúdo é "zerado" e reescrito a partir do zero, todas as imagens e outros recursos serão recarregados **.

No exemplo `chatDiv` acima da linha` chatDiv.innerHTML + = "Como vai?", Recria o conteúdo HTML e recarrega `smile.gif` (espero que seja armazenado em cache). Se `chatDiv tiver muitos outros textos e imagens, o recarregamento torna-se claramente visível.

Existem outros efeitos colaterais também. Por exemplo, se o texto existente foi selecionado com o mouse, a maioria dos navegadores removerá a seleção ao reescrever `innerHTML`. E se houvesse um `<input>` com um texto inserido pelo visitante, o texto será removido. E assim por diante.

Por sorte, existem outras maneiras de adicionar HTML além do `innerHTML`, e as estudaremos em breve.

## outerHTML: HTML completo do elemento

A propriedade `outerHTML` contém o HTML completo do elemento. Isso é como `innerHTML` mais o elemento em si.

Aqui está um exemplo:

`` `html run
<div id = "elem"> Olá <b> Mundo </ b> </ div>

<script>
alerta (elem.outerHTML); // <div id = "elem"> Olá <b> Mundo </ b> </ div>
</ script>
`` `

** Cuidado: ao contrário de `innerHTML`, escrever para` outerHTML` não altera o elemento. Em vez disso, o substitui como um todo no contexto externo. **

Sim, parece estranho e estranho, é por isso que fazemos uma nota separada sobre isso aqui. Dê uma olhada.

Considere o exemplo:

`` `html run
<div> Olá, mundo! </ div>

<script>
Deixe div = document.querySelector ('div');

*! *
// substitua div.outerHTML com <p> ... </ p>
* /! *
div.outerHTML = '<p> Um novo elemento! </ p>'; // (*)

*! *
// Uau! O div ainda é o mesmo!
* /! *
alerta (div.outerHTML); // <div> Olá, mundo! </ div>
</ script>
`` `

Na linha `(*)` tomamos o HTML completo de `<div> ... </ div>` e substituí-lo por `<p> ... </ p>`. No documento externo, podemos ver o novo conteúdo em vez do `<div>`. Mas a antiga variável `div` ainda é a mesma.

A atribuição `outerHTML` não modifica o elemento DOM, mas extrai-lo do contexto externo e insere uma nova peça de HTML ao invés disso.

Novice desenvolvedores, por vezes, fazem um erro aqui: eles modificam `div.outerHTML` e continuam a trabalhar com` div` como se tivesse o novo conteúdo nele.

Isso é possível com `innerHTML`, mas não com` outerHTML`.

Podemos escrever para `outerHTML`, mas deve ter em mente que não altera o elemento ao qual estamos escrevendo. Ele cria o novo conteúdo em seu lugar. Podemos obter uma referência a novos elementos consultando o DOM.

## nodeValue / data: conteúdo do nó de texto

A propriedade `innerHTML` é válida somente para nós de elemento.

Outros tipos de nó têm sua contrapartida: propriedades `nodeValue` e` data`. Estes dois são quase o mesmo para uso prático, existem apenas pequenas diferenças de especificação. Então, usaremos `data`, porque é mais curto.

Podemos lê-lo, assim:

`` `html run height =" 50 "
<corpo>
Olá
<! - Comentário ->
<script>
deixe text = document.body.firstChild;
*! *
alerta (text.data); // Olá
* /! *

Deixe o comment = text.nextSibling;
*! *
alerta (comment.data); // Comente
* /! *
</ script>
</ body>
`` `

Para os nós de texto, podemos imaginar um motivo para lê-los ou modificá-los, mas por que comentários? Geralmente, eles não são interessantes, mas às vezes os desenvolvedores incorporam informações em HTML neles, como este:

`` `html
<! - if isAdmin ->
<div> Bem-vindo, administrador! </ div>
<! - / if ->
`` `

... Então o JavaScript pode lê-lo e processar instruções embutidas.

## textContent: texto puro

O `textContent` fornece acesso ao * texto * dentro do elemento: apenas texto, menos todos` <tags> `.

Por exemplo:

`` `html run
<div id = "news">
<h1> Headline! </ h1>
<p> Marcianos atacam pessoas! </ p>
</ div>

<script>
// Headline! Os marcianos atacam pessoas!
alerta (news.textContent);
</ script>
`` `

Como podemos ver, apenas o texto é retornado, como se todos os `<tags>` fossem cortados, mas o texto neles permaneceu.

Na prática, raramente é necessário ler esse texto.

** Escrever para `textContent` é muito mais útil, pois permite escrever o texto de forma segura. **

Digamos que temos uma string arbitrária, por exemplo, inserida por um usuário, e quer mostrar isso.

- Com `innerHTML`, teremos inserido" como HTML ", com todas as tags HTML.
- Com `textContent`, teremos inserido" como texto ", todos os símbolos são tratados literalmente.

Compare os dois:

`` `html run
<div id = "elem1"> </ div>
<div id = "elem2"> </ div>

<script>
let name = prompt ("Qual é o seu nome?", "<b> Winnie-the-pooh! </ b>");

elem1.innerHTML = nome;
elem2.textContent = nome;
</ script>
`` `

1. O primeiro `<div>` recebe o nome "como HTML": todas as tags tornam-se tags, então vemos o nome ousado.
2. O segundo `<div>` recebe o nome "como texto", então nós literalmente vemos `<b> Winnie-the-pooh! </ B>`.

Na maioria dos casos, esperamos o texto de um usuário e queremos tratá-lo como texto. Não queremos HTML inesperado em nosso site. Uma tarefa para `textContent` faz exatamente isso.

## A propriedade "escondida"

O atributo "oculto" e a propriedade DOM especificam se o elemento está visível ou não.

Podemos usá-lo em HTML ou atribuir usando JavaScript, como este:

`` `html run height =" 80 "
<div> Ambos os divs abaixo estão escondidos </ div>

<div hidden> Com o atributo "hidden" </ div>

<div id = "elem"> JavaScript atribuiu a propriedade "oculta" </ div>

<script>
element.hidden = true;
</ script>
`` `

Tecnicamente, `hidden` funciona da mesma forma que` style = "display: none" `. Mas é mais curto para escrever.

Aqui está um elemento intermitente:


`` `html run height = 50
<div id = "elem"> Um elemento piscando </ div>

<script>
setInterval (() => element.hidden =! element.hidden, 1000);
</ script>
`` `

## Mais propriedades

Os elementos DOM também possuem propriedades adicionais, muitas delas fornecidas pela classe:

- `value` - o valor para` <input> `,` <select> `e` <textarea> `(` HTMLInputElement`, `HTMLSelectElement` ...).
- `href` - o" href "para` <a href="..."> `(` HTMLAnchorElement`).
- `id` - o valor do atributo" id ", para todos os elementos (` HTMLElement`).
- ...e muito mais...

Por exemplo:

`` `html run height =" 80 "
<input type = "text" id = "elem" value = "value">

<script>
alerta (elem.type); // "texto"
alert (elem.id); // "elemento"
alerta (elem.value); // valor
</ script>
`` `

A maioria dos atributos HTML padrão tem a propriedade DOM correspondente, e podemos acessá-lo assim.

Se quisermos conhecer a lista completa de propriedades suportadas para uma determinada classe, podemos encontrá-las na especificação. Por exemplo, HTMLInputElement está documentado em <https://html.spec.whatwg.org/#htmlinputelement>.

Ou se quisermos buscá-los rapidamente ou interessados ​​no navegador concreto - sempre podemos enviar o elemento usando `console.dir (elem)` e ler as propriedades. Ou explore "propriedades DOM" na guia Elementos das ferramentas do desenvolvedor do navegador.

## Resumo

Cada nó DOM pertence a uma determinada classe. As classes formam uma hierarquia. O conjunto completo de propriedades e métodos vem como resultado da herança.

As principais propriedades do nó DOM são:

`nodeType`
: Tipo de nó. Podemos obtê-lo da classe de objeto DOM, mas muitas vezes precisamos apenas para ver se é um nó de texto ou elemento. A propriedade `nodeType` é boa para isso. Tem valores numéricos, os mais importantes são: `1` - para elementos,` 3` - para nós de texto. Somente leitura.

`nodeName / tagName`
: Para elementos, nome da etiqueta (em maiúsculas a menos que seja no modo XML). Para nós não-elemento `nodeName` descreve o que é isso. Somente leitura.

`innerHTML`
: O conteúdo HTML do elemento. Pode modificar.

`outerHTML`
: O HTML completo do elemento. Uma operação de gravação em `elem.outerHTML` não toca` elem`. Em vez disso, ele é substituído pelo novo HTML no contexto externo.

`nodeValue / data`
: O conteúdo de um nó que não é elemento (texto, comentário). Estes dois são quase iguais, geralmente usamos `data`. Pode modificar.

`textContent`
: O texto dentro do elemento, basicamente HTML menos todos `<tags>`. Escrever nele coloca o texto dentro do elemento, com todos os caracteres e tags especiais tratados exatamente como texto. Pode inserir com segurança o texto gerado pelo usuário e proteger as inserções de HTML indesejadas.

`escondido '
: Quando definido como `true`, faz o mesmo que CSS` display: none`.

Os nós DOM também possuem outras propriedades dependendo da classe. Por exemplo, os elementos `<input>` (`HTMLInputElement`) suportam` value`, `type`, enquanto que` <a> `elements (` HTMLAnchorElement`) suportam `href` etc. A maioria dos atributos HTML padrão possuem a propriedade DOM correspondente .

Mas os atributos HTML e as propriedades DOM não são sempre os mesmos, como veremos no próximo capítulo.
