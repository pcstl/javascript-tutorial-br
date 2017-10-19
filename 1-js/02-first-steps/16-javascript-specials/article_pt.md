# Especialidades do JavaScript

Esse capítulo relembra brevemente as funcionalidades do JavaScript que aprendemos até agora, prestando atenção nos detalhes mais sutis.

## Estrutura do código

Declarações são delimitadas por um ponto e vírgula:

```js run no-beautify
alert('Hello'); alert('World');```

Normalmente, a quebra de linha também funciona como delimitador, então desse jeito também funciona:

```js run no-beautify
alert('Hello')
alert('World')
```

Isso é uma inserção automática de ponto e vírgula. Eventualmente pode não funcionar, por exemplo:

```js run no-beautify
alert(“Vai haver um erro depois dessa mensagem")

[1, 2].forEach(alert)
```

A maioria dos manuais de boas práticas concordam que é recomendado colocar o ponto e vírgula após toda declaração.

Ponto e vírgulas não são necessário após blocos de código {...} e construções de sintaxe com eles, a exemplo de loops:

```js
function f() {
  // ponto e vírgula não necessário após declarar uma função
}

for(;;) {
  // ponto e vírgula não necessário após um loop
} 
```

... Mas mesmo que possamos colocar um ponto-e-vírgula "extra" em algum lugar, isso não é um erro. Será ignorado.

Para mais informações: <info:structure>.

## Modo estrito

Para liberar totalmente as funcionalidades do JavaScript atual, devemos iniciar scripts com `" use strict "`.

```js
'use strict';

...
```

A diretiva tem que estar no topo de um script ou no começo de uma função.

Sem o `'use strict'`, o código ainda vai funcionar, mas algumas funcionalidades funcionam à moda antiga, de modo "compatível". Geralmente preferimos o jeito moderno.

Algumas funcionalidades modernas da linguagem (a exemplo de classes que estudaremos no futuro) habilitam o strict mode implicitamente.

Para mais informações: <info:strict-mode>.

## Variáveis

Podem ser declaradas usando:

- `let`
- `const` (constante, não pode ser modificada)
- `var` (ultrapassado, veremos mais depois)

O nome de uma variável pode conter:

- Letras e dígitos, mas o primeiro caractere não pode ser um dígito.
- Caracteres `$` e `_` são normais como as letras.
- Alfabetos não-latinos and hieróglifos também são permitidos, mas não comumente usados.

Variáveis são tipadas dinamicamente. Elas podem armazenar qualquer valor:

```js
let x = 5;
x = "John";
```

Há 7 tipos de valores:

- `number` para números, inteiros ou de ponto flutuante,
- `string` para strings,
- `boolean` para valores lógicos:`true/false`,
- `null` -- um tipo com o valor único `null` (nulo) que significa "vazio" ou "inexistente",
- `undefined` -- um tipo com um valor único `undefined`, que significa "não-atribuído",
- `object` e `symbol` -- para estruturas de dados complexas e identificadores únicos, que não aprendemos ainda.

O operador `typeof` retorna o tipo de um valor, com duas exceções:

```js
typeof null == "object" // erro da linguagem
função typeof () {} == "função" // funções são tratadas separadamente
```

Para mais informações: <info:variables> e <info:types>.

## Interação

Estamos usando o navegador como ambiente de trabalho, portanto as funções básicas de UI são:

[`prompt(pergunta[, default])`](mdn:api/Window/prompt)
: Faça uma `pergunta` e retorne o que o usuário preencheu ou `null` se ele apertou "cancelar".

[`confirm (question)`](mdn:api/Window/confirm)
: Faça uma `pergunta` e sugira ao usuário escolher entre Ok e Cancelar. A escolha é retornada como `true/false`.

[`alert(message)`](mdn:api/Window/alert)
: Exibir uma `message`.

Todas essas funções são *modais*, elas pausam a execução do código e impedem o visitante de interagir com a página até que ele responda.

Por exemplo:

```js run
let userName = prompt("Seu nome?", "Alice");
let isTeaWanted = confirm("Você quer uma xícara de chá?");

alert( "Visitante: " + userName ); // Alice
alert( "Quer chá: " + isTeaWanted ); // true
```

Para mais informações: <info:alert-prompt-confirm>.

## Operadores

JavaScript suporta os seguintes operadores:

