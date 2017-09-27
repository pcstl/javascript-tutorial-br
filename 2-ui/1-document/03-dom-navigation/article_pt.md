libs:
- Coloque
- domtree

---


# Walking the DOM

O DOM permite fazer qualquer coisa com elementos e seus conteúdos, mas primeiro precisamos alcançar o objeto DOM correspondente, inseri-lo em uma variável, e então podemos modificá-lo.

Todas as operações no DOM começam com o objeto `document`. A partir dele podemos acessar qualquer nó.

[cortar]

Aqui está uma imagem de links que permitem viajar entre os nós DOM:

!] [] (dom-links.png)

Vamos discuti-los com mais detalhes.

## No topo: documentElement e corpo

Os nós de árvore mais altos estão disponíveis diretamente como propriedades de 'documento':

`<html>` = `document.documentElement`
: O nó de documento mais alto é `document.documentElement`. Esse é o nó DOM da marca `<html>`.

`<body>` = `document.body`
: Outro nó DOM amplamente utilizado é o elemento `<body>` - `document.body`.

`<head>` = `document.head`
: A marca `<head>` está disponível como `document.head`.

`` `` warn header = "Há uma captura:` document.body` pode ser `null`"
Um script não pode acessar um elemento que não existe no momento da execução.

Em particular, se um script estiver dentro de `<head>`, então `document.body` não está disponível, porque o navegador ainda não o leu.

Então, no exemplo abaixo, o primeiro 'alerta' mostra `nulo ':

`` `html run
<html>

<head>
<script>
*! *
alerta ("From HEAD:" + document.body); // nulo, ainda não existe <body>
* /! *
</ script>
</ head>

<corpo>

<script>
alerta ("From BODY:" + document.body); // HTMLBodyElement, agora existe
</ script>

</ body>
</ html>
`` `
`` ``

`` `smart header =" No mundo DOM 'null` significa \ "não existe \" "
No DOM, o valor "nulo" significa "não existe" ou "nenhum tal nó".
`` `

## Children: childNodes, firstChild, lastChild

Há dois termos que usaremos a partir de agora:

- ** Nódulos infantis (ou crianças) ** - elementos que são crianças diretas. Em outras palavras, eles estão aninhados exatamente no dado. Por exemplo, `<head>` e `<body>` são filhos do elemento `<html>`.
- ** Descendentes ** - todos os elementos que estão aninhados no dado, incluindo crianças, seus filhos e assim por diante.

Por exemplo, aqui `<body>` tem filhos `<div>` e `<ul>` (e poucos nós de texto em branco):

`` `html run
<html>
<corpo>
<div> Comece </ div>

<Ul>
<li>
<b> Informação </ b>
</ li>
</ Ul>
</ body>
</ html>
`` `

... E se pedimos todos os descendentes de `<body>`, então recebemos crianças diretas `<div>`, `<ul>` e também mais elementos aninhados como `<li>` (sendo um filho de ` <ul> `) e` <b> `(sendo um filho de` <li> `) - toda a subárvore.

** A coleção `childNodes` fornece acesso a todos os nós secundários, incluindo nós de texto. **

O exemplo abaixo mostra crianças de `document.body`:

`` `html run
<html>
<corpo>
<div> Comece </ div>

<Ul>
<Li> As informações </ li>
</ Ul>

<div> Fim </ div>

<script>
*! *
para (deixe i = 0; i <document.body.childNodes.length; i ++) {
alerta (document.body.childNodes [i]); // Texto, DIV, Texto, UL, ..., SCRIPT
}
* /! *
</ script>
...mais coisas...
</ body>
</ html>
`` `

Por favor, note um detalhe interessante aqui. Se executarmos o exemplo acima, o último elemento mostrado é `<script>`. Na verdade, o documento tem mais coisas abaixo, mas no momento da execução do script o navegador ainda não o leu, então o script não o vê.

** Propriedades `primeiro filho 'e' último filho 'dão acesso rápido ao primeiro e último filho. **

