# Agendamento: setTimeout e setInterval

Podemos decidir executar uma função não agora, mas em um certo tempo depois. Isso é chamado de "agendamento de uma chamada".

Existem dois métodos para isso:

- `setTimeout` permite executar uma função uma vez após o intervalo de tempo.
- `setInterval` permite executar uma função regularmente com o intervalo entre as execuções.

Esses métodos não fazem parte da especificação JavaScript. Mas a maioria dos ambientes tem o agendador interno e fornece esses métodos. Em particular, eles são suportados em todos os navegadores e Node.JS.


[cortar]

## setTimeout

A sintaxe:

`` `js
deixe timerId = setTimeout (func | code, delay [, arg1, arg2 ...])
`` `

Parâmetros:

`func | code`
: Função ou uma seqüência de código para executar.
Normalmente, essa é uma função. Por motivos históricos, uma série de códigos pode ser aprovada, mas isso não é recomendado.

`atraso '
: O atraso antes da execução, em milissegundos (1000 ms = 1 segundo).

`arg1`,` arg2` ...
: Argumentos para a função (não suportada no IE9-)

Por exemplo, esse código chama `sayHi ()` após um segundo:

`` `js run
função sayHi () {

}

*! *
setTimeout (sayHi, 1000);
* /! *
`` `

Com argumentos:

`` `js run
função sayHi (frase, quem) {
alerta (frase + ',' + quem);
}

*! *
setTimeout (sayHi, 1000, "Hello", "John"); // Olá john
* /! *
`` `

Se o primeiro argumento for uma string, o JavaScript criará uma função a partir dele.

Então, isso também funcionará:

`` `js run no-embellecer

`` `

Mas o uso de strings não é recomendado, use funções em vez de elas, assim:

`` `js run no-embellecer

`` `

`` `` cabeçalho inteligente = "Passar uma função, mas não executá-la"
Os desenvolvedores novatos às vezes cometem um erro ao adicionar suportes `()` após a função:

`` `js
// errado!
setTimeout (sayHi (), 1000);
`` `
Isso não funciona, porque `setTimeout` espera que uma referência funcione. E aqui `sayHi ()` executa a função, e o * resultado de sua execução * é passado para `setTimeout`. Em nosso caso, o resultado de `sayHi ()` é `undefined` (a função não retorna nada), então nada está agendado.
`` ``

### Cancelando com clearTimeout

Uma chamada para `setTimeout` retorna um" identificador de timer "` timerId` que podemos usar para cancelar a execução.

A sintaxe a cancelar:

`` `js
deixe timerId = setTimeout (...);
clearTimeout (timerId);
`` `

No código abaixo, agendamos a função e depois cancelamos (mudamos nossa mente). Como resultado, nada acontece:

`` `js run no-embellecer
deixe timerId = setTimeout (() => alerta ("nunca acontece"), 1000);
alerta (timerId); // identificador do temporizador

clearTimeout (timerId);
alerta (timerId); // mesmo identificador (não se torna nulo após o cancelamento)
`` `

Como podemos ver a partir da saída de "alerta", em um navegador o identificador do timer é um número. Em outros ambientes, isso pode ser outra coisa. Por exemplo, Node.JS retorna um objeto temporizador com métodos adicionais.

Novamente, não existe uma especificação universal para esses métodos, então está tudo bem.

Para os navegadores, os temporizadores são descritos na [seção de temporizadores] (https://www.w3.org/TR/html5/webappapis.html#timers) do padrão HTML5.

## setInterval

O método `setInterval` possui a mesma sintaxe que` setTimeout`:

`` `js
deixe timerId = setInterval (func | code, delay [, arg1, arg2 ...])
`` `

Todos os argumentos têm o mesmo significado. Mas, ao contrário de `setTimeout`, ele executa a função não apenas uma vez, mas regularmente após o intervalo de tempo dado.

Para parar mais chamadas, devemos chamar `clearInterval (timerId)`.

O exemplo a seguir mostrará a mensagem a cada 2 segundos. Após 5 segundos, a saída é parada:

`` `js run
// repete com o intervalo de 2 segundos
Deixe timerId = setInterval (() => alert ('tick'), 2000);