Aritméticas
: Normais: `* + - /`, e também `%` para o restante `**` para potenciação.

    Binário mais `+` concatena strings. Se qualquer um dos operadores for uma string, o outro é
    convertido em string também:

    ```js run
    alert( '1' + 2 ); // '12', string
    alert( 1 + '2' ); // '12', string
    ```

Atribuições
: Existem atribuições simples: `a = b` e combinadas, como `a *= 2`.

Bitwise
: Operadores bitwise (bit por bit) funcionam para inteiros a nível de bits; ver documentos quando for preciso: [docs](mdn:/JavaScript/Reference/Operators/Bitwise_Operators)

Ternário
: O único operador com três parâmetros: `cond ? resultA : result B`. Se `cond` for verdade, retorna `resultA`, caso contrário, retorna `resultB`.

Operadores lógicos
: AND `&&` e OR `||` executam uma avaliação de curto-circuito e, depois, retornam o valor onde ele parou.

Comparações
: Verificar igualdade `==` para valores de diferentes tipos os converte em números (com exceção de valores `null` e `undefined` porque equivalem apenas a eles mesmos), então esses aqui são iguais:

    ```js run
    alert( 0 == false ); // true
    alert( 0 == '' ); // true
    ```

    Outras comparações também são convertidas em números.

    O operador de igualdade estrita `===` não realiza a conversão: valores de tipos diferentes sempre são diferentes, portanto:

    Valores `null` e `undefined` são especiais: são iguais `==` entre si e não equivalem a mais nada.

    Comparações maior/menor comparam strings caractere por caractere, outros tipos são convertidos em número.

    Operadores lógicos
    : Há alguns outros a mais, como o operador vírgula.

    Para mais informações: <info:operators>, <info:comparison>, <info:logical-operators>.

## Loops

Nós cobrimos 3 tipos de loops:

```js
// 1
while (condition) {
  ...
}

// 2
do {
  ...
} while (condition);

// 3
for(let i = 0; i < 10; i++) {
  ...
}
```

- A variável declarada no loop `for(let...)` só é visível dentro do loop. Mas também podemos omitir `let` e reutilizar uma variável pré-existente.
- As diretivas `break/continue` permitem sair do loop/iteração atual. Utilizar labels para quebrar loops aninhados (um loop dentro de outro loop).

Para mais detalhes: <info:while-for>.

Mais tarde vamos estudar mais tipos de loops para lidar com objetos.

## O construto "switch"

O construto "switch" (troca) pode substituir múltiplas checagens `if` . Ele utiliza `===` para comparações.

Por exemplo:

```js run
let idade = prompt('Sua idade?', 18);

switch (idade) {
  case 18:
    alert("Não funciona"); // o resultado do prompt é uma string, não um número

  case "18":
    alert("Funciona!"");
    break;

  default:
    alert("Qualquer valor não-igual ao valor acima");
}
```

Para mais informações: <info:switch>.

## Funções

Cobrimos três formas de criar uma função em JavaScript:

1. Declaração de função: a função no fluxo de código principal

    ```js
    function soma(a, b) {
      let resultado = a + b;

      return resultado;
    }
    ```

2. Função expressão: a função no contexto de uma expressão

    ```js
    let soma = function(a, b) {
      let resultado = a + b;

      return resultado;
    }
    ```

    Funções expressões podem ter nomes, como `soma = function nome(a, b)`, mas `nome` só é visível dentro da função. 

3. Funções seta:

    ```js
    // expressão do lado direito
    let soma = (a, b) => a + b;

    // ou sintaxe de muitas linhas { ... }, pedem return:
    let soma = (a, b) => {
      // ...
      return a + b;
    }

    // sem argumentos
    let digaOi = () => alert("Oi");

    // com um argumento só
    let double = n => n * 2;
    ```


    - Funções podem ter variáveis locais, ou seja, aquelas declaradas em seu corpo. Essas variáveis são visíveis apenas dentro da função.
    - Parâmetros podem ter valores padrão: `function sum(a=1, b=2) {...}`.
    - Funções sempre retornam algo. Se não há declaração de `return`, o resultado é `undefined`.


  | Função declaração | Função expressão |
    |----------------------|---------------------|
    | visível em todo o bloco de código | criada quando a execução a alcança |
    |   - | pode pussuir um nome, apenas visível dentro da função |

    Para mais informações: <info:function-basics>, <info:function-expressions-arrows>.

## Mais por vir

Essa foi uma breve lista das funcionalidades do JavaScript. Até agora, apenas estudamos o básico. Mais adiante no tutorial vai haver mais especiais e funcionalidade avançadas de JavaScript.
