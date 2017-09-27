# Operadores

Muitas operadoras são conhecidas da escola. Eles são adição `+`, uma multiplicação `*`, uma subtração `-` e assim por diante.

Neste capítulo, nos concentramos em aspectos que não são cobertos pela aritmética escolar.

[cortar]

## Termos: "unary", "binário", "operando"

Antes de seguir em frente, vamos entender a terminologia comum.

- * Um operando * - é o que os operadores são aplicados. Por exemplo, na multiplicação `5 * 2`, existem dois operandos: o operando esquerdo é` 5 'e o operando direito é `2`. Às vezes, as pessoas dizem "argumentos" em vez de "operandos".
- Um operador é * unário * se tiver um único operando. Por exemplo, a negação unária `" - "inverte o sinal do número:

`` `js run
deixe x = 1;

*! *
x = -x;
* /! *
alerta (x); // -1, foi aplicada uma negação unária
`` `
- Um operador é * binário * se tiver dois operandos. O mesmo menos existe na forma binária também:

`` `js run no-embellecer
deixe x = 1, y = 3;
alerta (y - x); // 2, menos binário subtrai valores
`` `

Formalmente, estamos falando sobre dois operadores diferentes aqui: a negação unária (operando único, inverte o sinal) e a subtração binária (dois operandos, subtrai).

## Metadados de concatenação, binário +

Agora, vejamos características especiais dos operadores de JavaScript que estão além da aritmética escolar.

Normalmente, o operador de mais `'+'` resume números.

Mas se o binário `+` for aplicado a strings, ele os mescla (os concatena):

`` `js
Deixe s = "my" + "string";
alerta (s); // mystring
`` `

Observe que, se algum dos operandos for uma string, o outro também será convertido em uma string.

Por exemplo:

`` `js run
alerta ('1' + 2); // "12"
alerta (2 + '1'); // "21"
`` `

Veja, não importa se o primeiro operando é uma string ou a segunda. A regra é simples: se qualquer operando for uma string, então converta o outro em uma string também.

String concatenação e conversão é uma característica especial do binário plus `" + "`. Outros operadores aritméticos funcionam apenas com números. Eles sempre convertem seus operandos em números.

Por exemplo, subtração e divisão:

`` `js run
alerta (2 - '1'); // 1
alerta ('6' / '2'); // 3
`` `

## Conversão numérica, unary +

O plus `+` existe em duas formas. A forma binária que usamos acima e a forma unária.

A vantagem un ou, em outras palavras, o operador plus `+` aplicado a um único valor, não faz nada com números, mas se o operando não for um número, ele será convertido.

Por exemplo:

`` `js run
// Nenhum efeito nos números
deixe x = 1;
alerta (+ x); // 1

deixe y = -2;
alerta (+ y); // -2

*! *
// Converte números que não são
alerta (+ verdadeiro); // 1
alerta (+ ""); // 0
* /! *
`` `

Na verdade, ele faz o mesmo que `Number (...)`, mas mais curto.

É frequente a necessidade de converter seqüência de caracteres para números. Por exemplo, se estamos obtendo valores de campos de formulário HTML, eles geralmente são strings.

E se quisermos sumá-los?

O binário mais os adicionaria como strings:

`` `js run
deixe as maçãs = "2";
deixe as laranjas = "3";

alerta (maçãs + laranjas); // "23", o binário plus concatena cordas
`` `

Se queremos tratá-los como números, então podemos converter e, em seguida, somar:

`` `js run
deixe as maçãs = "2";
deixe as laranjas = "3";

*! *
// ambos os valores convertidos em números antes do binário mais
alerta (+ maçãs + + laranjas); // 5
* /! *

// a variante mais longa
// alerta (Número (maçãs) + Número (laranjas)); // 5
`` `

Do ponto de vista de um matemático, a abundância de vantagens pode parecer estranha. Mas, do ponto de vista de um programador, não há nada de especial: as vantagens unárias são aplicadas primeiro, elas convertem strings em números e, em seguida, o binário acrescenta-se.

Por que as vantagens unárias são aplicadas aos valores antes do binário? Como vamos ver, isso é por causa de * maior precedência *.

