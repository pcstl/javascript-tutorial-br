# Arrays

Os objetos permitem armazenar coleções de valores com chave. Isso é bom.

Mas muitas vezes achamos que precisamos de uma * coleção ordenada *, onde temos um 1º, um 2º, um 3º elemento e assim por diante. Por exemplo, precisamos disso para armazenar uma lista de algo: usuários, bens, elementos HTML etc.

Não é conveniente usar um objeto aqui, porque não fornece métodos para gerenciar a ordem dos elementos. Não podemos inserir uma nova propriedade "entre" as existentes. Os objetos não são apenas para tal uso.

Existe uma estrutura de dados especial denominada `Array`, para armazenar coleções ordenadas.

[cortar]

## Declaração

Existem duas sintaxes para criar uma matriz vazia:

`` `js
Deixe arr = new Array ();
deixe arr = [];
`` `

Quase todo o tempo, a segunda sintaxe é usada. Podemos fornecer elementos iniciais nos suportes:

`` `js
Deixe frutas = ["Maçã", "Laranja", "Ameixa"];
`` `

Os elementos de matriz são numerados, começando com zero.

Podemos obter um elemento por seu número entre colchetes:

`` `js run
Deixe frutas = ["Maçã", "Laranja", "Ameixa"];

alerta (frutas [0]); // Maçã
alerta (frutas [1]); // Laranja
alerta (frutas [2]); // Ameixa
`` `

Podemos substituir um elemento:

`` `js
frutos [2] = 'Pêra'; // agora ["Apple", "Orange", "Pear"]
`` `

... Ou adicione um novo na matriz:

`` `js
frutas [3] = 'Limão'; // agora ["Maçã", "Laranja", "Pera", "Limão"]
`` `

A contagem total dos elementos na matriz é o `length`:

`` `js run
Deixe frutas = ["Maçã", "Laranja", "Ameixa"];

alerta (fruits.length); // 3
`` `

Nós também podemos usar `alert` para mostrar toda a matriz.

`` `js run
Deixe frutas = ["Maçã", "Laranja", "Ameixa"];

alerta (frutas); // Maçã, Laranja, Ameixa
`` `

Uma matriz pode armazenar elementos de qualquer tipo.

Por exemplo:

`` `js run no-embellecer
// mistura de valores


// obtenha o objeto no índice 1 e depois mostre seu nome
alerta (arr [1] .name); // John

// obtenha a função no índice 3 e execute-a
arr [3] (); // Olá
`` `


`` `` cabeçalho inteligente = "rótulo da vírgula"
Uma matriz, assim como um objeto, pode terminar com uma vírgula:
`` `js
deixe frutas = [
"Maçã",
"Laranja",
"Ameixa"*!*,*/!*
];
`` `

O estilo "vírgula de fuga" facilita a inserção / remoção de itens, pois todas as linhas se tornam iguais.
`` ``


## Métodos pop / push, shift / unshift

A [fila] (https://en.wikipedia.org/wiki/Queue_ (abstract_data_type)) é uma das utilizações mais comuns de uma matriz. Na informática, isso significa uma coleção ordenada de elementos que suporta duas operações:

- `push` adiciona um elemento ao final.
- `shift` ganha um elemento desde o início, avançando a fila, de modo que o 2º elemento se torne o 1º.

!] (queue.png)

Arrays suportam ambas as operações.

Na prática, nos encontramos com muita frequência. Por exemplo, uma fila de mensagens que precisam ser exibidas na tela.

Há outro caso de uso para arrays - a estrutura de dados denominada [stack] (https://en.wikipedia.org/wiki/Stack_ (abstract_data_type)).

Ele suporta duas operações:

- `push` adiciona um elemento ao final.
- `pop` leva um elemento até o fim.

Portanto, novos elementos são adicionados ou tomados sempre a partir do "final".

Uma pilha geralmente é ilustrada como um pacote de cartas: novos cartões são adicionados ao topo ou tirados da parte superior:

!] (stack.png)

Para as pilhas, o item empurrado mais recente é recebido primeiro, também chamado de princípio LIFO (Último-Em-Primeiro-Fora). Para filas, temos FIFO (First-In-First-Out).

