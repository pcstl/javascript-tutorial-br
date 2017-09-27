# Estrutura do código

A primeira coisa a estudar são os blocos de construção do código.

[cortar]

## Afirmações

Declarações são construções de sintaxe e comandos que executam ações.



Podemos ter tantas declarações no código como desejamos. Outra declaração pode ser separada com um ponto-e-vírgula.

Por exemplo, aqui dividimos a mensagem em dois:

`` `js run no-embellecer

`` `

Geralmente cada declaração é escrita em uma linha separada - assim o código torna-se mais legível:

`` `js run no-embellecer

alerta ('Mundo');
`` `

## Semicolons [#semicolon]

Um ponto e vírgula pode ser omitido na maioria dos casos quando existe uma ruptura de linha.

Isso também funcionaria:

`` `js run no-embellecer

alerta ('Mundo')
`` `

Aqui, o JavaScript interpreta o intervalo de linha como um ponto e vírgula "implícito". Isso também é chamado de [inserção automática de ponto e vírgula] (https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion).

** Na maioria dos casos, uma nova linha implica um ponto-e-vírgula. Mas "na maioria dos casos" não significa "sempre"! **

Há casos em que uma nova linha não significa um ponto-e-vírgula, por exemplo:

`` `js run no-embellecer
alerta (3 +
1
+ 2);
`` `

O código emite `6`, porque o JavaScript não insere semicólitos aqui. É intuitivamente óbvio que, se a linha terminar com um plus `" + "`, então é uma "expressão incompleta", não é necessário um ponto e vírgula. E neste caso funciona como pretendido.

** Mas existem situações em que o JavaScript "falha" assume um ponto-e-vírgula onde é realmente necessário. **

Os erros que ocorrem em tais casos são bastante difíceis de encontrar e corrigir.

`` `` cabeçalho inteligente = "Um exemplo de erro"
Se você tiver curiosidade em ver um exemplo concreto de tal erro, verifique este código:

`` `js run
[1, 2] .forEach (alerta)
`` `

Não há necessidade de pensar sobre o significado dos colchetes `[]` e `forEach` ainda. Nós os estudaremos mais tarde, por enquanto não importa. Vamos lembrar o resultado: mostra `1`, depois` 2`.

Agora vamos adicionar um "alerta" antes do código e * não * terminar com um ponto-e-vírgula:

`` `js run no-embellecer
alerta ("Haverá um erro")

[1, 2] .forEach (alerta)
`` `

Agora, se o executarmos, apenas o primeiro 'alerta' é mostrado, e então temos um erro!

Mas tudo está bem novamente se adicionarmos um ponto-e-víraco após o alerta:
`` `js run
alerta ("All fine now");

[1, 2] .forEach (alerta)
`` `

Agora temos a mensagem "All fine now" e depois `1` e` 2`.


O erro na variante sem semicolon ocorre porque o JavaScript não implica um ponto e vírgula antes dos colchetes "[...]`.

Então, porque o ponto-e-vírgula não é inserido automaticamente, o código no primeiro exemplo é tratado como uma única afirmação. É assim que o motor vê:

`` `js run no-embellecer
alerta ("Haverá um erro") [1, 2] .forEach (alerta)
`` `

Mas deve ser duas declarações separadas, nem uma única. Essa fusão neste caso é simplesmente errada, daí o erro. Há outras situações em que tal coisa acontece.
`` ``

Recomenda-se colocar pontos-e-vírgulas entre as declarações, mesmo que sejam separadas por novas linhas. Esta regra é amplamente adotada pela comunidade. Vamos notar mais uma vez - * é possível * deixar para fora os pontos-e-um semicora na maioria das vezes. Mas é mais seguro - especialmente para um iniciante - usá-los.

## Comentários

Com o passar do tempo, o programa se torna cada vez mais complexo. Torna-se necessário adicionar * comentários * que descrevem o que acontece e por quê.

Os comentários podem ser colocados em qualquer lugar do script. Eles não afetam a execução, porque o motor simplesmente os ignora.

** Os comentários de uma linha começam com os dois caracteres de barra invertidos `//`.**

O resto da linha é um comentário. Pode ocupar uma linha completa própria ou seguir uma declaração.

Como aqui:
`` `js run
// Este comentário ocupa uma linha própria


alerta ('Mundo'); // Este comentário segue a declaração
`` `

** Os comentários multilíngües começam com uma barra diagonal e um asterisco <code> / * </ code> e terminam com um asterisco e uma barra invertida <code> * / </ code>. **

Como isso:

`` `js run
/ * Um exemplo com duas mensagens.
Este é um comentário de várias linhas.
* /

alerta ('Mundo');
`` `

O conteúdo dos comentários é ignorado, então, se colocarmos o código dentro de <code> / * ... * / </ code>, ele não será executado.

Às vezes, é útil para desativar temporariamente uma parte do código:

`` `js run
/ * Comentando o código

* /
alerta ('Mundo');
`` `

`` `cabeçalho inteligente =" Usar teclas rápidas! "
Na maioria dos editores, uma linha de código pode ser comentada pela tecla 'chave: Ctrl + / `para um comentário de uma única linha e algo como` chave: Ctrl + Shift + / `- para comentários de várias linhas (selecione um código e pressione o botão tecla de atalho). Para Mac, tente a tecla `: Cmd` em vez de` key: Ctrl`.
`` `

`` `` cabeçalho de aviso = "Os comentários aninhados não são suportados!"
Pode não haver `/*...*/` dentro de outro `/*...*/`.

Esse código morre com um erro:

`` `js run no-embellecer
/ *
/ * comentário aninhado?!? * /
* /
alerta ('Mundo');
`` `
`` ``

Por favor, não hesite em comentar seu código.

Os comentários aumentam a pegada geral do código, mas isso não é um problema. Existem muitas ferramentas que minimizam o código antes de serem publicadas no servidor de produção. Eles removem comentários, então eles não aparecem nos scripts de trabalho. Portanto, os comentários não têm efeitos negativos sobre a produção.

Além disso, no tutorial, haverá um capítulo <info: coding-style> que também explica como escrever melhores comentários.
