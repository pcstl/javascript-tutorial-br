# Envio de formulário: envio de evento e método

O evento `submit` desencadeia quando o formulário é submetido, geralmente é usado para validar o formulário antes de enviá-lo para o servidor ou abortar o envio e processá-lo em JavaScript.

O método `form.submit ()` permite iniciar o envio de formulário a partir do JavaScript. Podemos usá-lo para criar e enviar dinamicamente nossos próprios formulários para o servidor.

Vamos ver mais detalhes sobre eles.

[cortar]

## Evento: enviar

Existem duas formas principais de enviar um formulário:

1. O primeiro - clicar em '<input type = "submit"> `ou` <input type = "image"> `.
2. O segundo - pressione a tecla '' '' '' '' '"em um campo de entrada.

Ambas as ações levam ao evento "enviar" no formulário. O manipulador pode verificar os dados e, se houver erros, mostre-os e ligue para `event.preventDefault ()`, então o formulário não será enviado para o servidor.

No formulário abaixo:
1. Vá para o campo de texto e pressione a tecla `` `'' '' '' ''".
2. Clique em `<input type =" submit ">`.

Ambas as ações mostram 'alerta' e o formulário não é enviado em qualquer lugar devido a `return false`:

`` `html autorun height = 60 não-embelezar

Primeiro: Digite o campo de entrada <input type = "text" value = "text"> <br>
Segundo: clique em "enviar": <input type = "submit" value = "Submit">
</ form>
`` `

`` `` smart header = "Relação entre` submit` e `click`"
Quando um formulário é enviado usando `key: Enter` em um campo de entrada, um evento` click` desencadeia no `<input type =" submit ">`.

Isso é engraçado, porque não havia nenhum clique.

Aqui está a demonstração:
`` `html autorun height = 60
<form onsubmit = "return false">
<input type = "text" size = "30" value = "Foco aqui e pressione enter">

</ form>
`` `

`` ``

## Método: enviar

Para enviar um formulário ao servidor manualmente, podemos chamar `form.submit ()`.

Em seguida, o evento `submit` não é gerado. Supõe-se que, se o programador chamar `form.submit ()`, o script já fez todo o processamento relacionado.

Às vezes, isso é usado para criar e enviar manualmente um formulário, assim:

`` `js run
deixe form = document.createElement ('form');
form.action = 'https://google.com/search';
form.method = 'GET';

form.innerHTML = '<input name = "q" value = "test">';

// o formulário deve estar no documento para enviá-lo
document.body.append (formulário);

form.submit ();
`` `
