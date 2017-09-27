importância: 3

---

# Mudar uma matriz

Escreva a função `shuffle (array)` que melhora (aleatoriamente reordena) os elementos da matriz.

Várias corridas de "shuffle" podem levar a diferentes ordens de elementos. Por exemplo:

`` `js
deixe arr = [1, 2, 3];

baralhamento (arr);
// arr = [3, 2, 1]

baralhamento (arr);
// arr = [2, 1, 3]

baralhamento (arr);
// arr = [3, 1, 2]
// ...
`` `

Todas as ordens de elementos devem ter uma probabilidade igual. Por exemplo, `[1,2,3]` pode ser reordenado como `[1,2,3]` ou `[1,3,2]` ou `[3,1,2]` etc, com igual probabilidade de cada caso.
