# Números

Todos os números em JavaScript são armazenados no formato de 64 bits [IEEE-754] (http://en.wikipedia.org/wiki/IEEE_754-1985), também conhecido como "dupla precisão".

Vamos recapitulhar e expandir o que conhecemos atualmente sobre eles.

## Mais formas de escrever um número

Imagine que precisamos escrever 1 bilhão. O caminho óbvio é:

`` `js
Deixe bilhões = 1000000000;
`` `

Mas na vida real geralmente evitamos escrever uma longa série de zeros, pois é fácil de digitar. Além disso, somos preguiçosos. Normalmente, escreveremos algo como "1 bilhão" por um bilhão ou "7,3 bilhões" para 7 bilhões de 300 milhões. O mesmo é verdade para a maioria dos grandes números.

Em JavaScript, encurtamos um número adicionando a letra `" e "` ao número e especificando a contagem de zeros:

`` `js run
Deixe bilhões = 1e9; // 1 bilhão, literalmente: 1 e 9 zeros

alerta (7.3e9); // 7,3 bilhões (7,300,000,000)
`` `

Em outras palavras, `" e "` multiplica o número por `1` com a contagem de zeros dada.

`` `js
1e3 = 1 * 1000
1,23 oh 6 = 1,23 * 1000000
`` `


Agora vamos escrever algo muito pequeno. Digamos, 1 microssegundo (um milhão de segundo):

`` `js
let ms = 0,000001;
`` `

Assim como antes, usar `" e "` pode ajudar. Se quisermos evitar escrever os zeros explicitamente, poderíamos dizer:

`` `js
let ms = 1e-6; // seis zeros à esquerda de 1
`` `

Se contamos os zeros em `0.000001`, existem 6 deles. Então, naturalmente, é `1e-6`.

Em outras palavras, um número negativo depois de "e" significa uma divisão em 1 com o número dado de zeros:

`` `js
// -3 dividido por 1 com 3 zeros
1º-3 = 1/1000 (= 0,001)

// -6 dividido por 1 com 6 zeros
1. Dê = 1.23 / 1.000.000 (= 0.00000123)
`` `

### Hex, números binários e octal

[Hexadecimal] (https://en.wikipedia.org/wiki/Hexadecimal) são amplamente utilizados em JavaScript para representar cores, codificar caracteres e para muitas outras coisas. Então, naturalmente, existe uma maneira mais curta de escrevê-los: `0x` e depois o número.

Por exemplo:

`` `js run
alerta (0xff); // 255
alerta (0xFF); // 255 (o mesmo, caso não importa)
`` `

Os sistemas numéricos binário e octal raramente são usados, mas também são suportados usando os prefixos `0b` e` 0o`:


`` `js run
deixe a = 0b11111111; // forma binária de 255
deixe b = 0o377; // forma octal de 255

alerta (a == b); // verdadeiro, o mesmo número 255 nos dois lados
`` `

Existem apenas 3 sistemas numéricos com esse suporte. Para outros sistemas numéricos, devemos usar a função `parseInt` (que veremos mais adiante neste capítulo).

## toString (base)

O método `num.toString (base)` retorna uma representação de cadeia de `num` no sistema numérico com a 'base' fornecida.

Por exemplo:
`` `js run
deixe num = 255;

alerta (num.toString (16)); // ff
alerta (num.toString (2)); // 11111111
`` `

O `base` pode variar de` 2` para `36`. Por padrão, é `10`.

Casos de uso comum para isso são:

- ** base = 16 ** é usado para cores hexadecimais, codificações de caracteres etc., os dígitos podem ser `0..9` ou` A..F`.
- ** base = 2 ** é principalmente para depurar operações bit a bit, os dígitos podem ser `0` ou` 1`.
- ** base = 36 ** é o máximo, os dígitos podem ser `0..9` ou` A..Z`. Todo o alfabeto latino é usado para representar um número. Um caso engraçado, mas útil para `36`, é quando precisamos transformar um identificador numérico longo em algo mais curto, por exemplo, para fazer um url curto. Pode simplesmente representá-lo no sistema numeral com base `36`:

`` `js run
alerta (123456..toString (36)); // 2n9c
`` `

`` `warn header =" Dois pontos para chamar um método "
Tenha em atenção que dois pontos no `123456..toString (36)` não são um erro de digitação. Se quisermos chamar um método diretamente em um número, como `toString` no exemplo acima, então precisamos colocar dois pontos` ..` depois dele.

Se colocássemos um único ponto: `123456.toString (36)`, então haveria um erro, porque a sintaxe JavaScript implica a parte decimal após o primeiro ponto. E se colocarmos mais um ponto, o JavaScript sabe que a parte decimal está vazia e agora segue o método.

Também poderia escrever `(123456) .toString (36)`.
`` `

# Redondeamento

Uma das operações mais usadas ao trabalhar com números é o arredondamento.

Existem várias funções internas para o arredondamento:

`Math.floor`
: Rounds down: `3.1` torna-se` 3`, e `-1.1` se torna` -2`.

`Math.ceil`
: Rounds up: `3.1` torna-se` 4`, e `-1.1` se torna` -1`.

`Math.round`
: Rondas para o inteiro mais próximo: `3.1` torna-se` 3`, `3.6` torna-se` 4` e `-1.1` se torna` -1`.

`Math.trunc` (não suportado pelo Internet Explorer)
: Remove qualquer coisa após o ponto decimal sem arredondamento: `3.1` torna-se` 3`, `-1.1` se torna` -1`.

Aqui está a tabela para resumir as diferenças entre eles:

| | `Math.floor` | `Math.ceil` | `Math.round` | `Math.trunc` |
| --- | --------- | -------- | --------- | --------- |
| `` 3.1` | `3` | `4` | `3` | `3` |
| `3.6` | `3` | `4` | `4` | `3` |
| -1.1` | `-2` | `-1` | `-1` | `-1` |
| `-1.6` | `-2` | `-1` | `-2` | `-1` |


Essas funções cobrem todas as formas possíveis de lidar com a parte decimal de um número. Mas e se quisermos arredondar o número para o dígito `n-th 'após o decimal?

Por exemplo, temos `1.2345 'e queremos arredondá-lo para 2 dígitos, recebendo apenas` 1.23`.

Existem duas maneiras de fazê-lo:

1. Multiplica-e-divide.

Por exemplo, para arredondar o número para o 2º dígito após o decimal, podemos multiplicar o número por `100`, chamar a função de arredondamento e depois dividi-lo novamente.
`` `js run
deixe num = 1.23456;

alerta (Math.floor (num * 100) / 100); // 1.23456 -> 123.456 -> 123 -> 1.23
`` `

2. O método [toFixed (n)] (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed) arredonda o número para "n" dígitos após o ponto e retorna uma representação de string do resultado.

`` `js run
deixe num = 12,34;
alerta (num.toFixed (1)); // "12.3"
`` `

Isso arredonda para cima ou para baixo para o valor mais próximo, semelhante ao `Math.round`:

`` `js run
deixe num = 12,36;
alerta (num.toFixed (1)); // "12.4"
`` `

Observe que o resultado de `toFixed` é uma string. Se a parte decimal for menor que o necessário, os zeros são anexados ao final:

`` `js run
deixe num = 12,34;
alerta (num.toFixed (5)); // "12.34000", adicionou zeros para fazer exatamente 5 dígitos
`` `

Podemos convertê-lo em um número usando o unary plus ou uma chamada `Number ()`: `+ num.toFixed (5)`.

## Cálculos Imprecisos

Internamente, um número é representado no formato de 64 bits [IEEE-754] (http://en.wikipedia.org/wiki/IEEE_754-1985), então há exatamente 64 bits para armazenar um número: 52 deles são usados para armazenar os dígitos, 11 deles armazenam a posição do ponto decimal (são zero para números inteiros) e 1 bit é para o sinal.

Se um número é muito grande, ele transbordaria o armazenamento de 64 bits, potencialmente dando um infinito:

`` `js run
alerta (1e500); // Infinito
`` `

O que pode ser um pouco menos óbvio, mas acontece com bastante frequência, é a perda de precisão.

Considere isso (falsy!) Teste:

`` `js run
alerta (0,1 + 0,2 = 0,3); // *! * false * /! *
`` `

É verdade, se verificarmos se a soma de `0.1` e` 0.2` é `0.3`, ficamos` falso '.

Estranho! O que é, então, se não `0.3?

`` `js run
alerta (0,1 + 0,2); // 0.30000000000000004
`` `

Ouch! Há mais conseqüências do que uma comparação incorreta aqui. Imagine que você está fazendo um site de compras eletrônicas e o visitante coloca bens "$ 0.10" e "$ 0.20" em seu gráfico. O total do pedido será `$ 0.30000000000000004`. Isso surpreenderia qualquer um.

Mas por que isso acontece?

Um número é armazenado na memória em sua forma binária, uma seqüência de zero e zero. Mas as frações como `0.1`,` 0.2` que se parecem simples no sistema decimal numérico são realmente frações intermináveis ​​em sua forma binária.

Em outras palavras, o que é `0.1? É um dividido por dez `1 / 10`, um décimo. No sistema de números decimais, esses números são facilmente representáveis. Compare em um terço: `1 / 3`. Torna-se uma fração infinita `0.33333 (3)`.

Assim, a divisão por poderes "10" é garantida para funcionar bem no sistema decimal, mas a divisão por `3` não é. Pelo mesmo motivo, no sistema de numeração binária, a divisão por potências de `2 'é garantida para funcionar, mas` 1 / 10` torna-se uma fração binária infinita.

Não há como armazenar * exatamente 0,1 * ou * exatamente 0,2 * usando o sistema binário, assim como não há maneira de armazenar um terço como uma fração decimal.

O formato numérico IEEE-754 resolve isso arredondando para o número mais próximo possível. Essas regras de arredondamento normalmente não nos permitem ver essa "pequena perda de precisão", então o número aparece como `0.3 '. Mas cuidado, a perda ainda existe.

Podemos ver isso em ação:
`` `js run
alerta (0.1.toFixed (20)); // 0.10000000000000000555
`` `

E quando somamos dois números, suas "perdas de precisão" se somam.

É por isso que `0.1 + 0.2` não é exatamente` 0.3 '.

`` `cabeçalho inteligente =" Não apenas JavaScript "
O mesmo problema existe em muitas outras linguagens de programação.

PHP, Java, C, Perl, Ruby dão exatamente o mesmo resultado, porque eles são baseados no mesmo formato numérico.
`` `

Podemos resolver o problema? Claro, existem várias maneiras:

1. Podemos arredondar o resultado com a ajuda de um método [toFixed (n)] (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed):

`` `js run
let sum = 0.1 + 0.2;
alerta (sum.toFixed (2)); // 0,30
`` `

Observe que `toFixed` sempre retorna uma string. Ele garante que ele tenha 2 dígitos após o ponto decimal. Isso é realmente conveniente se tivermos um e-shopping e precisamos mostrar `$ 0.30`. Para outros casos, podemos usar o mais unário para coagi-lo em um número:

`` `js run
let sum = 0.1 + 0.2;
alerta (+ sum.toFixed (2)); // 0.3
`` `

2. Podemos alternar temporariamente os números em números inteiros para as matemáticas e depois reverter. Funciona assim:

`` `js run
alerta ((0,1 * 10 + 0,2 * 10) / 10); // 0.3
`` `

Isso funciona porque, quando fazemos "0,1 * 10 = 1" e "0,2 * 10 = 2", ambos os números tornam-se inteiros e não há perda de precisão.

3. Se estivéssemos lidando com uma loja, a solução mais radical seria armazenar todos os preços em centavos e não usar fracções. Mas e se aplicarmos um desconto de 30%? Na prática, as frações de evasão total raramente são viáveis, então as soluções acima ajudam a evitar essa armadilha.

`` `` cabeçalho inteligente = "O engraçado"
Tente executar isso:

`` `js run
// Olá! Eu sou um número auto-crescente!
alerta (9999999999999999); // mostra 10000000000000000
`` `

Isso sofre do mesmo problema: uma perda de precisão. Existem 64 bits para o número, 52 deles podem ser usados ​​para armazenar dígitos, mas isso não é suficiente. Portanto, os dígitos menos significativos desaparecem.

O JavaScript não gera um erro em tais eventos. Ele faz o melhor para ajustar o número ao formato desejado, mas, infelizmente, esse formato não é suficientemente grande.
`` ``

`` `cabeçalho inteligente =" dois zeros "
Outra consequência engraçada da representação interna dos números é a existência de dois zeros: `0` e` -0`.

Isso porque um sinal é representado por um único bit, então cada número pode ser positivo ou negativo, incluindo um zero.

Na maioria dos casos, a distinção é imperceptível, porque os operadores são adequados para tratá-los como os mesmos.
`` `



## Testes: isFinite e isNaN

Lembre-se desses dois valores numéricos especiais?

- `Infinite` (e` -Infinite`) é um valor numérico especial que é maior (menos) do que qualquer coisa.
- `NaN` representa um erro.

Eles pertencem ao tipo `number`, mas não são números" normais ", então há funções especiais para verificar se eles são:


- `isNaN (value)` converte seu argumento em um número e depois o teste para ser 'NaN`:

`` `js run
alerta (isNaN (NaN)); // verdade
alerta (isNaN ("str")); // verdade
`` `

Mas precisamos dessa função? Não podemos usar a comparação `=== NaN`? Desculpe, mas a resposta é não. O valor `NaN` é único em que não é igual a nada, incluindo a si próprio:

`` `js run
alerta (NaN === NaN); // falso
`` `

- `isFinite (value)` converte seu argumento em um número e retorna `true` se for um número regular, e não` NaN / Infinity / -Infinity`:

`` `js run
alerta (isFinite ("15")); // verdade
alerta (isFinite ("str")); // falso, porque um valor especial: NaN
alerta (isFinite (Infinity)); // falso, porque um valor especial: infinito
`` `

Às vezes, `isFinite` é usado para validar se um valor de seqüência é um número regular:


`` `js run
Deixe num = + prompt ("Digite um número", '');

// será verdadeiro a menos que você insira Infinity, -Infinity ou não um número
alerta (isFinite (num));
`` `

Observe que uma seqüência de caracteres vazia ou única só é tratada como "0" em todas as funções numéricas, incluindo `isFinite`.

`` `smart header =" Compare com `Object.is`"

Existe um método interno especial [Object.is] (mdn: js / Object / is) que compara valores como `===`, mas é mais confiável para dois casos de borda:

1. Funciona com `NaN`:` Object.is (NaN, NaN) === true`, isso é uma coisa boa.
2. Os valores `0` e` -0 são diferentes: `Object.is (0, -0) === false`, raramente importa, mas esses valores tecnicamente são diferentes.

Em todos os outros casos, `Object.is (a, b)` é o mesmo que `a === b`.

Esse modo de comparação é freqüentemente usado na especificação JavaScript. Quando um algoritmo interno precisa comparar dois valores por ser exatamente o mesmo, ele usa `Object.is` (chamado internamente [SameValue] (https://tc39.github.io/ecma262/#sec-samevalue)).
`` `


## parseInt e parseFloat

A conversão numérica usando um plus `+` ou `Number ()` é rigorosa. Se um valor não for exatamente um número, ele falhará:

`` `js run
alerta (+ "100px"); // NaN
`` `

A única exceção é espaços no início ou no final da seqüência de caracteres, pois são ignorados.

Mas na vida real muitas vezes temos valores em unidades, como `" 100px "` ou `" 12pt "` em CSS. Também em muitos países, o símbolo da moeda segue o valor, então temos "19 €" e gostaria de extrair um valor numérico disso.

É por isso que `parseInt` e` parseFloat` são para.

Eles "lêem" um número de uma corda até que possam. Em caso de erro, o número coletado é retornado. A função `parseInt` retorna um inteiro, enquanto` parseFloat` retornará um número de ponto flutuante:

`` `js run
alerta (parseInt ('100px')); // 100
alerta (parseFloat ('12 .5em ')); // 12.5

alerta (parseInt ('12 .3 ')); // 12, apenas a parte inteira é retornada
alerta (parseFloat ('12 .3.4 ')); // 12.3, o segundo ponto pára a leitura
`` `

Há situações em que `parseInt / parseFloat` retornará` NaN`. Acontece quando nenhum dígito pode ser lido:

`` `js run
alerta (parseInt ('a123')); // NaN, o primeiro símbolo pára o processo
`` `

`` `` smart header = "O segundo argumento de` parseInt (str, radix) `"
A função `parseInt ()` possui um segundo parâmetro opcional. Especifica a base do sistema numérico, então `parseInt` também pode analisar cadeias de números hexadecimais, números binários e assim por diante:

`` `js run
alerta (parseInt ('0xff', 16)); // 255
alerta (parseInt ('ff', 16)); // 255, sem 0x também funciona

alerta (parseInt ('2n9c', 36)); // 123456
`` `
`` ``

## Outras funções de matemática

O JavaScript possui um objeto integrado [Matemática] (https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Math) que contém uma pequena biblioteca de funções e constantes matemáticas.

Alguns exemplos:

`Math.random ()`
: Retorna um número aleatório de 0 para 1 (não incluindo 1)

`` `js run
alerta (Math.random ()); // 0.1234567894322
alerta (Math.random ()); // 0.5435252343232
alerta (Math.random ()); // ... (qualquer número aleatório)
`` `

`Math.max (a, b, c ...)` / `Math.min (a, b, c ...)`
: Retorna o maior / menor do número arbitrário de argumentos.

`` `js run
alerta (Math.max (3, 5, -10, 0, 1)); // 5
alerta (Math.min (1, 2)); // 1
`` `

`Math.pow (n, power)`
: Retorna `n` levantou o poder dado

`` `js run
alerta (Math.pow (2, 10)); // 2 no poder 10 = 1024
`` `

Existem mais funções e constantes no objeto `Math`, incluindo a trigonometria, que você pode encontrar nos [documentos para a Matemática] (https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/ Matemática).

## Resumo

Para escrever grandes números:

- Anexe `" e "` com os zeros contam para o número. Como: `123e6` é` 123` com 6 zeros.
- Um número negativo após `" e "` faz com que o número seja dividido por 1 com zeros fornecidos. Isso é para uma milionésima ou tal.

Para diferentes sistemas numéricos:

- Pode escrever números diretamente nos sistemas hexadecimal (`0x`), octal (` 0o`) e binário (`0b`)
- `parseInt (str, base)` analisa um número inteiro de qualquer sistema numérico com base: `2 ≤ base ≤ 36`.
- `num.toString (base)` converte um número em uma string no sistema numérico com a 'base' fornecida.

Para converter valores como `12pt` e` 100px` para um número:

- Use `parseInt / parseFloat` para a conversão" soft ", que lê um número de uma string e, em seguida, retorna o valor que eles podem ler antes do erro.

Para as frações:

- Round usando `Math.floor`,` Math.ceil`, `Math.trunc`,` Math.round` ou `num.toFixed (precision)`.
- Certifique-se de lembrar que há uma perda de precisão ao trabalhar com frações.

Mais funções matemáticas:

- Veja o objeto [Matemática] (https://developer.mozilla.org/pt/docs/Web/JavaScript/Reference/Global_Objects/Math) quando você precisar deles. A biblioteca é muito pequena, mas pode cobrir as necessidades básicas.


