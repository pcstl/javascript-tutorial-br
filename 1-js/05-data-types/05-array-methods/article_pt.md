# Métodos de Array

Arrays fornecem muitos métodos. Para facilitar as coisas, neste capítulo são divididos em grupos.

[cortar]

## Adicionar / remover itens

Nós já conhecemos métodos que adicionam e removem itens desde o início ou o final:

- `arr.push (... items)` - adiciona itens ao final,
- `arr.pop ()` - extrai um item do final,
- `arr.shift ()` - extrai um item desde o início,
- `arr.unshift (... items)` - adiciona itens ao início.

Aqui estão alguns outros.

### splice

Como excluir um elemento da matriz?

Os arrays são objetos, então podemos tentar usar `delete`:

`` `js run
deixe arr = ["I", "go", "home"];

apagar arr [1]; // remove "go"

alerta (arr [1]); // Indefinido

// agora arr = ["I",, "home"];
alerta (arr.length); // 3
`` `

O elemento foi removido, mas a matriz ainda possui 3 elementos, podemos ver que `arr.length == 3`.

Isso é natural, porque `delete obj.key` remove um valor pela` chave`. É tudo o que faz. Bom para objetos. Mas, para os arrays, geralmente queremos que o resto dos elementos se movam e ocupem o lugar libertado. Esperamos ter uma matriz mais curta agora.

Portanto, métodos especiais devem ser usados.

O método [arr.splice (str)] (mdn: js / Array / splice) é uma faca de exército suíça para arrays. Pode fazer tudo: adicionar, remover e inserir elementos.

A sintaxe é:

`` `js
arr.splice (index [, deleteCount, elem1, ..., elemN])
`` `

Começa a partir da posição `index`: remove elementos` delete Count` e, em seguida, insere `elem1, ..., elemeNT` em seu lugar. Retorna a matriz de elementos removidos.

Este método é fácil de entender por meio de exemplos.

Vamos começar com a exclusão:

`` `js run
deixe arr = ["I", "estudo", "JavaScript"];

*! *
arr.splice (1, 1); // do índice 1 remove 1 elemento
* /! *

alerta (arr); // ["I", "JavaScript"]
`` `

Fácil, certo? A partir do índice `1` removeu o elemento` 1`.

No próximo exemplo, removemos 3 elementos e substituí-los pelos outros dois:

`` `js run
Deixe arr = [*! * "I", "estudo", "JavaScript", * /! * "direito", "agora"];

// remover 3 primeiros elementos e substituí-los por outro
arr.splice (0, 3, "Vamos", "dança")

alerta (arr) // agora [*! * "Vamos", "dance" * /! *, "certo", "agora"]
`` `

Aqui podemos ver que `splice` retorna a matriz de elementos removidos:

`` `js run
Deixe arr = [*! * "I", "estudo", * /! * "JavaScript", "certo", "agora"];

// remove 2 primeiros elementos
Deixe removido = arr.splice (0, 2);

alerta (removido); // "Eu", "estudo" <- conjunto de elementos removidos
`` `

O método `splice` também é capaz de inserir os elementos sem remoções. Para isso precisamos definir `deleteCount` para` 0`:

`` `js run
deixe arr = ["I", "estudo", "JavaScript"];

// do índice 2
// apagar 0
// então insira "complexo" e "idioma"
arr.splice (2, 0, "complexo", "idioma");

alerta (arr); // "eu", "estudo", "complexo", "linguagem", "JavaScript"
`` `

`` `` cabeçalho inteligente = "Índices negativos permitidos"
Aqui e em outros métodos de matriz, os índices negativos são permitidos. Eles especificam a posição do final da matriz, como aqui:

`` `js run
deixe arr = [1, 2, 5]

// do índice -1 (um passo do final)
// apagar 0 elementos,
// então insira 3 e 4
arr.splice (-1, 0, 3, 4);

alerta (arr); // 1,2,3,4,5
`` `
`` ``

### fatia

O método [arr.slice] (mdn: js / Array / slice) é muito mais simples que o `arr.splice 'de aparência semelhante.

A sintaxe é:

`` `js
arr.slice (start, end)
`` `

Ele retorna uma nova matriz onde copia todos os itens inicialize o índice `" start "` to `" end "` (não incluindo `" end "`). Tanto o `start 'quanto o` end` podem ser negativos, nesse caso, a posição do terminal da matriz é assumida.

