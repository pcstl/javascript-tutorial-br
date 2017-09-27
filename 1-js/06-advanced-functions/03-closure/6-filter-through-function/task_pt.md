importância: 5

---

# Filtra através da função

Temos um método incorporado `arr.filter (f)` para arrays. Ele filtra todos os elementos através da função `f`. Se retornar `true`, esse elemento será retornado na matriz resultante.

Faça um conjunto de filtros "prontos para usar":

- `inBetween (a, b)` - entre `a` e` b` ou igual a eles (inclusive).
- `inArray ([...])` - na matriz dada.

O uso deve ser assim:

- `arr.filter (inBetween (3,6)) - seleciona apenas valores entre 3 e 6.
- `arr.filter (inArray ([1,2,3])) - seleciona apenas elementos que correspondem a um dos membros de` [1,2,3] `.

Por exemplo:

`` `js
/ * .. seu código para dentro e em Array * /
deixe arr = [1, 2, 3, 4, 5, 6, 7];

alerta (arr.filter (inBetween (3, 6))); // 3,4,5,6

alerta (arr.filter (inArray ([1, 2, 10]))); // 1,2
`` `

