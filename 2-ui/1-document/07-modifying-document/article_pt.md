# Modificando o documento

As modificações DOM são a chave para criar páginas "ao vivo".

Aqui vamos ver como criar novos elementos "on the fly" e modificar o conteúdo da página existente.

Primeiro, veremos um exemplo simples e depois explicaremos os métodos.

[cortar]

## Exemplo: mostrar uma mensagem

Para o início, vejamos como adicionar uma mensagem na página que parece mais agradável do que "alerta".

Veja como ficará:

`` `html autorun height =" 80 "
<style>
.alerta {
estofamento: 15px;
borda: 1px sólido # d6e9c6;
raio de borda: 4px;
cor: # 3c763d;
cor de fundo: # dff0d8;
}
</ style>

*! *
<div class = "alert">
<strong> Olá! </ strong> Você leu uma mensagem importante.
</ div>
* /! *
`` `

Isso foi um exemplo HTML. Agora vamos criar o mesmo `div` com JavaScript (assumindo que os estilos ainda estão no HTML ou um CSS externo).

## Criando um elemento


Para criar nós DOM, existem dois métodos:

`document.createElement (tag)`
: Cria um novo elemento com a tag fornecida:

`` `js
deixe div = document.createElement ('div');
`` `

`document.createTextNode (texto)`
: Cria um novo * nó de texto * com o texto fornecido:

`` `js
deixe textNode = document.createTextNode ('Here I am');
`` `

### Criando a mensagem

No nosso caso, queremos fazer um `div` com determinadas classes e a mensagem nele:

`` `js
deixe div = document.createElement ('div');
div.className = "alert alert-success";
div.innerHTML = "<strong> Olá! </ strong> Você leu uma mensagem importante.";
`` `

Depois disso, temos um elemento DOM pronto. Agora, está na variável, mas não pode ser visto, porque não foi inserido na página ainda.

## Métodos de inserção

Para fazer o `div` aparecer, precisamos inseri-lo em algum lugar no` documento '. Por exemplo, em `document.body`.

Existe um método especial para isso: `document.body.appendChild (div)`.

Aqui está o código completo:

`` `html run height =" 80 "
<style>
.alerta {
estofamento: 15px;
borda: 1px sólido # d6e9c6;
raio de borda: 4px;
cor: # 3c763d;
cor de fundo: # dff0d8;
}
</ style>

<script>
deixe div = document.createElement ('div');
div.className = "alert alert-success";
div.innerHTML = "<strong> Olá! </ strong> Você leu uma mensagem importante.";

*! *
document.body.appendChild (div);
* /! *
</ script>
`` `

Aqui está uma breve lista de métodos para inserir um nó em um elemento pai (`parentElem` para breve):

`parentElem.appendChild (node)`
: Anexa `node` como o último filho de` parentElem`.

O exemplo a seguir adiciona um novo `<li>` ao final de `<ol>`:

`` `html run height = 100
<ol id = "list">
<Li> 0 </ li>
<Li> 1 </ li>
<Li> 2 </ li>
</ ol>

<script>
deixe newLi = document.createElement ('li');
newLi.innerHTML = 'Olá, mundo!';

list.appendChild (newLi);
</ script>
`` `

`parentElem.insertBefore (node, nextSibling)`
: Insere `node` antes de` nextSibling` em `parentElem`.

O código a seguir insere um novo item de lista antes do segundo `<li>`:

`` `html run height = 100
<ol id = "list">
<Li> 0 </ li>
<Li> 1 </ li>
<Li> 2 </ li>
</ ol>
<script>
deixe newLi = document.createElement ('li');
newLi.innerHTML = 'Olá, mundo!';

