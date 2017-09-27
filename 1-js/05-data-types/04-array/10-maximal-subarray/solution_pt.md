# A solução lenta

Podemos calcular todos os subsums possíveis.

A maneira mais simples é levar cada elemento e calcular somas de todos os subarrays a partir dele.

Por exemplo, para `[-1, 2, 3, -9, 11]`:

`` `js no-embellecer
// A partir de -1:
-1
-1 + 2
-1 + 2 + 3
-1 + 2 + 3 + (-9)
-1 + 2 + 3 + (-9) + 11

// A partir de 2:
2
2 + 3
2 + 3 + (-9)
2 + 3 + (-9) + 11

// A partir de 3:
3
3 + (-9)
3 + (-9) + 11

// A partir de -9
-9
-9 + 11

// A partir de -11
-11
`` `

O código é realmente um loop aninhado: o loop externo sobre os elementos da matriz e os subsums de conta interna começando com o elemento atual.

`` `js run
function getMaxSubSum (arr) {
deixe maxSum = 0; // se não tomarmos nenhum elemento, zero será retornado

para (deixe i = 0; i <arr.length; i ++) {
Deixe sumFixedStart = 0;
para (let j = i; j <arr.length; j ++) {
sumFixedStart + = arr [j];
maxSum = Math.max (maxSum, sumFixedStart);
}
}

retorno maxSum;
}

alerta (getMaxSubSum ([- 1, 2, 3, -9])); // 5
alerta (getMaxSubSum ([- 1, 2, 3, -9, 11])); // 11
alerta (getMaxSubSum ([- 2, -1, 1, 2])); // 3
alerta (getMaxSubSum ([1, 2, 3])); // 6
alerta (getMaxSubSum ([100, -9, 2, -3, 5])); // 100
`` `

A solução tem um tempo complexo de [O (n <sup> 2 </ sup>)] (https://en.wikipedia.org/wiki/Big_O_notation). Em outras palavras, se aumentarmos o tamanho da matriz 2 vezes, o algoritmo funcionará 4 vezes mais.

Para grandes arrays (1000, 10000 ou mais itens), esses algoritmos podem levar a uma lentidão séria.

# Solução rápida

Vamos andar na matriz e manter a soma parcial atual dos elementos na variável `s`. Se `s` se tornar negativo em algum ponto, então atribua` s = 0`. O máximo de todos esses "s" será a resposta.

Se a descrição for muito vaga, veja o código, é suficientemente curto:

`` `js run
function getMaxSubSum (arr) {
deixe maxSum = 0;
Deixe partialSum = 0;

para (deixe item de arr) {// para cada item de arr
partialSum + = item; // adicione-o a parcialSum
maxSum = Math.max (maxSum, partialSum); // lembre-se do máximo
se (partialSum <0) partialSum = 0; // zero se negativo
}

retorno maxSum;
}

alerta (getMaxSubSum ([- 1, 2, 3, -9])); // 5
alerta (getMaxSubSum ([- 1, 2, 3, -9, 11])); // 11
alerta (getMaxSubSum ([- 2, -1, 1, 2])); // 3
alerta (getMaxSubSum ([100, -9, 2, -3, 5])); // 100
alerta (getMaxSubSum ([1, 2, 3])); // 6
alerta (getMaxSubSum ([- 1, -2, -3])); // 0
`` `

O algoritmo requer exatamente 1 passagem de matriz, então a complexidade do tempo é O (n).

Você pode encontrar mais informações detalhadas sobre o algoritmo aqui: [Problema de subarray máximo] (http://en.wikipedia.org/wiki/Maximum_subarray_problem). Se ainda não é óbvio por que isso funciona, então trate o algoritmo nos exemplos acima, veja como ele funciona, é melhor do que qualquer palavra.
