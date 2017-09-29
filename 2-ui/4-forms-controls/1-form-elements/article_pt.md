# Propriedades e métodos do formulário

Os formulários e elementos de controle, como `<input>`, têm muitas propriedades e eventos especiais.

Trabalhar com formulários pode ser muito mais conveniente se os conhecemos.

[cortar]

## Navegação: forma e elementos

Os formulários de documento são membros da coleção especial `document.forms`.

Essa é uma coleção * chamada *: podemos usar o nome e o número para obter o formulário.

`` `js no-embellecer
document.forms.my - o formulário com name = "my"
document.forms [0] - o primeiro formulário no documento
`` `

Quando temos um formulário, qualquer elemento está disponível na coleção nomeada `form.elements`.

Por exemplo:

`` `html run height = 40
<form name = "my">
<input name = "one" value = "1">
<input name = "two" value = "2">
</ form>

<script>
// obtenha o formulário
deixe form = document.forms.my; // <form name = "my"> elemento

// obtenha o elemento
Deixe elem = form.elements.one; // <input name = "one"> elemento

alert (elem.value); // 1
</ script>
`` `

Pode haver vários elementos com o mesmo nome, muitas vezes o caso com botões de rádio.

Nesse caso, `form.elements [name]` é uma coleção, por exemplo:

`` `html run height = 40
<form>
<input type = "radio" *! * name = "age" * /! * value = "10">
<input type = "radio" *! * name = "age" * /! * value = "20">
</ form>

<script>
Deixe form = document.forms [0];

Deixe ageElems = form.elements.age;

alerta (ageElems [0] .value); // 10, o primeiro valor de entrada
</ script>
`` `

Essas propriedades de navegação não dependem da estrutura da tag. Todos os elementos, por mais profundos que estejam na forma, estão disponíveis em `form.elements`.


`` `` cabeçalho inteligente = "Fieldsets como \" subformulários \ ""
Um formulário pode ter um ou vários elementos `<fieldset>` dentro dele. Eles também suportam a propriedade `elements`.

Por exemplo:

`` `html run height = 80
<corpo>
<form id = "form">
<fieldset name = "userFields">
<legenda> info </ legend>
<input name = "login" type = "text">
</ fieldset>
</ form>

<script>
alerta (form.elements.login); // <input name = "login">

*! *
Deixe fieldset = form.elements.userFields;
alerta (fieldset); // HTMLFieldSetElement

// podemos obter a entrada tanto do formulário quanto do campo
alerta (fieldset.elements.login == form.elements.login); // verdade
* /! *
</ script>
</ body>
`` `
`` ``

`` `` warn header = "Notação mais curta:` form.name` "
Há uma notação mais curta: podemos acessar o elemento como `form [index / name]`.

Em vez de `form.elements.login` podemos escrever` form.login`.

Isso também funciona, mas há um problema menor: se acessarmos um elemento e, em seguida, alterar o `nome`, então ainda está disponível sob o nome antigo (assim como no novo).

Isso é fácil de ver em um exemplo:

`` `html run height = 40
<form id = "form">
<input name = "login">
</ form>

<script>
alerta (form.elements.login == form.login); // verdadeiro, o mesmo <input>

form.login.name = "nome de usuário"; // muda o nome da entrada

// form.elements atualizou o nome:
alerta (form.elements.login); // Indefinido
alerta (form.elements.username); // entrada

*! *
// o acesso direto agora pode usar ambos os nomes: o novo e o antigo
alerta (form.username == form.login); // verdade
* /! *
</ script>
`` `

Isso geralmente não é um problema, porque raramente mudamos os nomes dos elementos do formulário.

`` ``

## Backreference: element.form

Para qualquer elemento, o formulário está disponível como `element.form`. Assim, uma forma faz referência a todos os elementos e elementos
faça referência ao formulário.

Aqui está a foto:

! [] (form-navigation.png)

Por exemplo:

`` `html run height = 40
<form id = "form">
<input type = "text" name = "login">
</ form>

<script>
*! *
// forma -> elemento
deixe login = form.login;

// elemento -> forma
alerta (login.form); // HTMLFormElement
* /! *
</ script>
`` `

## Elementos de formulário

Vamos falar sobre controles de formulário, preste atenção às suas características específicas.

### input and textarea

Normalmente, podemos acessar o valor como `input.value` ou` input.checked` para caixas de seleção.

