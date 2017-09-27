# Console do desenvolvedor

O código é propenso a erros. É provável que cometa erros ... Ah, do que estou falando? Você é * absolutamente * vai fazer erros, pelo menos, se você é humano, não um [robô] (https://en.wikipedia.org/wiki/Bender_ (Futurama)).

Mas no navegador, um usuário não vê os erros por padrão. Então, se algo der errado no script, não veremos o que está quebrado e não conseguimos corrigi-lo.

Para ver erros e obter muitas outras informações úteis sobre scripts, os navegadores têm "ferramentas de desenvolvedor" incorporadas.

Na maioria das vezes, os desenvolvedores se inclinam para o Chrome ou o Firefox para o desenvolvimento, porque esses navegadores possuem as melhores ferramentas de desenvolvimento. Outros navegadores também fornecem ferramentas para desenvolvedores, às vezes com recursos especiais, mas geralmente são "catch-up" para o Chrome ou o Firefox. Então, a maioria das pessoas tem um navegador "favorito" e muda para outros se um problema for específico do navegador.

As ferramentas do desenvolvedor são realmente poderosas, existem muitos recursos. Para começar, aprenderemos como abri-los, ver erros e executar comandos JavaScript.

[cortar]

## Google Chrome

Abra a página [bug.html] (bug.html).

Há um erro no código JavaScript nela. Está escondido dos olhos de um visitante regular, então vamos abrir as ferramentas do desenvolvedor para vê-lo.

Pressione `key: F12` ou, se você estiver no Mac, então a tecla ': Cmd + Opt + J`.

As ferramentas do desenvolvedor serão abertas na guia Console por padrão.

Parece um pouco assim:

! [cromo] (chrome.png)

O aspecto exato das ferramentas do desenvolvedor depende da sua versão do Chrome. Ele muda de tempos em tempos, mas deve ser semelhante.

- Aqui podemos ver a mensagem de erro de cor vermelha. Neste caso, o script contém um comando desconhecido "lalala".
- À direita, existe um link clicável na fonte `bug.html: 12` com o número da linha onde o erro ocorreu.

Abaixo da mensagem de erro, há um símbolo azul `` `` `` `''. Marca uma "linha de comando" onde podemos digitar comandos de JavaScript e pressionar a tecla 'Enter' para executá-los (`key: Shift + Enter` para inserir comandos de múltiplas linhas).

Agora podemos ver erros e isso é suficiente para o início. Voltarei às ferramentas do desenvolvedor mais tarde e cobrire a depuração mais aprofundada no capítulo <info: debugging-chrome>.


## Firefox, Edge e outros

A maioria dos outros navegadores usa `key: F12` para abrir as ferramentas do desenvolvedor.

A aparência deles é bem parecida. Uma vez que você sabe como usar um deles (você pode começar com o Chrome), você pode mudar facilmente para outro.

## Safari

O Safari (navegador Mac, não suportado pelo Windows / Linux) é um pouco especial aqui. Precisamos primeiro habilitar o "Develop menu".

Abra Preferências e vá para o painel "Avançado". Há uma caixa de seleção na parte inferior:

! [safari] (safari.png)

Agora `chave: Cmd + Opt + C` pode alternar o console. Observe também que o novo item do menu superior, "Desenvolvimento", apareceu. Tem muitos comandos e opções.

## Resumo

- As ferramentas do desenvolvedor nos permitem ver erros, executar comandos, examinar variáveis ​​e muito mais.
- Eles podem ser abertos com `key: F12` para a maioria dos navegadores no Windows. O Chrome para Mac precisa de 'chave: Cmd + Opt + J`, Safari: `chave: Cmd + Opt + C` (precisa ativar primeiro).

Agora temos o ambiente pronto. Na próxima seção, chegaremos ao JavaScript.
