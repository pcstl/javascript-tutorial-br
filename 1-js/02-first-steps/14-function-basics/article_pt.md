# Funções

Muitas vezes, precisamos realizar uma ação similar em muitos lugares do script.

Por exemplo, precisamos mostrar uma mensagem agradável quando um visitante faz logon, faz logon e talvez em outro lugar.

As funções são os principais "blocos de construção" do programa. Eles permitem que o código seja chamado muitas vezes sem repetição.

[cortar]

Já vimos exemplos de funções integradas, como "alerta (mensagem)", "prompt (mensagem, padrão)" e "confirmar (pergunta)". Mas também podemos criar funções próprias.

## Declaração de função

Para criar uma função, podemos usar uma * declaração de função *.

Parece assim:

`` `js
function showMessage () {

}
`` `

A palavra-chave `function` vai primeiro e depois o * nome da função *, então uma lista de * parâmetros * nos colchetes (vazio no exemplo acima) e, finalmente, o código da função, também denominado" corpo da função " .

!] [] (function_basics.png)

Nossa nova função pode ser chamada pelo nome: `showMessage ()`.

Por exemplo:

`` `js run
function showMessage () {

}

*! *
Mostrar mensagem();
Mostrar mensagem();
* /! *
`` `

A chamada `showMessage ()` executa o código da função. Aqui vamos ver a mensagem duas vezes.

Este exemplo demonstra claramente um dos principais propósitos das funções: evadir a duplicação de código.

Se alguma vez precisamos alterar a mensagem ou a maneira como ela é mostrada, basta modificar o código em um só lugar: a função que o exibe.

## Variáveis ​​locais

Uma variável declarada dentro de uma função só é visível dentro dessa função.

Por exemplo:

`` `js run
function showMessage () {
*! *
Deixe a mensagem = "Olá, eu sou JavaScript!"; // variável local
* /! *

alerta (mensagem);
}

Mostrar mensagem(); // Olá, eu sou JavaScript!

alerta (mensagem); // <- Erro! A variável é local para a função
`` `

## Variáveis ​​externas

Uma função também pode acessar uma variável externa, por exemplo:

`` `js run no-embellecer
deixe *! * userName * /! * = 'John';

function showMessage () {
Deixe a mensagem = 'Olá,' + *! * userName * /! *;
alerta (mensagem);
}

Mostrar mensagem(); // Olá john
`` `

A função possui acesso total à variável externa. Ele também pode modificá-lo.

Por exemplo:

`` `js run
deixe *! * userName * /! * = 'John';

function showMessage () {
*! * userName * /! * = "Bob"; // (1) alterou a variável externa

Deixe a mensagem = 'Olá,' + *! * userName * /! *;
alerta (mensagem);
}

alerta (userName); // *! * John * /! * Antes da chamada de função

Mostrar mensagem();

alerta (userName); // *! * Bob * /! *, O valor foi modificado pela função
`` `

A variável externa só é usada se não houver local. Portanto, uma modificação ocasional pode acontecer se esquecermos de "deixar".

Se uma mesma variável nomeada for declarada dentro da função, então * * sombras * a externa. Por exemplo, no código abaixo, a função usa o `nome do usuário 'local. O externo é ignorado:

`` `js run
Deixe UserName = 'John';

function showMessage () {
*! *
Deixe UserName = "Bob"; // declara uma variável local
* /! *

Deixe mensagem = 'Olá,' + userName; // *! * Bob * /! *
alerta (mensagem);
}

// a função criará e usará seu próprio nome de usuário
Mostrar mensagem();

alerta (userName); // *! * John * /! *, Inalterado, a função não acessou a variável externa
`` `

`` `cabeçalho inteligente =" Variáveis ​​globais "
As variáveis ​​declaradas fora de qualquer função, como o "nome do usuário" externo no código acima, são chamadas de * global *.

As variáveis ​​globais são visíveis a partir de qualquer função (a menos que sejam exibidas pelos locais).

