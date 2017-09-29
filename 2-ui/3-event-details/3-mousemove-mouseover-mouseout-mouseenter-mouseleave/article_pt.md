# Movendo: mouseover / out, mouseenter / leave

Vamos mergulhar em mais detalhes sobre eventos que acontecem quando o mouse se move entre os elementos.

[cortar]

## Mouseover / mouseout, relatedTarget

O evento `mouseover` ocorre quando um ponteiro do mouse vem sobre um elemento e` mouseout` - quando ele sai.

!] [] (mouseover-mouseout.png)

Esses eventos são especiais, porque eles têm um `relatedTarget`.

Para `mouseover`:

- `event.target` - é o elemento onde o mouse apareceu.
- `event.relatedTarget` - é o elemento do qual o mouse veio.

Para `mouseout` no reverso:

- `event.target` - é o elemento que sai do mouse.
- `event.relatedTarget` - é o novo elemento sob o ponteiro (para o mouse).

`` `online
No exemplo abaixo, cada recurso de rosto é um elemento. Quando você move o mouse, você pode ver os eventos do mouse na área de texto.

Cada evento tem a informação sobre onde o elemento veio e de onde veio.

[codetabs src = "mouseoverout" height = 280]
`` `

`` `warn header =" `relatedTarget` pode ser 'null`"
A propriedade `relatedTarget` pode ser 'nula'.

Isso é normal e significa que o mouse não veio de outro elemento, mas de fora da janela. Ou que deixou a janela.

Devemos ter essa possibilidade em mente ao usar `event.relatedTarget` em nosso código. Se acessarmos `event.relatedTarget.tagName`, então haverá um erro.
`` `

## Frequência de eventos

O evento `mousemove` dispara quando o mouse se move. Mas isso não significa que cada pixel leva a um evento.

O navegador verifica a posição do mouse ocasionalmente. E, se perceber alterações, desencadeia os eventos.

Isso significa que, se o visitante estiver movendo o mouse muito rápido, os elementos DOM podem ser ignorados:

! [] (mouseover-mouseout-over-elems.png)

Se o mouse se mover muito rápido dos elementos `# FROM` para` # TO` como pintado acima, então o intermediário `<div>` (ou alguns deles) pode ser ignorado. O evento `mouseout` pode disparar em` # FROM` e, em seguida, "mouseover" no `# TO '.

Na prática, isso é útil, porque se houver muitos elementos intermediários. Nós realmente não queremos processar dentro e fora de cada um.

Do outro lado, devemos ter em mente que não podemos assumir que o mouse se move lentamente de um evento para outro. Não, ele pode "pular".

Em particular, é possível que o cursor salte para dentro do meio da página de fora da janela. E `relatedTarget = null`, porque veio de" nenhum lugar ":

!] [] (mouseover-mouseout-from-outside.png)

<div style = "display: none">
No caso de um movimento rápido, elementos intermediários podem desencadear eventos. Mas se o mouse entrar no elemento (`mouseover`), quando é garantido ter` mouseout` quando o deixa.
</ div>

`` `online
Confira "ao vivo" em um teste abaixo.

O HTML é dois elementos `<div>` aninhados. Se você mover o mouse rapidamente sobre eles, então pode não haver nenhum evento, ou talvez apenas o div vermelho desencadeia eventos, ou talvez o verde.

Tente também mover o ponteiro sobre o `div` vermelho e, em seguida, movê-lo rapidamente para baixo através do verde. Se o movimento for rápido o suficiente, o elemento pai será ignorado.

[codetabs height = 360 src = "mouseoverout-fast"]
`` `

## "Extra" mouseout quando sair para uma criança

Imagine - um ponteiro do mouse inseriu um elemento. O "mouseover" disparou. Em seguida, o cursor passa para um elemento filho. O fato interessante é que o mouseout se desencadeia nesse caso. O cursor ainda está no elemento, mas temos um "mouseout" dele!

!] [] (mouseover-to-child.png)

Isso parece estranho, mas pode ser facilmente explicado.

** De acordo com a lógica do navegador, o cursor do mouse pode ser apenas sobre um elemento * single * a qualquer momento - o mais aninhado (e top by z-index). **

Então, se ele for para outro elemento (mesmo um descendente), então ele deixa o anterior. Que simples.

Há uma consequência engraçada que podemos ver no exemplo abaixo.

O vermelho `<div>` está aninhado dentro do azul. O azul `<div>` tem manipuladores `mouseover / out` que registram todos os eventos na área de texto abaixo.

Tente entrar no elemento azul e, em seguida, movendo o mouse para o vermelho - e assista os eventos:

[codetabs height = 360 src = "mouseoverout-child"]