Funciona como `str.slice`, mas faz subarrays em vez de substrings.

Por exemplo:

`` `js run
deixe str = "teste";
deixe arr = ["t", "e", "s", "t"];

alerta (str.slice (1, 3)); // es
alerta (arr.slice (1, 3)); // e, s

alerta (str.slice (-2)); // st
alerta (arr.slice (-2)); // s, t
`` `

### concat

O método [arr.concat] (mdn: js / Array / concat) junta a matriz com outros arrays e / ou itens.

A sintaxe é:

`` `js
arr.concat (arg1, arg2 ...)
`` `

Ele aceita qualquer número de argumentos - arrays ou valores.

O resultado é uma nova matriz contendo itens de `arr`, então` arg1`, `arg2` etc.

Se um argumento for uma matriz ou tiver uma propriedade `Symbol.isConcatSpreadable`, todos os seus elementos serão copiados. Caso contrário, o próprio argumento é copiado.

Por exemplo:

`` `js run
deixe arr = [1, 2];

// fusionar arr com [3,4]
alerta (arr.concat ([3, 4])); // 1,2,3,4

// combinar arr com [3,4] e [5,6]
alerta (arr.concat ([3, 4], [5, 6])); // 1,2,3,4,5,6

// combinar arr com [3,4], então adicionar valores 5 e 6
alerta (arr.concat ([3, 4], 5, 6)); // 1,2,3,4,5,6
`` `

Normalmente, ele apenas copia elementos de arrays ("os espalha"), outros objetos, mesmo que pareçam arrays e adicionados como um todo:

`` `js run
deixe arr = [1, 2];

Deixe arrayLike = {
0: "algo",
comprimento: 1
};

alerta (arr.concat (arrayLike)); // 1,2, [object Object]
// [1, 2, arrayLike]
`` `

... Mas se um objeto tipo array tiver uma propriedade `Symbol.isConcatSpreadable`, então seus elementos serão adicionados em vez disso:

`` `js run
deixe arr = [1, 2];

Deixe arrayLike = {
0: "algo",
1: "else",
*! *
[Symbol.isConcatSpreadable]: true
* /! *
comprimento: 2
};

alerta (arr.concat (arrayLike)); // 1,2, algo, senão
`` `

## Searching in array

Estes são métodos para procurar algo em uma matriz.

### indexOf / lastIndexOf e inclui

Os métodos [arr.indexOf] (mdn: js / Array / indexOf), [arr.lastIndexOf] (mdn: js / Array / lastIndexOf) e [arr.includes] (mdn: js / Array / includes) têm a mesma sintaxe e fazem essencialmente o mesmo que as suas contrapartes de string, mas operam em itens em vez de caracteres:

- `arr.indexOf (item, from)` procura por `item` a partir do índice` from`, e retorna o índice onde foi encontrado, outro '-1'.
- `arr.lastIndexOf (item, from)` - o mesmo, mas parece da direita para a esquerda.
- `arr.includes (item, from)` - procura por `item 'a partir do índice` from`, retorna `true` se encontrado.

Por exemplo:

`` `js run
deixe arr = [1, 0, false];

alerta (arr.indexOf (0)); // 1
alerta (arr.indexOf (falso)); // 2
alerta (arr.indexOf (nulo)); // -1

alerta (arr.includes (1)); // verdade
`` `

Observe que os métodos usam a comparação `===`. Então, se procuramos 'falso', ele encontra exatamente 'falso' e não o zero.

Se quisermos verificar a inclusão e não queremos saber o índice exato, então o `arr.includes` é preferido.


### find and findIndex

Imagine que temos uma série de objetos. Como encontramos um objeto com a condição específica?

Aqui, o método [arr.find] (mdn: js / Array / find) é útil.

A sintaxe é:
`` `js
let result = arr.find (função (item, índice, matriz) {
// deve retornar verdadeiro se o item for o que estamos procurando
});
`` `

A função é chamada repetidamente para cada elemento da matriz:

- `item` é o elemento.
- `index` é seu índice.
- `array` é a própria matriz.

Se retornar `true`, a pesquisa é parada, o 'item' é retornado. Se nada for encontrado, 'indefinido' é retornado.

Por exemplo, temos uma variedade de usuários, cada um com os campos `id` e` name`. Vamos encontrar aquele com `id == 1`:

`` `js run
deixe os usuários = [
{id: 1, nome: "John"},
{id: 2, nome: "Pete"},
{id: 3, nome: "Mary"}
];

