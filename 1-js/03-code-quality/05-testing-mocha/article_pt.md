# Testes automatizados com mocha

Os testes automatizados serão usados ​​em tarefas adicionais.

Na verdade, é parte do "mínimo educacional" de um desenvolvedor.

[cortar]

## Por que precisamos de testes?

Quando escrevemos uma função, geralmente podemos imaginar o que deveria fazer: quais parâmetros dão quais resultados.

Durante o desenvolvimento, podemos verificar a função executando-a e comparando o resultado com o esperado. Por exemplo, podemos fazer isso no console.

Se algo está errado - então nós corrigimos o código, corremos novamente, verificamos o resultado - e assim por diante até que ele funcione.

Mas tais "re-runs" manuais são imperfeitos.

** Ao testar um código por re-runs manual, é fácil perder alguma coisa. **

Por exemplo, estamos criando uma função `f`. Escreveu algum código, testando: `f (1)` funciona, mas `f (2)` não funciona. Nós corrigimos o código e agora `f (2)` funciona. Parece completo? Mas nós esquecemos de re-testar `f (1)`. Isso pode levar a um erro.

Isso é muito típico. Quando desenvolvemos algo, mantemos muitos casos de uso possíveis em mente. Mas é difícil esperar que um programador cheque todos eles manualmente após cada mudança. Então, torna-se fácil consertar uma coisa e quebrar outra.

** Testes automatizados significa que os testes são escritos separadamente, além do código. Eles podem ser executados facilmente e verificar todos os principais casos de uso. **

## Behavior Driven Development (BDD)

