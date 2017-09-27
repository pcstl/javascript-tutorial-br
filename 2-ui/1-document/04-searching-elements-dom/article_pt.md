# Pesquisando: getElement * e querySelector *

As propriedades de navegação DOM são ótimas quando os elementos estão próximos uns dos outros. E se não o fizerem? Como obter um elemento arbitrário da página?

Existem métodos de pesquisa adicionais para isso.
[cortar]

## document.getElementById ou apenas id

Se um elemento tiver o atributo `id`, então há uma variável global pelo nome daquela` id`.

Podemos usá-lo para acessar o elemento, assim:

`` `html run
<div id = "*! * element * /! *">
<div id = "*! * elem-content * /! *"> Element </ div>
</ div>

<script>
alerta (elemento); // elemento DOM com id = "elemento"
alerta (window.elem); // acessando uma variável global como esta também funciona

// para elem-content as coisas são um pouco mais complexas
// que tem um traço para dentro, por isso não pode ser um nome de variável
alerta (janela ['elem-content']); // ... mas acessível usando colchetes [...]
</ script>
`` `

Isso é a menos que declaremos a mesma variável nomeada pela nossa:

`` `html executar alturas não confiáveis ​​= 0
<div id = "element"> </ div>

<script>
deixe elemento = 5;

alerta (elem); // a variável substitui o elemento
</ script>
`` `

