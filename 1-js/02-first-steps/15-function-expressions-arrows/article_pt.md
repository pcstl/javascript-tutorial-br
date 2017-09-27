# Função expressões e setas

Em JavaScript, uma função não é uma "estrutura de linguagem mágica", mas um tipo especial de valor.

[cortar]

A sintaxe que usamos antes é chamada de * Declaração de Função *:

`` `js
função sayHi () {

}
`` `

Existe outra sintaxe para criar uma função que é chamada de * Expressão de função *.

Parece assim:

`` `js
deixe sayHi = function () {

};
`` `

Aqui, a função é criada e atribuída à variável explicitamente, como qualquer outro valor. Não importa como a função é definida, é apenas um valor armazenado na variável `sayHi`.


O significado dessas amostras de código é o mesmo: "crie uma função e coloque-a na variável` sayHi` ".

Podemos até mesmo imprimir esse valor usando `alert`:

`` `js run
função sayHi () {

}

*! *
alerta (sayHi); // mostra o código da função
* /! *
`` `

Observe que a última linha não executa a função, porque não há parênteses após `sayHi`. Existem linguagens de programação onde qualquer menção de um nome de função causa sua execução, mas o JavaScript não é assim.

Em JavaScript, uma função é um valor, para que possamos lidar com isso como um valor. O código acima mostra sua representação de string, que é o código-fonte.

É um valor especial, é claro, no sentido de que podemos chamá-lo de `sayHi ()`.

Mas ainda é um valor. Então podemos trabalhar com ele como com outros tipos de valores.

Podemos copiar uma função para outra variável:

`` `js run no-embellecer
function sayHi () {// (1) create

}

deixe func = sayHi; // (2) copiar

func (); // Olá // (3) execute a cópia (funciona)!
diga oi(); // Olá // isso ainda funciona também (por que não)
`` `

Aqui está o que acontece acima em detalhes:

1. A Declaração de Função `(1)` cria a função e coloca-a na variável chamada `sayHi`.
2. A linha `(2)` o copia na variável `func`.

Por favor, note novamente: não há parênteses após `sayHi`. Se houvesse, então, `func = sayHi ()` escreveria * o resultado da chamada * `sayHi ()` em `func`, não * a função *` sayHi` em si.
3. Agora, a função pode ser chamada como `sayHi ()` e `func ()`.

Note que também poderíamos ter usado uma expressão de função para declarar `sayHi`, na primeira linha:

`` `js
let sayHi = function () {...};

deixe func = sayHi;
// ...
`` `

Tudo funcionaria da mesma forma. Ainda mais óbvio o que está acontecendo, certo?


`` `` cabeçalho inteligente = "Por que há um ponto-e-vírgula no final?"
Pode haver uma pergunta, por que a expressão de função tem um ponto-e-vírgula `;` no final, e a Declaração de função não:

`` `js
função sayHi () {
// ...
}

deixe sayHi = function () {
// ...
} *! *; * /! *
`` `

A resposta é simples:
- Não há necessidade de `;` no final dos blocos de código e estruturas de sintaxe que os usam como `if {...}` `` para {} `,` function f {} `etc.
- Uma expressão de função é usada dentro da instrução: `let sayHi = ...;`, como um valor. Não é um bloco de código. O ponto-e-vírgula `` `é recomendado no final das instruções, independentemente do valor. Portanto, o ponto-e-vírgula aqui não está relacionado com a Expressão de Função em si mesmo, de qualquer forma, acaba de encerrar a declaração.
`` ``

## Funções de retorno de chamada

Vejamos mais exemplos de passando funções como valores e usando expressões de função.

Escreveremos uma função `ask (question, yes, no)` com três parâmetros:

`pergunta '
: Texto da pergunta

`sim '
: Função para executar se a resposta for "Sim"