## Pré-requisitos dos operadores

Se uma expressão tiver mais de um operador, a ordem de execução é definida pela * precedência *, ou, em outras palavras, existe uma ordem de prioridade implícita entre os operadores.

Na escola, todos sabemos que a multiplicação na expressão `1 + 2 * 2` deve ser calculada antes da adição. Essa é exatamente a coisa precedente. A multiplicação é dito ter * maior precedência * do que a adição.

Parênteses substituem qualquer precedência, portanto, se não estivermos satisfeitos com a ordem, podemos usá-los, como: `(1 + 2) * 2`.

Existem muitos operadores em JavaScript. Todo operador possui um número de precedência correspondente. Aquele com o número maior é executado primeiro. Se a precedência for a mesma, a ordem de execução é da esquerda para a direita.

Um extracto da [tabela de precedência] (https://developer.mozilla.org/en/JavaScript/Reference/operators/operator_precedence) (você não precisa se lembrar disso, mas observe que os operadores unários são superiores aos binários correspondentes ):

| Precedência | Nome | Assinar |
| ------------ | ------ | ------ |
| ... | ... | ... |
| 16 | unary plus | `+` |
| 16 | negação unária | `` `|
| 14 | multiplicação | `*` |
| 14 | divisão | `/` |
| 13 | adição | `+` |
| 13 | subtração | `` `|
| ... | ... | ... |
| 3 | atribuição | `=` |
| ... | ... | ... |

Como podemos ver, o "unary plus" tem uma prioridade de `16`, que é superior a` 13` para a "adição" (binário mais). É por isso que na expressão `" + maçãs + + laranjas "` unary pluses trabalho primeiro e, em seguida, a adição.

## Tarefa

Notemos que uma atribuição `=` também é um operador. Está listado na tabela de precedência com a prioridade muito baixa de `3`.

É por isso que, quando atribuímos uma variável, como `x = 2 * 2 + 1`, então os cálculos são feitos primeiro e depois é avaliado` = `, armazenando o resultado em` x`.

`` `js
deixe x = 2 * 2 + 1;

alerta (x); // 5
`` `

É possível encadear tarefas:

`` `js run
deixe a, b, c;

*! *
a = b = c = 2 + 2;
* /! *

alerta (a); // 4
alerta (b); // 4
alerta (c); // 4
`` `

As atribuições acorrentadas avaliam da direita para a esquerda. Primeiro, a expressão mais à direita `2 + 2` é avaliada e atribuída às variáveis ​​à esquerda:` c`, `b` e` a`. No final, todas as variáveis ​​compartilham um único valor.

`` `` smart header = "O operador de atribuição` \ "= \" `retorna um valor"
Um operador sempre retorna um valor. Isso é óbvio para a maioria deles como uma adição `+` ou uma multiplicação `*`. Mas o operador de atribuição segue essa regra também.

A chamada `x = value` escreve o` value` em `x` * e depois retorna *.

Aqui está a demonstração que usa uma tarefa como parte de uma expressão mais complexa:

`` `js run
deixe a = 1;
deixe b = 2;

*! *
deixe c = 3 - (a = b + 1);
* /! *

alerta (a); // 3
alerta (c); // 0
`` `

No exemplo acima, o resultado de `(a = b + 1)` é o valor atribuído a `a` (que é` 3 '). É então usado para subtrair de `3`.

Código divertido, não é? Devemos entender como funciona, porque às vezes podemos vê-lo em bibliotecas de terceiros, mas não devemos escrever nada assim nós mesmos. Esses truques definitivamente não tornam o código mais claro e legível.
`` ``

## Resto%

O operador restante `%` apesar de seu aspecto não tem relação com os por cento.

O resultado de `a% b` é o restante da divisão inteira de` a` por `b`.

Por exemplo:

`` `js run
alerta (5% 2); // 1 é um restante de 5 dividido por 2
alerta (8% 3); // 2 é um restante de 8 dividido por 3
alerta (6% 3); // 0 é um restante de 6 dividido por 3
`` `

## Exponenciação **

O operador de exponenciação `**` é uma adição recente ao idioma.

Para um número natural `b`, o resultado de` a ** b` é `a` multiplicado por si mesmo` b` vezes.

Por exemplo:

`` `js run
alerta (2 ** 2); // 4 (2 * 2)
alerta (2 ** 3); // 8 (2 * 2 * 2)
alerta (2 ** 4); // 16 (2 * 2 * 2 * 2)
`` `

O operador também funciona para números não inteiros de `a` e` b`, por exemplo:

`` `js run
alerta (4 ** (1/2)); // 2 (poder de 1/2 é o mesmo que uma raiz quadrada, isso é matemática)
alerta (8 ** (1/3)); // 2 (o poder de 1/3 é o mesmo que uma raiz cúbica)
`` `

## Incremento / decremento

<! - Não pode usar - no título, porque a análise integrada transforma-o em - ->

Aumentar ou diminuir um número por um é uma das operações numéricas mais comuns.

Então, há operadores especiais para isso:

- ** Incremento ** `++` aumenta uma variável em 1:

`` `js run no-embellecer
Deixe o contador = 2;
contador ++; // funciona igual a counter = contador + 1, mas mais curto
alerta (contador); // 3
`` `
- ** Decrementar ** `-` diminui uma variável em 1:

`` `js run no-embellecer
Deixe o contador = 2;
contador--; // funciona da mesma forma que counter = counter-1, mas mais curto
alerta (contador); // 1
`` `

`` `warn
Increment / decrement só pode ser aplicado a uma variável. Uma tentativa de usá-lo em um valor como `5 ++` dará um erro.
`` `

Operadores `++` e `-` podem ser colocados após e antes da variável.

- Quando o operador segue a variável, ele é chamado de "formulário postfix": `counter ++`.
- O "formulário de prefixo" é quando o operador fica antes da variável: `++ counter`.

Ambos os registros fazem o mesmo: aumentar `counter` por` 1`.

Existe alguma diferença? Sim, mas só podemos vê-lo se usarmos o valor retornado de `++ / -`.

Vamos esclarecer. Como sabemos, todos os operadores retornam um valor. Increment / decrement não é uma exceção aqui. O formulário de prefixo retorna o novo valor, enquanto o formulário postfix retorna o valor antigo (antes do incremento / decremento).

Para ver a diferença, aqui está o exemplo:

`` `js run
Deixe o contador = 1;
Deixe um contador = ++; // (*)

alerta (a); // *! * 2 * /! *
`` `

Aqui na linha `(*)` o prefixo chamar `++ counter` incrementa` counter` e retorna o novo valor que é `2`. Então, o `alert` mostra` 2`.

Agora vamos usar o formulário postfix:

`` `js run
Deixe o contador = 1;
deixe a = contador ++; // (*) alterou ++ contador para contador ++

alerta (a); // *! * 1 * /! *
`` `

Na linha `(*)` o * postfix * form `counter ++` também incrementa `counter`, mas retorna o * valor antigo * (antes do incremento). Então, o `alert` mostra` 1`.

Para resumir:

- Se o resultado de incremento / decremento não for usado, então não há diferença em qual forma usar:

`` `js run
Deixe contador = 0;
contador ++;
contador ++;
alerta (contador); // 2, as linhas acima fizeram o mesmo
`` `
- Se quisermos aumentar o valor * e * usar o resultado do operador agora, precisamos do formulário de prefixo:

`` `js run
Deixe contador = 0;
alerta (+ + contador); // 1
`` `
- Se quisermos incrementar, mas usar o valor anterior, então precisamos do formulário postfix:

`` `js run
Deixe contador = 0;
alerta (contador ++); // 0
`` `

`` `` cabeçalho inteligente = "Incremento / decremento entre outros operadores"
Os operadores `++ / -` também podem ser usados ​​dentro de uma expressão. Sua precedência é maior do que a maioria das outras operações aritméticas.

Por exemplo:

`` `js run
Deixe o contador = 1;
alerta (contador 2 * ++); // 4
`` `

Compare com:

`` `js run
Deixe o contador = 1;
alerta (2 * contador ++); // 2, porque o contador ++ retorna o valor "antigo"
`` `

Embora tecnicamente permitido, essa notação geralmente torna o código menos legível. Uma linha faz várias coisas - não é boa.

Ao ler o código, uma visão rápida "vertical" pode facilmente perca o `contador ++`, e não será óbvio que a variável aumenta.

O estilo "uma linha - uma ação" é aconselhado:

`` `js run
Deixe o contador = 1;
alerta (2 * contador);
contador ++;
`` `
`` ``

## Operadores Bitwise

Os operadores Bitwise tratam os argumentos como números inteiros de 32 bits e trabalham no nível de sua representação binária.

Esses operadores não são específicos do JavaScript. Eles são suportados na maioria das linguagens de programação.

A lista de operadores:

- AND (`&`)
- OR (`|`)
- XOR (`^`)
- NÃO (`~`)
- SHIFT ESQUERDO (`<<`)
- SHIFT DIREITO (`>>`)
- ZERO-FILL RIGHT SHIFT (`>>>`)

Esses operadores são usados ​​muito raramente. Para compreendê-los, devemos aprofundar a representação numérica de baixo nível, e não seria ótimo fazê-lo agora. Especialmente porque não precisamos deles em breve. Se você é curioso, você pode ler o artigo [Operadores Bitwise] (https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators) no MDN. Seria mais prático fazer isso quando surgir uma necessidade real.

## Modificar no local

Muitas vezes, precisamos aplicar um operador a uma variável e armazenar o novo resultado nele.

Por exemplo:

`` `js
deixe n = 2;
n = n + 5;
n = n * 2;
`` `

Esta notação pode ser encurtada usando operadores `+ =` e `* =`:

`` `js run
deixe n = 2;
n + = 5; // agora n = 7 (o mesmo que n = n + 5)
n * = 2; // agora n = 14 (o mesmo que n = n * 2)

alerta (n); // 14
`` `

Existem operadores curtos de "modificar e atribuir" para todos os operadores aritméticos e bit a bit: `/ =`, `- =` etc.

Esses operadores têm a mesma precedência que uma atribuição normal, então eles correm após a maioria dos outros cálculos:

`` `js run
deixe n = 2;

n * = 3 + 5;

alerta (n); // 16 (parte direita avaliada primeiro, igual a n * = 8)
`` `

## Comma

O operador de vírgula `',' 'é um dos operadores mais raros e incomuns. Às vezes, é usado para escrever código mais curto, então precisamos saber isso para entender o que está acontecendo.

O operador de vírgula nos permite avaliar várias expressões, dividindo-as com uma vírgula `','`. Cada um deles é avaliado, mas o resultado de apenas o último é retornado.

Por exemplo:

`` `js run
*! *
deixe a = (1 + 2, 3 + 4);
* /! *

alerta (a); // 7 (o resultado de 3 + 4)
`` `

Aqui, a primeira expressão `1 + 2` é avaliada, e seu resultado é descartado, então` 3 + 4` é avaliado e retornado como resultado.

`` `cabeçalho inteligente =" Comma tem uma precedência muito baixa "
Observe que o operador de vírgula possui uma prioridade muito baixa, menor que `=`, de modo que os parênteses são importantes no exemplo acima.

Sem eles: `a = 1 + 2,3 + 4` avalia primeiro` + `, somando os números em` a = 3,7`, então o operador de atribuição `=` atribui `a = 3` e, em seguida, o número Após a vírgula `7` não é processada de qualquer forma, então é ignorado.
`` `

Por que precisamos de um operador que jogue tudo, exceto a última parte?

Às vezes, as pessoas usam em construções mais complexas para colocar várias ações em uma única linha.

Por exemplo:

`` `js
// três operações em uma linha
para (*! * a = 1, b = 3, c = a * b * /! *; a <10; a ++) {
...
}
`` `

Esses truques são usados ​​em muitos frameworks de JavaScript, é por isso que os mencionamos. Mas geralmente eles não melhoram a legibilidade do código, então devemos pensar bem antes de escrever assim.