*! *
list.insertBefore (newLi, list.children [1]);
* /! *
</ script>
`` `

Para inserir como o primeiro elemento, podemos fazer assim:

`` `js
list.insertBefore (newLi, list.firstChild);
`` `

`parentElem.replaceChild (node, oldChild)`
: Substitui `oldChild` por` node` entre filhos de `parentElem`.

Todos esses métodos retornam o nó inserido. Em outras palavras, `parentElem.appendChild (node)` retorna `node`. Mas geralmente o valor retornado não é usado, nós apenas executamos o método.

Esses métodos são "old school": eles existem nos tempos antigos e podemos encontrá-los em muitos scripts antigos. Infelizmente, existem algumas tarefas difíceis de resolver com elas.

Por exemplo, como inserir * html * se o tivermos como uma string? Ou, dado um nó, como inserir outro nó * antes * dele? Claro, tudo isso é possível, mas não de maneira elegante.

Portanto, existem dois outros conjuntos de métodos de inserção para lidar com todos os casos com facilidade.

### prepend / append / before / after

Este conjunto de métodos fornece inserções mais flexíveis:

- `node.append (... nodes ou strings)` - anexar nós ou strings no final de `node`,
- `node.prepend (... nodes ou strings)` - inserir nós ou cadeias no início do `node`,
- `node.before (... nodes ou strings)` - inserir nós ou strings antes do `node`,
- `node.after (... nodes ou strings)` - inserir nós ou strings após o `node`,
- `node.replaceWith (... nodes ou strings)` - substitui `node` por nós ou strings fornecidos.

Aqui está um exemplo de usar esses métodos para adicionar mais itens a uma lista e o texto antes / depois dele:

`` `html autorun
<ol id = "ol">
<Li> 0 </ li>
<Li> 1 </ li>
<Li> 2 </ li>
</ ol>

<script>
ol.before ('before');
ol.after ('after');

deixe prepend = document.createElement ('li');
prepend.innerHTML = 'prepend';
ol.prepend (prepend);

Deixe append = document.createElement ('li');
append.innerHTML = 'append';
ol.append (anexar);
</ script>
`` `

Aqui está uma pequena imagem que métodos fazem:

! [] (before-prepend-append-after.png)

Então, a lista final será:

`` `html
antes
<ol id = "ol">
<li> prepend </ li>
<Li> 0 </ li>
<Li> 1 </ li>
<Li> 2 </ li>
<li> adicionar </ li>
</ ol>
depois de
`` `

Esses métodos podem inserir múltiplas listas de nós e peças de texto em uma única chamada.

Por exemplo, aqui é inserida uma string e um elemento:

`` `html run
<div id = "div"> </ div>
<script>
div.before ('<p> Olá </ p>', document.createElement ('hr'));
</ script>
`` `

Todo o texto está inserido * como texto *.

Então o HTML final é:

`` `html run
*! *
& lt; p & gt; Hello & lt; / p & gt;
* /! *
<hr>
<div id = "div"> </ div>
`` `

Em outras palavras, as strings são inseridas de forma segura, como `elem.textContent`.

Assim, esses métodos só podem ser usados ​​para inserir nós de DOM ou peças de texto.

Mas e se quisermos inserir HTML "como html", com todas as tags e coisas funcionando, como `elem.innerHTML`?

### insertAdjacentHTML / Texto / Elemento

Há outro método bastante versátil: `elem.insertAdjacentHTML (onde, html)`.

O primeiro parâmetro é uma string, especificando onde inserir, deve ser um dos seguintes:

- `" beforebegin "- inserir` html` antes do `element`,
- `" afterbegin "` - insira `html` em` elem`, no início,
- `" beforeend "- inserir` html` em `elem`, no final,
- `" afterend "` - insira `html` após` elem`.

O segundo parâmetro é uma string HTML, inserida "como está".

Por exemplo:

`` `html run
<div id = "div"> </ div>
<script>
div.insertAdjacentHTML ('beforebegin', '<p> Olá </ p>');
div.insertAdjacentHTML ('afterend', '<p> Bye </ p>');
</ script>
`` `

... Isso levaria a:

`` `html run
<p> Olá </ p>
<div id = "div"> </ div>
<p> Bye </ p>
`` `

É assim que podemos adicionar um HTML arbitrário à nossa página.

Aqui está a imagem das variantes de inserção:

!] [] (inserir-adjacente.png)

Podemos notar facilmente semelhanças entre esta e a imagem anterior. Os pontos de inserção são realmente os mesmos, mas esse método insere o HTML.

O método tem dois irmãos:

- `elem.insertAdjacentText (onde, texto)` - a mesma sintaxe, mas uma seqüência de `texto 'em inserido como texto em vez de HTML,
- `elem.insertAdjacentElement (onde, elem)` - a mesma sintaxe, mas insere um elemento.

