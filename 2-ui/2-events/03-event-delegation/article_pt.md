
# Delegação de eventos

Capturar e borbulhar permitem implementar um dos padrões de manipulação de eventos mais poderosos, chamado * delegação de eventos *.

A idéia é que, se tivermos muitos elementos manipulados de forma semelhante, em vez de atribuir um manipulador a cada um deles - colocamos um único manipulador em seu antepassado comum.

No manipulador, recebemos `event.target`, veja onde o evento realmente aconteceu e lide com ele.

Vamos ver um exemplo - o [diagrama Ba-Gua] (http://en.wikipedia.org/wiki/Ba_gua) que reflete a filosofia chinesa antiga.

Aqui está:

[iframe height = 350 src = "bagua" edita o link]

O HTML é assim:

`` `html
<tabela>
<tr>
<th colspan = "3"> <em> Bagua </ em> Gráfico: Direção, Elemento, Cor, Significado </ th>
</ tr>
<tr>
<td> ... <strong> Northwest </ strong> ... </ td>
<Td> ... </ td>
<Td> ... </ td>
</ tr>
<tr> ... 2 linhas mais deste tipo ... </ tr>
<tr> ... 2 linhas mais deste tipo ... </ tr>
</ table>
`` `

A tabela tem 9 células, mas pode ser 99 ou 9999, não importa.

** Nossa tarefa é destacar uma célula `<td>` no clique. **

Em vez de atribuir um manipulador 'onclick` a cada `<td>` (pode ser muitos) - configuraremos o manipulador "catch-all" no elemento `<table>`.

Ele usará `event.target` para obter o elemento clicado e destacá-lo.

O código:

`` `js
deixe selecionadoTd;

*! *
table.onclick = function (event) {
deixe target = event.target; // onde foi o clique?

se (target.tagName! = 'TD') retornar; // não em TD? Então não estamos interessados

destaque (alvo); // destaque
};
* /! *

destaque da função (td) {
se (selectedTd) {// remover o destaque existente, se houver
selectedTd.classList.remove ('highlight');
}
selectedTd = td;
selectedTd.classList.add ('highlight'); // destaque o novo td
}
`` `

Tal código não diz respeito a quantas células existem na tabela. Podemos adicionar / remover `<td>` dinamicamente a qualquer momento e o destaque ainda funcionará.

Ainda assim, há uma desvantagem.

O clique pode ocorrer não no `<td>`, mas dentro dele.

No nosso caso, se olharmos dentro do HTML, podemos ver tags aninhadas dentro de `<td>`, como `<strong>`:

`` `html
<td>
*! *
<strong> Northwest </ strong>
* /! *
...
</ td>
`` `

Naturalmente, se ocorrer um clique nesse `<strong>`, então ele se torna o valor de `event.target`.

! [] (bagua-bubble.png)

No manipulador `table.onclick`, devemos ter" evento.target "e descobrir se o clique estava dentro de` <td> `ou não.

Aqui está o código melhorado:

`` `js
table.onclick = function (event) {
Deixe td = event.target.closest ('td'); // (1)

se (! td) retornar; // (2)

se (! table.contains (td)) retornar; // (3)

destaque (td); // (4)
};
`` `

Explicações:
1. O método `elem.closest (selector) 'retorna o antepassado mais próximo que corresponde ao seletor. No nosso caso, procuramos `<td>` no caminho do elemento de origem.
2. Se `event.target` não estiver dentro de qualquer` <td> `, a chamada retornará` null`, e não precisamos fazer nada.
3. No caso de tabelas aninhadas, `event.target` pode ser um` <td> `que se encontra fora da tabela atual. Então, verificamos se isso é realmente * nossa tabela * `<td>`.
4. E, se for assim, então destaque.

## Exemplo de delegação: ações na marcação

A delegação de eventos pode ser usada para otimizar o gerenciamento de eventos. Usamos um único manipulador para ações semelhantes em muitos elementos. Como fizemos para destacar `<td>`.

Mas também podemos usar um único manipulador como ponto de entrada para muitas coisas diferentes.

Por exemplo, queremos fazer um menu com os botões "Salvar", "Carregar", "Pesquisar" e assim por diante. E há um objeto com métodos `save`,` load`, `search` ....

A primeira idéia pode ser atribuir um manipulador separado a cada botão. Mas há uma solução mais elegante. Podemos adicionar um manipulador para todos os atributos do menu e da "ação de dados" para os botões que possuem o método a ser chamado:

`` `html
<botão *! * data-action = "save" * /! *> Clique para salvar </ button>
`` `

O manipulador lê o atributo e executa o método. Dê uma olhada no exemplo de trabalho:

`` `html autorun height = 60 run
<div id = "menu">
<button data-action = "save"> Salvar </ button>
<button data-action = "load"> Carregar </ button>
<button data-action = "search"> Pesquisa </ button>
</ div>

<script>
classe Menu {
construtor (elemento) {
this._elem = elemento;
elem.onclick = this.onClick.bind (this); // (*)
}

Salve () {
alerta ('salvar');
}

carga() {
alerta ('carregar');
}

pesquisa () {
alerta ('pesquisa');
}

onClick (evento) {
*! *
Deixe action = event.target.dataset.action;
se (ação) {
esta acção]();
}
* /! *
};
}