Deixe user = users.find (item => item.id == 1);

alerta (user.name); // John
`` `

Na vida real, arrays de objetos são comuns, então o método `find` é muito útil.

Observe que, no exemplo, fornecemos para "encontrar" uma função de argumento único `item => item.id == 1`. Outros parâmetros de `find` raramente são usados.

O método [arr.findIndex] (mdn: js / Array / findIndex) é essencialmente o mesmo, mas retorna o índice onde o elemento foi encontrado em vez do próprio elemento.

### filtro

O método `find` procura um elemento único (primeiro) que faz a função retornar` true`.

Se houver muitos, podemos usar [arr.filter (fn)] (mdn: js / Array / filter).

A sintaxe é aproximadamente igual a `find`, mas retorna uma matriz de elementos correspondentes:

`` `js
Deixe resultados = arr.filter (função (item, índice, matriz) {
// deve retornar verdadeiro se o item passar pelo filtro
});
`` `

Por exemplo:

`` `js run
deixe os usuários = [
{id: 1, nome: "John"},
{id: 2, nome: "Pete"},
{id: 3, nome: "Mary"}
];

// retorna matriz dos dois primeiros usuários
deixe someUsers = users.filter (item => item.id <3);

alerta (someUs.length); // 2
`` `

## Transformar uma matriz

Esta seção trata sobre os métodos que transformam ou reordem a matriz.


### mapa

O método [arr.map] (mdn: js / Array / map) é um dos mais úteis e freqüentemente usado.

A sintaxe é:

`` `js
Deixe resultado = arr.map (função (item, índice, matriz) {
// retorna o novo valor em vez do item
})
`` `

Ele chama a função para cada elemento da matriz e retorna a matriz de resultados.

Por exemplo, aqui transformamos cada elemento em seu comprimento:

`` `js run
Let lengths = ["Bilbo", "Gandalf", "Nazgul"]. mapa (item => item.length)
alerta (comprimentos); // 5,7,6
`` `

### classificar (fn)

O método [arr.sort] (mdn: js / Array / sort) classifica a matriz * no lugar *.

Por exemplo:

`` `js run
deixe arr = [1, 2, 15];

// o método reordena o conteúdo de arr (e o retorna)
arr.sort ();

alerta (arr); // *! * 1, 15, 2 * /! *
`` `

Você notou algo estranho no resultado?

A ordem tornou-se "1, 15, 2". Incorreta. Mas por que?

** Os itens são classificados como strings por padrão. **

Literalmente, todos os elementos são convertidos em strings e depois comparados. Então, o pedido lexicográfico é aplicado e, de fato, "2"> "15" `.

Para usar nossa própria ordem de classificação, precisamos fornecer uma função de dois argumentos como o argumento de `arr.sort ()`.

A função deve funcionar assim:
`` `js
comparação de função (a, b) {
se (a> b) retornar 1;
se (a == b) retornar 0;
se (a <b) retornar -1;
}
`` `

Por exemplo:

`` `js run
function compareNumeric (a, b) {
se (a> b) retornar 1;
se (a == b) retornar 0;
se (a <b) retornar -1;
}

deixe arr = [1, 2, 15];

*! *
arr.sort (compareNumeric);
* /! *

alerta (arr); // *! * 1, 2, 15 * /! *
`` `

Agora funciona como pretendido.

Deixemos de lado e pensemos no que está acontecendo. O `arr` pode ser uma variedade de coisa, certo? Pode conter números ou cordas ou elementos html ou o que for. Temos um conjunto de * algo *. Para ordená-lo, precisamos de uma * função de pedido * que sabe comparar seus elementos. O padrão é uma ordem de string.

