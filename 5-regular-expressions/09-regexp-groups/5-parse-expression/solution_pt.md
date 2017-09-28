Um regex para um número é: `padrão: -? \ D + (\. \ D +)?`. Criamos isso em tarefas anteriores.

Um operador é padrão: [- + * /] `. Nós colocamos um padrão de dash: - o primeiro, porque no meio significaria um intervalo de caracteres, não precisamos disso.

Observe que uma barra deve ser escapada dentro de um padrão regex do JavaScript: /.../ `.

Precisamos de um número, de um operador e de outro numero. E espaços opcionais entre eles.

A expressão regular completa: `padrão: -? \ D + (\. \ D +)? \ S * [- + * /] \ s * -? \ D + (\. \ D +)?`.

Para obter um resultado como uma matriz, vamos colocar parênteses em torno dos dados que precisamos: números e o operador: `padrão: (-? \ D + (\. \ D +)?) \ S * ([- + * /]) \ s * (-? \ d + (\. \ d +)?) `.

Em ação:

`` `js run
Deixe reg = /(-?\d+(\.\d+)?)\s*([-+*\])\s*(-?\d+(\.\d+)?)/;

alerta ("1,2 + 12" .match (reg));
```

O resultado inclui:

- `resultado [0] ==" 1,2 + 12 "(jogo completo)
- `result [1] ==" 1 "` (primeiro parênteses)
- `result [2] ==" 2 "` (segundo parênteses - a parte decimal `(\. \ d +)?`)
- `result [3] ==" + "` (...)
- `resultado [4] ==" 12 "` (...)
- `result [5] == indefinido` (a última parte decimal está ausente, portanto, não está definido)

Nós precisamos apenas de números e do operador. Não precisamos de peças decimais.

Então, vamos remover grupos extras da captura por padrão adicionado:?: `, Por exemplo:` padrão: (?: \. \ D +)? `.

A solução final:

`` `js run
Parse de função (expr) {
Deixe reg = /(-?\d+(?:\.\d+)?)\s*([-+*\])\s*(-?\d+(?:\.+) ;

Deixe resultado = expr.match (reg);

se (! resultado) retornar;
result.shift ();

resultado de retorno;
}

alerta (parse ("- 1.23 * 3.45")); // -1,23, *, 3,45
```
