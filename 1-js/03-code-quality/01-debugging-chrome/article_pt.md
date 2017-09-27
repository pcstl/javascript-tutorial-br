# Depuração no Chrome

Antes de escrever um código mais complexo, vamos falar sobre depuração.

Todos os navegadores modernos e a maioria dos outros ambientes suportam "depuração" - uma UI especial em ferramentas para desenvolvedores que facilita muito a busca e a reparação de erros.

Vamos usar o Chrome aqui, porque provavelmente é o mais rico em recursos neste aspecto.

[cortar]

## O painel "fontes"

Sua versão do Chrome pode parecer um pouco diferente, mas ainda deve ser óbvio o que há.

- Abra a [página de exemplo] (depuração / index.html) no Chrome.
- Ligue as ferramentas do desenvolvedor com `key: F12` (Mac:` key: Cmd + Opt + I`).
- Selecione o painel `sources`.

Aqui está o que você deve ver se você estiver fazendo isso pela primeira vez:

! [] (chrome-open-sources.png)

O botão toggler <span class = "devtools" style = "background-position: -168px -76px"> </ span> abre a guia com arquivos.

Vamos clicar nele e selecionar `index.html` e depois` hello.js` na vista em árvore. Aqui está o que deve aparecer:

! [] (chrome-tabs.png)

Aqui podemos ver três zonas:

1. ** Zona de recursos ** lista html, javascript, css e outros arquivos, incluindo imagens que estão anexadas à página. As extensões do Chrome podem aparecer aqui também.
2. ** A zona de origem ** mostra o código-fonte.
3. ** Informação e área de controle ** é para depuração, vamos explorar isso em breve.

Agora, você pode clicar no mesmo estilo toggler <span class = "devtools" style = "background-position: -200px -76px"> </ span> novamente para ocultar a lista de recursos e dar ao código algum espaço.

## Console

Se pressionarmos `Esc`, um console será aberto abaixo. Podemos digitar comandos lá e pressionar a tecla `` `` '' '' '' '' '' '' para executar.

Depois que uma declaração é executada, seu resultado é mostrado abaixo.

Por exemplo, aqui `1 + 2` resultados em` 3` e `hello (" depurador ")` não retorna nada, então o resultado é 'indefinido':

!] [] (chrome-sources-console.png)

## Breakpoints

Vamos examinar o que está acontecendo dentro do código da [página de exemplo] (depuração / index.html). Em `hello.js`, clique no número da linha` 4`. Sim, diretamente no "4" `dígito, não no código.

Parabéns! Você definiu um ponto de interrupção. Por favor, clique no número da linha `8`.

Deve ter essa aparência (o azul é onde você deve clicar):

!] [] (chrome-sources-breakpoint.png)

A * ponto de interrupção * é um ponto de código onde o depurador irá pausar automaticamente a execução de JavaScript.

Enquanto o código está em pausa, podemos examinar as variáveis ​​atuais, executar comandos na consola, etc. Em outras palavras, podemos detê-lo.

Nós sempre podemos encontrar uma lista de pontos de interrupção no painel direito. Isso é útil quando temos muitos pontos de interrupção em vários arquivos. Permite:
- Salte rapidamente para o ponto de interrupção no código (clicando nele no painel direito).
- Desative temporariamente o ponto de interrupção desmarcando-o.
- Remova o ponto de interrupção clicando com o botão direito do mouse e selecionando Remover.
- ...E assim por diante.

`` `cabeçalho inteligente =" Pontos de interrupção condicionais "
* O botão direito do mouse * no número da linha permite criar um ponto de interrupção * condicional *. Isso só desencadeia quando a expressão dada é verdadeira.

Isso é útil quando precisamos parar apenas para um determinado valor de variável ou para certos parâmetros de função.
`` `

## Comando Debugger

Também podemos pausar o código usando o comando `debugger`, como este:

`` `js
Funcione oi (nome) {
let phrase = `Hello, $ {name}!`;

*! *
depurador; // <- o depurador pára aqui
* /! *

diga (frase);
}
`` `

Isso é muito conveniente quando estamos em um editor de código e não queremos mudar para o navegador e procurar o script nas ferramentas do desenvolvedor para definir o ponto de interrupção.


## Pausar e olhar ao redor

No nosso exemplo, `hello ()` é chamado durante a carga da página, então a maneira mais fácil de ativar o depurador é recarregar a página. Então, vamos pressionar `key: F5` (Windows, Linux) ou` key: Cmd + R` (Mac).

À medida que o ponto de interrupção é definido, a execução faz uma pausa na 4ª linha:

!] [] (chrome-sources-debugger-pause.png)

Abra os menus suspensivos informativos à direita (rotulados com setas). Eles permitem que você examine o estado atual do código:

1. ** `Assista '- mostra os valores atuais para quaisquer expressões. **

Você pode clicar no plus `+` e inserir uma expressão. O depurador mostrará seu valor a qualquer momento, recalculando-o automaticamente no processo de execução.

