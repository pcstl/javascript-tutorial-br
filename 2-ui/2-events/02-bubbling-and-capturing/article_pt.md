# Bubbling e captura

Vamos começar com um exemplo.

Este manipulador é atribuído a `<div>`, mas também é executado se você clicar em qualquer tag aninhada como `<em>` ou `<code>`:

`` `html autorun height = 60

<em> Se você clicar em <code> EM </ code>, o manipulador em <code> DIV </ code> é executado. </ em>
</ div>
`` `

Não é um pouco estranho? Por que o manipulador em `<div>` é executado se o clique real foi em `<em>`?

# Bubbling

O princípio de borbulhar é simples.

** Quando um evento acontece em um elemento, ele primeiro executa os manipuladores sobre ele, depois em seu pai, então todo o caminho até outros antepassados. **

Digamos que temos 3 elementos aninhados `FORM> DIV> P` com um manipulador em cada um deles:

`` `html execute o autorun
<style>
corpo * {
margem: 10px;
borda: 1px sólido azul;
}
</ style>




</ div>
</ form>
`` `

Um clique no interior `<p>` primeiro executa 'onclick`:
1. Sobre isso `<p>`.
2. Então no exterior `<div>`.
3. Então, no exterior `<form>`.
4. E assim por diante até o objeto `document`.

! [] (event-order-bubbling.png)

Então, se clicarmos em `<p>`, veremos 3 alertas: `p` ->` div` -> `form`.

O processo é chamado de "borbulhar", porque os eventos "borbulham" do elemento interno através de pais como uma bolha na água.

`` `warn header =" * Quase * todos os eventos borbulham ".
A palavra-chave nesta frase é "quase".

Por exemplo, um evento `focus` não faz bolhas. Há outros exemplos também, vamos encontrá-los. Mas ainda é uma exceção, em vez de uma regra, a maioria dos eventos faz bolhas.
`` `

## event.target

Um manipulador em um elemento pai sempre pode obter os detalhes sobre onde realmente aconteceu.

** O elemento mais profundamente aninhado que causou o evento é chamado de elemento * target *, acessível como `event.target`. **

Observe as diferenças de `this` (=` event.currentTarget`):

- `event.target` - é o elemento" alvo "que iniciou o evento, ele não muda através do processo de borbulhamento.
- `this` - é o elemento" atual ", aquele que possui um manipulador atualmente executado nela.

Por exemplo, se nós possuímos um único formador `form.onclick`, então ele pode" capturar "todos os cliques dentro do formulário. Não importa onde o clique aconteceu, ele faz borbulhar até `<form>` e executa o manipulador.

No manipulador `form.onclick`:

- `this` (` = event.currentTarget`) é o elemento `<form>`, porque o manipulador é executado nele.
- `event.target` é o elemento concreto dentro do formulário que realmente foi clicado.

Confira:

[codetabs height = 220 src = "bubble-target"]

É possível que `event.target` seja igual a` this` - quando o clique é feito diretamente no elemento `<form>`.

## Parando borbulhando

Um evento borbulhante vai do elemento alvo diretamente para cima. Normalmente, ele vai até `<html>` e, em seguida, para 'documentar' o objeto, e alguns eventos até chegam a 'janela', chamando todos os manipuladores no caminho.

Mas qualquer manipulador pode decidir que o evento foi totalmente processado e para a borbulhar.

O método para isso é `event.stopPropagation ()`.

Por exemplo, aqui `body.onclick` não funciona se você clicar em` <button> `:

`` `html executar autorun height = 60

<button onclick = "event.stopPropagation ()"> Clique em mim </ button>
</ body>
`` `

`` `smart header =" event.stopImmediatePropagation () "
Se um elemento tiver vários manipuladores de eventos em um único evento, mesmo que um deles pare a borbulhar, os outros ainda são executados.

Em outras palavras, `event.stopPropagation ()` pára o movimento para cima, mas no elemento atual todos os outros manipuladores serão executados.

Para impedir que os manipuladores de borbulhamento e eventos no elemento atual sejam executados, há um método `event.stopImmediatePropagation ()`. Depois disso, nenhum outro manipulador executa.
`` `

`` `warn header =" Não pare de borbulhar sem necessidade! "
Bubbling é conveniente. Não o detenha sem uma necessidade real: óbvia e arquitetonicamente bem pensada.

Às vezes, `event.stopPropagation ()` cria armadilhas ocultas que mais tarde podem se tornar problemas.

Por exemplo:

1. Criamos um menu aninhado. Cada submenu lida com cliques em seus elementos e chama `stopPropagation` para que o menu externo não acione.
2. Mais tarde, decidimos capturar cliques em toda a janela, para rastrear o comportamento dos usuários (onde as pessoas clicam). Alguns sistemas analíticos fazem isso. Geralmente, o código usa `document.addEventListener ('clique' ...)` para capturar todos os cliques.
3. Nossa análise não funcionará na área em que os cliques são interrompidos por `stopPropagation`. Temos uma "zona morta".

Geralmente não há necessidade real de evitar a borbulhar. Uma tarefa que aparentemente exige que possa ser resolvida por outros meios. Um deles é usar eventos personalizados, vamos cobri-los mais tarde. Também podemos escrever nossos dados no objeto `event` em um manipulador e lê-lo em outro, para que possamos passar para manipuladores sobre a informação dos pais sobre o processamento abaixo.
`` `


