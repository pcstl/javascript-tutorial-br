
# Map, Set, WeakMap e WeakSet

Agora, conhecemos as seguintes estruturas de dados complexas:

- Objetos para armazenar coleções com chave.
- Arrays para armazenar coleções encomendadas.

Mas isso não é suficiente para a vida real. É por isso que também existem `Map` e` Set`.

## Map

[Mapa] (mdn: js / Map) é uma coleção de itens de dados com chave. Assim como um `Objeto '. Mas a principal diferença é que `Map` permite as chaves de qualquer tipo.

Os principais métodos são:

- `new Map ()` - cria o mapa.
- `map.set (key, value)` - armazena o valor pela chave.
- `map.get (key)` - retorna o valor pela chave.
- `map.has (key)` - retorna `true` se a` key` existe, `false` caso contrário.
- `map.delete (key)` - remove o valor pela chave.
- `map.clear ()` - limpa o mapa
- `map.size` - é a contagem de elementos atuais.

Por exemplo:

`` `js run
Deixe map = new Map ();

map.set ('1', 'str1'); // uma chave de string
map.set (1, 'num1'); // uma tecla numérica
map.set (true, 'bool1'); // uma chave booleana

// lembre-se do objeto comum? ele converteria chaves para string
// O mapa mantém o tipo, então estes dois são diferentes:
alerta (map.get (1)); // 'num1'
alerta (map.get ('1')); // 'str1'

alerta (map.size); // 3
`` `

Como podemos ver, ao contrário dos objetos, as chaves não são convertidas em strings. Qualquer tipo de chave é possível.

** O mapa também pode usar objetos como chaves. **

Por exemplo:
`` `js run
deixe john = {name: "John"};

// para cada usuário, vamos armazenar sua contagem de visitas
deixe visitasCountMap = novo Mapa ();

// john é a chave para o mapa
visitasCountMap.set (john, 123);

alerta (visitasCountMap.get (john)); // 123
`` `

Usar objetos como chaves é um dos recursos mais importantes e importantes do `Map`. Para as chaves de string, `Object` pode estar bem, mas seria difícil substituir o` Map` por um 'Objeto' regular no exemplo acima.

Nos velhos tempos, antes do `Mapa 'existia, as pessoas adicionaram identificadores exclusivos aos objetos para isso:

`` `js run
// adicionamos o campo id
deixe john = {nome: "John", *! * id: 1 * /! *};

deixe visitasCounts = {};

// agora armazena o valor por id
visitasContas [john.id] = 123;

alerta (visitsCounts [john.id]); // 123
`` `

... Mas `Map` é muito mais elegante.


`` `smart header =" Como `Map` compara chaves"
Para testar valores de equivalência, `Map` usa o algoritmo [SameValueZero] (https://tc39.github.io/ecma262/#sec-samevaluezero). É aproximadamente o mesmo que a igualdade estrita `===`, mas a diferença é que `NaN` é considerado igual a` NaN`. Então, `NaN` pode ser usado como a chave também.

Este algoritmo não pode ser alterado ou personalizado.
`` `


`` `` smart header = "Chaining"

Toda chamada `map.set` retorna o próprio mapa, para que possamos" encadear "as chamadas:

`` `js
map.set ('1', 'str1')
.set (1, 'num1')
.set (true, 'bool1');
`` `
`` ``

## Map from Object

Quando um `Map` é criado, podemos passar uma matriz (ou outra iterável) com pares chave-valor, como este:

`` `js
// matriz de pares [chave, valor]
let map = new Map ([
['1', 'str1'],
[1, 'num1'],
[verdadeiro, 'bool1']
]);
`` `

Existe um método incorporado [Object.entries (obj)] (mdn: js / Object / entries) que retorna a matriz de pares de chave / valor para um objeto exatamente nesse formato.

Então podemos inicializar um mapa de um objeto como este:

`` `js
Deixe map = novo Mapa (Object.entries ({
Nome: "John",
idade: 30
}));
`` `

Aqui, `Object.entries` retorna a matriz de pares de chave / valor:` [["name", "John"], ["age", 30]] `. É isso que o `Map` precisa.

## Iteração sobre o Mapa

