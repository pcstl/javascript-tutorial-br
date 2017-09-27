# Conversões de tipos

Na maioria das vezes, operadores e funções convertem automaticamente um valor para o tipo correto. Isso é chamado de "conversão de tipo".

Por exemplo, `alert` converte automaticamente qualquer valor para uma string para mostrá-lo. Operações matemáticas convertem valores em números.

Também há casos em que precisamos converter explicitamente um valor para corrigir as coisas.

[cortar]

`` `cabeçalho inteligente =" Não falando sobre objetos ainda "
Neste capítulo ainda não cobrimos os objetos. Aqui, estudamos primeiro os primitivos. Mais tarde, depois de aprendermos objetos, veremos como a conversão de objetos funciona no capítulo <info: object-toprimitive>.
`` `

## Para sequenciar

A conversão de cadeias ocorre quando precisamos da forma de string de um valor.

Por exemplo, `alert (value)` faz isso para mostrar o valor.

Também podemos usar uma função de chamada `String (value)` para isso:

`` `js run
Deixe valor = verdadeiro;
alerta (tipo de valor); // boleano

*! *
value = String (value); // agora valor é uma string "true"
alerta (tipo de valor); // corda
* /! *
`` `

A conversão de cordas é principalmente óbvia. Um "falso" torna-se "falso", "nulo" torna-se "nulo" etc.

## ToNumber

A conversão numérica ocorre em funções matemáticas e expressões automaticamente.

Por exemplo, quando a divisão `/` é aplicada a não-números:

`` `js run
alerta ("6" / "2"); // 3, as seqüências de caracteres são convertidas em números
`` `

Podemos usar uma função `Number (value)` para converter explicitamente um `value`:

`` `js run
deixe str = "123";
alerta (typeof str); // corda

Deixe num = Número (str); // se torna um número 123

alerta (tipo de número); // número
`` `

A conversão explícita geralmente é necessária quando lemos um valor de uma fonte baseada em string como um formulário de texto, mas esperamos que seja inserido um número.

Se a string não for um número válido, o resultado dessa conversão é `NaN`, por exemplo:

`` `js run
let age = Number ("uma string arbitrária em vez de um número");

alerta (idade); // NaN, a conversão falhou
`` `

Regras de conversão numérica:

| Valor | Se torna ... |
| ------- | ------------- |
| `indefinido` |` NaN` |
| 'null` | `0` |
| <code> true & nbsp; e & nbsp; false </ code> | `1` e` 0` |
| `string` | Espaços brancos desde o início e o final são removidos. Então, se a sequência restante estiver vazia, o resultado é `0`. Caso contrário, o número é "lido" da string. Um erro fornece `NaN`. |

Exemplos:

`` `js run
alerta (Número ("123")); // 123
alerta (Número ("123z")); // NaN (erro ao ler um número em "z")
alerta (número (verdadeiro)); // 1
alerta (Número (falso)); // 0
`` `

Observe que `null` e` undefined` se comportam de forma diferente aqui: `null` torna-se um zero, enquanto` indefinido 'se torna' NaN`.

`` `` smart header = "Addition '+' concatena cordas"
Quase todas as operações matemáticas convertem valores em números. Com uma exceção notável da adição `+`. Se um dos valores adicionados for uma string, outro também será convertido em uma string.

Em seguida, concatena (junta):

`` `js run
alerta (1 + '2'); // '12' (string para a direita)
alerta ('1' + 2); // '12' (string para a esquerda)
`` `

Isso só acontece quando um dos argumentos é uma string. Caso contrário, os valores são convertidos em números.
`` ``

## ToBoolean

A conversão booleana é a mais simples.

Isso ocorre em operações lógicas (mais tarde, veremos testes de condição e outros tipos), mas também pode ser realizada manualmente com a chamada de `Boolean (value)`.

A regra de conversão:

- Valores que são intuitivamente "vazios", como `0 ', uma string vazia,` null`, `undefined` e` NaN` tornam-se `falso'.
- Outros valores se tornam "verdade".

Por exemplo:

`` `js run
alerta (Booleano (1)); // verdade
alerta (Booleano (0)); // falso

alerta (Boolean ("hello")); // verdade
alerta (booleano ("")); // falso
`` `

`` `` warn header = "Observação: a string com zero` \ "0 \" `é` true` "
Alguns idiomas (ou seja, PHP) tratam `" 0 "` como `falso '. Mas, em JavaScript, uma string não vazia é sempre verdadeira.

`` `js run
alerta (booleano ("0")); // verdade
alerta (booleano ("")); // espaços, também verdadeiro (qualquer string não vazio é verdadeira)
`` `
`` ``


## Resumo

Existem três conversões de tipos mais amplamente utilizadas: a cadeia, ao número e ao booleano.

** `ToString` ** - Ocorre quando produzimos algo, pode ser executado com` String (value) `. A conversão para cadeia geralmente é óbvia para valores primitivos.

** `ToNumber` ** - Ocorre em operações matemáticas, pode ser executado com` Number (value) `.

A conversão segue as regras:

| Valor | Se torna ... |
| ------- | ------------- |
| `indefinido` |` NaN` |
| 'null` | `0` |
| <code> true & nbsp; / & nbsp; false </ code> | `1 / 0` |
| `string` | A string é lida "como está", os espaços em branco de ambos os lados são ignorados. Uma seqüência vazia torna-se `0 '. Um erro fornece `NaN`. |

** `ToBoolean` ** - Ocorre em operações lógicas, ou pode ser executado com` Boolean (value) `.

Seguem as regras:

| Valor | Se torna ... |
| ------- | ------------- |
| `0`,` null`, `undefined`,` NaN`, `` `` | `false` |
| qualquer outro valor | `true` |


A maioria dessas regras é fácil de entender e memorizar. As notáveis ​​exceções em que as pessoas costumam cometer erros são:

- `undefined` é` NaN` como um número, não `0`.
- `" 0 "` e as seqüências somente de espaço como `` `` são verdadeiras como booleanas.

Os objetos não são cobertos aqui, retornaremos para eles mais adiante no capítulo <info: object-toprimitive> que é dedicado exclusivamente aos objetos, depois de aprendermos coisas mais básicas sobre o JavaScript.