# Capturar

Há outra fase de processamento de eventos chamada "capturar". Raramente é usado em código real, mas às vezes pode ser útil.

O padrão [Eventos DOM] (http://www.w3.org/TR/DOM-Level-3-Events/) descreve 3 fases de propagação de eventos:

1. Fase de captura - o evento passa para o elemento.
2. Fase alvo - o evento atingiu o elemento alvo.
3. Fase de borbulhação - o evento borbulha do elemento.

Aqui está a imagem de um clique em `<td>` dentro de uma tabela, tirada da especificação:

!] [eventflow.png]

Isto é: para um clique no `<td>` o evento primeiro passa pela cadeia dos antepassados ​​até o elemento (captura), então ele atinge o alvo, e então ele sobe (bolhas), chamando os manipuladores no caminho.

** Antes de falar apenas sobre bolhas, porque a fase de captura raramente é usada. Normalmente, é invisível para nós. **

Os manipuladores adicionados usando `on <event>` -property ou usando atributos HTML ou usando `addEventListener (evento, manipulador)` não sabem nada sobre a captura, eles só são executados nas 2ª e 3ª fases.

Para pegar um evento na fase de captura, precisamos configurar o terceiro argumento de `addEventListener` para` true`.

Existem dois valores possíveis para esse último argumento opcional:

- Se for "falso" (padrão), o manipulador está configurado na fase de borbulhamento.
- Se é `true`, o manipulador está configurado na fase de captura.

Observe que, enquanto formalmente, existem 3 fases, a 2ª fase ("fase-alvo": o evento atingiu o elemento) não é tratada separadamente: manipuladores nas fases de captura e borbulhamento desencadeiam nessa fase.

Se alguém colocar manipuladores de captura e borbulhação no elemento de destino, o manipulador de captura desencadeia o último na fase de captura e o manipulador de bolhas dispara primeiro na fase de borbulhamento.

Vamos ver isso em ação:

`` `html execute autorun height = 140 edit
<style>
corpo * {
margem: 10px;
borda: 1px sólido azul;
}
</ style>

Formulário <form>
<Div> DIV
<P> P </ p>
</ div>
</ form>

<script>
para (deixe elem de document.querySelectorAll ('*')) {
elem.addEventListener ("clique", e => alerta (`Capturar: $ {elem.tagName}`), true);
elem.addEventListener ("clique", e => alerta (`Bubbling: $ {elem.tagName}`));
}
</ script>
`` `

O código define manipuladores de cliques em * cada * elemento no documento para ver quais estão funcionando.

Se você clicar em `<p>`, então a seqüência é:

1. `HTML` ->` BODY` -> `FORM` ->` DIV` -> `P` (fase de captura, o primeiro ouvinte) e, em seguida,:
2. `P` ->` DIV` -> `FORM` ->` BODY` -> `HTML` (fase de borbulhamento, o segundo ouvinte).

Por favor, note que `P` aparece duas vezes: no final da captura e no início da borbulhação.

Existe uma propriedade `event.eventPhase` que nos informa o número da fase em que o evento foi capturado. Mas raramente é usado, porque geralmente o conhecemos no manipulador.

## Resumo

O processo de tratamento de eventos:

- Quando ocorre um evento - o elemento mais aninhado onde acontece é rotulado como o "elemento alvo" (`event.target`).
- Então o evento primeiro se move da raiz do documento para baixo do `event.target`, manipuladores de chamadas atribuídos com` addEventListener (...., true) `no caminho.
- Então o evento se move de `event.target` para a raiz, manipuladores de chamadas atribuídos usando` on <event> `e` addEventListener` sem o 3º argumento ou com o 3º argumento `false`.

Cada manipulador pode acessar as propriedades do objeto "evento":

- `event.target` - o elemento mais profundo que originou o evento.
- `event.currentTarget` (=` this`) - o elemento atual que lida com o evento (aquele que possui o manipulador)
- `event.eventPhase` - a fase atual (captura = 1, borbulhando = 3).

Qualquer manipulador de eventos pode parar o evento chamando `event.stopPropagation ()`, mas isso não é recomendado, porque não podemos realmente ter certeza de que não precisamos disso acima, talvez por coisas completamente diferentes.

A fase de captação é usada muito raramente, geralmente lidamos com eventos em borbulhamento. E há uma lógica por trás disso.

No mundo real, quando ocorre um acidente, as autoridades locais reagem primeiro. Eles conhecem melhor a área onde aconteceu. Então, autoridades de nível superior, se necessário.

O mesmo para os manipuladores de eventos. O código que define o manipulador em um elemento específico conhece os detalhes máximos sobre o elemento e o que ele faz. Um manipulador em um `<td>` específico pode ser adequado para isso exatamente `<td>`, ele sabe tudo sobre isso, então deve ter a chance primeiro. Então, seu pai imediato também conhece o contexto, mas um pouco menos, e assim por diante até o elemento superior que lida com conceitos gerais e executa o último.

A borbulhar e capturar as bases para a "delegação de eventos" - um padrão de manipulação de eventos extremamente poderoso que estudamos no próximo capítulo.
