libs:
- Coloque
- domtree

---

# Árvore DOM

A espinha dorsal de um documento HTML é uma etiqueta.

De acordo com Document Object Model (DOM), cada HTML-tag é um objeto. As tags aninhadas são chamadas de "crianças" do anexo.

O texto dentro de uma etiqueta é também um objeto.

Todos esses objetos estão acessíveis usando o JavaScript.

## Um exemplo de DOM

Por exemplo, vamos explorar o DOM para este documento:

`` `html run no-embellecer
<! DOCTYPE HTML>
<html>
<head>
<title> Sobre os alces </ title>
</ head>
<corpo>
A verdade sobre os alces.
</ body>
</ html>
`` `

O DOM representa o HTML como uma estrutura em árvore de tags. Veja como parece:

<div class = "domtree"> </ div>

<script>
Deixe node1 = {"name": "HTML", "nodeType": 1, "children": [{"name": "HEAD", "nodeType": 1, "children": [{"name": "# texto "," nodeType ": 3," content ":" \ n "}, {" name ":" TITLE "," nodeType ": 1," children ": [{" name ":" # text "," nodeType ": 3," content ":" About elks "}]}, {" name ":" # text "," nodeType ": 3," content ":" \ n "}]}, {" name ": "#text", "nodeType": 3, "content": "\ n"}, {"name": "BODY", "nodeType": 1, "children": [{"name": "# text" , "nodeType": 3, "content": "\ n A verdade sobre os alces."}]}]}}

drawHtmlTree (node1, 'div.domtree', 690, 320);
</ script>

`` `online
Na imagem acima dos nós de elemento, você pode clicar em nós de elemento. Seus filhos vão abrir / colapsar.
`` `

As tags são chamadas * nodos de elementos * (ou apenas elementos). As tags aninhadas tornam-se crianças dos envolventes. Como resultado, temos uma árvore de elementos: `<html>` está na raiz, então `<head>` e `<body>` são seus filhos etc.

O texto dentro dos elementos forma * nós de texto *, rotulado como `# texto '. Um nó de texto contém apenas uma string. Pode não ter filhos e é sempre uma folha da árvore.

Por exemplo, a tag `<title>` tem o texto `" Sobre elks "`.

Observe os caracteres especiais nos nós de texto:

- uma nova linha: `↵` (em JavaScript conhecido como` \ n`)
- um espaço: `␣`

Espaços e novas linhas - são caracteres totalmente válidos, eles formam nós de texto e se tornam parte do DOM. Assim, por exemplo, no exemplo acima, a marca `<head>` contém espaços antes do `<title>`, e esse texto se torna um nó `# text` (ele contém apenas uma nova linha e apenas alguns espaços).

Existem apenas duas exclusões de alto nível:
1. Espaços e novas linhas antes de `<head>` são ignorados por motivos históricos,
2. Se colocarmos algo depois de `</ body>`, então isso é movido automaticamente dentro do `body`, no final, como a especificação HTML exige que todo o conteúdo esteja dentro de` <body> `. Portanto, pode não haver espaços após `</ body>`.

Em outros casos, tudo é honesto - se houver espaços (assim como qualquer personagem) no documento, eles anotam nós em DOM e, se os removermos, não haverá nenhum.

Aqui não há nós de texto apenas para o espaço:

`` `html não embelezam
<! DOCTYPE HTML>
<html> <head> <title> Sobre os alks </ title> </ head> <body> A verdade sobre os alces. </ body> </ html>
`` `

<div class = "domtree"> </ div>

<script>
Deixe node2 = {"name": "HTML", "nodeType": 1, "children": [{"name": "HEAD", "nodeType": 1, "children": [{"name": "TITLE "," nodeType ": 1," children ": [{" name ":" # text "," nodeType ": 3," content ":" Sobre elks "}]}]}, {" name ":" BODY "," nodeType ": 1," children ": [{" name ":" # text "," nodeType ": 3," content ":" A verdade sobre os alces ".}]}]}

drawHtmlTree (node2, 'div.domtree', 690, 210);
</ script>

`` `cabeçalho inteligente =" Espaços de borda e texto em branco incompleto geralmente são ocultos nas ferramentas "
As ferramentas do navegador (para serem cobertas em breve) que funcionam com DOM geralmente não mostram espaços no início / fim do texto e nos nós de texto vazios (quebras de linha) entre as tags. .

Isso porque eles são usados ​​principalmente para decorar HTML e não afetam (na maioria dos casos) como é mostrado.

Em outras fotos de DOM às vezes as omitiremos onde elas são irrelevantes, para manter as coisas curtas.
`` `


## Auto correção

Se o navegador encontrar um HTML malformado, ele o corrige automaticamente ao fazer o DOM.

Por exemplo, a tag superior é sempre `<html>`. Mesmo que não exista no documento - será no DOM, o navegador irá criá-lo. O mesmo sobre `<body>`.

Como exemplo, se o arquivo HTML for uma única palavra `" Olá ", o navegador irá envolvê-lo em` <html> `e` <body> `, adicionar o` `head>` necessário, e o DOM será :