1. Ao entrar no azul - nós conseguimos `mouseover [target: blue]`.
2. Então, depois de passar do azul para o vermelho - nós recebemos `mouseout [target: blue]` (deixou o pai).
3. ... E imediatamente `mouseover [target: red]`.

Então, para um manipulador que não leva o `target` em conta, parece que nós deixamos o pai no 'mouseout' em` (2) `e devolvido de volta por` mouseover` em `(3)`.

Se realizarmos algumas ações ao entrar / sair do elemento, então teremos muitas corridas "falsas" extras. Para coisas simples, pode ser imperceptível. Para coisas complexas que podem trazer efeitos colaterais indesejados.

Podemos corrigi-lo usando os eventos `mouseenter / mouseleave` em vez disso.

## Eventos mouseenter e mouseleave

Os eventos `mouseenter / mouseleave` são como` mouseover / mouseout`. Eles também disparam quando o ponteiro do mouse entra / sai do elemento.

Mas há duas diferenças:

1. As transições dentro do elemento não são contadas.
2. Os eventos `mouseenter / mouseleave` não fazem bolhas.

Esses eventos são intuitivamente muito claros.

Quando o ponteiro entra em um elemento - o `mouseenter` desencadeia, e então não importa onde ele vai enquanto dentro do elemento. O evento `mouseleave` só dispara quando o cursor o deixa.

Se formos o mesmo exemplo, coloque `mouseenter / mouseleave` no azul` <div> `, e faça o mesmo - podemos ver que os eventos se desencadeiam apenas ao entrar e sair do azul` <div> `. Não há eventos extras quando vai para o vermelho e para trás. As crianças são ignoradas.

[codetabs height = 340 src = "mouseleave"]

## Event delegation

Os eventos `mouseenter / leave` são muito simples e fáceis de usar. Mas eles não borbulham. Portanto, não podemos usar a delegação de eventos com eles.

Imagine que queremos manipular o mouse enter / leave para as células da tabela. E há centenas de células.

A solução natural seria - configurar o manipulador em `<tabela>` e processar eventos lá. Mas `mouseenter / leave` não faz bolhas. Então, se esse evento acontecer em `<td>`, então apenas um manipulador nesse `<td>` pode pegá-lo.

Os manipuladores para `mouseenter / leave` em` <table> `apenas acionam a entrada / saída de toda a tabela. É impossível obter qualquer informação sobre transições dentro dele.

Não é problema - vamos usar `mouseover / mouseout`.

Um manipulador simples pode ser assim:

`` `js
// vamos destacar as células no mouse
table.onmouseover = função (evento) {
deixe target = event.target;
target.style.background = 'pink';
};

table.onmouseout = function (event) {
deixe target = event.target;
target.style.background = '';
};
`` `

`` `online
[codetabs height = 480 src = "mouseenter-mouseleave-delegation"]
`` `

Esses manipuladores funcionam quando vão de qualquer elemento a qualquer dentro da tabela.

Mas gostaríamos de lidar apenas com transições dentro e fora de `<td>` como um todo. E destacar as células como um todo. Não queremos lidar com as transições que acontecem entre os filhos de `<td>`.

Uma das soluções:

- Lembre-se do `<td>` atualmente com destaque em uma variável.
- No `mouseover` - ignore o evento se ainda estivéssemos dentro do` `td`` atual.
- No `mouseout` - ignore se não deixamos o atual` <td> `.

Isso filtra os eventos "extras" quando nos movemos entre os filhos de `<td>`.

`` `desconectado
Os detalhes estão no [exemplo completo] (sandbox: mouseenter-mouseleave-delegation-2).
`` `

`` `online
Aqui está o exemplo completo com todos os detalhes:

[codetabs height = 380 src = "mouseenter-mouseleave-delegation-2"]

Tente mover o cursor para dentro e para fora das células da tabela e dentro delas. Rápido ou lento - não é melhor. Somente `<td>` como um todo é destacado ao contrário do exemplo anterior.
`` `


## Resumo

Cobrimos eventos `mouseover`,` mouseout`, `mousemove`,` mouseenter` e `mouseleave`.

Coisas que são boas de notar:

- Um movimento rápido do mouse pode fazer 'mouseover, mousemove, mouseout` para ignorar elementos intermediários.
- Eventos `mouseover / out` e` mouseenter / leave` têm um alvo adicional: `relatedTarget`. Esse é o elemento do qual viemos / para, complementar a `target`.
- O evento 'mouseover / out` desencadeia mesmo quando passamos do elemento pai para um elemento filho. Eles assumem que o mouse pode ser apenas mais de um elemento ao mesmo tempo - o mais profundo.
- Eventos `mouseenter / leave` não borbulham e não se acionam quando o mouse passa para um elemento filho. Eles apenas rastreiam se o mouse vem dentro e fora do elemento como um todo.
