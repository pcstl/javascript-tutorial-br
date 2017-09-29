# Focalização: foco / borrão

Um elemento recebe um foco quando o usuário clica nele ou usa a tecla 'Chave: Tab' no teclado. Há também um atributo HTML "Autofocus" que coloca o foco em um elemento por padrão quando uma página é carregada e outros meios para obter um foco.

O foco geralmente significa: "prepare-se para aceitar os dados aqui", então é o momento em que podemos executar o código para inicializar ou carregar algo.

O momento de perder o foco ("borrão") pode ser ainda mais importante. Isso é quando um usuário clica em outro lugar ou pressiona `chave: Tab` para ir ao próximo campo de formulário, ou também há outros meios.

Perder o foco geralmente significa: "os dados foram inseridos", para que possamos executar o código para verificá-lo ou mesmo para salvá-lo no servidor e assim por diante.

Existem importantes peculiaridades quando se trabalha com eventos de foco. Faremos o melhor para cobri-los aqui.

[cortar]

## Eventos foco / borrão

O evento `focus` é chamado de foco e` blurry` - quando o elemento perde o foco.

Vamos usá-los para validação de um campo de entrada.

No exemplo abaixo:

- O manipulador `blur` verifica se o campo que o email está inserido e, se não, mostra um erro.
- O manipulador `focus` esconde a mensagem de erro (no` borrão 'será verificado novamente):

`` `html executar autorun height = 60
<style>
.invalid {border-color: red; }
#error {color: red}
</ style>

Seu email, por favor: <input type = "email" id = "input">

<div id = "error"> </ div>

<script>
*! * input.onblur * /! * = function () {
se (! input.value.includes ('@')) {// não e-mail
input.classList.add ('invalid');
error.innerHTML = 'Digite um email correto.'
}
};

*! * input.onfocus * /! * = function () {
se (this.classList.contains ('invalid')) {
// remove a indicação "erro", porque o usuário deseja reentrar algo
this.classList.remove ('invalid');
error.innerHTML = "";
}
};
</ script>
`` `

O HTML moderno permite realizar várias validações usando atributos de entrada: `required`,` pattern` e assim por diante. E às vezes são exatamente o que precisamos. O JavaScript pode ser usado quando queremos mais flexibilidade. Também podemos enviar automaticamente o valor alterado no servidor se estiver correto.


## Métodos foco / borrão

Métodos `elem.focus ()` e `elem.blur ()` definir / desarmar o foco no elemento.

Por exemplo, vamos tornar o visitante incapaz de deixar a entrada se o valor for inválido:

`` `html executar autorun height = 80
<style>
.error {
fundo: vermelho;
}
</ style>

Seu email, por favor: <input type = "email" id = "input">

<script>
input.onblur = function () {
se (! this.value.includes ('@')) {// não e-mail
// mostra o erro
this.classList.add ("error");
*! *
// ... e volte a colocar o foco
input.focus ();
* /! *
} outro {
this.classList.remove ("erro");
}
};
</ script>
`` `

Ele funciona em todos os navegadores, exceto o Firefox ([bug] (https://bugzilla.mozilla.org/show_bug.cgi?id=53579)).

Se inserimos algo na entrada e depois tentemos usar a tecla `` chave '' ou clique de distância do '<entrada> `, então' onblur 'retorna o foco novamente.

Por favor, note que não podemos "impedir a perda de foco" chamando `event.preventDefault ()` no `onblur`, porque` onblur` funciona * depois * o elemento perdeu o foco.

`` `warn header =" perda de foco iniciada por JavaScript "
Uma perda de foco pode ocorrer por muitas razões.

Um deles é quando o visitante clica em outro lugar. Mas também o próprio JavaScript pode causar isso, por exemplo:

- Um "alerta" move o foco para si mesmo, por isso causa a perda de foco no elemento (evento 'borrão'), e quando o 'alerta' é descartado, o foco volta (evento 'foco').
- Se um elemento for removido do DOM, isso também causa a perda de foco. Se for reinserido mais tarde, o foco não retornará.

Esses recursos, às vezes, fazem com que os manipuladores do "foco / borrão" se comportem mal - desencadeiam quando não são necessários.

A melhor receita é ter cuidado ao usar esses eventos. Se quisermos rastrear a perda de foco iniciada pelo usuário, então devemos evadir-nos causando por nós mesmos.
`` `
# Permitir focar em qualquer elemento: tabindex

Por padrão, muitos elementos não suportam a focagem.

A lista varia entre os navegadores, mas uma coisa está sempre correta: o suporte a "foco / borrão" é garantido para os elementos com os quais um visitante pode interagir: `<button>`, `<input>`, `<select>` `` ` a> `e assim por diante.

Por outro lado, os elementos que existem para formatar algo como `<div>`, `<span>`, `<tabela>` - são inabaláveis ​​por padrão. O método `elem.focus ()` não funciona neles e os eventos `focus / blur` nunca são disparados.

Isso pode ser alterado usando HTML-attribute `tabindex`.

O objetivo desse atributo é especificar o número da ordem do elemento quando a tecla 'chave' é usada para alternar entre eles.

Isto é: se temos dois elementos, o primeiro tem `tabindex =" 1 "`, e o segundo tem `tabindex =" 2 "`, depois pressionando `chave: Tab` enquanto no primeiro elemento - nos move para o o segundo.

Existem dois valores especiais:

- `tabindex =" 0 "` torna o elemento o último.
- `tabindex =" - 1 "` significa que `chave: Tab` deve ignorar esse elemento.

** Qualquer elemento suporta foco se tiver 'tabindex`. **

Por exemplo, aqui está uma lista. Clique no primeiro item e pressione a tecla `` `: Tab`:

`` `html autorun no-embellecer
Clique no primeiro item e pressione Tab. Acompanhe a ordem. Observe que muitas abas subseqüentes podem mover o foco do iframe com o exemplo.
<Ul>
<li tabindex = "1"> One </ li>
<li tabindex = "0"> Zero </ li>
<li tabindex = "2"> Dois </ li>
<li tabindex = "- 1"> Menos um </ li>
</ Ul>

<style>
li {cursor: ponteiro; }
: foco {contorno: 1px verde tracejado; }
</ style>
`` `

A ordem é assim: `1 - 2 - 0` (zero é sempre o último). Normalmente, `<li>` não suporta foco, mas o `tabindex` completo o habilita, juntamente com eventos e estilo com`: focus`.

`` `cabeçalho inteligente =" `elem.tabIndex` também funciona"
Podemos adicionar `tabindex` do JavaScript usando a propriedade` elem.tabIndex`. Isso tem o mesmo efeito.
`` `

## Delegação: focusin / focusout

Eventos `focus` e` blur` não fazem bolhas.

Por exemplo, não podemos colocar `focus` no` <form> `para realçá-lo, assim:

`` `html autorun height = 80
<! - na focagem no formulário - adicione a classe ->
<form *! * onfocus = "this.className = 'focused'" * /! *>
<input type = "text" name = "name" value = "Name">
<input type = "text" name = "sobrenome" value = "Sobrenome">
</ form>

<style> .focused {contorno: 1px sólido vermelho; } </ style>
`` `

O exemplo acima não funciona, porque quando o usuário se concentra em um `<input>`, o evento `focus` desencadeia apenas nessa entrada. Não faz bolhas. Então, `form.onfocus` nunca dispara.

Existem duas soluções.

Primeiro, há uma característica histórica engraçada: `focus / blur` não se espalha, mas se propaga na fase de captura.

Isso funcionará:

`` `html autorun height = 80
<form id = "form">
<input type = "text" name = "name" value = "Name">
<input type = "text" name = "sobrenome" value = "Sobrenome">
</ form>

<style> .focused {contorno: 1px sólido vermelho; } </ style>

<script>
*! *
// coloque o manipulador na fase de captura (último argumento verdadeiro)
form.addEventListener ("focus", () => form.classList.add ('focused'), true);
form.addEventListener ("blur", () => form.classList.remove ('focused'), true);
* /! *
</ script>
`` `

Em segundo lugar, há eventos `focusin` e` focusout` - exatamente o mesmo que `focus / blur`, mas eles borbulham.

Observe que eles devem ser atribuídos usando `elem.addEventListener`, não` em <event> `.

Então, aqui está outra variante de trabalho:

`` `html autorun height = 80
<form id = "form">
<input type = "text" name = "name" value = "Name">
<input type = "text" name = "sobrenome" value = "Sobrenome">
</ form>

<style> .focused {contorno: 1px sólido vermelho; } </ style>

<script>
*! *
// coloque o manipulador na fase de captura (último argumento verdadeiro)
form.addEventListener ("focusin", () => form.classList.add ('focado'));
form.addEventListener ("focusout", () => form.classList.remove ('focused'));
* /! *
</ script>
`` `

## Resumo

Eventos `focus` e` blur` desencadeiam na focagem / perda de foco no elemento.

Suas promoções são:
- Não borbulham. Pode usar o estado de captura em vez disso ou `focusin / focusout`.
- A maioria dos elementos não suporta foco por padrão. Use `tabindex` para fazer qualquer coisa focável.

O elemento focado atual está disponível como `document.activeElement`.
