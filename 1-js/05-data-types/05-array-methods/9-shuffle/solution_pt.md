A solução simples poderia ser:

`` `js run
*! *
função shuffle (array) {
array.sort (() => Math.random () - 0.5);
}
* /! *

deixe arr = [1, 2, 3];
baralhamento (arr);
alerta (arr);
`` `

Isso funciona um pouco, porque `Math.random () - 0.5` é um número aleatório que pode ser positivo ou negativo, então a função de classificação reordena aleatoriamente os elementos.

Mas porque a função de classificação não se destina a ser usada dessa maneira, nem todas as permutações têm a mesma probabilidade.

Por exemplo, considere o código abaixo. Ele corre "shuffle" 1000000 vezes e conta as aparências de todos os resultados possíveis:

`` `js run
função shuffle (array) {
array.sort (() => Math.random () - 0.5);
}

// contagens de aparências para todas as permutações possíveis
deixe count = {
'123': 0,
'132': 0,
'213': 0,
'231': 0,
'321': 0,
'312': 0
};

para (vamos i = 0; i <1000000; i ++) {
Deixe array = [1, 2, 3];
shuffle (array);
contagem [array.join ('')] ++;
}

// mostra contagens de todas as permutações possíveis
para (let key in count) {
alerta (`$ {key}: $ {count [key]}`);
}
`` `

Um exemplo de resultado (para V8, julho de 2017):

`` `js
123: 250706
132: 124425
213: 249618
231: 124880
312: 125148
321: 125223
`` `

Podemos ver o viés claramente: `123` e` 213` aparecem muito mais frequentemente do que outros.

O resultado do código pode variar entre os motores JavaScript, mas já podemos ver que a abordagem não é confiável.

Por que não funciona? De um modo geral, `sort` é uma" caixa preta ": lançamos uma matriz e uma função de comparação nela e esperamos que a matriz seja ordenada. Mas, devido à aleatoriedade total da comparação, a caixa preta fica louca e, como isso fica louco, depende da implementação concreta que difere entre os motores.

Existem outras boas maneiras de fazer a tarefa. Por exemplo, há um ótimo algoritmo chamado [Fisher-Yates shuffle] (https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle). A idéia é andar na matriz na ordem inversa e trocar cada elemento por um aleatório antes dele:

`` `js
função shuffle (array) {
para (let i = array.length - 1; i> 0; i--) {
deixe j = Math.floor (Math.random () * (i + 1)); // índice aleatório de 0 a i
[array [i], array [j]] = [array [j], array [i]]; // elementos de troca
}
}
`` `

Vamos testá-lo da mesma maneira:

`` `js run
função shuffle (array) {
para (let i = array.length - 1; i> 0; i--) {
deixe j = Math.floor (Math.random () * (i + 1));
[array [i], array [j]] = [array [j], array [i]];
}
}

// contagens de aparências para todas as permutações possíveis
deixe count = {
'123': 0,
'132': 0,
'213': 0,
'231': 0,
'321': 0,
'312': 0
};

para (vamos i = 0; i <1000000; i ++) {
Deixe array = [1, 2, 3];
shuffle (array);
contagem [array.join ('')] ++;
}

// mostra contagens de todas as permutações possíveis
para (let key in count) {
alerta (`$ {key}: $ {count [key]}`);
}
`` `

O exemplo de saída:

`` `js
123: 166693
132: 166647
213: 166628
231: 167517
312: 166199
321: 166316
`` `

Parece bom agora: todas as permutações aparecem com a mesma probabilidade.

Além disso, o algoritmo Fisher-Yates é muito melhor, não há "classificação" de despesas gerais.