Arrays em JavaScript podem funcionar como uma fila e como uma pilha. Eles permitem adicionar / remover elementos tanto para o início como para o final.

Na ciência da computação, a estrutura de dados que a permite é chamada [deque] (https://en.wikipedia.org/wiki/Double-ended_queue).

** Métodos que funcionam com o final da matriz: **

`pop`
: Extrai o último elemento da matriz e o retorna:

`` `js run
Deixe frutas = ["Maçã", "Laranja", "Pêra"];

alerta (fruits.pop ()); // remove "Pear" e alerta-a

alerta (frutas); // Maçã, Laranja
`` `

`push`
: Anexe o elemento ao final da matriz:

`` `js run
Deixe frutas = ["Maçã", "Laranja"];

fruit.push ("Pear");

alerta (frutas); // Maçã, Laranja, Pêra
`` `

A chamada `fruits.push (...)` é igual a `fruits [fruit.length] = ...`.

** Métodos que funcionam com o início da matriz: **

`shift`
: Extrai o primeiro elemento da matriz e o retorna:

`` `js
Deixe frutas = ["Maçã", "Laranja", "Pêra"];

alerta (fruits.shift ()); // remove a Apple e alerta-a

alerta (frutas); // Laranja, Pêra
`` `

`sem mudança '
: Adicione o elemento ao início da matriz:

`` `js
Deixe frutas = ["Laranja", "Pêra"];

fruits.unshift ('Apple');

alerta (frutas); // Maçã, Laranja, Pêra
`` `

Métodos `push` e` unshift` podem adicionar vários elementos ao mesmo tempo:

`` `js run
Deixe frutas = ["Maçã"];

fruit.push ("Orange", "Peach");
fruits.unshift ("Abacaxi", "Limão");

// ["Abacaxi", "Limão", "Maçã", "Laranja", "Pêssego"]
alerta (frutas);
`` `

## Internals

Uma matriz é um tipo especial de objeto. Os colchetes usados ​​para acessar uma propriedade `arr [0]` realmente vêm da sintaxe do objeto. Os números são usados ​​como chaves.

Eles estendem objetos que fornecem métodos especiais para trabalhar com coleções de dados encomendadas e também a propriedade `length`. Mas no núcleo ainda é um objeto.

Lembre-se, existem apenas 7 tipos básicos em JavaScript. Array é um objeto e, portanto, se comporta como um objeto.

Por exemplo, ele é copiado por referência:

`` `js run
Deixe frutas = ["Banana"]

deixe arr = frutas; // copiar por referência (duas variáveis ​​referem a mesma matriz)

alerta (arr === frutas); // verdade

arr.push ("Pear"); // modifica a matriz por referência

alerta (frutas); // Banana, Pear - 2 itens agora
`` `

... Mas o que torna os arrays realmente especiais é a representação interna deles. O mecanismo tenta armazenar seus elementos na área de memória contígua, um após o outro, tal como descrito nas ilustrações neste capítulo, e também existem outras otimizações, para que as matrizes funcionem muito rápido.

Mas todos eles quebram se deixamos de trabalhar com uma matriz como com uma "coleção ordenada" e começamos a trabalhar com ela como se fosse um objeto comum.

Por exemplo, tecnicamente, podemos fazer isso:

`` `js
deixe frutas = []; // faça uma matriz

frutas [99999] = 5; // atribuir uma propriedade com o índice muito maior que o seu comprimento

fruits.age = 25; // cria uma propriedade com um nome arbitrário
`` `

Isso é possível, porque os arrays são objetos na base deles. Podemos adicionar quaisquer propriedades para eles.

Mas o motor verá que estamos trabalhando com a matriz como com um objeto comum. As otimizações específicas de matriz não são adequadas para tais casos e serão desativadas, os benefícios desaparecerão.

As formas de abusar de uma matriz:

- Adicione uma propriedade não numérica como `arr.test = 5`.
- Faça buracos, como: adicione `arr [0]` e depois `arr [1000]` (e nada entre eles).
- Preencha a matriz na ordem inversa, como `arr [1000]`, `arr [999]` e assim por diante.

Por favor, pense em arrays como estruturas especiais para trabalhar com os * dados pedidos *. Eles fornecem métodos especiais para isso. As matrizes são cuidadosamente sintonizadas dentro dos mecanismos de JavaScript para trabalhar com dados ordenados contíguos, use-os desta forma. E se você precisar de chaves arbitrárias, as chances são altas de que você realmente precisa de um objeto regular `{}`.

## Atuação

Os métodos `push / pop` correm rapidamente, enquanto` shift / unshift` são lentos.

! [] (array-speed.png)

Por que é mais rápido trabalhar com o final de uma matriz do que com o seu início? Vamos ver o que acontece durante a execução:

`` `js
fruits.shift (); // pegue 1 elemento desde o início
`` `

Não é suficiente pegar e remover o elemento com o número `0`. Outros elementos precisam ser renumerados também.

A operação `shift` deve fazer 3 coisas:

1. Remova o elemento com o índice `0`.
2. Mova todos os elementos para a esquerda, renumerá-os do índice `1` para` 0`, de `2` para` 1` e assim por diante.
3. Atualize a propriedade `length`.

!] [] (array-shift.png)

** Quanto mais elementos na matriz, mais tempo para movê-los, mais operações na memória. **

A coisa semelhante acontece com `unshift`: para adicionar um elemento ao início da matriz, precisamos primeiro mover elementos existentes para a direita, aumentando seus índices.

E o que é com `push / pop`? Eles não precisam mover nada. Para extrair um elemento do final, o método `pop` limpa o índice e reduz o" comprimento ".

As ações para a operação `pop`:

`` `js
fruits.pop (); // pegue 1 elemento do final
`` `

! [] (array-pop.png)

** O método `pop` não precisa mover nada, porque outros elementos mantêm seus índices. É por isso que é incrivelmente rápido. **

A coisa semelhante ao método `push`.

## Rotações

Uma das maneiras mais antigas para o ciclo de itens de matriz é o 'for` loop over indexes:

`` `js run
deixe arr = ["Apple", "Orange", "Pear"];

*! *
para (deixe i = 0; i <arr.length; i ++) {
* /! *
alerta (arr [i]);
}
`` `

Mas para arrays há outra forma de loop, `for..of`:

`` `js run
Deixe frutas = ["Maçã", "Laranja", "Ameixa"];

// itera sobre elementos de matriz
para (deixe frutas de frutas) {
alerta (fruta);
}
`` `

O `for..of` não dá acesso ao número do elemento atual, apenas seu valor, mas na maioria dos casos é suficiente. E é mais curto.

Tecnicamente, porque arrays são objetos, também é possível usar `for..in`:

`` `js run
deixe arr = ["Apple", "Orange", "Pear"];

*! *
para (deixe a chave em arr) {
* /! *
alerta (arr [chave]); // Maçã, Laranja, Pêra
}
`` `

Mas essa é realmente uma má idéia. Existem problemas potenciais com ele:

1. O loop `for..in 'itera sobre * todas as propriedades *, não apenas as numéricas.

Existem objetos chamados de "array-like" no navegador e em outros ambientes, que * se parecem com arrays *. Ou seja, eles possuem propriedades de "comprimento" e índices, mas também podem ter outras propriedades e métodos não-numéricos, que geralmente não precisamos. O loop `for..in 'irá listá-los embora. Então, se precisamos trabalhar com objetos semelhantes a matriz, então essas propriedades "extras" podem se tornar um problema.

2. O loop `for..in 'é otimizado para objetos genéricos, não arrays, e assim é 10-100 vezes mais lento. Claro, ainda é muito rápido. A aceleração pode ser importante apenas em estrangulamentos ou apenas irrelevante. Mas ainda assim devemos estar cientes da diferença.

Geralmente, não devemos usar `for..in` para arrays.


## Uma palavra sobre "comprimento"

A propriedade `length` atualiza automaticamente quando modificamos a matriz. Para ser preciso, na verdade não é a contagem de valores na matriz, mas o maior índice numérico mais um.

Por exemplo, um único elemento com um índice grande dá um grande comprimento:

`` `js run
deixe frutas = [];
frutas [123] = "Maçã";

alerta (fruits.length); // 124
`` `

Observe que geralmente não usamos matrizes como essa.

Outra coisa interessante sobre a propriedade `length` é que é gravável.

Se a aumentarmos manualmente, nada interessante acontece. Mas se diminuí-lo, a matriz é truncada. O processo é irreversível, aqui está o exemplo:

`` `js run
deixe arr = [1, 2, 3, 4, 5];

arr.length = 2; // truncar para 2 elementos
alerta (arr); // [1, 2]

arr.length = 5; // volta volta de volta
alerta (arr [3]); // indefinido: os valores não retornam
`` `

Então, a maneira mais simples de limpar a matriz é: `arr.length = 0`.


## new Array () [# new-array]

Existe mais uma sintaxe para criar uma matriz:

`` `js
deixe arr = *! * novo Array * /! * ("Apple", "Pear", "etc");
`` `

Raramente é usado, porque os colchetes `[]` são mais curtos. Além disso, há um recurso complicado com ele.

Se `novo Array` é chamado com um único argumento que é um número, então ele cria uma matriz * sem itens, mas com o comprimento * fornecido.

Vamos ver como alguém pode se atirar no pé:

`` `js run
Deixe arr = new Array (2); // criará uma matriz de [2]?

alerta (arr [0]); // Indefinido! sem elementos.

alerta (arr.length); // comprimento 2
`` `

No código acima, `new Array (number)` tem todos os elementos `indefinidos '.

Para evitar tais surpresas, geralmente usamos colchetes, a menos que realmente saibamos o que estamos fazendo.

## Arrays multidimensionais

Arrays podem ter itens que também são arrays. Podemos usá-lo para arrays multidimensionais, para armazenar matrizes:

`` `js run
Deixe matrix = [
[1, 2, 3],
[4, 5, 6],
[7, 8, 9]
];

alerta (matriz [1] [1]); // o elemento central
`` `

## para sequenciar

As matrizes têm sua própria implementação do método 'toString` que retorna uma lista separada por vírgulas de elementos.

Por exemplo:


`` `js run
deixe arr = [1, 2, 3];

alerta (arr); // 1,2,3
alerta (String (arr) === '1,2,3'); // verdade
`` `

Além disso, vamos tentar isso:

`` `js run
alerta ([] + 1); // "1"
alerta ([1] + 1); // "11"
alerta ([1,2] + 1); // "1,21"
`` `

Arrays não têm `Symbol.toPrimitive`, nem um" valueOf "viável, eles implementam apenas a conversão` toString`, então aqui `[]` se torna uma string vazia, `[1]` torna-se `" 1 "` e `[ 1,2] `torna-se` "1,2" `.

Quando o operador binário plus `" + "` adiciona algo a uma string, ele também o converte em uma string, então o próximo passo é assim:

`` `js run
alerta ("" + 1); // "1"
alerta ("1" + 1); // "11"
alerta ("1,2" + 1); // "1,21"
`` `

## Resumo

Array é um tipo especial de objetos, adequado para armazenar e gerenciar itens de dados ordenados.

- A declaração:

`` `js
// colchetes (usual)
Deixe arr = [item1, item2 ...];

// nova matriz (excepcionalmente rara)
let arr = new Array (item1, item2 ...);
`` `

A chamada para `new Array (number)` cria uma matriz com o comprimento dado, mas sem elementos.

- A propriedade `length` é o comprimento da matriz ou, para ser preciso, seu último índice numérico mais um. É ajustado automaticamente por métodos de matriz.
- Se encurtarmos `length` manualmente, a matriz é truncada.

Podemos usar uma matriz como um deque com as seguintes operações:

- `push (... items)` adiciona `items` ao final.
- `pop ()` remove o elemento do final e o retorna.
- `shift ()` remove o elemento desde o início e o retorna.
- `unshift (... items)` adiciona itens ao início.

Para fazer um loop sobre os elementos da matriz:
- `for (let i = 0; i <arr.length; i ++)` - funciona mais rápido, compatível com o navegador antigo.
- `para (deixe item de arr)` - a sintaxe moderna apenas para itens,
- `para (deixei em arr)` - nunca use.

Voltaremos a arrays e estudaremos mais métodos para adicionar, remover, extrair elementos e classificar arrays no capítulo <info: array-methods>.