Eles existem principalmente para tornar a sintaxe "uniforme". Na prática, a maior parte do tempo só é utilizado `insertAdjacentHTML`, pois para elementos e texto, temos métodos` anexar / prepend / before / after` - eles são mais curtos para escrever e podem inserir nós / peças de texto.

Então, aqui está uma variante alternativa de mostrar uma mensagem:

`` `html run
<style>
.alerta {
estofamento: 15px;
borda: 1px sólido # d6e9c6;
raio de borda: 4px;
cor: # 3c763d;
cor de fundo: # dff0d8;
}
</ style>

<script>
document.body.insertAdjacentHTML ("afterbegin", `<div class =" alerta-sucesso de alerta ">
<strong> Olá! </ strong> Você leu uma mensagem importante.
</ div> `);
</ script>
`` `

## Clonagem de nós: cloneNode

Como inserir mais uma mensagem semelhante?

Podemos fazer uma função e colocar o código lá. Mas a maneira alternativa seria * clonar * o `div existente 'e modificar o texto dentro dele (se necessário).

Às vezes, quando temos um grande elemento, isso pode ser mais rápido e mais simples.

- A chamada `elem.cloneNode (true)` cria um clone "profundo" do elemento - com todos os atributos e subelementos. Se chamarmos `elem.cloneNode (falso)`, o clone é feito sem elementos filho.

Um exemplo de copiar a mensagem:

`` `html run height =" 120 "
<style>
.alerta {
estofamento: 15px;
borda: 1px sólido # d6e9c6;
raio de borda: 4px;
cor: # 3c763d;
cor de fundo: # dff0d8;
}
</ style>

<div class = "alert" id = "div">
<strong> Olá! </ strong> Você leu uma mensagem importante.
</ div>

<script>
*! *
deixe div2 = div.cloneNode (true); // clone a mensagem
div2.querySelector ('forte'). innerHTML = 'Bye there!'; // altera o clone

div.after (div2); // mostra o clone após a div existente
* /! *
</ script>
`` `

## Métodos de remoção

Para remover nós, existem os seguintes métodos:


`parentElem.removeChild (nó)`
: Remove `elem` de` parentElem` (assumindo que é uma criança).

`node.remove ()`
: Remove o `node` do seu lugar.

Podemos ver facilmente que o segundo método é muito mais curto. O primeiro existe por razões históricas.

`` `smart
Se quisermos * mover * um elemento para outro lugar - não há necessidade de removê-lo do antigo.

** Todos os métodos de inserção removem automaticamente o nó do local antigo. **

Por exemplo, vamos trocar elementos:

`` `html run height = 50
<div id = "first"> Primeiro </ div>
<div id = "second"> Segundo </ div>
<script>
// não é necessário chamar remover
segundo. depois (primeiro); // pegue # segundo e depois - insira # primeiro
</ script>
`` `
`` ``

Vamos deixar nossa mensagem desaparecer depois de um segundo:

`` `html não é confiável
<style>
.alerta {
estofamento: 15px;
borda: 1px sólido # d6e9c6;
raio de borda: 4px;
cor: # 3c763d;
cor de fundo: # dff0d8;
}
</ style>

<script>
deixe div = document.createElement ('div');
div.className = "alert alert-success";
div.innerHTML = "<strong> Olá! </ strong> Você leu uma mensagem importante.";

document.body.append (div);
*! *
setTimeout (() => div.remove (), 1000);
// ou setTimeout (() => document.body.removeChild (div), 1000);
* /! *
</ script>
`` `