Vamos usar uma técnica chamada [Behavior Driven Development] (http://en.wikipedia.org/wiki/Behavior-driven_development) ou, em suma, BDD. Essa abordagem é usada entre muitos projetos. O BDD não é apenas um teste. Isso é mais.

** BDD é três coisas em um: testes E documentação E exemplos. **

Bastante palavras. Vejamos o exemplo.

## Desenvolvimento de "pow": a especificação

Digamos que queremos fazer uma função `pow (x, n)` que eleva `x` para uma potência inteira 'n`. Assumimos que `n≥0`.

Essa tarefa é apenas um exemplo: há o operador `**` em JavaScript que pode fazer isso, mas aqui nos concentramos no fluxo de desenvolvimento que pode ser aplicado a tarefas mais complexas também.

Antes de criar o código de `pow`, podemos imaginar o que a função deve fazer e descrevê-lo.

Essa descrição é chamada de * especificação * ou, em suma, uma especificação, e se parece com isto:

`` `js
descreva ("pow", function () {

("eleva a n-ésima potência", função () {
assert.equal (pow (2, 3), 8);
});

});
`` `

Uma especificação tem três blocos de construção principais que você pode ver acima:

`descreva (" título ", função () {...})`
: Qual a funcionalidade que estamos descrevendo. Usos para agrupar "trabalhadores" - o `it` bloqueia. No nosso caso, estamos descrevendo a função `pow`.

`it (" title ", function () {...})`
: No título de `it` nós * de forma humanamente legível * descrevemos o caso de uso particular, e o segundo argumento é uma função que o testa.

`assert.equal (value1, value2)`
: O código dentro do bloco `it`, se a implementação estiver correta, deve ser executado sem erros.

As funções `assert. *` São usadas para verificar se `pow` funciona conforme o esperado. Aqui estamos usando um deles - 'assert.equal`, ele compara argumentos e produz um erro se não forem iguais. Aqui verifica se o resultado de `pow (2, 3)` é igual a `8`.

Existem outros tipos de comparações e verificações que veremos mais.

## O fluxo de desenvolvimento

O fluxo de desenvolvimento geralmente se parece com isto:

1. Uma especificação inicial é escrita, com testes para a funcionalidade mais básica.
2. Uma implementação inicial é criada.
3. Para verificar se funciona, executamos a estrutura de testes [Mocha] (http://mochajs.org/) (mais detalhes em breve) que executa as especificações. Os erros são exibidos. Fazemos correções até que tudo funcione.
4. Agora, temos uma implementação inicial inicial com testes.
5. Adicionamos mais casos de uso à especificação, provavelmente ainda não suportada pelas implementações. Os testes começam a falhar.
6. Vá para 3, atualize a implementação até que os testes não forneçam erros.
7. Repita as etapas 3-6 até que a funcionalidade esteja pronta.

Assim, o desenvolvimento é * iterativo *. Nós escrevemos as especificações, implementamos, asseguramos que os testes passem, então escreva mais testes, assegure-se de que eles funcionem, etc. No final, temos uma implementação e testes funcionais para isso.

No nosso caso, o primeiro passo está completo: temos uma especificação inicial para `pow`. Então, vamos fazer uma implementação. Mas antes disso, vamos fazer uma corrida "zero" da especificação, apenas para ver que os testes estão funcionando (todos falharão).

## The spec in action

Aqui no tutorial, estaremos usando as seguintes bibliotecas de JavaScript para testes:

- [Mocha] (http://mochajs.org/) - o framework central: fornece funções de teste comuns, incluindo `describe` e` it` e a função principal que executa testes.
- [Chai] (http://chaijs.com) - a biblioteca com muitas afirmações. Ele permite usar muitas afirmações diferentes, por enquanto precisamos apenas de 'assert.equal`.
- [Sinon] (http://sinonjs.org/) - uma biblioteca para espiar funções, emular funções integradas e muito mais, precisaremos muito mais tarde.

Essas bibliotecas são adequadas para testes no navegador e no servidor. Aqui vamos considerar a variante do navegador.

A página HTML completa com esses frameworks e especificações `pow`:

`` `html src =" index.html "
`` `

A página pode ser dividida em quatro partes:

1. O `<head>` - adicione bibliotecas e estilos de terceiros para testes.
2. O `<script>` com a função para testar, no nosso caso - com o código para `pow`.
3. Os testes - no nosso caso, um script externo `test.js` que descreveu (" pow ", ...)" de cima.
4. O elemento HTML `<div id =" mocha ">` será usado pela Mocha para produzir resultados.
5. Os testes são iniciados pelo comando `mocha.run ()`.

O resultado:

[iframe height = 250 src = "pow-1" border = 1 edit]

A partir de agora, o teste falha, há um erro. Isso é lógico: temos um código de função vazio em `pow`, então` pow (2,3) `retorna` undefined` em vez de `8`.

Para o futuro, notemos que existem avançados test-runners, como [karma] (https://karma-runner.github.io/) e outros. Portanto, geralmente não é um problema para configurar muitos testes diferentes.

## Implementação inicial

Vamos fazer uma implementação simples de `pow`, para que os testes passem:

`` `js
function pow () {
retorno 8; // :) nós enganamos!
}
`` `

Uau, agora funciona!

[iframe height = 250 src = "pow-min" border = 1 edit]

## Melhorando a especificação

O que fizemos é definitivamente uma fraude. A função não funciona: uma tentativa de calcular `pow (3,4)` daria um resultado incorreto, mas os testes passam.

... Mas a situação é bastante típica, acontece na prática. Os testes passam, mas a função funciona de forma errada. Nossa especificação é imperfeita. Precisamos adicionar mais casos de uso a ele.

Vamos adicionar mais um teste para ver se `pow (3, 4) = 81`.

Podemos selecionar uma das duas maneiras de organizar o teste aqui:

1. A primeira variante - adicione mais um "assert" no mesmo `it`:

`` `js
descreva ("pow", function () {

("eleva a n-ésima potência", função () {
assert.equal (pow (2, 3), 8);
*! *
assert.equal (pow (3, 4), 81);
* /! *
});

});
`` `
2. O segundo - faça dois testes:

`` `js
descreva ("pow", function () {

("2 elevado ao poder 3 é 8", função () {
assert.equal (pow (2, 3), 8);
});

("3 elevado ao poder 3 é 27", função () {
assert.equal (pow (3, 3), 27);
});

});
`` `

A principal diferença é que quando "assert" desencadeia um erro, o bloco `it` termina imediatamente. Então, na primeira variante, se o primeiro "afirmativo" falhar, nunca veremos o resultado do segundo "afirmativo".

Fazer testes separados é útil para obter mais informações sobre o que está acontecendo, então a segunda variante é melhor.

E além disso, há uma regra mais que é bom seguir.

** Um teste verifica uma coisa. **

Se olharmos para o teste e ver duas verificações independentes, é melhor dividi-lo em dois mais simples.

Então, vamos continuar com a segunda variante.

O resultado:

[iframe height = 250 src = "pow-2" editar border = "1"]

Como poderíamos esperar, o segundo teste falhou. Claro, nossa função sempre retorna `8`, enquanto o` assert` espera '27'.

## Melhorando a implementação

Vamos escrever algo mais real para que os testes passem:

`` `js
função pow (x, n) {
Deixe o resultado = 1;

para (vamos i = 0; i <n; i ++) {
resultado * = x;
}

resultado de retorno;
}
`` `

Para ter certeza de que a função funciona bem, vamos testá-lo para obter mais valores. Em vez de escrever `it` blocos manualmente, podemos gerá-los no` for`:

`` `js
descreva ("pow", function () {

function makeTest (x) {
Vamos esperar = x * x * x;
(`$ {x} no poder 3 é $ {esperado}`, função () {
assert.equal (pow (x, 3), esperado);
});
}

para (vamos x = 1; x <= 5; x ++) {
makeTest (x);
}

});
`` `

O resultado:

[iframe height = 250 src = "pow-3" editar border = "1"]

## Nested describe

Vamos adicionar mais testes. Mas antes disso, vejamos que a função helper `makeTest` e` for` devem ser agrupadas. Não precisamos de `makeTest` em outros testes, é necessário apenas em` for`: sua tarefa comum é verificar como `pow` aumenta o poder dado.

O agrupamento é feito com um `descrito` aninhado:

`` `js
descreva ("pow", function () {

*! *
descreva ("levanta x para power n", function () {
* /! *

function makeTest (x) {
Vamos esperar = x * x * x;
(`$ {x} no poder 3 é $ {esperado}`, função () {
assert.equal (pow (x, 3), esperado);
});
}

para (vamos x = 1; x <= 5; x ++) {
makeTest (x);
}

*! *
});
* /! *

// ... mais testes para seguir aqui, ambos descrevem e podem ser adicionados
});
`` `

O "descrição" aninhado define um novo "subgrupo" de testes. Na saída, podemos ver o recuo do título:

[iframe height = 250 src = "pow-4" editar border = "1"]

No futuro, podemos adicionar mais `it` e` descrever 'no nível superior com as funções auxiliares próprias, eles não verão `makeTest`.

`` `` smart header = "` before / after` e `beforeEach / afterEach`"
Podemos configurar as funções `before / after` que executam antes / depois de executar testes, e também` beforeEach / afterEach` funções que executam antes / depois de * cada * `it`.

Por exemplo:

`` `js no-embellecer
descreva ("teste", função () {




beforeEach (() => alert ("Antes de um teste - insira um teste"));
afterEach (() => alert ("Após um teste - saia de um teste"));




});
`` `

A sequência de execução será:

`` `
Testes iniciados - antes de todos os testes (antes)
Antes de um teste - insira um teste (antes de cada)
1
Após um teste - saia de um teste (depois de cada)
Antes de um teste - insira um teste (antes de cada)
2
Após um teste - saia de um teste (depois de cada)
Teste concluído - após todas as provas (depois)
`` `

[edit src = "beforeafter" title = "Abra o exemplo na caixa de areia."]

Geralmente, `beforeEach / afterEach` (` before / each`) são usados ​​para executar inicialização, contadores fora de zero ou fazer outra coisa entre os testes (ou grupos de teste).
`` ``

## Estendendo a especificação

A funcionalidade básica do `pow` está completa. A primeira iteração do desenvolvimento é feita. Quando terminarmos de celebrar e beber champanhe - vamos continuar e melhorar.

Como foi dito, a função `pow (x, n)` deve funcionar com valores inteiros positivos n.

Para indicar um erro matemático, as funções JavaScript geralmente retornam `NaN`. Vamos fazer o mesmo por valores inválidos de `n`.

Vamos primeiro adicionar o comportamento à especificação (!):

`` `js
descreva ("pow", function () {

// ...

("para negativo n o resultado é NaN", function () {
*! *
assert.isNaN (pow (2, -1));
* /! *
});

("for non-enterger n o resultado é NaN", function () {
*! *
assert.isNaN (Pow (2, 1.5));
* /! *
});

});
`` `

O resultado com novos testes:

[iframe height = 530 src = "pow-nan" editar border = "1"]

Os testes recém-adicionados falham, porque nossa implementação não os suporta. É assim que o BDD é feito: primeiro escrevemos testes de falha e, em seguida, criamos uma implementação para eles.

`` `cabeçalho inteligente =" Outras afirmações "

Por favor, note a afirmação `assert.isNaN`: verifica` NaN`.

Há também outras afirmações em Chai, por exemplo:

- `assert.equal (value1, value2)` - verifica a equivalência `value1 == value2`.
- `assert.strictEqual (value1, value2)` - verifica a igualdade rigorosa `value1 === value2`.
- `assert.notEqual`,` assert.notStrictEqual` - cheques inversos aos acima.
- `assert.isTrue (value)` - verifica que `value === true`
- `assert.isFalse (value)` - verifica que `value === false`
- ... a lista completa está nos [docs] (http://chaijs.com/api/assert/)
`` `

Então, devemos adicionar algumas linhas para `pow`:

`` `js
função pow (x, n) {
*! *
se (n <0) retornar NaN;
se (Math.round (n)! = n) retornar NaN;
* /! *

Deixe o resultado = 1;

para (vamos i = 0; i <n; i ++) {
resultado * = x;
}

resultado de retorno;
}
`` `

Agora funciona, todos os exames passam:

[iframe height = 300 src = "pow-full" editar border = "1"]

[edit src = "pow-full" title = "Abra o exemplo final completo na caixa de areia."]

## Resumo

No BDD, a especificação é primeira, seguida da implementação. No final, temos as especificações e o código.

As especificações podem ser usadas de três maneiras:

1. ** Testes ** garantem que o código funcione corretamente.
2. ** Docs ** - os títulos de `descrever 'e` it` dizem o que a função faz.
3. ** Exemplos ** - os testes são realmente exemplos funcionais mostrando como uma função pode ser usada.

Com a especificação, podemos melhorar, mudar, até mesmo reescrever a função do zero e garantir que ela ainda funcione corretamente.

Isso é especialmente importante em grandes projetos quando uma função é usada em muitos lugares. Quando mudamos essa função, não há como verificar manualmente se todos os locais que a utilizam ainda funcionam corretamente.

Sem testes, as pessoas têm duas maneiras:

1. Para executar a mudança, não importa o quê. E então, nossos usuários conhecem os erros e relatá-los. Se pudermos pagar isso.
2. Ou as pessoas têm medo de modificar essas funções, se a punição por erros for dura. Então, torna-se velho, coberto de teias de aranha, ninguém quer entrar nele, e isso não é bom.

** O código testado automaticamente é contrário a isso! **

Se o projeto estiver coberto por testes, não existe esse problema. Podemos executar testes e ver muitos verificações feitas em questão de segundos.

** Além disso, um código bem testado possui melhor arquitetura. **

Naturalmente, é porque é mais fácil mudar e melhorar. Mas não só isso.

Para escrever testes, o código deve ser organizado de tal forma que cada função tenha uma tarefa claramente descrita, entrada e saída bem definidas. Isso significa uma boa arquitetura desde o início.

Na vida real, às vezes não é tão fácil. Às vezes, é difícil escrever uma especificação antes do código real, porque ainda não está claro como deve se comportar. Mas, em geral, os testes de escrita tornam o desenvolvimento mais rápido e estável.

## E agora?

Mais tarde, no tutorial, você encontrará muitas tarefas com testes cozidos. Então você verá exemplos mais práticos.

Testes de escrita requer um bom conhecimento de JavaScript. Mas estamos apenas começando a aprender. Então, para resolver tudo, até agora você não precisa escrever testes, mas você já deve ser capaz de lê-los, mesmo que seja um pouco mais complexo do que neste capítulo.