Para fazer um loop sobre um `map`, existem 3 métodos:

- `map.keys ()` - retorna uma iterável para chaves,
- `map.values ​​()` - retorna um iterable para valores,
- `map.entries ()` - retorna um iterable para entradas `[key, value]`, é usado por padrão em `for..of`.

Por exemplo:

`` `js run
deixe recipeMap = novo mapa ([
['pepino', 500],
['tomates', 350],
['cebola', 50]
]);

// iterar sobre chaves (vegetais)
para (deixe vegetais de recipeMap.keys ()) {
alerta (vegetal); // pepino, tomate, cebola
}

// iterar sobre valores (valores)
para (deixe o valor de recipeMap.values ​​()) {
alerta (quantidade); // 500, 350, 50
}

// iterar sobre entradas [chave, valor]
para (deixe entrada de recipeMap) {// o mesmo que de recipeMap.entries ()
alerta (entrada); // pepino, 50 (e assim por diante)
}
`` `

`` `cabeçalho inteligente =" A ordem de inserção é usada "
A iteração vai na mesma ordem em que os valores foram inseridos. `Map` preserva essa ordem, ao contrário de um" Objeto "regular.
`` `

Além disso, `Map` possui um método 'forEach' incorporado, semelhante ao` Array`:

`` `js
recipeMap.forEach ((valor, chave, mapa) => {
alerta (`$ {key}: $ {value}`); // pepino: 50 etc.
});
`` `


## Set

`Set` - é uma coleção de valores, onde cada valor pode ocorrer apenas uma vez.

Os principais métodos são:

- `new Set (iterable)` - cria o conjunto, opcionalmente a partir de uma matriz de valores (qualquer iterable fará).
- `set.add (value)` - adiciona um valor, retorna o conjunto em si.
- `set.delete (value)` - remove o valor, retorna `true` se` value` existiu no momento da chamada, caso contrário, `false`.
- `set.has (value)` - retorna `true` se o valor existe no conjunto, caso contrário,` false`.
- `set.clear ()` - remove tudo do conjunto.
- `set.size` - é a contagem de elementos.

Por exemplo, temos visitantes chegando, e gostaríamos de nos lembrar de todos. Mas visitas repetidas não devem levar a duplicações. Um visitante deve ser "contado" apenas uma vez.

`Set` é apenas a coisa certa para isso:

`` `js run
Deixe set = new Set ();

deixe john = {name: "John"};
let pete = {name: "Pete"};
deixe mary = {nome: "Mary"};

// visitas, alguns usuários vêm várias vezes
set.add (john);
set.add (pete);
set.add (mary);
set.add (john);
set.add (mary);

// set mantém apenas valores exclusivos
alerta (set.size); // 3

para (deixe o usuário do conjunto) {
alerta (user.name); // John (então Pete e Mary)
}
`` `

A alternativa para `Set` pode ser uma matriz de usuários e o código para verificar duplicatas em cada inserção usando [arr.find] (mdn: js / Array / find). Mas o desempenho seria muito pior, porque este método atravessa toda a matriz verificando todos os elementos. `Set` é muito melhor otimizado internamente para verificações de singularidade.

## Iteration over Set

Podemos encaminhar um conjunto com `for..of` ou usando` forEach`:

`` `js run
deixe set = new Set (["laranjas", "maçãs", "bananas"]);

para (deixar valor do conjunto) alerta (valor);

// o mesmo com forEach:
set.forEach ((value, valueAgain, set) => {
alerta (valor);
});
`` `

Observe o engraçado. A função `forEach` no` Set` tem 3 argumentos: um valor, então * novamente um valor * e, em seguida, o objeto alvo. Na verdade, o mesmo valor aparece nos argumentos duas vezes.

Isso é feito para compatibilidade com `Map`, onde` forEach` tem três argumentos.

Os mesmos métodos que `Map` para iteradores também são suportados:

- `set.keys ()` - retorna um objeto iterable para valores,
- `set.values ​​()` - same as `set.keys`, para compatibilidade com` Map`,
- `set.entries ()` - retorna um objeto iterável para entradas `[value, value]`, existe para compatibilidade com `Map`.

## WeakMap e WeakSet

