# Tipos de dados

Uma variável em JavaScript pode conter dados. Uma variável pode, em um momento, ser uma string e depois receber um valor numérico:

`` `js
// nenhum erro
Deixe a mensagem = "olá";
mensagem = 123456;
`` `

Os idiomas de programação que permitem que tais coisas sejam chamados de "digitado dinamicamente", o que significa que existem tipos de dados, mas as variáveis ​​não estão vinculadas a nenhum deles.

Existem sete tipos de dados básicos em JavaScript. Aqui vamos estudar o básico e, nos próximos capítulos, falaremos sobre cada um deles em detalhes.

[cortar]

## Um número

`` `js
deixe n = 123;
n = 12,345;
`` `

O tipo * number * serve tanto para números inteiros quanto em ponto flutuante.

Existem muitas operações para números, e. multiplicação `*`, divisão `/`, além `+`, subtração `-` e assim por diante.

Além dos números regulares, existem os chamados "valores numéricos especiais" que também pertencem a esse tipo: `Infinity`,` -Infinity` e `NaN`.

- `Infinity` representa o matemático [Infinity] (https://en.wikipedia.org/wiki/Infinity) ∞. É um valor especial que é maior do que qualquer número.

Podemos obtê-lo como resultado da divisão por zero:

`` `js run
alerta (1/0); // Infinito
`` `

Ou apenas mencioná-lo diretamente no código:

`` `js run
alerta (Infinito); // Infinito
`` `
- `NaN` representa um erro computacional. É um resultado de uma operação matemática incorreta ou indefinida, por exemplo:

`` `js run
alerta ("não um número" / 2); // NaN, essa divisão é errônea
`` `

`NaN` é pegajoso. Qualquer outra operação no `NaN` daria` NaN`:

`` `js run
alerta ("não um número" / 2 + 5); // NaN
`` `

Então, se há 'NaN' em algum lugar em uma expressão matemática, ele se propaga para todo o resultado.

`` `cabeçalho inteligente =" operações matemáticas são seguras "
Fazer matemática é seguro em JavaScript. Podemos fazer qualquer coisa: divida por zero, trate cordas não-numéricas como números, etc.

O script nunca irá parar com um erro fatal ("morrer"). Na pior das hipóteses, obteremos "NaN" como resultado.
`` `

Os valores numéricos especiais pertencem formalmente ao tipo "número". Claro que eles não são números em um senso comum desta palavra.

Veremos mais sobre trabalhar com números no capítulo <info: number>.

## Uma linha

Uma string em JavaScript deve ser citada.

`` `js
Deixe str = "Olá";
Deixe str2 = 'Citações simples também estão ok';
let phrase = `pode incorporar $ {str}`;
`` `

Em JavaScript, existem 3 tipos de citações.

1. Cotações duplas: `" Hello "`.
2. Cotações simples: `'Hello``.
3. Backticks: <code> `Hello` </ code>.

Citações duplas e soltas são citações "simples". Não há diferença entre eles em JavaScript.

Backticks são citações de "funcionalidade estendida". Eles nos permitem incorporar variáveis ​​e expressões em uma string, envolvendo-as em `$ {...}`, por exemplo:

`` `js run
Deixe o nome = "John";

// incorporar uma variável
alerta (`Olá, *! * $ {name} * /! *!`); // Olá john!

// incorpora uma expressão
alerta (`o resultado é *! * $ {1 + 2} * /! *`); // o resultado é 3
`` `

A expressão dentro de `$ {...}` é avaliada e o resultado se torna uma parte da string. Podemos colocar qualquer coisa lá: uma variável como `nome 'ou uma expressão aritmetica como` 1 + 2` ou algo mais complexo.

Por favor, note que isso só pode ser feito em backticks. Outras citações não permitem tal incorporação!
`` `js run
alerta ("o resultado é $ {1 + 2}"); // o resultado é $ {1 + 2} (as citações duplas não fazem nada)
`` `

Vamos abordar as cordas mais detalhadamente no capítulo <info: string>.

`` `cabeçalho inteligente =" Não existe * tipo de caractere * ".
Em algumas línguas, existe um tipo especial de "personagem" para um único personagem. Por exemplo, na linguagem C e em Java é `char`.

Em JavaScript, não existe esse tipo. Existe apenas um tipo: `string`. Uma string pode consistir de um único personagem ou muitos deles.
`` `

## A boolean (tipo lógico)

O tipo booleano possui apenas dois valores: `true` e` false`.

Esse tipo é comumente usado para armazenar valores de sim / não: `true` significa" sim, correto "e" falso "significa" não, incorreto ".

Por exemplo:

`` `js
Deixe o nomeFieldChecked = true; // sim, o campo de nome está marcado
deixa ageFieldChecked = false; // não, o campo de idade não está marcado
`` `

Os valores booleanos também vêm como resultado de comparações:

`` `js run
deixe isGreater = 4> 1;

alerta (isGreater); // verdadeiro (o resultado da comparação é "sim")
`` `

Vamos cobrir booleans mais profundamente mais adiante no capítulo <info: log-operators>.

## O valor "nulo"

O valor especial 'nulo' não pertence a nenhum tipo descrito acima.

Ele forma um tipo separado próprio, que contém apenas o valor 'nulo':

`` `js
deixar idade = nula;
`` `

Em JavaScript, `null` não é uma" referência a um objeto não existente "ou um" ponteiro nulo ", como em alguns outros idiomas.

É apenas um valor especial que tem o senso de "nada", "vazio" ou "valor desconhecido".

O código acima indica que o `age` é desconhecido ou vazio por algum motivo.

## O valor "indefinido"

O valor especial "indefinido" se distingue. Faz um tipo próprio, como "nulo".

O significado de `undefined` é" o valor não é atribuído ".

Se uma variável for declarada, mas não atribuída, então seu valor é exatamente 'indefinido':

`` `js run
deixe x;

alerta (x); // mostra "indefinido"
`` `

Tecnicamente, é possível atribuir `indefinido 'a qualquer variável:

`` `js run
deixe x = 123;

x = indefinido;

alerta (x); // "Indefinido"
`` `

... Mas não é recomendado fazer isso. Normalmente, usamos `null` para escrever um valor" vazio "ou" desconhecido "na variável, e` undefined` é usado apenas para cheques, para ver se a variável é atribuída ou similar.

## Objetos e símbolos

O tipo `object` é especial.

Todos os outros tipos são chamados de "primitivos", porque seus valores podem conter apenas uma única coisa (seja ela uma string ou um número ou qualquer outro). Em contraste, os objetos são usados ​​para armazenar coleções de dados e entidades mais complexas. Nós lidaremos com eles mais adiante no capítulo <info: object> depois de sabermos o suficiente sobre as primitivas.

O tipo de "símbolo" é usado para criar identificadores exclusivos para objetos. Nós devemos mencioná-lo aqui para completar, mas é melhor estudá-los depois de objetos.

## O tipo de operador [# type-typeof]

O operador `typeof` retorna o tipo do argumento. É útil quando queremos processar valores de diferentes tipos de forma diferente, ou simplesmente queremos fazer uma verificação rápida.

Ele suporta duas formas de sintaxe:

1. Como operador: `typeof x`.
2. Estilo de função: `typeof (x)`.

Em outras palavras, funciona tanto com os parênteses quanto sem eles. O resultado é o mesmo.

A chamada para `typeof x` retorna uma string com o nome do tipo:

`` `js
typeof indeterminado // "indefinido"

typeof 0 // "number"

typeof true // "boolean"

typeof "foo" // "string"

typeof Símbolo ("id") // "símbolo"

*! *
typeof Math // "objeto" (1)
* /! *

*! *
typeof null // "object" (2)
* /! *

*! *
typeof alerta // "função" (3)
* /! *
`` `

As últimas três linhas podem precisar de explicações adicionais:

1. `Math` é um objeto interno que fornece operações matemáticas. Nós aprenderemos no capítulo <info: number>. Aqui serve como um exemplo de um objeto.
2. O resultado de `typeof null` é` "object" `. Isto é errado. É um erro oficialmente reconhecido no `typeof`, mantido para compatibilidade. Claro, `null` não é um objeto. É um valor especial com um tipo separado. Então, novamente, isso é um erro no idioma.
3. O resultado de `typeof alert` é` "function" `, porque` alert` é uma função do idioma. Estudaremos as funções nos próximos capítulos, e veremos que não há nenhum tipo especial de "função" no idioma. As funções pertencem ao tipo de objeto. Mas `typeof` trata-os de forma diferente. Formalmente, é incorreto, mas muito conveniente na prática.


## Resumo

Existem 7 tipos básicos em JavaScript.

- `number` para números de qualquer tipo: inteiro ou ponto flutuante.
- `string` para strings. Uma string pode ter um ou mais caracteres, não existe nenhum tipo de caractere separado.
- `boolean` para` true` / `false`.
- `null` para valores desconhecidos - um tipo autônomo que possui um valor único 'null`.
- `undefined` para valores não atribuídos - um tipo autônomo que possui um único valor 'indefinido'.
- `object` para estruturas de dados mais complexas.
- `simbolo 'para identificadores exclusivos.

O operador `typeof` nos permite ver qual tipo é armazenado na variável.

- Duas formas: `typeof x` ou` typeof (x) `.
- Retorna uma string com o nome do tipo, como `" string "`.
- Para `null` retorna` `objeto '' - isso é um erro no idioma, não é um objeto de fato.

Nos próximos capítulos, nos concentraremos em valores primitivos e, uma vez que estamos familiarizados com eles, seguiremos para objetos.