<div class = "domtree"> </ div>

<script>
Deixe node3 = {"name": "HTML", "nodeType": 1, "children": [{"name": "HEAD", "nodeType": 1, "children": []}, {"name" : "BODY", "nodeType": 1, "children": [{"name": "# text", "nodeType": 3, "content": "Hello"}]}]}}

drawHtmlTree (node3, 'div.domtree', 690, 150);
</ script>

Ao gerar DOM, o navegador processa automaticamente erros no documento, fecha tags e assim por diante.

Esse documento "inválido":

`` `html não embelezam
<p> Olá
<Li> Mom
<li> e
<Li> Dad
`` `

... Será um DOM normal, como o navegador ler as tags e restaura as partes que faltam:

<div class = "domtree"> </ div>

<script>
let node4 = {"name": "HTML", "nodeType": 1, "children": [{"name": "HEAD", "nodeType": 1, "children": []}, {"name" : "BODY", "nodeType": 1, "children": [{"name": "P", "nodeType": 1, "children": [{"name": "# text", "nodeType": 3, "content": "Hello"}]}, {"name": "LI", "nodeType": 1, "children": [{"name": "# text", "nodeType": 3, " conteúdo ":" Mãe "}]}, {" nome ":" LI "," nodeType ": 1," crianças ": [{" nome ":" # texto "," nodeType ": 3," conteúdo ": "e"}]}, {"name": "LI", "nodeType": 1, "children": [{"name": "# text", "nodeType": 3, "content": "Dad" }]}]}}]}

drawHtmlTree (node4, 'div.domtree', 690, 360);
</ script>

`` `` cabeçalho de aviso = "Tabelas sempre têm` <tbody> `"
Um interessante "caso especial" é tabelas. Com a especificação DOM, eles devem ter `<tbody>`, mas o texto HTML pode (oficialmente) omiti-lo. Em seguida, o navegador cria `<tbody>` em DOM automaticamente.

Para o HTML:

`` `html não embelezam
<table id = "table"> <tr> <td> 1 </ td> </ tr> </ table>
`` `

DOM-estrutura será:
<div class = "domtree"> </ div>