O comportamento é descrito [na especificação] (http://www.whatwg.org/specs/web-apps/current-work/#dom-window-nameditem), mas é suportado principalmente pela compatibilidade. O navegador tenta nos ajudar, misturando namespaces de JS e DOM. Bom para scripts muito simples, mas pode haver conflitos de nomes. Além disso, quando olhamos em JS e não temos HTML em vista, não é óbvio de onde a variável vem.

A melhor alternativa é usar um método especial `document.getElementById (id)`.

Por exemplo:

`` `html run
<div id = "element">
<div id = "elem-content"> Elemento </ div>
</ div>

<script>
*! *
Deixe element = document.getElementById ('elemento');
* /! *

elem.style.background = 'red';
</ script>
`` `

Aqui, no tutorial, usamos frequentemente id para referenciar diretamente um elemento, mas isso é apenas para manter as coisas curtas. Na vida real, `document.getElementById` é o método preferido.

`` `cabeçalho inteligente =" Pode haver apenas um "
O `id` deve ser exclusivo. Pode haver apenas um elemento no documento com o dado `id`.

Se houver vários elementos com o mesmo "id", o comportamento dos métodos correspondentes é imprevisível. O navegador pode retornar qualquer um deles ao acaso. Então, fique com a regra e mantenha o "id" único.
`` `

`` `warn header =" Somente `document.getElementById`, não` anyNode.getElementById` "
O método `getElementById` que pode ser chamado apenas no objeto 'documento'. Ele procura o dado 'id` no documento inteiro.
`` `

## getElementsBy *

Existem também outros métodos para procurar nós:

- `elem.getElementsByTagName (tag)` procura por elementos com a tag fornecida e retorna a coleção deles. O parâmetro `tag` também pode ser uma estrela` `*" `para" todas as tags ".

Por exemplo:
`` `js
// obtenha todas as divs no documento
deixe divs = document.getElementsByTagName ('div');
`` `

Esse método é chamado no contexto de qualquer elemento DOM.

Vamos encontrar todas as "entradas" dentro da tabela:

`` `html run height = 50
<table id = "table">
<tr>
<td> Sua idade: </ td>

<td>
<label>
<input type = "radio" name = "age" value = "young" verificado> menos de 18
</ label>
<label>
<input type = "radio" name = "age" value = "mature"> de 18 para 50
</ label>
<label>
<input type = "radio" name = "age" value = "senior"> mais de 60
</ label>
</ td>
</ tr>
</ table>

<script>
*! *
deixe inputs = table.getElementsByTagName ('input');
* /! *

para (deixe entrada de entradas) {
alerta (input.value + ':' + input.checked);
}
</ script>
`` `

`` `warn header =" Não esqueça a letra `\" s \ "`! "
Novos desenvolvedores às vezes esquecem a letra `` s ''. Ou seja, eles tentam chamar `getElementsByTagName` ao invés de <code> getElementbyid <b> s </ b> TagName </ code>.

A letra `` s '' está ausente no `getElementById`, porque retorna um único elemento. Mas `getElementsByTagName` retorna uma coleção de elementos, então há '` s' 'dentro.
`` `

`` `` warn header = "Ele retorna uma coleção, não um elemento!"
Outro erro novato generalizado é escrever como:

`` `js
// não funciona
document.getElementsByTagName ('input'). value = 5;
`` `

Isso não funcionará, porque é preciso uma * coleção * de insumos e atribui o valor a ele, em vez de elementos dentro dele.

Devemos alternar entre a coleção ou obter um elemento pelo número, e depois atribuir, assim:

`` `js
// deve funcionar (se houver uma entrada)
document.getElementsByTagName ('input') [0] .value = 5;
`` `
`` ``

Existem também outros métodos raramente usados ​​desse tipo:

- `elem.getElementsByClassName (className)` retorna elementos que possuem a classe CSS fornecida. Os elementos também podem ter outras classes.
- `document.getElementsByName (name)` retorna elementos com o atributo `name` atribuído, em todo o documento. Existe por razões históricas, muito raramente usadas, mencionamos aqui apenas por completo.

Por exemplo:

`` `html run height = 50
<form name = "my-form">
<div class = "article"> Artigo </ div>
<div class = "long article"> Artigo longo </ div>
</ form>

<script>
// atributo find by name
Deixe form = document.getElementsByName ('my-form') [0];

// encontre por classe dentro do formulário
deixe articles = form.getElementsByClassName ('artigo');
alerta (articles.length); // 2, encontrou dois elementos com classe "artigo"
</ script>
`` `

## querySelectorAll [#querySelectorAll]

Agora vai a artilharia pesada.

A chamada para `elem.querySelectorAll (css)` retorna todos os elementos dentro de `elem` correspondentes ao seletor CSS fornecido. Esse é o método mais utilizado e poderoso.

Aqui procuramos todos os elementos `<li>` que são os últimos filhos:

`` `html run
<Ul>
<li> O </ li>
<> Teste-los </ li>
</ Ul>
<Ul>
<Li> tem </ li>
<li> passou </ li>
</ Ul>
<script>
*! *
Deixe elements = document.querySelectorAll ('ul> li: last-child');
* /! *

para (deixe elem de elementos) {
alerta (elem.innerHTML); // "teste", "passado"
}
</ script>
`` `

Esse método é realmente poderoso, pois qualquer seletor CSS pode ser usado.

`` `smart header =" Pode usar pseudo-classes também "
Pseudo-classes no seletor CSS como `: hover` e`: active` também são suportadas. Por exemplo, `document.querySelectorAll (': hover')` retornará a coleção com elementos que o ponteiro acabou agora (na ordem de aninhamento: do `<html>` mais externo para o mais aninhado).
`` `


## querySelector [#querySelector]

A chamada para `elem.querySelector (css)` retorna o primeiro elemento para o seletor CSS fornecido.

Em outras palavras, o resultado é o mesmo que `elem.querySelectorAll (css) [0]`, mas o último está procurando por * all * elements e escolhendo um, enquanto `elem.querySelector 'apenas procura um. Então, é mais rápido e mais curto para escrever.

## fósforos

Métodos anteriores estavam buscando o DOM.

O [elem.matches (css)] (http://dom.spec.whatwg.org/#dom-element-matches) não procura nada, ele apenas verifica se `elem` corresponde ao determinado CSS-selector. Retorna `true` ou` false`.

O método é útil quando estamos iterando sobre elementos (como em matriz ou algo assim) e tentando filtrar aqueles que nos interessam.

Por exemplo:

`` `html run
<a href="http://example.com/file.zip"> ... </a>
<a href="http://ya.ru"> ... </a>

<script>
// pode ser qualquer coleção em vez de document.body.children
para (deixe elem de document.body.children) {
*! *
se (elem.matches ('a [href $ = "zip"]')) {
* /! *
alerta ("A referência do arquivo:" + elem.href);
}
}
</ script>
`` `

## mais próximo

Todos os elementos que estão diretamente acima do dado são chamados de * antepassados ​​*.

Em outras palavras, os antepassados ​​são: pai, pai do pai, pai pai e assim por diante. Os antepassados ​​juntos formam a cadeia de pais do elemento para o topo.

O método `elem.closest (css) 'parece o antepassado mais próximo que corresponde ao seletor CSS. O `elem` em si também está incluído na pesquisa.

Em outras palavras, o método "mais próximo" sobe do elemento e verifica cada um dos pais. Se corresponder ao seletor, a busca pára e o antepassado é retornado.

Por exemplo:

`` `html run
<h1> Conteúdo </ h1>

<div class = "contents">
<ul class = "book">
<li class = "chapter"> Capítulo 1 </ li>
<li class = "chapter"> Capítulo 1 </ li>
</ Ul>
</ div>

<script>
Deixe chapter = document.querySelector ('. chapter'); // LI

alerta (chapter.closest ('. book')); // UL
alerta (chapter.closest ('. contents')); // DIV

alerta (chapter.closest ('h1')); // nulo (porque h1 não é um antepassado)
</ script>
`` `

## Live collections

Todos os métodos `" getElementsBy * "retornam uma coleção * ao vivo *. Tais coleções sempre refletem o estado atual do documento e "atualização automática" quando ele muda.

No exemplo abaixo, existem dois scripts.

1. O primeiro cria uma referência à coleção de `<div>`. A partir de agora, o comprimento é `1`.
2. O segundo script é executado depois que o navegador encontrar um mais `<div>`, então o comprimento é `2`.

`` `html run
<div> Primeira div </ div>

<script>
deixe divs = document.getElementsByTagName ('div');
alerta (divs.length); // 1
</ script>

<div> Segunda div </ div>

<script>
*! *
alerta (divs.length); // 2
* /! *
</ script>
`` `

Em contraste, `querySelectorAll` retorna uma coleção * static *. É como uma série fixa de elementos.

Se usarmos, em vez disso, ambos os scripts produzem `1`:


`` `html run
<div> Primeira div </ div>

<script>
deixe divs = document.querySelectorAll ('div');
alerta (divs.length); // 1
</ script>

<div> Segunda div </ div>

<script>
*! *
alerta (divs.length); // 1
* /! *
</ script>
`` `

Agora podemos ver facilmente a diferença. A coleta estática não aumentou após a aparição de um novo `div` no documento.

Aqui, usamos scripts separados para ilustrar como a adição de elemento afeta a coleção, mas todas as manipulações DOM as afetam. Em breve veremos mais deles.

## Resumo

Existem 6 métodos principais para procurar por nós no DOM:

<tabela>
<thead>
<tr>
<td> Método </ td>
<td> Pesquisas por ... </ td>
<td> Pode chamar um elemento? </ td>
<td> Live? </ td>
</ tr>
</ thead>
<tbody>
<tr>
<td> <code> getElementById </ code> </ td>
<td> <code> id </ code> </ td>
<Td> - </ td>
<Td> - </ td>
</ tr>
<tr>
<td> <code> getElementsByName </ code> </ td>
<td> <code> nome </ code> </ td>
<Td> - </ td>
<Td> ✔ </ td>
</ tr>
<tr>
<td> <code> getElementsByTagName </ code> </ td>
<td> tag ou <code> '*' </ code> </ td>
<Td> ✔ </ td>
<Td> ✔ </ td>
</ tr>
<tr>
<td> <code> getElementsByClassName </ code> </ td>
<td> classe </ td>
<Td> ✔ </ td>
<Td> ✔ </ td>
</ tr>
<tr>
<td> <code> querySelector </ code> </ td>
<td> Seletor CSS </ td>
<Td> ✔ </ td>
<Td> - </ td>
</ tr>
<tr>
<td> <code> querySelectorAll </ code> </ td>
<td> Seletor CSS </ td>
<Td> ✔ </ td>
<Td> - </ td>
</ tr>
</ tbody>
</ table>

Tenha em atenção que os métodos `getElementById` e` getElementsByName` só podem ser chamados no contexto do documento: `document.getElementById (...)`. Mas não em um elemento: `elem.getElementById (...)` causaria um erro.

Outros métodos também podem ser chamados de elementos. Por exemplo `elem.querySelectorAll (...)` pesquisará dentro de `elem` (na subárvore DOM).

Além disso:

- Há `element.matches (css)` para verificar se `element` corresponde ao seletor CSS fornecido.
- Há `elem.closest (css)` para procurar um antepassado mais próximo que corresponda ao determinado CSS-selector. O `elem` em si também é verificado.

E vamos mencionar um método mais aqui para verificar a relação filho-pai:
- `elenA.contains (elemB)` retorna true se `elemENT` estiver dentro de` elenA` (um descendente de `elemA`) ou quando` elemA == elemB`.
