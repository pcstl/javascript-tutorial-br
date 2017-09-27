# Recursão e pilha

Vamos voltar às funções e estudá-las mais profundamente.

Nosso primeiro tópico será * recursão *.

Se você não é novo na programação, provavelmente é familiar e você pode ignorar este capítulo.

A recursão é um padrão de programação que é útil em situações em que uma tarefa pode ser naturalmente dividida em várias tarefas do mesmo tipo, mas é mais simples. Ou quando uma tarefa pode ser simplificada em uma ação fácil mais uma variante mais simples da mesma tarefa. Ou, como veremos em breve, para lidar com certas estruturas de dados.

Quando uma função resolve uma tarefa, no processo pode chamar muitas outras funções. Um caso parcial disso é quando uma função chama * * * *. Isso é chamado de * recursão *.

[cortar]

## Duas maneiras de pensar

Para algo simples de começar - vamos escrever uma função `pow (x, n)` que eleva `x` para uma potência natural de` n`. Em outras palavras, multiplica `x` por si mesmo n vezes.

`` `js
Pow (2, 2) = 4
Pow (2, 3) = 8
Pow (2, 4) = 16
`` `

Existem duas maneiras de implementá-lo.

1. Pensamento iterativo: o loop for for:

`` `js run
função pow (x, n) {
Deixe o resultado = 1;

// multiplique o resultado por x n vezes no loop
para (vamos i = 0; i <n; i ++) {
resultado * = x;
}

resultado de retorno;
}

alerta (Pow (2, 3)); // 8
`` `

2. Pensamento recursivo: simplifique a tarefa e ligue para si mesmo:

`` `js run
função pow (x, n) {
se (n == 1) {
retorno x;
} outro {
retorno x * pow (x, n - 1);
}
}

alerta (Pow (2, 3)); // 8
`` `

Observe como a variante recursiva é fundamentalmente diferente.

Quando `pow (x, n)` é chamado, a execução divide-se em dois ramos:

`` `js
se n == 1 = x
/
Pow (x, n) =
\
else = x * pow (x, n - 1)
`` `

1. Se `n == 1`, então tudo é trivial. É chamado * a base * de recursão, porque produz imediatamente o resultado óbvio: `pow (x, 1)` é igual a `x`.
2. Caso contrário, podemos representar `pow (x, n)` como `x * pow (x, n-1)`. Em matemática, escreveria <code> x <sup> n </ sup> = x * x <sup> n-1 </ sup> </ code>. Isso é chamado * um passo recursivo *: transformamos a tarefa em uma ação mais simples (multiplicação por `x`) e uma chamada mais simples da mesma tarefa (` pow` com 'n' inferior). Próximas etapas simplificam ainda mais até que `n 'chegue a` 1`.

Podemos também dizer que `pow` * recursivamente se chama * até` n == 1`.

! [diagrama recursivo de Pow] (recursion-pow.png)


Por exemplo, para calcular `pow (2, 4)` a variante recursiva, estas etapas:

1. Pow (2, 4) = 2 * Pow (2, 3) `
2. Pow (2, 3) = 2 * Pow (2, 2) `
3. Pow (2, 2) = 2 * Pow (2, 1) `
4. `pow (2, 1) = 2`

Assim, a recursão reduz uma chamada de função para uma mais simples e, em seguida, até mesmo mais simples, e assim por diante, até o resultado se tornar óbvio.

`` `` cabeçalho inteligente = "Recursão geralmente é menor"
Uma solução recursiva geralmente é mais curta do que uma iterativa.

Aqui podemos reescrever o mesmo usando o operador `` `ternário em vez de` if` para fazer `pow (x, n)` mais conciso e ainda muito legível:

`` `js run
função pow (x, n) {
retorno (n == 1)? x: (x * pow (x, n-1));
}
`` `
`` ``

O número máximo de chamadas aninhadas (incluindo o primeiro) é chamado de * profundidade de recursão *. No nosso caso, será exatamente `n`.

A profundidade máxima de recursão é limitada pelo mecanismo JavaScript. Podemos garantir que cerca de 10000, alguns motores permitem mais, mas 100000 provavelmente está fora do limite para a maioria deles. Existem otimizações automáticas que ajudam a aliviar isso ("otimizações de chamadas de cauda"), mas ainda não são suportadas em todos os lugares e funcionam apenas em casos simples.

Isso limita a aplicação da recursão, mas ainda permanece muito ampla. Existem muitas tarefas em que o modo de pensamento recursivo dá um código mais simples, mais fácil de manter.

## A pilha de execução

Agora, vamos examinar como as chamadas recursivas funcionam. Para isso, veremos sob o capô das funções.

A informação sobre uma execução de função é armazenada em seu * contexto de execução *.

O [contexto de execução] (https://tc39.github.io/ecma262/#sec-execution-contexts) é uma estrutura de dados interna que contém detalhes sobre a execução de uma função: onde o fluxo de controle é agora, as variáveis ​​atuais , o valor de `isto '(não o usamos aqui) e alguns outros detalhes internos.