Geralmente, uma função declara todas as variáveis ​​específicas de sua tarefa, e as variáveis ​​globais apenas armazenam dados do nível do projeto, tão importante que ele realmente deve ser visto em qualquer lugar. O código moderno tem poucos ou nenhuns globais. A maioria das variáveis ​​residem em suas funções.
`` `

## Parameters

Podemos passar dados arbitrários para funções usando parâmetros (também chamados de * argumentos de função *).

No exemplo abaixo, a função tem dois parâmetros: `from` e` text`.

`` `js run
function showMessage (*! * from, text * /! *) {// argumentos: de, texto
alerta (de + ':' + texto);
}

*! *
showMessage ('Ann', 'Hello!'); // Ann: Olá! (*)
showMessage ('Ann', "What's Up?"); // Ann: o que há de novo? (**)
* /! *
`` `

Quando a função é chamada em linhas `(*)` e `(**)`, os valores dados são copiados para variáveis ​​locais `de` e` texto`. Então a função os usa.

Aqui está mais um exemplo: temos uma variável `from` e passamos para a função. Observe: a função muda `de`, mas a mudança não é vista fora, porque uma função sempre recebe uma cópia do valor:


`` `js run
function showMessage (from, text) {

*! *
de = '*' + de + '*'; // make "from" look kinder
* /! *

alerta (de + ':' + texto);
}

Deixe de = "Ann";

showMessage (from, "Hello"); // * Ann *: Olá

// o valor de "de" é o mesmo, a função modificou uma cópia local
alerta (de); // Ann
`` `

## Valores padrão

Se um parâmetro não for fornecido, seu valor se torna "indefinido".

Por exemplo, a função "showMessage (from, text)` acima mencionada pode ser chamada com um único argumento:

`` `js
showMessage ("Ann");
`` `

Isso não é um erro. Essa chamada emitiria `" Ann: indefinido ". Não há `texto ', então é assumido que` text === indefinido`.

Se quisermos usar um "texto" padrão "" neste caso, podemos especificá-lo depois de `=`:

`` `js run
function showMessage (from, *! * text = "no text given" * /! *) {
alerta (de + ":" + texto);
}

showMessage ("Ann"); // Ann: nenhum texto dado
`` `

Agora, se o parâmetro `text` não for passado, ele receberá o valor` "no text given" `

Aqui "nenhum texto fornecido" é uma string, mas pode ser uma expressão mais complexa, que só é avaliada e atribuída se o parâmetro estiver faltando. Então, isso também é possível:

`` `js run
function showMessage (from, text = anotherFunction ()) {
// anotherFunction () apenas executado se nenhum texto dado
// seu resultado se torna o valor do texto
}
`` `


`` `` cabeçalho inteligente = "Parâmetros padrão de estilo antigo"
As antigas edições do JavaScript não suporta parâmetros padrão. Portanto, há maneiras alternativas de apoiá-los, que você pode encontrar principalmente nos scripts antigos.

Por exemplo, uma verificação explícita para ser `indefinido ':

`` `js
function showMessage (from, text) {
*! *
se (texto === indefinido) {
texto = 'não dado texto';
}
* /! *

alerta (de + ":" + texto);
}
`` `

... Ou o operador `||`:

`` `js
function showMessage (from, text) {
// se o texto for falso, o texto obtém o valor "padrão"
texto = texto || 'nenhum texto dado';
...
}
`` `


`` ``


## Retornando um valor

Uma função pode retornar um valor ao código de chamada como resultado.

O exemplo mais simples seria uma função que somasse dois valores:

`` `js run no-embellecer
soma de função (a, b) {
*! * retorno * /! * a + b;
}

Deixe o resultado = soma (1, 2);
alerta (resultado); // 3
`` `

A directiva `return` pode estar em qualquer lugar da função. Quando a execução o atinge, a função pára e o valor é retornado ao código de chamada (atribuído ao `resultado 'acima).

Pode haver muitas ocorrências de `return` em uma única função. Por exemplo:

`` `js run
função checkAge (age) {
se (idade> 18) {
*! *
retornar verdadeiro;
* /! *
} outro {
*! *
retorno confirmar ('Obter uma permissão dos pais?');
* /! *
}
}

let age = prompt ('Quantos anos você tem?', 18);

se (checkAge (idade)) {
alerta ('Acesso concedido');
} outro {
alerta ('Acesso negado');
}
`` `

É possível usar `return` sem um valor. Isso faz com que a função saia imediatamente.

Por exemplo:

`` `js
show de funçãoMovie (idade) {
se (! checkAge (idade)) {
*! *
Retorna;
* /! *
}

alerta ("Mostrando o filme"); // (*)
// ...
}
`` `

No código acima, se `checkAge (age)` retornar `false`, então` showMovie` não irá para o `alert`.

`` `` cabeçalho inteligente = "uma função com um" retorno "vazio ou sem retorna` indefinido '
Se uma função não retornar um valor, é o mesmo que se retornar `undefined`:

`` `js run
function doNothing () {/ * empty * /}

alerta (doNothing () === indefinido); // verdade
`` `

Um "retorno" vazio também é o mesmo que `return undefined`:

`` `js run
function doNothing () {
Retorna;
}

alerta (doNothing () === indefinido); // verdade
`` `
`` ``

`` `` warn header = "Nunca adicione uma nova linha entre` return` e o valor "
Para uma expressão longa no "retorno", pode ser tentador colocá-lo em uma linha separada, como esta:

`` `js
Retorna
(alguns + expressão longa + + ou + qualquer * f (a) + f (b))
`` `
Isso não funciona, porque o JavaScript assume um ponto e vírgula após o "retorno". Isso funcionará da mesma forma que:

`` `js
Retorna*!*;*/!*
(alguns + expressão longa + + ou + qualquer * f (a) + f (b))
`` `
Então, ele se torna efetivamente um retorno vazio. Em vez disso, devemos colocar o valor na mesma linha.
`` ``

## Nomeando uma função [# nome-função]

As funções são ações. Então, seu nome é geralmente um verbo. Deve brevemente, mas com a maior precisão possível, descreva o que a função faz. Para que uma pessoa que lê o código obtém a pista certa.

É uma prática generalizada para iniciar uma função com um prefixo verbal que descreve vagamente a ação. Deve haver um acordo dentro da equipe sobre o significado dos prefixos.

Por exemplo, as funções que começam com `` show '' geralmente mostram algo.

Função começando com ...

- `" get ... "` - devolver um valor,
- `` calc ... '' - calcula algo,
- "crie ..." - crie algo,
- "verifique ..." - verifique algo e retorne um booleano, etc.

Exemplos de tais nomes:

`` `js no-embellecer
showMessage (...) // mostra uma mensagem
getAge (...) // retorna a idade (obtém de alguma forma)
calcSum (...) // calcula uma soma e retorna o resultado
createForm (...) // cria um formulário (e geralmente o retorna)
checkPermission (...) // verifica uma permissão, retorna true / false
`` `

Com prefixos no lugar, um olhar sobre um nome de função dá uma compreensão do tipo de trabalho que ele faz e que tipo de valor ele retorna.

`` `cabeçalho inteligente =" Uma função - uma ação "
Uma função deve fazer exatamente o que é sugerido pelo seu nome, não mais.

Duas ações independentes geralmente merecem duas funções, mesmo que geralmente sejam chamadas em conjunto (nesse caso, podemos fazer uma 3ª função que as chama).

Alguns exemplos de quebrar esta regra:

- `getAge` - seria ruim se mostrar um 'alerta' com a idade (só deve ser obtido).
- `createForm` - seria ruim se modificasse o documento, adicionando-lhe um formulário (deve apenas criá-lo e retornar).
- `checkPermission` - seria ruim se exibir a mensagem` acesso concedido / negado` (deve apenas executar a verificação e retornar o resultado).

Esses exemplos assumem significados comuns de prefixos. O que eles significam para você é determinado por você e sua equipe. Talvez seja bastante normal que seu código se comporte de forma diferente. Mas você deve ter uma compreensão firme do que significa um prefixo, o que uma função prefixada pode e não pode fazer. Todas as funções de mesmo prefixo devem obedecer as regras. E a equipe deve compartilhar o conhecimento.
`` `