2. ** `Call Stack` - mostra a cadeia de chamadas aninhadas. **

No momento atual, o depurador está dentro da chamada 'hello () `, chamada por um script em` index.html` (sem função, então é chamado de "anônimo").

Se você clicar em um item de pilha, o depurador salta para o código correspondente e todas as suas variáveis ​​também podem ser examinadas.
3. ** `Scope` - variáveis ​​atuais. **

`Local` mostra as variáveis ​​de função local. Você também pode ver seus valores destacados diretamente sobre a fonte.

`Global` possui variáveis ​​globais (de quaisquer funções).

Há também essa palavra-chave que não estudamos ainda, mas faremos isso em breve.

## Rastreando a execução

Agora é hora de * rastrear * o script.

Existem botões para ele no topo do painel direito. Vamos envolvê-los.

<span class = "devtools" style = "background-position: -7px -76px"> </ span> - continue a execução, tecla de atalho `chave: F8`.
: Retoma a execução. Se não houver pontos de interrupção adicionais, a execução apenas continua e o depurador perde o controle.

Aqui está o que podemos ver depois de clicar nele:

!] [] (chrome-sources-debugger-trace-1.png)

A execução foi retomada, atingiu outro ponto de interrupção dentro de `say ()` e pausou lá. Dê uma olhada na "Pilha de chamadas" à direita. Aumentou mais uma vez. Estamos dentro de `say ()` now.

<span class = "devtools" style = "background-position: -137px -76px"> </ span> - faça um passo (execute o próximo comando), mas * não entre na função *, tecla de atalho `chave : F10`.
: Se clicarmos agora, o "alerta" será mostrado. O importante é que o "alerta" pode ser qualquer função, a execução "passa por cima", ignorando a função interna.

<span class = "devtools" style = "background-position: -72px -76px"> </ span> - faça um passo, tecla de atalho `chave: F11`.
: O mesmo que o anterior, mas "entra em" funções aninhadas. Ao clicar nessa, você passará por todas as ações de script uma a uma.

<span class = "devtools" style = "background-position: -104px -76px"> </ span> - continue a execução até o final da função atual, tecla de atalho `key: Shift + F11`.
: A execução parará na última linha da função atual. Isso é útil quando inserimos acidentalmente uma chamada aninhada usando <span class = "devtools" style = "background-position: -72px -76px"> </ span>, mas não nos interessa e queremos continuar até o fim o mais cedo possível.

<span class = "devtools" style = "background-position: -7px -28px"> </ span> - habilite / desative todos os pontos de interrupção.
: Esse botão não move a execução. Apenas uma massa de ligar / desligar para pontos de interrupção.

<span class = "devtools" style = "background-position: -264px -4px"> </ span> - habilite / desative a pausa automática em caso de erro.
: Quando ativado e as ferramentas do desenvolvedor estão abertas, um erro de script interrompe automaticamente a execução. Então podemos analisar variáveis ​​para ver o que deu errado. Então, se o nosso script morrer com um erro, podemos abrir o depurador, ativar esta opção e recarregar a página para ver onde ela morre e qual o contexto nesse momento.

`` `cabeçalho inteligente =" Continuar até aqui "
Clique com o botão direito do mouse em uma linha de código abre o menu de contexto com uma ótima opção chamada "Continuar até aqui".

Isso é útil quando queremos mover vários passos para a frente, mas somos muito preguiçosos para definir um ponto de interrupção.
`` `

## Exploração madeireira

Para exibir algo para consola, há uma função `console.log`.

Por exemplo, isso produz valores de `0 'para` 4 para consola:

`` `js run
// console aberto para ver
para (vamos i = 0; i <5; i ++) {
console.log ("valor", i);
}
`` `

Usuários regulares não vêem essa saída, está no console. Para vê-lo, abra a aba da Consola das ferramentas do desenvolvedor ou pressione a tecla `` Esc 'em outra aba: que abre o console na parte inferior.

Se tivermos o login suficiente em nosso código, podemos ver o que está acontecendo nos registros, sem o depurador.

## Resumo

Como podemos ver, existem três formas principais de pausar um script:
1. Um ponto de interrupção.
2. As declarações do `depurador '.
3. Um erro (se as ferramentas do dev estiverem abertas eo botão <span class = "devtools" style = "background-position: -264px -4px"> </ span> está "ligado")

Então, podemos examinar as variáveis ​​e avançar para ver onde a execução está errada.

Existem muitas outras opções em ferramentas para desenvolvedores do que abordadas aqui. O manual completo está em <https://developers.google.com/web/tools/chrome-devtools>

As informações deste capítulo são suficientes para começar a depurar, mas, mais tarde, especialmente se você fizer muitas coisas do navegador, vá lá e veja recursos mais avançados das ferramentas do desenvolvedor.

Ah, e também você pode clicar em vários locais de ferramentas de desenvolvimento e apenas ver o que está aparecendo. Esse é provavelmente o caminho mais rápido para aprender ferramentas de dev. Não se esqueça do clique direito também!