Uma chamada de função tem exatamente um contexto de execução associado a ele.

Quando uma função faz uma chamada aninhada, acontece o seguinte:

- A função atual está em pausa.
- O contexto de execução associado a ele é lembrado em uma estrutura de dados especial chamada * pilha de contexto de execução *.
- A chamada aninhada é executada.
- Depois que ele termina, o contexto de execução antigo é recuperado da pilha e a função externa é retomada de onde ele parou.

Vamos ver o que acontece durante a chamada `pow (2, 3)`.

### pow (2, 3)

No início da chamada `pow (2, 3)` o contexto de execução irá armazenar variáveis: `x = 2, n = 3`, o fluxo de execução está na linha` 1` da função.

Podemos esboçá-lo como:

<ul class = "função-execução-contexto-lista">
<li>
<span class = "function-execution-context"> Contexto: {x: 2, n: 3, na linha 1} </ span>
<span class = "function-execution-context-call"> pow (2, 3) </ span>
</ li>
</ Ul>

É quando a função começa a ser executada. A condição `n == 1` é falsa, então o fluxo continua no segundo ramo de` if`:

`` `js run
função pow (x, n) {
se (n == 1) {
retorno x;
} outro {
*! *
retorno x * pow (x, n - 1);
* /! *
}
}

alerta (Pow (2, 3));
`` `


As variáveis ​​são iguais, mas a linha muda, então o contexto agora é:

<ul class = "função-execução-contexto-lista">
<li>
<span class = "function-execution-context"> Contexto: {x: 2, n: 3, na linha 5} </ span>
<span class = "function-execution-context-call"> pow (2, 3) </ span>
</ li>
</ Ul>

Para calcular `x * pow (x, n-1)`, precisamos fazer um subcall de `pow` com novos argumentos` pow (2, 2) `.

### pow (2, 2)

Para fazer uma chamada aninhada, o JavaScript lembra o contexto de execução atual na * pilha de contexto de execução *.

Aqui chamamos a mesma função `pow`, mas isso absolutamente não importa. O processo é o mesmo para todas as funções:

1. O contexto atual é "lembrado" em cima da pilha.
2. O novo contexto é criado para o subcall.
3. Quando a subcall for concluída - o contexto anterior é exibido da pilha e sua execução continua.

Aqui está a pilha de contexto quando inserimos o subcall `pow (2, 2)`:

<ul class = "função-execução-contexto-lista">
<li>
<span class = "function-execution-context"> Contexto: {x: 2, n: 2, na linha 1} </ span>
<span class = "function-execution-context-call"> pow (2, 2) </ span>
</ li>
<li>
<span class = "function-execution-context"> Contexto: {x: 2, n: 3, na linha 5} </ span>
<span class = "function-execution-context-call"> pow (2, 3) </ span>
</ li>
</ Ul>

O novo contexto de execução atual está no topo (e em negrito), e os contextos lembrados anteriores estão abaixo.

Quando terminamos o subcall - é fácil retomar o contexto anterior, porque ele mantém as duas variáveis ​​e o local exato do código onde ele parou. Aqui na imagem usamos a palavra "linha", mas é claro que é mais preciso.

### pow (2, 1)

O processo se repete: uma nova subcall é feita na linha `5`, agora com argumentos` x = 2`, `n = 1`.

Um novo contexto de execução é criado, o anterior é empurrado para cima da pilha:

<ul class = "função-execução-contexto-lista">
<li>
<span class = "function-execution-context"> Contexto: {x: 2, n: 1, na linha 1} </ span>
<span class = "function-execution-context-call"> pow (2, 1) </ span>
</ li>
<li>
<span class = "function-execution-context"> Contexto: {x: 2, n: 2, na linha 5} </ span>
<span class = "function-execution-context-call"> pow (2, 2) </ span>
</ li>
<li>
<span class = "function-execution-context"> Contexto: {x: 2, n: 3, na linha 5} </ span>
<span class = "function-execution-context-call"> pow (2, 3) </ span>
</ li>
</ Ul>