`` `cabeçalho inteligente =" nomes de função Ultrashort "
Funções que são usadas * muitas vezes * às vezes possuem nomes ultrashort.

Por exemplo, a estrutura [jQuery] (http://jquery.com) define uma função `$`, a biblioteca [LoDash] (http://lodash.com/) tem a função principal chamada `_`.

Estas são exceções. Geralmente as funções nomes devem ser concisas, mas descritivas.
`` `

## Funções == Comentários

As funções devem ser curtas e fazer exatamente uma coisa. Se essa coisa é grande, talvez valha a pena dividir a função em algumas funções menores. Às vezes, seguir esta regra pode não ser tão fácil, mas definitivamente é uma coisa boa.

Uma função separada não é apenas mais fácil de testar e depurar - sua própria existência é um excelente comentário!

Por exemplo, compare as duas funções `showPrimes (n)` abaixo. Cada um produz [números primos] (https://en.wikipedia.org/wiki/Prime_number) até `n`.

A primeira variante usa um rótulo:

`` `js
show de funçãoPrimes (n) {
nextPrime: para (deixe i = 2; i <n; i ++) {

para (let j = 2; j <i; j ++) {
se (i% j == 0) continue nextPrime;
}

alerta (i); // um primeiro
}
}
`` `

A segunda variante usa uma função adicional `isPrime (n)` para testar a prioridade:

`` `js
show de funçãoPrimes (n) {

para (deixe i = 2; i <n; i ++) {
*! * if (! isPrime (i)) continue; * /! *

alerta (i); // um primeiro
}
}

function isPrime (n) {
para (deixe i = 2; i <n; i ++) {
se (n% i == 0) retornar falso;
}
retornar verdadeiro;
}
`` `

A segunda variante é mais fácil de entender, não é? Em vez da peça de código, vemos um nome da ação (`isPrime`). Às vezes, as pessoas se referem a um código como * auto-descrevendo *.

Assim, as funções podem ser criadas, mesmo que não pretendamos reutilizá-las. Eles estruturam o código e tornam-no legível.

## Resumo

Uma declaração de função parece assim:

`` `js
nome da função (parâmetros, delimitados, por, vírgula) {
/ * código * /
}
`` `

- Os valores passados ​​para uma função como parâmetros são copiados para suas variáveis ​​locais.
- Uma função pode acessar variáveis ​​externas. Mas funciona apenas de dentro para fora. O código fora da função não vê suas variáveis ​​locais.
- Uma função pode retornar um valor. Se não for, o resultado é "indefinido".

Para tornar o código limpo e fácil de entender, é recomendável usar principalmente variáveis ​​e parâmetros locais na função, e não as variáveis ​​externas.

É sempre mais fácil entender uma função que obtém parâmetros, trabalha com eles e retorna um resultado que uma função que não possui parâmetros, mas modifica as variáveis ​​externas como efeito colateral.

Nomenclatura da função:

- Um nome deve descrever claramente qual é a função. Quando vemos uma chamada de função no código, um bom nome instantaneamente nos dá um entendimento sobre o que ele faz e retorna.
- Uma função é uma ação, então os nomes das funções geralmente são verbais.
- Existem muitos prefixos de funções bem conhecidos como `create ...`, `show ...`, `get ...`, `check ...` e assim por diante. Use-os para sugerir o que uma função faz.

As funções são os principais blocos de construção dos scripts. Agora, cobrimos o básico, então podemos começar a criá-los e usá-los. Mas esse é apenas o início do caminho. Vamos voltar para eles muitas vezes, indo mais profundamente em seus recursos avançados.
