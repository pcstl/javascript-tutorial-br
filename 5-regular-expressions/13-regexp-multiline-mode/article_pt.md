# Modo multilinha, bandeira "m"

O modo multilinha é habilitado pelo padrão flag `: /.../ m`.

[cortar]

Isso afeta apenas o comportamento de `pattern: ^` e `pattern: $`.

No modo multilinha, eles não coincidem apenas no início e no final da string, mas também no início / fim da linha.

## Linha de início ^

No exemplo abaixo, o texto tem várias linhas. O padrão `padrão: / ^ \ d + / gm` leva um número desde o início de cada um:

`` `js run
deixe str = `1º lugar: Winnie
2º lugar: Leitão
33º lugar: Eeyore`;

*!*
alerta (str.match (/ ^ \ d + / gm)); // 1, 2, 33
*/!*
```

Sem a bandeira `padrão: /.../ m`, apenas o primeiro número corresponde:


`` `js run
deixe str = `1º lugar: Winnie
2º lugar: Leitão
33º lugar: Eeyore`;

*!*
alerta (str.match (/ ^ \ d + / g)); // 1
*/!*
```

Isso ocorre porque, por padrão, um padrão do tapete: ^ `corresponde apenas no início do texto e no modo multilinha - no início de uma linha.

O motor de expressão regular se move ao longo do texto e procura por um começo de cadeia 'padrão: ^ `, quando encontra - continua a combinar o resto do padrão padrão: \ d +`.

## Line end $

O signo de dólar `padrão: $` comporta-se de forma semelhante.

A expressão regular `padrão: \ w + $` encontra a última palavra em cada linha

`` `js run
deixe str = `1º lugar: Winnie
2º lugar: Leitão
33º lugar: Eeyore`;

alerta (str.match (/ \ w + $ / gim)); // Winnie, Piglet, Eeyore
```

Sem o padrão `pattern: /.../ m`, o padrão do dólar: $` só corresponderia ao fim de toda a cadeia, então somente a última palavra seria encontrada.

## Anchors ^ $ versus \ n

Para encontrar uma nova linha, podemos usar não apenas o `pattern: ^` e `pattern: $`, mas também o caractere da nova linha `\ n`.

A primeira diferença é que, ao contrário das âncoras, o caractere `\ n`" consome "o caractere da nova linha e o adiciona ao resultado.

Por exemplo, aqui o usamos em vez de `pattern: $`:

`` `js run
deixe str = `1º lugar: Winnie
2º lugar: Leitão
33º lugar: Eeyore`;

alerta (str.match (/ \ w + \ n / gim)); // Winnie \ n, Piglet \ n
```

Aqui cada partida é uma palavra mais um caractere de nova linha.

E mais uma diferença - a nova linha `\ n` não corresponde ao fim da string. É por isso que `Eeyore` não é encontrado no exemplo acima.

Assim, as âncoras geralmente são melhores, elas estão mais próximas do que desejamos.