Existem dois contextos antigos agora e 1 atualmente em execução para `pow (2, 1)`.

### A saída

Durante a execução de `pow (2, 1)`, ao contrário de antes, a condição `n == 1` é verdade, então o primeiro ramo de` if` funciona:

`` `js
função pow (x, n) {
se (n == 1) {
*! *
retorno x;
* /! *
} outro {
retorno x * pow (x, n - 1);
}
}
`` `

Não há mais chamadas aninhadas, então a função acaba, retornando `2 '.

À medida que a função termina, seu contexto de execução não é mais necessário, portanto é removido da memória. O anterior foi restaurado no topo da pilha:


<ul class = "função-execução-contexto-lista">
<li>
<span class = "function-execution-context"> Contexto: {x: 2, n: 2, na linha 5} </ span>
<span class = "function-execution-context-call"> pow (2, 2) </ span>
</ li>
<li>
<span class = "function-execution-context"> Contexto: {x: 2, n: 3, na linha 5} </ span>
<span class = "function-execution-context-call"> pow (2, 3) </ span>
</ li>
</ Ul>

A execução do `pow (2, 2)` é retomada. Ele tem o resultado do subcall `pow (2, 1)`, então também pode terminar a avaliação de `x * pow (x, n-1)`, retornando `4`.

Em seguida, o contexto anterior é restaurado:

<ul class = "função-execução-contexto-lista">
<li>
<span class = "function-execution-context"> Contexto: {x: 2, n: 3, na linha 5} </ span>
<span class = "function-execution-context-call"> pow (2, 3) </ span>
</ li>
</ Ul>

Quando termina, temos um resultado de `pow (2, 3) = 8`.

A profundidade de recursão neste caso foi: ** 3 **.

Como podemos ver nas ilustrações acima, a profundidade de recursão é igual ao número máximo de contexto na pilha.

Observe os requisitos de memória. Os contextos tomam memória. No nosso caso, aumentar o poder de `n` realmente requer a memória para contextos 'n`, para todos os valores inferiores de` n`.

Um algoritmo baseado em loop é mais economizador de memória:

`` `js
função pow (x, n) {
Deixe o resultado = 1;

para (vamos i = 0; i <n; i ++) {
resultado * = x;
}

resultado de retorno;
}
`` `

O `pow 'iterativo usa um único contexto que altera` i` e `result` no processo. Os requisitos de memória são pequenos, fixos e não dependem de `n`.

** Qualquer recursão pode ser reescrita como um loop. A variante do loop geralmente pode ser mais eficaz. **

... Mas às vezes a reescrita não é trivial, especialmente quando a função usa diferentes subcalls recursivos dependendo das condições e mescla seus resultados ou quando a ramificação é mais intrincada. E a otimização pode ser desnecessária e não vale a pena os esforços.

A recursão pode dar um código mais curto, mais fácil de entender e suportar. As otimizações não são necessárias em todos os lugares, principalmente, precisamos de um bom código, é por isso que ele é usado.

## Recursive traversals

Outra excelente aplicação da recursão é uma passagem recursiva.

Imagine, temos uma empresa. A estrutura da equipe pode ser apresentada como um objeto:

`` `js
deixe company = {
vendas: [{
Nome: 'John',
salário: 1000
}, {
Nome: 'Alice'
salário: 600
}]

desenvolvimento: {
sites: [{
Nome: 'Peter',
salário: 2000
}, {
Nome: 'Alex',
salário: 1800
}]

internos [[
Nome: 'Jack',
salário: 1300
}]
}
};
`` `

Em outras palavras, uma empresa possui departamentos.

- Um departamento pode ter uma série de funcionários. Por exemplo, o departamento de "vendas" tem 2 funcionários: John e Alice.
- Ou um departamento pode dividir em sub-departamentos, como o `desenvolvimento 'tem duas filiais:' sites 'e' internos '. Cada um deles tem o próprio pessoal.
- Também é possível que, quando um sub-departamento cresça, ele se divide em subsubdependentes (ou equipes).

Por exemplo, o departamento de "sites" no futuro pode ser dividido em equipes para `siteA` e` siteB`. E eles, potencialmente, podem dividir ainda mais. Isso não está na foto, apenas algo a ter em mente.

Agora, digamos que queremos uma função para obter a soma de todos os salários. Como podemos fazer isso?

