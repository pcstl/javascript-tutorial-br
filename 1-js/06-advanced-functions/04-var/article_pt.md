
# O antigo "var"

No primeiro capítulo sobre [variáveis] (info: variáveis), mencionamos três maneiras de declaração de variável:

1. `let`
2. `const`
3. `var`

`let` e` const` se comportam exatamente da mesma maneira em termos de ambientes Lexical.

Mas `var 'é uma fera muito diferente, que se origina em tempos muito antigos. Geralmente não é usado em scripts modernos, mas ainda se esconde nos antigos.

Se você não planeja atender a esses scripts, pode ignorar este capítulo ou adiar, mas há uma chance de o morrer mais tarde.

[cortar]

Da primeira visão, `var` se comporta de forma semelhante a` let`. Ou seja, declara uma variável:

`` `js run
função sayHi () {
var phrase = "Hello"; // variável local, "var" em vez de "deixar"

alerta (frase); // Olá
}

SayHi ();

alerta (frase); // Erro, a frase não está definida
`` `

... Mas aqui estão as diferenças.

## "var" não possui um escopo de bloco

As variáveis ​​`var` são de toda a função ou globais, elas são visíveis através de blocos.

Por exemplo:

`` `js
se for verdade) {
var test = true; // use "var" em vez de "deixar"
}

*! *

* /! *
`` `

Se usássemos `let test 'na 2ª linha, então não seria visível para` alert`. Mas `var 'ignora blocos de código, então nós temos um" teste "global.

O mesmo para loops: `var` não pode ser bloqueado ou loop-local:

`` `js
para (onde i = 0; i <10; i ++) {
// ...
}

*! *
alerta (i); // 10, "i" é visível após loop, é uma variável global
* /! *
`` `

Se um bloco de código dentro de uma função, o `var` se torna uma variável de nível de função:

`` `js
função sayHi () {
se for verdade) {
var phrase = "Hello";
}

alerta (frase); // trabalho
}

SayHi ();
alerta (frase); // Erro: a frase não está definida
`` `

Como podemos ver, `var` perfura através de` if`, `for` ou outros blocos de código. Isso porque há muito tempo em blocos de JavaScript não havia Ambientes Lexicos. E `var` é uma reminiscência disso.

## "var" são processados ​​no início da função

As declarações `var` são processadas quando a função é iniciada (ou o script é iniciado para globals).

Em outras palavras, as variáveis ​​`var` são definidas desde o início da função, independentemente de onde a definição for (supondo que a definição não esteja na função aninhada).

Então, este código:

`` `js
função sayHi () {
frase = "Olá";

alerta (frase);

*! *
frase var;
* /! *
}
`` `

... É tecnicamente o mesmo que isso (mudou `var phrase` acima):

`` `js
função sayHi () {
*! *
frase var;
* /! *

frase = "Olá";

alerta (frase);
}
`` `

... Ou mesmo assim (lembre-se, os blocos de código são ignorados):

`` `js
função sayHi () {
frase = "Olá"; // (*)

*! *
se (falso) {
frase var;
}
* /! *

alerta (frase);
}
`` `

As pessoas também chamam esse comportamento de "elevação" (levantamento), porque todos os `var 'são" içados "(levantados) no topo da função.

Então, no exemplo acima, `se (falso)` ramo nunca é executado, mas isso não importa. O `var 'dentro dele é processado no início da função, então, no momento de` (*) `a variável existe.

** Declarações são içadas, mas as atribuições não são. **

É melhor demonstrar com um exemplo, assim:

`` `js run
função sayHi () {
alerta (frase);

*! *
var phrase = "Hello";
* /! *
}

SayHi ();
`` `

A linha `var phrase =" Hello "` tem duas ações nele:

1. Declaração variável `var`
2. Atribuição de variáveis ​​`=`.

A declaração é processada no início da execução da função ("ôntida"), mas a tarefa sempre funciona no local onde aparece. Então, o código funciona essencialmente assim:

`` `js run
função sayHi () {
*! *
frase var; // declaração funciona no início ...
* /! *

alerta (frase); // Indefinido

*! *
frase = "Olá"; // ... atribuição - quando a execução o atinge.
* /! *
}

SayHi ();
`` `

Como todas as declarações `var` são processadas no início da função, podemos fazer referência a elas em qualquer lugar. Mas as variáveis ​​são indefinidas até as atribuições.

Nos dois exemplos acima, o "alerta" é executado sem um erro, porque a variável "frase" existe. Mas seu valor ainda não foi atribuído, então ele mostra 'indefinido'.

## Resumo

Existem duas diferenças principais de `var`:

1. As variáveis ​​não possuem um escopo de bloco, elas são mínimas visíveis no nível da função.
2. As declarações de variáveis ​​são processadas no início da função.

Há mais uma diferença menor relacionada ao objeto global, cobriremos isso no próximo capítulo.

Essas diferenças são realmente uma coisa ruim a maior parte do tempo. Primeiro, não podemos criar variáveis ​​locais em bloco. E o levantamento apenas cria mais espaço para erros. Então, para novos scripts `var 'é usado excepcionalmente raramente.