novo menu (menu);
</ script>
`` `

Observe que `this.onClick` está vinculado a` this` em `(*)`. Isso é importante porque, de outra forma, "isto" dentro dele faria referência ao elemento DOM (`elem`), não ao objeto do menu, e a" ação []] não seria o que precisamos.

Então, o que a delegação nos dá aqui?

`` `comparar
+ Não precisamos escrever o código para atribuir um manipulador a cada botão. Basta fazer um método e colocá-lo na marcação.
+ A estrutura HTML é flexível, podemos adicionar / remover botões a qualquer momento.
`` `

Poderíamos também usar classes `.action-save`,` .action-load`, mas um atributo `data-action` é melhor semanticamente. E também podemos usá-lo em regras CSS.

## O padrão de "comportamento"

Também podemos usar a delegação de eventos para adicionar "comportamentos" a elementos * declarativamente *, com atributos e classes especiais.

O padrão tem duas partes:
1. Nós adicionamos um atributo especial a um elemento.
2. Um manipulador de todo o documento rastreia eventos, e se um evento acontecer em um elemento atribuído - executa a ação.

### Contador

Por exemplo, aqui o atributo `data-counter` adiciona um comportamento:" aumentar em clique "para botões:

`` `html executar autorun height = 60
Contador: <tipo de entrada = "botão" valor = "1" contador de dados>
Mais um contador: <input type = "button" value = "2" data-counter>

<script>
document.addEventListener ('clique', função (evento) {

se (event.target.dataset.counter! = undefined) {// se o atributo existe ...
event.target.value ++;
}

});
</ script>
`` `

Se clicarmos em um botão - seu valor é aumentado. Não botões, mas a abordagem geral é importante aqui.

Pode haver tantos atributos com `data-counter` como desejamos. Podemos adicionar novos em HTML a qualquer momento. Usando a delegação de eventos nós "estendemos" HTML, adicionamos um atributo que descreve um novo comportamento.

`` `warn header =" Para manipuladores de nível de documento - sempre `addEventListener`"
Quando atribuímos um manipulador de eventos ao objeto `document`, sempre devemos usar` addEventListener`, e não `document.onclick`, porque o último causará conflitos: os novos manipuladores substituem os antigos.

Para projetos reais, é normal que haja vários manipuladores no `documento 'definidos por diferentes partes do código.
`` `

### Toggler

Mais um exemplo. Um clique em um elemento com o atributo `data-toggle-id` mostrará / ocultará o elemento com o` id 'fornecido:

`` `html execução automática altura = 60
<botão *! * data-toggle-id = "subscribe-mail" * /! *>
Mostre o formulário de inscrição
</ button>

<form id = "subscribe-mail" hidden>
Seu correio: <input type = "email">
</ form>

<script>
*! *
document.addEventListener ('clique', função (evento) {
Deixe id = event.target.dataset.toggleId;
se (! id) retornar;

deixe elem = document.getElementById (id);

element.hidden =! element.hidden;
});
* /! *
</ script>
`` `

Vamos notar mais uma vez o que fizemos. Agora, para adicionar a funcionalidade de alternância a um elemento - não há necessidade de saber o JavaScript, basta usar o atributo `data-toggle-id`.

Isso pode se tornar realmente conveniente - não há necessidade de escrever JavaScript para cada elemento. Apenas use o comportamento. O manipulador de nível de documento faz com que funcione para qualquer elemento da página.

Podemos também combinar comportamentos múltiplos em um único elemento.

O padrão de "comportamento" pode ser uma alternativa de mini-fragmentos de JavaScript.

## Resumo

A delegação do evento é muito legal! É um dos padrões mais úteis para os eventos DOM.

Muitas vezes é usado para adicionar o mesmo tratamento para muitos elementos semelhantes, mas não só para isso.

O algoritmo:

1. Coloque um único manipulador no recipiente.
2. No manipulador - verifique o elemento de origem `event.target`.
3. Se o evento aconteceu dentro de um elemento que nos interessa, em seguida, lida com o evento.

Benefícios:

`` `comparar
+ Simplifica a inicialização e poupa memória: não há necessidade de adicionar muitos manipuladores.
+ Menos código: ao adicionar ou remover elementos, não é necessário adicionar / remover manipuladores.
+ Modificações DOM: podemos adicionar / remover elementos em massa com `innerHTML` e similares.
`` `

A delegação tem suas limitações, é claro:

`` `comparar
- Primeiro, o evento deve estar borbulhando. Alguns eventos não fazem bolhas. Além disso, manipuladores de baixo nível não devem usar `event.stopPropagation ()`.
- Em segundo lugar, a delegação pode adicionar carga de CPU, porque o manipulador de nível de contêiner reage em eventos em qualquer lugar do recipiente, não importando se eles nos interessam ou não. Mas, geralmente, a carga é insignificante, por isso não consideramos isso.
`` `