## Uma palavra sobre "document.write"

Há mais um método muito antigo de adicionar algo a uma página da web: `document.write`.

A sintaxe:

`` `html run
<p> Em algum lugar da página ... </ p>
*! *
<script>
document.write ('<b> Olá de JS </ b>');
</ script>
* /! *
<p> O fim </ p>
`` `

A chamada para `document.write (html)` escreve o `html` na página" aqui e agora ". A cadeia `html` pode ser gerada dinamicamente, por isso é um pouco flexível. Podemos usar o JavaScript para criar uma página web completa e escrevê-la.

O método vem de momentos em que não havia DOM, sem padrões ... Realmente velhos tempos. Ainda vive, porque há scripts usando isso.

Nos scripts modernos, raramente podemos vê-lo, devido à importante limitação.

** A chamada para `document.write` só funciona enquanto a página está sendo carregada. **

Se o chamarmos depois, o conteúdo do documento existente será apagado.

Por exemplo:

`` `html run
<p> Após um segundo, o conteúdo desta página será substituído ... </ p>
*! *
<script>
// document.write após 1 segundo
// é depois da página carregada, então ele apaga o conteúdo existente
setTimeout (() => document.write ('<b> ... Por isso. </ b>'), 1000);
</ script>
* /! *
`` `

Portanto, é meio inutilizável no estágio "depois de carregado", ao contrário de outros métodos de DOM que abordamos acima.

Essa foi a desvantagem.

Tecnicamente, quando `document.write` é chamado enquanto o navegador ainda está lendo HTML, ele acrescenta algo a ele e o navegador o consome exatamente como estava inicialmente.

Isso nos dá o lado positivo - ele funciona ardentemente rápido, porque não há * modificação DOM *. Ele grava diretamente no texto da página, enquanto o DOM ainda não foi construído e o navegador coloca-o em DOM em tempo de geração.

Então, se precisarmos adicionar muito texto em HTML de forma dinâmica, e estamos na fase de carregamento da página, e a velocidade é importante, pode ajudar. Mas, na prática, esses requisitos raramente se reúnem. E geralmente podemos ver esse método em scripts apenas porque eles são velhos.

## Resumo

Métodos para criar novos nós:

- `document.createElement (tag)` - cria um elemento com a etiqueta fornecida,
- `document.createTextNode (value)` - cria um nó de texto (raramente usado),
- `elem.cloneNode (deep)` - clona o elemento, se `deep == true` então com todos os descendentes.

Inserção e remoção de nós:

- Do pai:
- `parent.appendChild (node)`
- `parent.insertBefore (node, nextSibling)`
- `parent.removeChild (node)`
- `parent.replaceChild (newElem, node)`

Todos esses métodos retornam `node`.

- Dada uma lista de nós e strings:
- `node.append (... nodes ou strings)` - inserir no `node`, no final,
- `node.prepend (... nodes ou strings)` - inserir no `node`, no início,
- `node.before (... nodes ou strings)` - insira logo antes do `node`,
- `node.after (... nodes ou strings)` - inserir logo após `node`,
- `node.replaceWith (... nodes ou strings)` - substituir `node`.
- `node.remove ()` - remove o `node`.

As cadeias de texto são inseridas "como texto".

- Dado um pedaço de HTML: `element.insertAdjacentHTML (where, html)`, insere dependendo de onde:
- `` beforebegin '' - insira `html` antes do` elem`,
- `" afterbegin "` - insira `html` em` elem`, no início,
- `" beforeend "- inserir` html` em `elem`, no final,
- `" afterend "` - insira `html` logo após` elem`.

Também existem métodos semelhantes `elem.insertAdjacentText` e` elem.insertAdjacentElement`, eles inserem strings e elementos de texto, mas raramente são usados.

- Para adicionar HTML à página antes de terminar de carregar:
- `document.write (html)`

Depois que a página é carregada, essa chamada apaga o documento. Principalmente visto em scripts antigos.