`não '
: Função para executar se a resposta for "Não"

A função deve perguntar a `question` e, dependendo da resposta do usuário, ligue para` yes () `ou` no () `:

`` `js run
*! *
função perguntar (pergunta, sim, não) {
se (confirmar (pergunta)) sim ()
else no ();
}
* /! *

function showOk () {
alerta ("Você concordou.");
}

show de funçãoCancel () {
alerta ("Você cancelou a execução");
}

// uso: funções showOk, showCancel são passadas como argumentos para perguntar
pergunte ("Você concorda?", showOk, showCancel);
`` `

Antes de explorarmos como podemos escrever de forma muito mais curta, vejamos que, no navegador (e, no lado do servidor, em alguns casos), tais funções são bastante populares. A principal diferença entre uma implementação da vida real e o exemplo acima é que as funções da vida real usam formas mais complexas de interagir com o usuário do que uma simples "confirmação". No navegador, essa função normalmente desenha uma janela de perguntas agradável. Mas essa é outra história.

** Os argumentos de `ask` são chamados * funções de retorno de chamada * ou apenas * retorno de chamada *. **

A idéia é que nós passamos uma função e esperamos que ela seja "chamada de volta" mais tarde, se necessário. No nosso caso, `showOk` torna-se o retorno de chamada para a resposta" sim ", e` showCancel` para a resposta "não".

Podemos usar Expressões de funções para escrever a mesma função muito mais curta:

`` `js run no-embellecer
função perguntar (pergunta, sim, não) {
se (confirmar (pergunta)) sim ()
else no ();
}

*! *
pergunte (
"Você concorda?",
function () {alert ("Você concordou."); },
function () {alert ("Você cancelou a execução"); }
);
* /! *
`` `


Aqui, as funções são declaradas diretamente dentro da chamada `ask (...)`. Eles não têm nome, e assim são chamados * anônimos *. Tais funções não são acessíveis fora do `ask` (porque não são atribuídas a variáveis), mas é exatamente isso que desejamos aqui.

Esse código aparece em nossos scripts muito naturalmente, é no espírito do JavaScript.


`` `smart header =" Uma função é um valor que representa uma \ "ação \" "
Valores comuns, como strings ou números, representam o * data *.

Uma função pode ser percebida como uma * ação *.

Podemos passá-lo entre variáveis ​​e executar quando quisermos.
`` `


## Função expressão versus função Declaração

Vamos formular as principais diferenças entre Declarações de Função e Expressões.

Primeiro, a sintaxe: como ver o que é o que no código.

- * Declaração de função: * uma função, declarada como uma declaração separada, no fluxo de código principal.

`` `js
// Declaração de função
soma de função (a, b) {
Retornar a + b;
}
`` `
- * Função Expressão: * uma função, criada dentro de uma expressão ou dentro de outra construção de sintaxe.

Aqui, a função é criada no lado direito da "expressão de atribuição =":
`` `js
// Expressão de Função
permitir soma = função (a, b) {
Retornar a + b;
};
`` `

A diferença mais sutil é * quando * uma função é criada pelo mecanismo JavaScript.

** Uma expressão de função é criada quando a execução o atinge e é utilizável a partir daí. **

Uma vez que o fluxo de execução passa para o lado direito da tarefa `let sum = function ...` - aqui vamos, a função é criada e pode ser usada (atribuída, chamada etc) a partir de agora.

Declarações de função são diferentes.

** Uma declaração de função é utilizável em todo o bloco de script / código. **

Em outras palavras, quando o JavaScript * prepara * para executar o script ou um bloco de código, primeiro procura declarações de função nele e cria as funções. Podemos pensar nisso como um "estágio de inicialização".

E depois de todas as declarações de função serem processadas, a execução continua.

Como resultado, uma função declarada como Declaração de Função pode ser chamada mais cedo do que definida.

Por exemplo, isso funciona:

`` `js run refresh não confiável
*! *
sayHi ("John"); // Olá john
* /! *