`WeakSet` é um tipo especial de` Set` que não impede que o JavaScript remova seus itens da memória. `WeakMap` é a mesma coisa para` Map`.

Como sabemos do capítulo <info: basura-coleção>, o mecanismo JavaScript armazena um valor na memória enquanto ele é acessível (e potencialmente pode ser usado).

Por exemplo:
`` `js
deixe john = {name: "John"};

// o objeto pode ser acessado, john é a referência a ele

// substituir a referência
john = null;

*! *
// o objeto será removido da memória
* /! *
`` `

Geralmente, as propriedades de um objeto ou elementos de uma matriz ou outra estrutura de dados são consideradas acessíveis e mantidas na memória enquanto essa estrutura de dados está em memória.

Em um "Mapa" regular, não importa se nós armazenamos um objeto como uma chave ou como um valor. É mantido em memória mesmo que não haja mais referências a ele.

Por exemplo:
`` `js
deixe john = {name: "John"};

Deixe map = new Map ();
map.set (john, "...");

john = null; // substituir a referência

*! *
// john é armazenado dentro do mapa
// podemos obtê-lo usando map.keys ()
* /! *
`` `


Com exceção do `WeakMap / WeakSet`.

** `WeakMap / WeakSet` não impede a remoção do objeto da memória. **

Vamos começar com `WeakMap`.

A primeira diferença de `Map` é que suas chaves devem ser objetos, não valores primitivos:

`` `js run
deixe o MySMap fraco = novo WeakMap ();

deixe obj = {};

weakMap.set (obj, "ok"); // funciona bem (chave do objeto)

*! *
weakMap.set ("teste", "Whoops"); // Erro, porque "teste" é uma primitiva
* /! *
`` `

Agora, se usarmos um objeto como a chave nele, e não há outras referências a esse objeto - ele será removido da memória (e do mapa) automaticamente.

`` `js
deixe john = {name: "John"};

deixe o MySMap fraco = novo WeakMap ();
weakMap.set (john, "...");

john = null; // substituir a referência

// john é removido da memória!
`` `

Compare-o com o exemplo regular do `` Mapa 'acima. Agora, se `john` só existe como a chave do` WeakMap` - ele deve ser excluído automaticamente.

... E `WeakMap` não suporta métodos` keys () `,` values ​​() `,` entries () `, não podemos iterar sobre ele. Portanto, não há como receber todas as chaves ou valores.

`WeakMap` tem apenas os seguintes métodos:

- `weakMap.get (key)`
- `weakMap.set (chave, valor)`
- `weakMap.delete (chave, valor)`
- `weakMap.has (key)`

Por que tal limitação? Isso é por razões técnicas. Se o objeto perdeu todas as outras referências (como `john` no código acima), então ele deve ser excluído automaticamente. Mas, tecnicamente, não é exatamente especificado * quando ocorre a limpeza *.

O mecanismo de JavaScript decide isso. Pode optar por executar a limpeza da memória imediatamente ou aguardar e fazer a limpeza mais tarde, quando ocorrerem mais delegações. Então, tecnicamente, a contagem de elementos atual do `WeakMap` não é conhecida. O motor pode ter limpo ou não, ou foi parcialmente. Por esse motivo, os métodos que acessam `WeakMap` como um todo não são suportados.

Agora, onde precisamos de tal coisa?

A idéia de `WeakMap` é que podemos armazenar algo para um objeto que existe somente enquanto o objeto existe. Mas não forçamos o objeto a viver pelo mero fato de que nós armazenamos algo por isso.

`` `js
weakMap.put (john, "documentos secretos");
// se John morre, os documentos secretos serão destruídos
`` `

Isso é útil para situações em que temos um armazenamento principal para os objetos em algum lugar e precisamos manter informações adicionais que só são relevantes enquanto o objeto vive.

Vamos ver um exemplo.

Por exemplo, temos um código que mantém uma contagem de visitas para cada usuário. A informação é armazenada em um mapa: um usuário é a chave ea contagem da visita é o valor. Quando um usuário sai, não queremos armazenar seu número de visitas mais.

Uma maneira seria acompanhar a saída dos usuários e a limpeza manual do armazenamento:

`` `js run
deixe john = {name: "John"};

// mapa: usuário => contagem de visitas
deixe visitasCountMap = novo Mapa ();

// john é a chave para o mapa
visitasCountMap.set (john, 123);

// agora john nos deixa, não precisamos dele mais
john = null;

*! *
// mas ainda está no mapa, precisamos limpá-lo!
* /! *
alerta (viewsCountMap.size); // 1
// também está na memória, porque o Map usa como a chave
`` `

Outra maneira seria usar `WeakMap`:

`` `js
deixe john = {name: "John"};

deixe visitasCountMap = novo WeakMap ();

visitasCountMap.set (john, 123);

// agora john nos deixa, não precisamos dele mais
john = null;

// não há referências, exceto WeakMap,
// então o objeto é removido tanto da memória quanto de visitasCountMap automaticamente
`` `

Com um "Mapa" regular, limpar depois de um usuário ter deixado torna-se uma tarefa tediosa: não só precisamos remover o usuário de seu armazenamento principal (seja uma variável ou uma matriz), mas também precisamos limpar as lojas adicionais como `visitsCountMap`. E pode tornar-se complicado em casos mais complexos quando os usuários são gerenciados em um lugar do código e a estrutura adicional está em outro lugar e não está recebendo informações sobre remoções.

`WeakMap` pode tornar as coisas mais simples, porque é limpo automaticamente. A informação nela, como a contagem de visitas, no exemplo acima, mora apenas enquanto o objeto-chave existe.

`WeakSet` comporta-se de forma semelhante:

- É análogo a `Set`, mas podemos adicionar objetos a` WeakSet` (não primitivos).
- Existe um objeto no conjunto enquanto ele é acessível de outro lugar.
- Como `Set`, ele suporta` add`, `has` e` delete`, mas não `size`,` keys () `e não iterations.

Por exemplo, podemos usá-lo para acompanhar se um item está marcado:

`` `js
deixe mensagens = [
{texto: "Olá", de: "John"},
{texto: "Como vai?", de: "John"},
{text: "Até breve", de: "Alice"}
];

// preenchê-lo com elementos de matriz (3 itens)
Deixe unreadSet = new WeakSet (mensagens);

// podemos usar não lidosSet para ver se uma mensagem não está lida
alerta (unreadSet.has (mensagens [1])); // verdade
// remove-o do conjunto depois de ler
unreadSet.delete (mensagens [1]); // verdade

// e quando mudamos o histórico de nossas mensagens, o conjunto é limpo automaticamente
messages.shift ();
// não é necessário limpar não lidoSet, agora tem 2 itens
// infelizmente, não há nenhum método para obter a contagem exata de itens, portanto, não pode mostrar isso
`` `

A limitação mais notável do `WeakMap` e do` WeakSet` é a ausência de iterações e a incapacidade de obter todo o conteúdo atual. Isso pode parecer inconveniente, mas na verdade não impede que o `WeakMap / WeakSet` faça seu trabalho principal - seja um armazenamento" adicional "de dados para objetos armazenados / gerenciados em outro local.

## Resumo

- `Map` - é uma coleção de valores com chave.

As diferenças de um "Objeto" regular:

- Qualquer chave, objetos podem ser chaves.
- Iterates na ordem de inserção.
- Métodos convenientes adicionais, a propriedade `size`.

- `Set` - é uma coleção de valores únicos.

- Ao contrário de uma matriz, não permite reordenar elementos.
- Mantém a ordem de inserção.

- `WeakMap` - uma variante de` Map` que permite apenas objetos como chaves e os remove assim que eles se tornam inacessíveis por outros meios.

- Não suporta operações na estrutura como um todo: não `tamanho`, não` clear () `, sem iterações.

- `WeakSet` - é uma variante de` Set` que apenas armazena objetos e os remove depois de se tornarem inacessíveis por outros meios.

- Também não suporta `size / clear ()` e iterações.

`WeakMap` e` WeakSet` são usados ​​como estruturas de dados "secundárias", além do armazenamento de objetos "principal". Uma vez que o objeto é removido do armazenamento principal, então ele permanece apenas em `WeakMap / WeakSet`, eles são limpos aumatically.