Eles são apenas shorthands. Se existirem nós infantis, o seguinte é sempre verdadeiro:
`` `js
element.childNodes [0] === elem.firstChild
element.childNodes [elem.childNodes.length - 1] === elem.lastChild
`` `

Há também uma função especial `elem.hasChildNodes ()` para verificar se há algum nó filho.

### Coleções DOM

Como podemos ver, `childNodes` parece uma matriz. Mas, na verdade, não é uma matriz, mas sim uma * coleção * - uma matriz especial - como um objeto iterável.

Existem duas conseqüências importantes:

1. Podemos usar `for..of` para iterar sobre ele:
`` `js
para (vamos nó de document.body.childNodes) {
alerta (nó); // mostra todos os nós da coleção
}
`` `
Isso porque é iterable (fornece a propriedade `Symbol.iterator`, conforme necessário).

2. Os métodos de matriz não funcionarão, porque não é uma matriz:
`` `js run

`` `

A primeira coisa é legal. O segundo é tolerável, porque podemos usar `Array.from` para criar uma matriz" real "da coleção, se quisermos métodos de matriz:

`` `js run
alerta (Array.from (document.body.childNodes) .filter); // agora está lá
`` `

`` `cabeçalho de aviso =" coleções de DOM são somente leitura "
Coleções de DOM e ainda mais - * todas * propriedades de navegação listadas neste capítulo são somente leitura.

Não podemos substituir um filho por outra coisa, atribuindo `childNodes [i] = ...`.

Alterar DOM precisa de outros métodos, nós os veremos no próximo capítulo.
`` `

`` `cabeçalho de aviso =" coleções DOM estão ao vivo "
Quase todas as coleções DOM com pequenas exceções são * live *. Em outras palavras, eles refletem o estado atual de DOM.

Se mantivermos uma referência para `elem.childNodes`, e adicionar / remover nós em DOM, então eles aparecerão na coleção automaticamente.
`` `

`` `` warn header = "Não use` for..in` para loop over collections "
As coleções são iteráveis ​​usando `for..of`. Às vezes, as pessoas tentam usar `for..in` para isso.

Por favor, não. O loop `for..in 'itera sobre todas as propriedades enumeráveis. E as coleções possuem algumas propriedades "extra" raramente usadas que normalmente não queremos obter:

`` `html run
<corpo>
<script>
// mostra 0, 1, comprimento, item, valores e muito mais.
para (let prop in document.body.childNodes) alert (prop);
</ script>
</ body>
`` ``

## Irmãos e os pais

* Irmãos * são nós que são filhos do mesmo pai. Por exemplo, `<head>` e `<body>` são irmãos:

- `<body>` é dito ser o "próximo" ou "certo" irmão de `<head>`,
- `<head>` é o irmão "anterior" ou "esquerdo" de `<body>`.

O pai está disponível como `parentNode`.

O próximo nó no mesmo pai (próximo irmão) é `nextSibling`, e o anterior é` previousSibling`.

Por exemplo:

`` `html run
<html> <head> </ head> <body> <script>
// HTML é "denso" para evadir nós de texto "em branco" extra.

// pai de <corpo> é <html>
alerta (document.body.parentNode === document.documentElement); // verdade

// depois de <head> goes <body>
alerta (document.head.nextSibling); // HTMLBodyElement

// antes <body> vai <head>
alerta (document.body.previousSibling); // HTMLHeadElement
</ script> </ body> </ html>
`` `

## Navegação somente para elementos

As propriedades de navegação listadas acima referem-se aos nós * all *. Por exemplo, em `childNodes` podemos ver os nós de texto, nós de elementos e até mesmo nós de comentários, se existir.

Mas, para muitas tarefas, não queremos nós de texto ou comentário. Queremos manipular nós de elemento que representam tags e formam a estrutura da página.

Então, vejamos mais links de navegação que levam apenas * nodos de elemento * em consideração:

!] [] (dom-links-elements.png)

Os links são semelhantes aos dados acima, apenas com `Element` word inside:

- `children` - apenas aquelas crianças que são nós de elemento.
- `firstElementChild`,` lastElementChild` - primeiro e último elemento crianças.
- `previousElementSibling`,` nextElementSibling` - elementos vizinhos.
- `parentElement` - elemento pai.

`` `` smart header = "Why` parentElement`? O pai pode ser * não * um elemento? "
A propriedade `parentElement` retorna o pai do" elemento ", enquanto` parentNode` retorna o pai "any node". Essas propriedades geralmente são as mesmas: ambos obtêm o pai.

Com a única exceção de `document.documentElement`:

`` `js run
alerta (document.documentElement.parentNode); // documento
alerta (document.documentElement.parentElement); // nulo
`` `

Em outras palavras, o `documentElement` (` <html> `) é o nó raiz. Formalmente, ele tem 'documento' como pai. Mas `document` não é um elemento nó, então` parentNode` retorna e `parentElement` não.

Às vezes, isso importa quando estamos caminhando pela cadeia de pais e chamamos um método para cada um deles, mas o "documento" não o tem, então nós o excluímos.
`` ``

Vamos modificar um dos exemplos acima: substitua `childNodes` por` children`. Agora, ele mostra apenas elementos:

`` `html run
<html>
<corpo>
<div> Comece </ div>

<Ul>
<Li> As informações </ li>
</ Ul>

<div> Fim </ div>

<script>
*! *
para (deixe elem de document.body.children) {
alerta (elem); // DIV, UL, DIV, SCRIPT
}
* /! *
</ script>
...
</ body>
</ html>
`` `

## Mais links: tabelas [# dom-navigation-tables]

Até agora descrevemos as propriedades básicas de navegação.

Certos tipos de elementos DOM podem fornecer propriedades adicionais, específicas para seu tipo, por conveniência.

As tabelas são um excelente exemplo e importante caso particular disso.

** `<tabela>` ** elemento suporta (além do dado acima) essas propriedades:
- `table.rows` - ​​a coleção de elementos` <tr> `da tabela.
- `table.caption / tHead / tFoot` - referências a elementos` <legenda> `,` <thead> `,` <tfoot> `.
- `table.tBodies` - a coleção de elementos` <tbody> `(podem ser muitos de acordo com o padrão).

** `<thead>`, `<tfoot>`, `<tbody>` ** fornece a propriedade `rows`:
- `tbody.rows` - ​​a coleção de` <tr> `dentro.

** `<tr>`: **
- `tr.cells` - a coleção de células` <td> `e` <th> `dentro do dado` <tr> `.
- `tr.sectionRowIndex` - o número do dado` <tr> `dentro do` `thead> / <tbody>` incluído.
- `tr.rowIndex` - o número do` <tr> `na tabela.

** `<td>` e `<th>`: **
- `td.cellIndex` - o número da célula dentro do fechamento` <tr> `.

Um exemplo de uso:

`` `html run height = 100
<table id = "table">
<tr>
<td> one </ td> <td> dois </ td>
</ tr>
<tr>
<td> três </ td> <td> quatro </ td>
</ tr>
</ table>

<script>
// obtenha o conteúdo da primeira linha, segunda célula
alerta (tabela. *! * linhas [0] .cells [1] * /! *. innerHTML) // "dois"
</ script>
`` `

A especificação: [dados tabulares] (https://html.spec.whatwg.org/multipage/tables.html).

Existem também propriedades de navegação adicionais para formulários HTML. Nós vamos examiná-los mais tarde quando começar a trabalhar com formulários.

# Resumo

Dado um nó DOM, podemos ir aos seus vizinhos imediatos usando as propriedades de navegação.

Existem dois conjuntos principais deles:

- Para todos os nós: `parentNode`,` childNodes`, `firstChild`,` lastChild`, `previousSibling`,` nextSibling`.
- Somente para nós de elemento: `parentElement`,` children`, `firstElementChild`,` lastElementChild`, `previousElementSibling`,` nextElementSibling`.

Alguns tipos de elementos DOM, e. tabelas, forneça propriedades e coleções adicionais para acessar seu conteúdo.