função sayHi (name) {
alerta (`Hello, $ {name}`);
}
`` `

A Declaração de Função `sayHi` é criada quando o JavaScript está se preparando para iniciar o script e está visível em todos os lugares.

... Se fosse uma expressão de função, então não funcionaria:

`` `js run refresh não confiável
*! *
sayHi ("John"); // erro!
* /! *

deixe sayHi = função (nome) {// (*) nunca mais magia
alerta (`Hello, $ {name}`);
};
`` `

Função Expressões são criadas quando a execução as atinge. Isso aconteceria apenas na linha `(*)`. Muito tarde.

** Quando uma declaração de função é feita dentro de um bloco de código, é visível em todos os lugares dentro desse bloco. Mas não está fora disso. **

Às vezes, isso é útil para declarar uma função local necessária apenas nesse bloco. Mas esse recurso também pode causar problemas.

Por exemplo, imaginemos que precisamos declarar uma função `welcome ()` dependendo da variável `age` que recebemos em tempo de execução. E então planejamos usá-lo algum tempo depois.

O código abaixo não funciona:

`` `js run
let age = prompt ("Qual a sua idade?", 18);

// declara condicionalmente uma função
se (idade <18) {

function welcome () {

}

} outro {

function welcome () {
alerta ("Saudações!");
}

}

// ... use-o mais tarde
*! *
bem vinda(); // Erro: o bem-vindo não está definido
* /! *
`` `

Isso ocorre porque uma Declaração de Função só é visível dentro do bloco de código no qual reside.

Aqui está outro exemplo:

`` `js run
deixar idade = 16; // pegue 16 como exemplo

se (idade <18) {
*! *
bem vinda(); // \   (corre)
* /! *
// |
function welcome () {// |

} // | em todo o bloco onde é declarado
// |
*! *
bem vinda(); // /   (corre)
* /! *

} outro {

função bem-vinda () {// para idade = 16, esta "bem-vindo" nunca é criada
alerta ("Saudações!");
}
}

// Aqui estamos fora dos suportes da figura,
// então não podemos ver declarações de função feitas dentro delas.

*! *
bem vinda(); // Erro: o bem-vindo não está definido
* /! *
`` `

O que podemos fazer para tornar "bem-vindo" visível fora do `if`?

A abordagem correta seria usar uma expressão de função e atribuir `welcome` à variável que é declarada fora de` if` e tem a visibilidade adequada.

Agora funciona como pretendido:

`` `js run
let age = prompt ("Qual a sua idade?", 18);

deixe bem-vinda;

se (idade <18) {

bem-vindo = função () {

};

} outro {

bem-vindo = função () {
alerta ("Saudações!");
};

}

*! *
bem vinda(); // Certo, agora
* /! *
`` `

Ou podemos simplificar ainda mais usando um operador de ponto de interrogação `` `:

`` `js run
let age = prompt ("Qual a sua idade?", 18);

deixe bem = (idade <18)?

function () {alert ("Saudações!"); };

*! *
bem vinda(); // Certo, agora
* /! *
`` `


`` `smart header =" Quando escolher Function Declaration vs Function Expression? "
Como regra geral, quando precisamos declarar uma função, o primeiro a considerar é a sintaxe Declaração de Função, a que usamos antes. Dá mais liberdade em como organizar nosso código, porque podemos chamar essas funções antes de serem declaradas.

Também é um pouco mais fácil procurar "function f (...) {...}` no código do que `let f = function (...) {...}`. Declarações de função são mais "atraentes".

... Mas se uma Declaração de Função não nos convém por algum motivo (vimos um exemplo acima), então a Expressão de Função deve ser usada.
`` `


# Seta funções [# seta-funções]

Existe uma sintaxe muito simples e concisa para criar funções, muitas vezes melhor do que Expressões de Função. É chamado de "funções de seta", porque se parece com isto:


`` `js
deixe func = (arg1, arg2, ... argN) => expressão
`` `

... Isso cria uma função `func` que possui argumentos 'arg1..argN`, avalia a` expressão' no lado direito com seu uso e retorna seu resultado.

Em outras palavras, é aproximadamente o mesmo que:

`` `js
deixe func = função (arg1, arg2, ... argN) {
expressão de retorno;
}
`` `

... Mas muito mais conciso.

Vamos ver um exemplo:

`` `js run
Deixe sum = (a, b) => a + b;