Uma abordagem iterativa não é fácil, porque a estrutura não é simples. A primeira idéia pode ser fazer um "for" loop over `company` com sublote antigos em departamentos de primeiro nível. Mas, então, precisamos de mais sublocações aninhadas para iterar sobre a equipe em departamentos de 2º nível, como "sites". ... E então outro sublote dentro dos departamentos de 3º nível que possam aparecer no futuro? Devemos parar no nível 3 ou fazer 4 níveis de loops? Se colocarmos 3-4 subloteis aninhados no código para percorrer um único objeto, torna-se bastante feio.

Vamos tentar recursão.

Como podemos ver, quando nossa função obtém um departamento, os dois casos possíveis são possíveis:

1. Ou é um departamento "simples" com uma * matriz de pessoas * - então podemos somar os salários em um loop simples.
2. Ou é * um objeto com sub-departamentos 'N' * - então podemos fazer chamadas recursivas `N 'para obter a soma para cada subdeps e combinar os resultados.

O (1) é a base da recursão, o caso trivial.

O (2) é o passo recursivo. Uma tarefa complexa é dividida em subtarefas para departamentos menores. Eles podem, por sua vez, dividir novamente, mas, mais cedo ou mais tarde, a divisão terminará em (1).

O algoritmo provavelmente é mais fácil de ler do código:


`` `js run
Deixe company = {// o mesmo objeto, compactado por brevidade
vendas: [{nome: 'John', salário: 1000}, {nome: 'Alice', salário: 600}],
desenvolvimento: {
sites: [{nome: 'Peter', salário: 2000}, {nome: 'Alex', salário: 1800}],
internos: [{nome: 'Jack', salário: 1300}]
}
};

// A função para fazer o trabalho
*! *
função sumSalaries (departamento) {
se (Array.isArray (departamento)) {// caso (1)
return department.reduce ((prev, current) => prev + current.salary, 0); // somar a matriz
} else {// case (2)
deixe soma = 0;
para (subdep de Object.values ​​(departamento)) {
soma + = sumSalaries (subdep); // solicita recursivamente para subdepartamentos, somar os resultados
}
Soma de retorno;
}
}
* /! *

alerta (sumSalaries (empresa)); // 6700
`` `

O código é curto e fácil de entender (espero que?). Esse é o poder da recursão. Também funciona para qualquer nível de nidificação de subpartes.

Aqui está o diagrama das chamadas:

! [salários recursivos] (recursive-salaries.png)

Podemos ver facilmente o princípio: para um objeto `{...}` subcalls são feitos, enquanto arrays `[...]` são as "folhas" da árvore de recursão, eles dão resultado imediato.

Observe que o código usa recursos inteligentes que já abordamos antes:

- Método `arr.reduce` explicado no capítulo <info: array-methods> para obter a soma da matriz.
- Loop `for (val of Object.values ​​(obj))` para iterar sobre valores de objeto: `Object.values` retorna uma matriz deles.


## Estruturas recursivas

Uma estrutura de dados recursiva (definida recursivamente) é uma estrutura que se replica em partes.

Acabamos de ver isso no exemplo de uma estrutura da empresa acima.

Uma empresa * departamento * é:
- Ou uma série de pessoas.
- Ou um objeto com * departamentos *.

Para desenvolvedores web, há exemplos muito mais conhecidos: documentos HTML e XML.

No documento HTML, um * HTML-tag * pode conter uma lista de:
- Peças de texto.
- HTML-comments.
- Outras * tags HTML * (que, por sua vez, podem conter peças de texto / comentários ou outras etiquetas, etc.).

Essa é mais uma vez uma definição recursiva.

Para uma melhor compreensão, abordaremos uma estrutura mais recursiva denominada "Lista vinculada" que pode ser uma alternativa melhor para arrays em alguns casos.

### Lista vinculada

Imagine, queremos armazenar uma lista ordenada de objetos.

A escolha natural seria uma matriz:

`` `js
deixe arr = [obj1, obj2, obj3];
`` `

... Mas há um problema com arrays. As operações "excluir elementos" e "inserir elementos" são caras. Por exemplo, a operação `arr.unshift (obj)` deve renumar todos os elementos para abrir espaço para um novo 'obj`, e se a matriz for grande, leva tempo. Mesmo com `arr.shift ()`.

As únicas modificações estruturais que não exigem a renumeração em massa são aquelas que operam com o fim da matriz: `arr.push / pop`. Portanto, uma matriz pode ser bastante lenta para grandes filas.

