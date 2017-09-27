# Especialidades de JavaScript

Este capítulo recapitula brevemente os recursos do JavaScript que já aprendemos, prestando especial atenção aos momentos sutis.

[cortar]

## Estrutura do código

As declarações são delimitadas com um ponto-e-vírgula:

`` `js run no-embellecer

`` `

Geralmente, um intervalo de linha também é tratado como um delimitador, de modo que também funcionasse:

`` `js run no-embellecer

alerta ('Mundo')
`` `

Isso é chamado de "inserção automática de ponto e vírgula". Às vezes não funciona, por exemplo:

`` `js run
alerta ("Haverá um erro após esta mensagem")

[1, 2] .forEach (alerta)
`` `

A maioria dos guias de codestyle concorda que devemos colocar um ponto-e-vírgula após cada declaração.

Semicolons não são necessários após os blocos de código `{...}` e as construções de sintaxe com eles como loops:

`` `js
função f () {
// sem ponto e vírgula necessário após a declaração de função
}

para(;;) {
// sem ponto e vírgula necessário após o loop
}
`` `

... Mas mesmo que possamos colocar um ponto-e-vírgula "extra" em algum lugar, isso não é um erro. Será ignorado.

Mais em: <info: structure>.

## Modo estrito

Para habilitar completamente todos os recursos do JavaScript moderno, devemos iniciar scripts com `" use strict "`.

`` `js
"uso rigoroso";

...
`` `

A diretiva deve estar no topo de um script ou no início de uma função.

Sem `" use strict "`, tudo ainda funciona, mas alguns recursos se comportam de forma antiquada, "compatível". Geralmente, preferimos o comportamento moderno.

Algumas características modernas do idioma (como classes que estudaremos no futuro) permitem o modo estrito de forma implícita.

Mais em: <info: strict-mode>.

## Variáveis

Pode ser declarado usando:

- `vamos '
- `const` (constante, não pode ser alterado)
- `var` (antigo, verá mais tarde)

Um nome de variável pode incluir:
- Letras e dígitos, mas o primeiro personagem pode não ser um dígito.
- Personagens `$` e `_` são normais, a par com letras.
- Alfabetos não-latinos e hieróglifos também são permitidos, mas geralmente não são usados.

As variáveis ​​são dinamicamente digitadas. Eles podem armazenar qualquer valor:

`` `js
deixe x = 5;
x = "John";
`` `

Existem 7 tipos de dados:

- `number` para números de ponto flutuante e inteiro,
- `string` para strings,
- `boolean` para valores lógicos:` true / false`,
- `null` - um tipo com um valor único 'null`, que significa" vazio "ou" não existe ",
- `undefined` - um tipo com um valor único 'indefinido', que significa" não atribuído ",
- `object` e` symbol` - para estruturas de dados complexas e identificadores únicos, ainda não os aprendemos.

O operador `typeof` retorna o tipo para um valor, com duas exceções:
`` `js
typeof null == "object" // erro no idioma
função typeof () {} == "função" // as funções são tratadas especialmente
`` `

Mais em: <info: variáveis> e <info: types>.

## Interação

Estamos usando um navegador como um ambiente de trabalho, então as funções básicas de UI serão:

[`prompt (pergunta [, padrão])`] (mdn: api / Window / prompt)
: Pergunte uma "pergunta" e devolva o que o visitante entrou ou "nulo" se pressionou "cancelar".

[`confirm (question)`] (mdn: api / Window / confirm)
: Pergunte uma "pergunta" e sugira escolher entre Ok e Cancelar. A escolha é retornada como `true / false`.

[`alerta (mensagem)`] (mdn: api / Window / alert)
: Saída de uma "mensagem".

Todas estas funções são * modal *, eles pausam a execução do código e impedem o visitante de interagir com a página até ele responder.

Por exemplo:

`` `js run
deixe userName = prompt ("seu nome?", "Alice");
deixe isTeaWanted = confirmar ("Você quer um pouco de chá?");

alerta ("Visitante:" + nome do usuário); // Alice
alerta ("Tea wanted:" + isTeaWanted); // verdade
`` `

Mais em: <info: alert-prompt-confirm>.

## Operadores

O JavaScript suporta os seguintes operadores:

Aritmética
: Regular: `* + - /`, também `%` para o restante e `**` para o poder de um número.

Binary plus `+` concatena strings. E se algum dos operandos for uma string, o outro é convertido em string também:

`` `js run
alerta ('1' + 2); // '12', string
alerta (1 + '2'); // '12', string
`` `

atribuições
: Existe uma tarefa simples: `a = b` e combinadas como` a * = 2`.

Bitwise
: Operadores bit a bit funcionam com números inteiros no nível do bit: veja [docs] (mdn: / JavaScript / Reference / Operators / Bitwise_Operators) quando são necessários.

Ternário
: O único operador com três parâmetros: `cond? resultadoA: resultado B`. Se `cond` for verdade, retorna` resultA`, caso contrário `resultB`.

Operadores lógicos
: Logical AND `&&` e OR `||` executar avaliação de curto-circuito e, em seguida, retornar o valor onde ele parou.

Comparações
: Equality check `==` para valores de diferentes tipos converte-os para um número (exceto `null` e` undefined` que se igualam e nada mais), então estes são iguais:

`` `js run
alerta (0 == falso); // verdade
alerta (0 == ''); // verdade
`` `

Outras comparações convertem-se para um número também.

O operador de igualdade rigorosa `===` não faz a conversão: diferentes tipos sempre significam valores diferentes para ele, então:

Os valores `null` e` undefined` são especiais: eles são iguais `==` uns aos outros e não equivalem a qualquer outra coisa.

Comparações maiores / menores comparam as seqüências de caracteres por caracteres, outros tipos são convertidos em um número.

Operadores lógicos
: Há poucos outros, como um operador de vírgula.

Mais em: <info: operadores>, <info: comparação>, <info: log-operators>.

## Rotações

- Cobrimos 3 tipos de loops:

`` `js
// 1
enquanto (condição) {
...
}

// 2
Faz {
...
} enquanto (condição);

// 3
para (vamos i = 0; i <10; i ++) {
...
}
`` `

- A variável declarada no loop `for (let ...) 'é visível apenas dentro do loop. Mas também podemos omitir "deixar" e reutilizar uma variável existente.
- As diretivas `break / continue` permitem sair da iteração de loop / current atual. Use etiquetas para quebrar loops aninhados.

Detalhes em: <info: while-for>.

Mais tarde, estudaremos mais tipos de loops para lidar com objetos.

## A construção "switch"

A construção "switch" pode substituir várias verificações `if`. Ele usa `===` para comparações.

Por exemplo:

`` `js run
let age = prompt ('Your age?', 18);

interruptor (idade) {
caso 18:
alerta ("Não funcionará"); // o resultado do prompt é uma string, não um número

caso "18":
alerta ("Isso funciona!" ");
pausa;

padrão:
alerta ("Qualquer valor não igual a um acima");
}
`` `

Detalhes em: <info: switch>.

## Funções

Nós cobrimos três maneiras de criar uma função em JavaScript:

1. Declaração de função: a função no fluxo do código principal

`` `js
soma de função (a, b) {
Deixe resultado = a + b;

resultado de retorno;
}
`` `

2. Função Expressão: a função no contexto de uma expressão

`` `js
permitir soma = função (a, b) {
Deixe resultado = a + b;

resultado de retorno;
}
`` `

A expressão de função pode ter um nome, como `sum = nome da função (a, b)`, mas esse `nome` só é visível dentro dessa função.

3. Funções de seta:

`` `js
// expressão no lado direito
Deixe sum = (a, b) => a + b;

// ou sintaxe multi-line com {...}, precisa retornar aqui:
Deixe sum = (a, b) => {
// ...
Retornar a + b;
}

// sem argumentos


// com um único argumento
Deixe duplo = n => n * 2;
`` `


- As funções podem ter variáveis ​​locais: as declaradas dentro do corpo. Essas variáveis ​​só são visíveis dentro da função.
- Os parâmetros podem ter valores padrão: `soma da função (a = 1, b = 2) {...}`.
- As funções sempre retornam algo. Se não houver nenhuma instrução `return`, o resultado é 'indefinido'.


| Declaração de função | Expressão de função |
| ---------------------- | --------------------- |
| visível em todo o bloco de código | criado quando a execução o alcança |
| - | pode ter um nome, visível apenas dentro da função |

Mais: veja <info: função-básica>, <info: função-expressões-setas>.

## Mais para vir

Essa foi uma breve lista de recursos de JavaScript. A partir de agora, estudamos apenas o básico. Além disso, no tutorial, você encontrará mais especiais e recursos avançados do JavaScript.