/ * A função de seta é uma forma mais curta de:

permitir soma = função (a, b) {
Retornar a + b;
};
* /

alerta (soma (1, 2)); // 3

`` `

Se tivermos apenas um argumento, os parênteses podem ser omitidos, tornando isso ainda mais curto:

`` `js run
// igual a
// let double = function (n) {return n * 2}
*! *
Deixe duplo = n => n * 2;
* /! *

alerta (duplo (3)); // 6
`` `

Se não houver argumentos, os parênteses devem estar vazios (mas devem estar presentes):

`` `js run


SayHi ();
`` `

As funções de seta podem ser usadas da mesma maneira que as Expressões de Função.

Por exemplo, aqui está o exemplo reescrito com `welcome ()`:

`` `js run
let age = prompt ("Qual a sua idade?", 18);

deixe bem = (idade <18)?

() => alerta ("Saudações!");

bem vinda(); // Certo, agora
`` `

As funções de seta podem parecer desconhecidas e não muito legíveis no início, mas isso muda rapidamente à medida que os olhos se acostumam à estrutura.

Eles são muito convenientes para ações simples de uma linha, quando somos muito preguiçosos para escrever muitas palavras.

`` `cabeçalho inteligente =" Funções de seta de multilinha "

Os exemplos acima levaram argumentos da esquerda de `=>` e avaliaram a expressão do lado direito com eles.

Às vezes, precisamos de algo um pouco mais complexo, como expressões ou declarações múltiplas. Também é possível, mas devemos anexá-los em colchetes. Em seguida, use um "retorno" normal dentro deles.

Como isso:

`` `js run
let sum = (a, b) => {// o suporte da figura abre uma função multilinha
Deixe resultado = a + b;
*! *
resultado de retorno; // se usamos parênteses, use o retorno para obter resultados
* /! *
};

alerta (soma (1, 2)); // 3
`` `

`` `cabeçalho inteligente =" Mais para vir "
Aqui louvamos as funções das setas por brevidade. Mas isso não é tudo! As funções de seta têm outros recursos interessantes. Voltaremos a eles mais tarde no capítulo <info: arrow-functions>.

Por enquanto, já podemos usá-los para ações de linha única e callbacks.
`` `

## Resumo

- As funções são valores. Eles podem ser designados, copiados ou declarados em qualquer lugar do código.
- Se a função for declarada como uma declaração separada no fluxo do código principal, isso é chamado de "Declaração de Função".
- Se a função é criada como parte de uma expressão, ela é chamada de "Expressão de Função".
- As declarações de função são processadas antes do bloco de código ser executado. Eles são visíveis em todos os lugares do bloco.
- Função Expressões são criadas quando o fluxo de execução atinge-os.


Na maioria dos casos, quando precisamos declarar uma função, uma Declaração de Função é preferível, porque é visível antes da própria declaração. Isso nos dá mais flexibilidade na organização do código, e geralmente é mais legível.

Portanto, devemos usar uma Expressão de Função somente quando uma Declaração de Função não for adequada para a tarefa. Nós já vimos alguns exemplos disso neste capítulo e veremos mais no futuro.

As funções de seta são úteis para one-liners. Eles vêm em dois sabores:

1. Sem parênteses de figura: `(... args) => expressão` - o lado direito é uma expressão: a função o avalia e retorna o resultado.
2. Com parênteses de figura: `(... args) => {body}` - os brackets nos permitem escrever várias instruções dentro da função, mas precisamos de um "retorno" explícito para retornar algo.
