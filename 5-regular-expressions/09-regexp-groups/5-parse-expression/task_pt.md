# Analisar uma expressão

Uma expressão aritmetica consiste em 2 números e um operador entre eles, por exemplo:

- `1 + 2`
- `1.2 * 3.4`
- `-3 / -6`
- `-2 - 2`

O operador é um de: `" + "` `` `` - "` `` `` * "` ou `" / "`.

Pode haver espaços extras no início, no final ou entre as partes.

Crie uma função `parse (expr)` que leva uma expressão e retorna uma matriz de 3 itens:

1. O primeiro número.
2. O operador.
3. O segundo número.

Por exemplo:

`` `js
Deixe [a, op, b] = parse ("1.2 * 3.4");

alerta (a); // 1.2
alerta (op); // *
alerta (b); // 3.4
```