O método `arr.sort (fn)` possui uma implementação integrada do algoritmo de classificação. Não precisamos nos importar com o funcionamento exato (um otimizado [quicksort] (https://en.wikipedia.org/wiki/Quicksort) na maioria das vezes). Ele irá andar na matriz, comparar seus elementos usando a função fornecida e reorganizá-los, tudo o que precisamos é fornecer o `fn` que faz a comparação.

Por sinal, se quisermos saber quais elementos são comparados - nada impede de alertá-los:

`` `js run
[1, -2, 15, 2, 0, 8] .sort (função (a, b) {
alerta (a + "<>" + b);
});
`` `

O algoritmo pode comparar um elemento várias vezes no processo, mas tenta fazer o menor número possível de comparações.


`` `` cabeçalho inteligente = "Uma função de comparação pode retornar qualquer número"
Na verdade, uma função de comparação só é necessária para retornar um número positivo para dizer "maior" e um número negativo para dizer "menos".

Isso permite escrever funções mais curtas:

`` `js run
deixe arr = [1, 2, 15];

arr.sort (função (a, b) {return a - b;});

alerta (arr); // *! * 1, 2, 15 * /! *
`` `
`` ``

`` `smart header =" Seta funciona para o melhor "
Lembre-se [funções de seta] (info: função-expressão # seta-funções)? Podemos usá-los aqui para uma classificação mais pura:

`` `js
arr.sort ((a, b) => a - b);
`` `

Isso funciona exatamente o mesmo que o outro, mais longo, a versão acima.
`` ``

### marcha ré

O método [arr.reverse] (mdn: js / Array / reverse) inverte a ordem dos elementos em `arr`.

Por exemplo:

`` `js run
deixe arr = [1, 2, 3, 4, 5];
arr.verse ();

alerta (arr); // 5,4,3,2,1
`` `

Ele também retorna a matriz `arr` após a reversão.

### divide e junte-se

Aqui está a situação da vida real. Estamos escrevendo um aplicativo de mensagens, e a pessoa entra na lista de receptores delimitada por vírgulas: `John, Pete, Mary`. Mas para nós, uma série de nomes seria muito mais confortável do que uma única string. Como obtê-lo?

O método [str.split (delim)] (mdn: js / String / split) faz exatamente isso. Ele divide a string em uma matriz pelo delimitador delimitado `delim`.

No exemplo abaixo, dividimos por uma vírgula seguida de espaço:

`` `js run
Deixe nomes = 'Bilbo, Gandalf, Nazgul';

Deixe arr = names.split (',');

para (nome do arr) {
alerta (`Uma mensagem para $ {nome} .`); // Uma mensagem para Bilbo (e outros nomes)
}
`` `

O método `split` possui um segundo argumento numérico opcional - um limite no comprimento da matriz. Se for fornecido, os elementos extras são ignorados. Na prática, raramente é usado:

`` `js run
let arr = 'Bilbo, Gandalf, Nazgul, Saruman'.split (', ', 2);

alerta (arr); // Bilbo, Gandalf
`` `

`` `` cabeçalho inteligente = "dividir em letras"
A chamada para `split (s)` com um `s vazio 'dividiria a string em uma matriz de letras:

`` `js run
deixe str = "teste";

alerta (str.split ('')); // teste
`` `
`` ``

A chamada [arr.join (str)] (mdn: js / Array / join) faz o inverso para `split`. Ele cria uma série de itens `arr` colados por` str` entre eles.

Por exemplo:

`` `js run
deixe arr = ['Bilbo', 'Gandalf', 'Nazgul'];

Deixe str = arr.join (';');

alerta (str); // Bilbo; Gandalf; Nazgul
`` `

### reduzir / reduzirRight

Quando precisamos fazer uma iteração em uma matriz - podemos usar `forEach`.

Quando precisamos iterar e retornar os dados para cada elemento - podemos usar `map`.

Os métodos [arr.reduce] (mdn: js / Array / reduce) e [arr.reduceRight] (mdn: js / Array / reducedRight) também pertencem a essa raça, mas são um pouco mais intrincados. Eles são usados ​​para calcular um único valor com base na matriz.

A sintaxe é:

`` `js
Deixe value = arr.reduce (função (previousValue, item, index, arr) {
// ...
}, inicial);
`` `

A função é aplicada aos elementos. Você pode notar os argumentos familiares, a partir do 2º:

- `item` - é o item da matriz atual.
- `index` - é a sua posição.
- `arr` - é a matriz.

Até agora, como `forEach / map`. Mas há mais um argumento:

- `previousValue` - é o resultado da chamada de função anterior,` inicial` para a primeira chamada.

A maneira mais fácil de entender é o exemplo.

Aqui, obtemos uma soma de matriz em uma linha:

`` `js run
deixe arr = [1, 2, 3, 4, 5]

let result = arr.reduce ((soma, corrente) => soma + corrente, 0);

alerta (resultado); // 15
`` `

Aqui, usamos a variante mais comum de "reduzir", que usa apenas 2 argumentos.

Vamos ver os detalhes do que está acontecendo.

1. Na primeira execução, `sum` é o valor inicial (o último argumento de` reduzir`), igual a `0` e` current` é o primeiro elemento de matriz, é igual a `1`. Então, o resultado é `1`.
2. Na segunda execução, `sum = 1`, adicionamos o segundo elemento de matriz (` 2`) e retornamos.
3. Na 3ª execução, `sum = 3` e adicionamos mais um elemento a ele, e assim por diante ...

O fluxo de cálculo:

! [] (redução.png)

Ou na forma de uma tabela, onde cada linha representa é uma chamada de função no próximo elemento da matriz:

| | `sum` |` current` | `result` |
| --- | ----- | --------- | --------- |
| a primeira chamada | `0` |` 1` | `1` |
| a segunda chamada | `1` |` 2` | `3` |
| a terceira chamada | `3` |` 3` | `6` |
| quarta chamada | `6` |` 4` | `10` |
| o quinto chamado | `10` |` 5` | `15` |


Como podemos ver, o resultado da chamada anterior se torna o primeiro argumento do próximo.

Também podemos omitir o valor inicial:

`` `js run
deixe arr = [1, 2, 3, 4, 5];

// removeu valor inicial de reduzir (não 0)
Deixe resultado = arr.reduce ((soma, corrente) => soma + corrente);

alerta (resultado); // 15
`` `

O resultado é o mesmo. Isso ocorre porque, se não houver inicial, então, "reduzir" leva o primeiro elemento da matriz como valor inicial e inicia a iteração a partir do 2º elemento.

A tabela de cálculo é a mesma que acima, menos a primeira linha.

Mas esse uso requer um cuidado extremo. Se a matriz estiver vazia, a chamada "reduzir" sem valor inicial dá um erro.

Aqui está um exemplo:

`` `js run
deixe arr = [];

// Erro: Reduzir a matriz vazia sem valor inicial
// se o valor inicial existisse, reduziria devolvê-lo para o arranjo vazio.
arr.reduce ((soma, corrente) => soma + corrente);
`` `


Portanto, é aconselhável especificar sempre o valor inicial.

O método [arr.reduceRight] (mdn: js / Array / reductionRight) faz o mesmo, mas vai da direita para a esquerda.


## Iterate: forEach

O método [arr.forEach] (mdn: js / Array / forEach) permite executar uma função para cada elemento da matriz.

A sintaxe:
`` `js
arr.forEach (função (item, índice, matriz) {
// ... faça algo com item
});
`` `

Por exemplo, isso mostra cada elemento da matriz:

`` `js run
// para cada alerta de chamada de elemento
["Bilbo", "Gandalf", "Nazgul"]. Para todos (alerta);
`` `

E este código é mais elaborado sobre suas posições na matriz de destino:

`` `js run
["Bilbo", "Gandalf", "Nazgul"]. ForEach ((item, index, array) => {
alerta ($ $ item} está no índice $ {index} em $ {array} `);
});
`` `

O resultado da função (se retorna) é descartado e ignorado.

## Array.isArray

As matrizes não formam um tipo de idioma separado. Eles são baseados em objetos.

Então, `typeof` não ajuda a distinguir um objeto simples de uma matriz:

`` `js run
alerta (typeof {}); // objeto
alerta (typeof []); // mesmo
`` `

... Mas os arrays são usados ​​tantas vezes que existe um método especial para isso: [Array.isArray (value)] (mdn: js / Array / isArray). Ele retorna `true` se o` value` for uma matriz e `false` caso contrário.

`` `js run
alerta (Array.isArray ({})); // falso

