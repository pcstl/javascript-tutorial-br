Para encontrar todos os anagramas, divida cada palavra em letras e classifique-as. Quando ordenados por letras, todos os anagramas são iguais.

Por exemplo:

`` `
soneca, pan -> anp
orelha, era, são -> aer
trapaceiros, hectares, professores -> amherst
...
`` `

Usaremos as variantes classificadas por letras como chaves de mapa para armazenar apenas um valor por cada chave:

`` `js run
função aclean (arr) {
Deixe map = new Map ();

para (deixe a palavra de arr) {
// divide a palavra por letras, ordene-as e junte-se
*! *
Deixe ordenado = word.toLowerCase (). split (''). sort (). join (''); // (*)
* /! *
map.set (ordenado, palavra);
}

retornar Array.from (map.values ​​());
}

deixe arr = ["soneca", "professores", "trapaceiros", "PAN", "orelha", "era", "hectares"];

alerta (aclean (arr));
`` `

A classificação de letras é feita pela cadeia de chamadas na linha `(*)`.

Por conveniência, divida-o em várias linhas:

`` `js
Deixe ordenado = arr [i] // PAN
.toLowerCase () // pan
.split ('') // ['p', 'a', 'n']
.sort () // ['a', 'n', 'p']
.Junte-se(''); // anp
`` `

Duas palavras diferentes `` PAN'` e `'nap'` recebem o mesmo formulário ordenado por letra`' anp'`.

A próxima linha colocou a palavra no mapa:

`` `js
map.set (ordenado, palavra);
`` `

Se alguma vez encontrarmos uma palavra o mesmo formulário classificado por letras novamente, então ele substituiria o valor anterior pela mesma chave no mapa. Então, sempre teremos no máximo uma palavra por letra.

No final `Array.from (map.values ​​())` leva uma iterável sobre os valores do mapa (não precisamos de chaves no resultado) e retorna uma matriz delas.

Aqui, também podemos usar um objeto simples em vez do `Mapa ', porque as chaves são strings.

É assim que a solução pode parecer:

`` `js run
função aclean (arr) {
deixe obj = {};

para (deixe i = 0; i <arr.length; i ++) {
Deixe ordenado = arr [i] .toLowerCase (). split (""). sort (). join ("");
obj [ordenado] = arr [i];
}

retornar Array.from (Object.values ​​(obj));
}

deixe arr = ["soneca", "professores", "trapaceiros", "PAN", "orelha", "era", "hectares"];

alerta (aclean (arr));
`` `