Alternativamente, se realmente precisamos de rápida inserção / exclusão, podemos escolher outra estrutura de dados chamada [lista vinculada] (https://en.wikipedia.org/wiki/Linked_list).

O * elemento da lista vinculada * é recursivamente definido como um objeto com:
- `valor '.
- propriedade `next` referenciando o próximo * elemento da lista vinculada * ou` null` se esse for o fim.

Por exemplo:

`` `js
deixe list = {
valor: 1,
Próximo: {
valor: 2,
Próximo: {
valor: 3,
Próximo: {
valor: 4,
seguinte: nulo
}
}
}
};
`` `

Representação gráfica da lista:

! [lista vinculada] (linked-list.png)

Um código alternativo para criação:

`` `js no-embellecer
Deixe list = {value: 1};
list.next = {value: 2};
list.next.next = {value: 3};
list.next.next.next = {value: 4};
`` `

Aqui podemos ver ainda mais claro que existem vários objetos, cada um tem o `valor` e` próximo` apontando para o vizinho. A variável `list` é o primeiro objeto da cadeia, então, seguindo os ponteiros" próximos ", podemos alcançar qualquer elemento.

A lista pode ser facilmente dividida em várias partes e mais tarde juntou-se:

`` `js
Deixe o segundoList = list.next.next;
list.next.next = null;
`` `

! [lista vinculada]] (linked-list-split.png)

Juntar-se:

`` `js
list.next.next = secondList;
`` `

E certamente podemos inserir ou remover itens em qualquer lugar.

Por exemplo, para antecipar um novo valor, precisamos atualizar o cabeçalho da lista:

`` `js
Deixe list = {value: 1};
list.next = {value: 2};
list.next.next = {value: 3};
list.next.next.next = {value: 4};

*! *
// prepend o novo valor da lista
lista = {valor: "novo item", próximo: lista};
* /! *
`` `

! [lista ligada] (link-list-0.png)

Para remover um valor do meio, mude `próximo` do anterior:

`` `js
list.next = list.next.next;
`` `

! [lista vinculada] (link-list-remove-1.png)

Criamos `list.next` jump over` 1` para valorizar `2`. O valor `1` está agora excluído da cadeia. Se não for armazenado em nenhum outro lugar, ele será automaticamente removido da memória.

Ao contrário dos arrays, não há renumeração em massa, podemos reorganizar facilmente os elementos.

Naturalmente, as listas nem sempre são melhores do que os arrays. Caso contrário, todos usariam apenas listas.

A principal desvantagem é que não podemos acessar facilmente um elemento por seu número. Em uma matriz que é fácil: `arr [n]` é uma referência direta. Mas na lista, precisamos começar do primeiro item e ir `próximo` `N` vezes para obter o N-ésimo elemento.

... Mas nem sempre precisamos dessas operações. Por exemplo, quando precisamos de uma fila ou mesmo de [deque] (https://en.wikipedia.org/wiki/Double-ended_queue) - a estrutura ordenada que deve permitir elementos de adição / remoção muito rápidos de ambas as extremidades.

Às vezes, vale a pena adicionar outra variável chamada `tail` para rastrear o último elemento da lista (e atualizá-la ao adicionar / remover elementos do final). Para grandes conjuntos de elementos, a diferença de velocidade versus arrays é enorme.

## Resumo

Termos:
- * Recursão * é um termo de programação que significa uma função de "auto-chamada". Tais funções podem ser usadas para resolver determinadas tarefas de maneira elegante.

Quando uma função se chama, isso é chamado de * passo de recursão *. A * base * de recursão é argumentos de função que tornam a tarefa tão simples que a função não faz mais chamadas.

- Uma estrutura de dados [recursivamente definida] (https://en.wikipedia.org/wiki/Recursive_data_type) é uma estrutura de dados que pode ser definida usando-se.

Por exemplo, a lista vinculada pode ser definida como uma estrutura de dados consistindo em um objeto que faz referência a uma lista (ou nula).

`` `js
lista = {valor, próximo -> lista}
`` `

Árvores como a árvore de elementos HTML ou a árvore de departamento deste capítulo também são naturalmente recursivas: elas se ramificam e cada ramo pode ter outros ramos.

As funções recursivas podem ser usadas para caminhar como já vimos no exemplo `sumSalary`.

Qualquer função recursiva pode ser reescrita em uma iterativa. E às vezes é necessário otimizar as coisas. Mas, para muitas tarefas, uma solução recursiva é rápida e fácil de escrever e suportar.