// depois de 5 segundos parar
setTimeout (() => {clearInterval (timerId); alerta ('parar');}, 5000);
`` `

`` `smart header =" Tempo de congelamento de janelas modal no Chrome / Opera / Safari "
Nos navegadores IE e Firefox, o temporizador interno continua "marcando" enquanto mostra `alert / confirm / prompt`, mas no Chrome, Opera e Safari o temporizador interno se torna" congelado ".

Então, se você executar o código acima e não demitir a janela de "alerta" por algum tempo, então, no Firefox / IE seguinte, o "alerta" será mostrado imediatamente como você faz (2 segundos saiu da invocação anterior) e em Chrome / Opera / Safari - depois de mais 2 segundos (o temporizador não marcou durante o alerta).
`` `

## Recursive setTimeout

Há duas maneiras de executar algo regularmente.

Um é `setInterval`. O outro é um setTimeout recursivo, como este:

`` `js
/** ao invés de:
Deixe timerId = setInterval (() => alert ('tick'), 2000);
* /

deixe timerId = setTimeout (função tick () {
alerta ('tick');
*! *
timerId = setTimeout (tick, 2000); // (*)
* /! *
}, 2000);
`` `

O `setTimeout` acima agende a próxima chamada no final do atual` (*) `.

O "setTimeout" recursivo é um método mais flexível do que `setInterval`. Desta forma, a próxima chamada pode ser agendada de forma diferente, dependendo dos resultados da atual.

Por exemplo, precisamos escrever um serviço que cada 5 segundos envia uma solicitação ao servidor solicitando dados, mas no caso de o servidor estar sobrecarregado, ele deve aumentar o intervalo para 10, 20, 40 segundos ...

Aqui está o pseudocódigo:
`` `js
Deixe atrasar = 5000;

deixe timerId = setTimeout (função request () {
...enviar pedido...

se (a solicitação falhou devido à sobrecarga do servidor) {
// aumenta o intervalo para a próxima corrida
atraso * = 2;
}

timerId = setTimeout (pedido, atraso);

}, atraso);
`` `


E se tivermos tarefas com fome de CPU, podemos medir o tempo gasto pela execução e planejar a próxima chamada mais cedo ou mais tarde.

** Recursivo `setTimeout` garante um atraso entre as execuções,` setInterval` - não. **

Vamos comparar dois fragmentos de código. O primeiro usa `setInterval`:

`` `js
deixe i = 1;
setInterval (function () {
func (i);
}, 100);
`` `

O segundo usa o `setTimeout 'recursivo:

`` `js
deixe i = 1;
setTimeout (function run () {
func (i);
setTimeout (executar, 100);
}, 100);
`` `

Para `setInterval`, o programador interno executará` func (i) `a cada 100ms:

! [] (setinterval-interval.png)

Você notou? ...

** O atraso real entre chamadas `func` para` setInterval` é menor que no código! **

Isso é natural, porque o tempo de execução "func" consome "uma parte do intervalo.

É possível que a execução "func" seja mais longa do que esperávamos e leva mais de 100ms.

Nesse caso, o motor espera que `func` seja completado, depois verifica o programador e, se o tempo estiver ativado, então ele será executado novamente * imediatamente *.

No caso de borda, se a função sempre for executada mais do que `delay` ms, as chamadas ocorrerão sem uma pausa.

E aqui está a imagem para o `setTimeout 'recursivo:

!] [] (settimeout-interval.png)

** Recursivo `setTimeout` garante o atraso fixo (aqui 100ms). **

Isso ocorre porque uma nova chamada está planejada no final do anterior.

`` `` cabeçalho inteligente = "coleta de lixo"
Quando uma função é passada em `setInterval / setTimeout`, uma referência interna é criada e guardada no agendador. Ele impede que a função seja coletada, mesmo que não haja outras referências a ela.

`` `js
// a função permanece na memória até que o programador a chame
setTimeout (function () {...}, 100);
`` `

Para `setInterval`, a função permanece na memória até que 'cancelInterval` seja chamado.

Há um efeito colateral. Uma função faz referência ao ambiente lexical externo, portanto, enquanto ele vive, as variáveis ​​externas também são vivas. Eles podem levar muito mais memória do que a própria função. Então, quando não precisamos mais da função agendada, é melhor cancelá-la, mesmo que seja muito pequena.
`` ``

## setTimeout (..., 0)

Existe um caso de uso especial: `setTimeout (func, 0)`.

Isso programa a execução do `func` o mais rápido possível. Mas o agendador irá invocá-lo somente após o código atual estar completo.

Portanto, a função está agendada para executar "logo após" o código atual. Em outras palavras, * assíncronamente *.

Por exemplo, isso mostra "Olá", depois imediatamente "Mundo":

`` `js run
setTimeout (() => alerta ("Mundo"), 0);


`` `

A primeira linha "coloca a chamada no calendário após 0ms". Mas o programador só "verificará o calendário" após o código atual estar completo, então "Hello" `é primeiro e" World "` - depois disso.

### Divisão de tarefas com fome de CPU

Há um truque para dividir a tarefa de CPU com fome usando `setTimeout`.

Por exemplo, o script de realce de sintaxe (usado para colorizar exemplos de código nesta página) é bastante pesado por CPU. Para aprimorar o código, ele executa a análise, cria muitos elementos coloridos, os adiciona ao documento - para um texto grande que demora muito. Pode até provocar o "travar" do navegador, o que é inaceitável.

Então podemos dividir o texto longo em pedaços. Primeiramente 100 linhas, então planeje mais 100 linhas usando `setTimeout (..., 0)`, e assim por diante.

Para maior clareza, vamos tomar um exemplo mais simples para consideração. Temos uma função para contar de `1` para `1000000000`.

Se você executá-lo, a CPU irá pendurar. Para o JS do lado do servidor que é claramente perceptível, e se você o estiver executando no navegador, tente clicar em outros botões na página - você verá que o JavaScript inteiro está realmente pausado, nenhuma outra ação funciona até que ele termine.

`` `js run
deixe i = 0;

deixe start = Date.now ();

contagem de função () {

// faça um trabalho pesado
para (let j = 0; j <1e9; j ++) {
i ++;
}

alerta ("Concluído em" + (Date.now () - start) + 'ms');
}

contagem();
`` `

O navegador pode até mostrar o aviso "o script leva muito tempo" (mas espero que não, o número não é muito grande).

Vamos dividir o trabalho usando o `setTimeout 'aninhado:

`` `js run
deixe i = 0;

deixe start = Date.now ();

contagem de função () {

// faça um trabalho pesado (*)
Faz {
i ++;
} enquanto (i% 1e6! = 0);

se (i == 1e9) {
alerta ("Concluído em" + (Date.now () - start) + 'ms');
} outro {
setTimeout (contagem, 0); // agende a nova chamada (**)
}

}

contagem();
`` `

Agora, a interface do usuário do navegador está totalmente funcional durante o processo de "contagem".

Fazemos parte do trabalho `(*)`:

1. Execute primeiro: `i = 1 ... 1000000`.
2. Segunda execução: `i = 1000001..2000000`.
3. ... e assim por diante, o `while` verifica se` i` é dividido uniformemente por `100000`.

Então a próxima chamada está agendada em `(*)` se ainda não terminarmos.

Pausas entre as execuções de "count" fornecem apenas "respiração" suficiente para o mecanismo de JavaScript fazer outra coisa, para reagir em outras ações do usuário.

O importante é que ambas as variantes: com e sem dividir o trabalho por `setInterval` - são comparáveis ​​em velocidade. Não há muita diferença no tempo total de contagem.

Para torná-los mais próximos vamos fazer uma melhoria.

Vamos mover o agendamento no início do `count ()`:

`` `js run
deixe i = 0;

deixe start = Date.now ();

contagem de função () {

// move o agendamento no início
se (i <1e9 - 1e6) {
setTimeout (contagem, 0); // agende a nova chamada
}

Faz {
i ++;
} enquanto (i% 1e6! = 0);

se (i == 1e9) {
alerta ("Concluído em" + (Date.now () - start) + 'ms');
}

}

contagem();
`` `

Agora, quando começamos a `count ()` e sabemos que teremos que contar () `mais - agendamos isso imediatamente, antes de fazer o trabalho.

Se você executá-lo, é fácil notar que é preciso muito menos tempo.

`` `` smart header = "Minimal delay of anested timers in-browser"
No navegador, há uma limitação da frequência com que os temporizadores aninhados podem ser executados. O [padrão HTML5] (https://www.w3.org/TR/html5/webappapis.html#timers) diz: "após cinco temporizadores aninhados ..., o intervalo é forçado a ter pelo menos quatro milissegundos".

Vamos demonstrar o que significa pelo exemplo abaixo. A chamada `setTimeout` é re-agendada após` 0ms`. Cada chamada lembra o tempo real do anterior na matriz `times`. Como os atrasos reais se parecem? Vamos ver:

`` `js run
deixe start = Date.now ();
deixe times = [];

setTimeout (function run () {
times.push (Date.now () - start); // lembre-se do atraso da chamada anterior

se (iniciar + 100 <Date.now ()) alerta (vezes); // mostra os atrasos após 100ms
else setTimeout (executar, 0); // novamente, re-agendamento
}, 0);

// um exemplo da saída:
// 1,1,1,1,9,15,20,24,30,35,40,45,50,55,59,64,70,75,80,85,90,95,100
`` `

Os primeiros temporizadores são executados imediatamente (apenas como escrito na especificação) e, em seguida, o atraso entra em jogo e vemos `9, 15, 20, 24 ...`.

Essa limitação vem dos tempos antigos e muitos scripts dependem disso, por isso existe por razões históricas.

Para o JavaScript do lado do servidor, essa limitação não existe e existem outras formas de agendar um trabalho assíncrono imediato, como [process.nextTick] (https://nodejs.org/api/process.html) e [setImmediate] ( https://nodejs.org/api/timers.html) para Node.JS. Portanto, a noção é apenas para o navegador.
`` ``

### Permitir que o navegador seja processado

Outro benefício para os scripts no navegador é que eles podem mostrar uma barra de progresso ou algo para o usuário. Isso porque o navegador normalmente faz todos os "remanescentes" após o script estar completo.

Então, se fizermos uma única função enorme, mesmo que isso mude algo, as mudanças não são refletidas no documento até que ele termine.

Aqui está a demonstração:
`` `html run
<div id = "progresso"> </ div>

<script>
deixe i = 0;

contagem de função () {
para (let j = 0; j <1e6; j ++) {
i ++;
// coloque a corrente i no <div>
// (falamos mais sobre innerHTML no capítulo específico, deve ser óbvio aqui)
progress.innerHTML = i;
}
}

contagem();
</ script>
`` `

Se você executá-lo, as mudanças para `i` aparecerão após a conclusão total da contagem.

E se usarmos `setTimeout` para dividi-lo em pedaços, as mudanças são aplicadas entre as corridas, então isso parece melhor:

`` `html run
<div id = "progresso"> </ div>

<script>
deixe i = 0;

contagem de função () {

// faça um trabalho pesado (*)
Faz {
i ++;
progress.innerHTML = i;
} enquanto (i% 1e3! = 0);

se (i <1e9) {
setTimeout (contagem, 0);
}

}

contagem();
</ script>
`` `

Agora, o `<div>` mostra valores crescentes de `i`.

## Resumo

- Métodos 'setInterval (func, delay, ... args) `e` setTimeout (func, delay, ... args) `permitem executar o` func` regularmente / uma vez após o `delay` milissegundos.
- Para cancelar a execução, devemos chamar `clearInterval / clearTimeout` com o valor retornado por` setInterval / setTimeout`.
- As chamadas 'setTimeout' aninhadas são uma alternativa mais flexível para `setInterval`. Também podem garantir o tempo mínimo * entre * as execuções.
- Programação de tempo limite zero 'setTimeout (..., 0) `é usado para agendar a chamada" o mais rápido possível, mas após o código atual estar completo ".

Alguns usam casos de `setTimeout (..., 0)`:
- Para dividir as tarefas com fome de CPU em pedaços, de modo que o script não "seja suspenso"
- Para permitir que o navegador faça outra coisa enquanto o processo está acontecendo (coloque a barra de progresso).

Observe que todos os métodos de agendamento não * garantem * o atraso exato. Não devemos contar com isso no código agendado.

Por exemplo, o temporizador no navegador pode diminuir a velocidade por muitos motivos:
- A CPU está sobrecarregada.
- A guia do navegador está no modo de fundo.
- O laptop está na bateria.

Tudo isso pode diminuir a resolução mínima do temporizador (o atraso mínimo) para 300ms ou até 1000ms dependendo do navegador e das configurações.