alerta (Array.isArray ([])); // verdade
`` `

## A maioria dos métodos suporta "thisArg"

Quase todos os métodos de matriz que chamam funções - como `find`,` filter`, `map`, com uma exceção notável de` sort`, aceitam um parâmetro adicional opcional `thisArg`.

Esse parâmetro não é explicado nas seções acima, porque raramente é usado. Mas, por completo, temos que cobri-lo.

Aqui está a sintaxe completa desses métodos:

`` `js
arr.find (func, thisArg);
arr.filter (func, thisArg);
arr.map (func, thisArg);
// ...
// thisArg é o último argumento opcional
`` `

O valor do parâmetro `thisArg` se torna` this` para `func`.

Por exemplo, aqui usamos um método de objeto como um filtro e `thisArg` é útil:

`` `js run
Deixe o usuário = {
idade: 18,
mais novo (otherUser) {
retornar otherUser.age <this.age;
}
};

deixe os usuários = [
{idade: 12},
{idade: 16},
{idade: 32}
];

*! *
// encontra todos os usuários mais novos do que o usuário
deixe youngUsers = users.filter (user.younger, user);
* /! *

alerta (youngUsers.length); // 2
`` `

Na chamada acima, usamos `user.younger` como um filtro e também fornecemos` usuário 'como contexto para ele. Se não fornecemos o contexto, `users.filter (user.younger) 'chamaria` user.younger` como uma função autônoma, com `this = undefined`. Isso significaria um erro instantâneo.