<script>
let node5 = {"name": "TABLE", "nodeType": 1, "children": [{"name": "TBODY", "nodeType": 1, "children": [{"name": "TR" "," nodeType ": 1," children ": [{" name ":" TD "," nodeType ": 1," children ": [{" name ":" # text "," nodeType ": 3," conteúdo ":" 1 "}]}]}]}}]}};

drawHtmlTree (node5, 'div.domtree', 600, 200);
</ script>

Entende? O `<tbody>` apareceu do nada. Deve ter em mente enquanto trabalha com tabelas para evadir surpresas.
`` ``

## Outros tipos de nó

Vamos adicionar mais tags e um comentário à página:

`` `html
<! DOCTYPE HTML>
<html>
<corpo>
A verdade sobre os alces.
<ol>
<li> Um elk é inteligente </ li>
*! *
<! - comment ->
* /! *
<li> ... e animal astúcia! </ li>
</ ol>
</ body>
</ html>
`` `

<div class = "domtree"> </ div>

<script>
Deixe node6 = {"name": "HTML", "nodeType": 1, "children": [{"name": "HEAD", "nodeType": 1, "children": []}, {"name" : "BODY", "nodeType": 1, "children": [{"name": "# text", "nodeType": 3, "content": "\ n A verdade sobre os alces. \ N"}, { "nome": "OL", "nodeType": 1, "children": [{"name": "# text", "nodeType": 3, "content": "\ n"}, {"name": "LI", "nodeType": 1, "children": [{"name": "# text", "nodeType": 3, "content": "An elk is a smart"}]}, {"name" : "# text", "nodeType": 3, "content": "\ n"}, {"name": "# comment", "nodeType": 8, "content": "comment"}, {"name ":" # text "," nodeType ": 3," content ":" \ n "}, {" name ":" LI "," nodeType ": 1," children ": [{" name ":" # texto "," nodeType ": 3," content ":" ... e astuto animal! "}]}, {" name ":" # text "," nodeType ": 3," content ":" \ n " }]}, {"name": "# text", "nodeType": 3, "content": "\ n \ n"}]}]}};

drawHtmlTree (node6, 'div.domtree', 690, 500);
</ script>

Aqui vemos um novo tipo de nó de árvore - * nó de comentário *, rotulado como `# comentário '.

Podemos pensar - por que um comentário é adicionado ao DOM? Isso não afeta a representação visual de forma alguma. Mas há uma regra - se algo estiver em HTML, então também deve estar na árvore DOM.

** Tudo em HTML, até mesmo comentários, torna-se parte do DOM. **

Mesmo a diretiva `<! DOCTYPE ...>` no início do HTML também é um nó DOM. Está na árvore DOM antes de `<html>`. Nós não vamos tocar nesse nó, nem nos desenhamos em diagramas por esse motivo, mas está lá.

O objeto `document` que representa todo o documento é, formalmente, um nó DOM também.

Existem [12 tipos de nó] (https://dom.spec.whatwg.org/#node). Na prática, geralmente trabalhamos com 4 deles:

1. `documento '- o" ponto de entrada "em DOM.
2. nós de elemento - tags HTML, os blocos de construção da árvore.
3. nós de texto - contém texto.
4. comentários - às vezes podemos colocar a informação lá, não será mostrado, mas JS pode lê-lo de DOM.

# Veja-se você mesmo

Para ver a estrutura DOM em tempo real, tente [Visualizador de DOM ao vivo] (http://software.hixie.ch/utilities/js/live-dom-viewer/). Basta digitar o documento e mostrará DOM no instante.

## No inspetor do navegador

Outra maneira de explorar DOM é usar ferramentas de desenvolvedor de navegador. Na verdade, é isso que usamos ao desenvolver.

Para fazer isso, abra a página da web [elks.html] (elks.html), ative as ferramentas do desenvolvedor do navegador e mude para a guia Elementos.

Deveria ficar assim:

!] (Elks.png)

Você pode ver o DOM, clicar em elementos, ver seus detalhes e assim por diante.

Observe que a estrutura DOM em ferramentas para desenvolvedores é simplificada. Os nós de texto são mostrados apenas como texto. E não há nós de texto "em branco" (apenas espaço). Está tudo bem, porque a maior parte do tempo estamos interessados ​​em nós de elementos.

Ao clicar no botão <span class = "devtools" style = "background-position: -328px -124px"> </ span> no canto superior esquerdo, permite escolher um nó da página usando um mouse (ou outro dispositivo ponteiro) e "inspecionar" (rolar para ele na guia de elementos). Funciona muito bem quando temos uma enorme página HTML e gostaríamos de ver os DOM de um lugar específico nele.

Outra maneira de fazê-lo seria apenas clicar com o botão direito do mouse em uma página da Web e selecionar "Inspecionar" no menu de contexto.

!] (inspect.png)

Na parte direita das ferramentas, existem subpasões a seguir:
- Estilos - podemos ver CSS aplicado à regra de elemento atual por regra, incluindo regras incorporadas (cinza). Quase tudo pode ser editado no local, incluindo as dimensões / margens / remendos da caixa abaixo.
- Calculado - para ver CSS aplicado ao elemento por propriedade: para cada propriedade, podemos ver uma regra que lhe dá (incluindo a herança CSS e tal).
- Ouvintes do Evento - para ver os ouvintes do evento anexados aos elementos do DOM (os abordaremos na próxima parte do tutorial).
- ...e assim por diante.

A melhor maneira de estudá-los é clicar em torno. A maioria dos valores são editáveis ​​no local.

## Interação com console

À medida que exploramos o DOM, também gostaríamos de aplicar o JavaScript a ele. Como: obter um nó e executar algum código para modificá-lo, para ver como ele se parece. Aqui estão algumas dicas para viajar entre a guia Elements e o console.

- Selecione o primeiro `<li>` na guia Elementos.
- Pressione `key: Esc` - abrirá o console logo abaixo da guia Elements.

Agora, o último elemento selecionado está disponível como "$ 0", o selecionado anteriormente é $ 1, etc.

Podemos executar comandos neles. Por exemplo, `$ 0.style.background = 'red'` faz o item da lista selecionado vermelho, assim:

! [] (domconsole0.png)

Do outro lado, se estivermos no console e tivermos uma variável referenciando um nó DOM, então podemos usar o comando `inspecionar (nó)` para vê-lo no painel Elementos.

Ou podemos apenas exibi-lo no console e explorar "at-place", como `document.body` abaixo:

!] [] (domconsole1.png)

Isso é para fins de depuração, é claro. No próximo capítulo, acessaremos e modificaremos DOM usando o JavaScript.

As ferramentas do desenvolvedor do navegador são uma grande ajuda no desenvolvimento: podemos explorar os DOM, tentar as coisas e ver o que dá errado.

## Resumo

Um documento HTML / XML é representado dentro do navegador como a árvore DOM.

- As tags se tornam nós de elemento e formam a estrutura.
- O texto se torna nó de texto.
- ... etc, tudo em HTML tem seu lugar em DOM, até mesmo comentários.

Podemos usar ferramentas de desenvolvedor para inspecionar DOM e modificá-lo manualmente.

Aqui cobrimos o básico, as ações mais utilizadas e importantes para começar. Existe uma extensa documentação sobre as ferramentas de desenvolvimento do Chrome em <https://developers.google.com/web/tools/chrome-devtools>. A melhor maneira de aprender as ferramentas é clicar aqui e aí, ler menus: a maioria das opções é óbvia. Mais tarde, quando você os conhece em geral, leia os documentos e retire o resto.

Os nós DOM têm propriedades e métodos que permitem viajar entre eles, modificar, mover a página e mais. Nós vamos chegar até eles nos próximos capítulos.
