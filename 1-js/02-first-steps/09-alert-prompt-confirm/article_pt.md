# Interação: alerta, prompt, confirme

Esta parte do tutorial visa abranger JavaScript "tal como está", sem ajustes específicos do ambiente.

Mas ainda usamos um navegador como o ambiente de demonstração. Portanto, devemos saber pelo menos algumas funções de interface do usuário. Neste capítulo, nos familiarizaremos com as funções do navegador `alert`,` prompt` e `confirm`.

[cortar]

## alerta

Sintaxe:

`` `js
alerta (mensagem);
`` `

Isso mostra uma mensagem e faz uma pausa na execução do script até o usuário pressionar "OK".

Por exemplo:

`` `js run

`` `

A mini janela com a mensagem é chamada de * janela modal *. A palavra "modal" significa que o visitante não pode interagir com o resto da página, pressionar outros botões etc., até que tenham lidado com a janela. Neste caso - até pressionar "OK".

## pronto

Função `prompt` aceita dois argumentos:

`` `js no-embellecer
resultado = prompt (título [, padrão]);
`` `

Ele mostra uma janela modal com uma mensagem de texto, um campo de entrada para o visitante e os botões OK / CANCELAR.

`título '
: O texto para mostrar ao visitante.

`default`
: Um segundo parâmetro opcional, o valor inicial para o campo de entrada.

O visitante pode digitar algo no campo de entrada do prompt e pressionar OK. Ou eles podem cancelar a entrada pressionando o botão CANCELAR ou pressionando a tecla 'chave: Esc`.

A chamada para `prompt` retorna o texto do campo ou` null` se a entrada foi cancelada.

Por exemplo:

`` `js run
let age = prompt ('Quantos anos você tem?', 100);

alerta (`Você tem $ {idade} anos de idade! '); // Você tem 100 anos de idade!
`` `

`` `` lembre cabeçalho = "IE: sempre forneça um` padrão '
O segundo parâmetro é opcional. Mas se não o fornecemos, o Internet Explorer inseriria o texto "indefinido" no prompt.

Execute este código no Internet Explorer para ver isso:

`` `js run

`` `

Então, para ficar bem no IE, é recomendável fornecer sempre o segundo argumento:

`` `js run

`` `
`` ``

## confirme

A sintaxe:

`` `js
resultado = confirmar (pergunta);
`` `

A função `confirm` mostra uma janela modal com uma 'pergunta' e dois botões: OK e CANCELAR.

O resultado é `true` se OK for pressionado e` false` caso contrário.

Por exemplo:

`` `js run
deixe isBoss = confirmar ("Você é o chefe?");

alerta (isBoss); // verdadeiro se OK for pressionado
`` `

## Resumo

Cobrimos 3 funções específicas do navegador para interagir com o visitante:

`alerta '
: mostra uma mensagem.

`prompt`
: mostra uma mensagem pedindo ao usuário para inserir texto. Retorna o texto ou, se CANCEL ou `key: Esc` clicar, todos os navegadores retornam` null`.

`confirmar '
: mostra uma mensagem e espera que o usuário pressione "OK" ou "CANCELAR". Retorna `true` para OK e` false` para a tecla CANCEL / `: Esc`.

Todos esses métodos são modais: eles pausam a execução do script e não permitem que o visitante interaja com o resto da página até que a mensagem tenha sido descartada.

Existem duas limitações compartilhadas por todos os métodos acima:

1. A localização exata da janela modal é determinada pelo navegador. Normalmente está no centro.
2. O aspecto exato da janela também depende do navegador. Não podemos modificá-lo.

Esse é o preço por simplicidade. Existem outras maneiras de mostrar janelas mais agradáveis ​​e uma interação mais rica com o visitante, mas se "sinos e assobios" não importam muito, esses métodos funcionam bem.