## Resumo

Uma cheatsheet de métodos de matriz:

- Para adicionar / remover elementos:
- `push (... items)` - adiciona itens ao final,
- `pop ()` - extrai um item do final,
- `shift ()` - extrai um item desde o início,
- `unshift (... items)` - adiciona itens ao início.
- `splice (pos, deleteCount, ... items)` - no índice `pos` delete` deleteCount` elementos e insira `items`.
- `slice (start, end)` - cria uma nova matriz, copia elementos da posição `start` até` end` (não inclusivo) nele.
- `concat (... items)` - retorna uma nova matriz: copia todos os membros do atual e adiciona `itens 'a ele. Se algum dos `itens` for uma matriz, então seus elementos serão retirados.

- Procurar entre elementos:
- `indexOf / lastIndexOf (item, pos)` - procure `item` a partir da posição` pos`, retornar o índice ou `-1` se não for encontrado.
- `includes (value)` - retorna `true` se a matriz tiver` value`, caso contrário `false`.
- `find / filter (func)` - filtra elementos de através da função, retorna primeiro / todos os valores que o fazem retornar `true`.
- `FindIndex` é como` find`, mas retorna o índice em vez de um valor.

- Para transformar a matriz:
- `map (func)` - cria uma nova matriz a partir dos resultados da chamada `func` para cada elemento.
- `sort (func)` - classifica a matriz no local e depois retorna.
- `reverse ()` - inverte a matriz no local, depois retorna.
- `split / join` - converta uma string para array e vice-versa.
- `reduce (func, initial)` - calcule um único valor sobre a matriz chamando `func` para cada elemento e passando um resultado intermediário entre as chamadas.

- Para iterar sobre elementos:
- `forEach (func)` - chama `func` para cada elemento, não retorna nada.

- Além disso:
- `Array.isArray (arr)` verifica `arr` para ser uma matriz.

De todos esses métodos, apenas `sort`,` reverse` e ​​`splice` modificam a matriz em si, os outros apenas retornam um valor.

Esses métodos são os mais usados, eles cobrem 99% dos casos de uso. Mas há poucos outros:

- [arr.some (fn)] (mdn: js / Array / some) / [arr.every (fn)] (mdn: js / Array / every) verifica a matriz.

A função `fn` é chamada em cada elemento da matriz semelhante ao` map`. Se algum / todos os resultados forem verdadeiros, retorna `true`, caso contrário` false`.

- [arr.fill (value, start, end)] (mdn: js / Array / fill) - preenche a matriz com o `` `de repetição do índice` start` para `end`.

- [arr.copyWithin (target, start, end)] (mdn: js / Array / copyWithin) - copia os seus elementos da posição `start` até a posição` end` em * em si *, na posição `target` (sobrescreve existente ).

Para a lista completa, veja [manual] (mdn: js / Array).

Desde a primeira vista, pode parecer que há tantos métodos, bastante difíceis de lembrar. Mas na verdade isso é muito mais fácil do que parece.

Olhe através da cheatsheet apenas para estar ciente deles. Em seguida, resolva as tarefas deste capítulo para praticar, para que você tenha experiência com métodos de matriz.

Depois, sempre que você precisa fazer algo com uma matriz, e você não sabe como - venha aqui, olhe a cheatsheet e ache o método certo. Exemplos irão ajudá-lo a escrever corretamente. Em breve você lembrará automaticamente os métodos, sem esforços específicos do seu lado.