Como isso:

`` `js
input.value = "Novo valor";
textarea.value = "Novo texto";

input.checked = true; // para uma caixa de seleção ou botão de opção
`` `

`` `warn header =" Use `textarea.value`, não` textarea.innerHTML` "
Observe que nunca devemos usar `textarea.innerHTML`: ele armazena apenas o HTML que estava inicialmente na página, e não o valor atual.
`` `

### select and option

Um elemento `<select>` possui 3 propriedades importantes:

1. `select.options` - a coleção de elementos` <option> `,
2. `select.value` - o valor da opção escolhida,
3. `select.selectedIndex` - o número da opção selecionada.

Então, temos três maneiras de definir o valor de um `<selecionar>`:

1. Encontre o `<opção>` `necessário e configure` option.selected` para `true`.
2. Configure `select.value` para o valor.
3. Configure `select.selectedIndex` para o número da opção.

O primeiro caminho é o mais óbvio, mas `(2)` e `(3)` geralmente são mais convenientes.

Aqui está um exemplo:

`` `html run
<select id = "select">
<valor da opção = "maçã"> Apple </ option>
<valor da opção = "pera"> pera </ option>
<valor da opção = "banana"> banana </ option>
</ select>

<script>
// as três linhas fazem o mesmo
selecione.opções [2] .selected = true;
select.selectedIndex = 2;
select.value = 'banana';
</ script>
`` `

Ao contrário da maioria dos outros controles, `<select multiple>` permite a escolha múltipla. Nesse caso, precisamos percorrer `select.options` para obter todos os valores selecionados.

Como isso:

`` `html run
<select id = "select" *! * multiple * /! *>
<valor da opção = "blues" selecionado> Blues </ option>
<valor da opção = "rock" selecionado> Rock </ option>
<valor da opção = "clássico"> Clássico </ option>
</ select>

<script>
// obtém todos os valores selecionados de seleção múltipla
Deixe selecionado = Array.from (selecione.options)
.filter (opção => option.selected)
.map (opção => option.value);

alerta (selecionado); // blues, rock
</ script>
`` `

A especificação completa do elemento `<select>` está disponível em <https://html.spec.whatwg.org/multipage/forms.html#the-select-element>.

### new Option

Na especificação de [o elemento da opção] (https://html.spec.whatwg.org/multipage/forms.html#the-option-element) há uma sintaxe curta agradável para criar elementos `<option>`:

`` `js
opção = nova opção (texto, valor, padrãoSeleccionado, selecionado);
`` `

Parâmetros:

- `text` - o texto dentro da opção,
- `valor` - o valor da opção,
- `defaultSelected` - if` true`, então o atributo `selected` é criado,
- `selected` - if` true`, a opção é selecionada.

Por exemplo:

`` `js
Deixe a opção = nova opção ("Texto", "valor");
// cria <valor da opção = "valor"> Texto </ option>
`` `

O mesmo elemento selecionado:

`` `js
Deixe opção = nova opção ("Texto", "valor", verdadeiro, verdadeiro);
`` `

`` `cabeçalho inteligente =" Propriedades adicionais de `<opção>` "
Os elementos de opção possuem propriedades adicionais:

`selecionado '
: A opção está selecionada.

`índice '
: O número da opção entre os outros no `` <select> `.

`texto '
: Conteúdo do texto da opção (visto pelo que o visitante).
`` `

## Resumo

Navegação de formulário:

`document.forms`
: Um formulário está disponível como `document.forms [nome / índice]`.

`form.elements`
: Os elementos do formulário estão disponíveis como `form.elements [name / index]`, ou podem usar apenas `form [name / index]`. A propriedade `elements` também funciona para` <fieldset> `.

`element.form`
: Os elementos fazem referência ao seu formulário na propriedade `form`.

O valor está disponível como `input.value`,` textarea.value`, `select.value` etc, ou` input.checked` para caixas de seleção e botões de opção.

Para `<select>` também podemos obter o valor pelo índice `select.selectedIndex` ou através da coleção de opções` select.options`. A especificação completa deste e de outros elementos está em <https://html.spec.whatwg.org/multipage/forms.html>.

Estes são os conceitos básicos para começar a trabalhar com formulários. No próximo capítulo, iremos abordar os eventos `focus` e` blur` que podem ocorrer em qualquer elemento, mas são principalmente tratados em formulários.
