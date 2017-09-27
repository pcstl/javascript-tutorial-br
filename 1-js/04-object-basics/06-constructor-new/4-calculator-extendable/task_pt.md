importância: 5

---

# Criar uma calculadora extensível

Crie uma função de construtor `Calculadora 'que cria objetos de calculadora" extensíveis ".

A tarefa consiste em duas partes.

1. Primeiro, implemente o método `calcule (str)` que leve uma seqüência de caracteres como `" 1 + 2 "` no formato "NUMBER NUMBER operador" (espaço-delimitado) e retorna o resultado. Deve entender plus `+` e menos `-`.

Exemplo de uso:

`` `js
Deixe calc = calculadora nova;

alerta (calccalculate ("3 + 7")); // 10
`` `
2. Em seguida, adicione o método `addOperator (name, func)` que ensina à calculadora uma nova operação. Leva o nome do operador e a função de dois argumentos `func (a, b)` que o implementa.

Por exemplo, vamos adicionar a multiplicação `*`, division `/` e power `**`:

`` `js
deixe powerCalc = calculadora nova;
powerCalc.addMethod ("*", (a, b) => a * b);
powerCalc.addMethod ("/", (a, b) => a / b);
powerCalc.addMethod ("**", (a, b) => a ** b);

Deixe resultado = powerCalc.calculate ("2 ** 3");
alerta (resultado); // 8
`` `

- Sem colchetes ou expressões complexas nesta tarefa.
- Os números e o operador estão delimitados com exatamente um espaço.
- Pode haver tratamento de erros se você quiser adicioná-lo.
