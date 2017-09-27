Vamos caminhar pelos itens da matriz:
- Para cada item, verificaremos se a matriz resultante já possui esse item.
- Se é assim, então ignore, caso contrário, adicione aos resultados.

`` `js run
função única (arr) {
Deixe o resultado = [];

para (deixe str de arr) {
se (! result.includes (str)) {
result.push (str);
}
}

resultado de retorno;
}

Deixe strings = ["Hare", "Krishna", "Hare", "Krishna",
"Krishna", "Krishna", "Hare", "Hare", ": -O"
];

alerta (exclusivo (strings)); // Hare, Krishna,: -O
`` `

O código funciona, mas existe um problema de desempenho potencial nele.

O método `result.includes (str)` caminha internamente o array `result` e compara cada elemento contra` str` para encontrar a correspondência.

Então, se houver elementos `100` em` result` e nenhum coincide com `str`, então ele irá percorrer todo o` result` e fazer exatamente as comparações `100`. E se `result` for grande, como` 10000`, então haveria comparações `10000`.

Isso não é um problema por si só, porque os motores de JavaScript são muito rápidos, então a matriz do `10000` é uma questão de microssegundos.

Mas fazemos esse teste para cada elemento de `arr`, no loop 'for`.

Então, se `arr.length` for` 10000`, teremos algo como `10000 * 10000` = 100 milhões de comparações. Isso é muito.

Portanto, a solução só é boa para pequenos arrays.

Além disso, no capítulo <info: map-set-weakmap-weakset> veremos como otimizar.
