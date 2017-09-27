
Vamos examinar o que é feito dentro do `makeArmy`, e a solução ficará óbvia.

1. Ele cria uma matriz vazia `atiradores ':

`` `js
Deixe os atiradores = [];
`` `
2. Preenche o loop via `shooters.push (function ...)`.

Cada elemento é uma função, então a matriz resultante parece assim:

`` `js no-embellecer
atiradores = [
function () {alert (i); },
function () {alert (i); },
function () {alert (i); },
function () {alert (i); },
function () {alert (i); },
function () {alert (i); },
function () {alert (i); },
function () {alert (i); },
function () {alert (i); },
function () {alert (i); }
];
`` `

3. A matriz é retornada da função.

Então, mais tarde, a chamada para `army [5] ()` obterá o elemento `army [5]` da matriz (será uma função) e ligá-lo-á.

Agora, por que todas essas funções mostram o mesmo?

Isso ocorre porque não há nenhuma variável local `i` dentro de funções de" disparador ". Quando essa função é chamada, leva `i` do seu ambiente lexical externo.

Qual será o valor de `i`?

Se olharmos para a fonte:

`` `js
function makeArmy () {
...
deixe i = 0;
enquanto (i <10) {
deixe shooter = function () {// função do atirador
alerta (i); // deve mostrar seu número
};
...
}
...
}
`` `

... Podemos ver que ele vive no ambiente lexical associado à atual corrida `makeArmy ()`. Mas quando `army [5] ()` é chamado, `makeArmy` já terminou seu trabalho e` i` tem o último valor: `10` (o fim de` while`).

Como resultado, todas as funções `shooter` obtêm do ambiente lexical externo o mesmo, último valor` i = 10`.

A correção pode ser muito simples:

`` `js run
function makeArmy () {

Deixe os atiradores = [];

*! *
para (vamos i = 0; i <10; i ++) {
* /! *
deixe shooter = function () {// função do atirador
alerta (i); // deve mostrar seu número
};
shooters.push (atirador);
}

Tiradores de retorno;
}

Deixe army = makeArmy ();

exército [0] (); // 0
exército [5] (); // 5
`` `

Agora ele funciona corretamente, porque cada vez que o bloco de código em `for (..) {...}` é executado, um novo ambiente Lexical é criado para ele, com o valor correspondente de `i`.

Então, o valor de `i` agora mora um pouco mais perto. Não está no ambiente lógico MakeArmy () `, mas no ambiente Lexical que corresponde à iteração do ciclo atual. Um "atirador" obtém o valor exatamente de onde foi criado.

!] [] (lexenv-makearmy.png)

Aqui nós reescrevemos `while` em` for`.

Outro truque pode ser possível, vejamos isso para uma melhor compreensão do assunto:


`` `js run
function makeArmy () {
Deixe os atiradores = [];

deixe i = 0;
enquanto (i <10) {
*! *
deixe j = i;
* /! *
deixe shooter = function () {// função do atirador
alerta (*! * j * /! *); // deve mostrar seu número
};
shooters.push (atirador);
i ++;
}

Tiradores de retorno;
}

Deixe army = makeArmy ();

exército [0] (); // 0
exército [5] (); // 5
`` `

O loop `while`, assim como` for`, faz um novo ambiente Lexical para cada execução. Então, asseguremos que ele obtenha o valor certo para um "atirador".

Copiamos `let j = i`. Isso faz um corpo de loop local `j` e copia o valor de` i` para ele. Primitivas são copiadas "por valor", então nós realmente recebemos uma cópia independente completa de `i`, pertencente à iteração de loop atual.
