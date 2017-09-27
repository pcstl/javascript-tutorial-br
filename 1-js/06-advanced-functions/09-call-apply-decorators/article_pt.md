# Decoradores e encaminhamento, ligue / aplique

O JavaScript oferece flexibilidade excepcional ao lidar com funções. Eles podem ser transmitidos, usados ​​como objetos, e agora veremos como * encaminhar * chamadas entre eles e * decorá-los.

[cortar]

## Armazenamento em cache transparente

Digamos que temos uma função `slow (x)` que é pesada em CPU, mas seus resultados são estáveis. Em outras palavras, para o mesmo `x` sempre retorna o mesmo resultado.

Se a função é chamada com freqüência, podemos querer armazenar em cache (lembrar) os resultados para diferentes `x` para evitar gastar tempo extra nos recálculos.

Mas em vez de adicionar essa funcionalidade a `slow ()`, criaremos um wrapper. Como veremos, há muitos benefícios de fazê-lo.

Aqui está o código, e as explicações seguem:

`` `js run
função lenta (x) {
// pode haver um trabalho pesado intensivo em CPU aqui
alerta (`Chamado com $ {x}`);
retorno x;
}

função cachingDecorator (func) {
Deixe o cache = novo Mapa ();

função de retorno (x) {
se (cache.has (x)) {// se o resultado estiver no mapa
retornar cache.get (x); // devolver
}

Deixe resultado = func (x); // chamar de outra forma

cache.set (x, resultado); // e cache (lembrar) o resultado
resultado de retorno;
};
}

lento = cachingDecorator (lento);

alerta (lento (1)); // lento (1) está em cache
alerta ("Novamente:" + lento (1)); // o mesmo

alerta (lento (2)); // lento (2) está em cache
alerta ("Novamente:" + lento (2)); // o mesmo que a linha anterior
`` `

No código acima, "cachingDecorator" é um * decorador *: uma função especial que leva outra função e altera seu comportamento.

A idéia é que podemos chamar `cachingDecorator` para qualquer função e retornará o wrapper de cache. Isso é ótimo, porque podemos ter muitas funções que podem usar esse recurso, e tudo o que precisamos fazer é aplicar `cachingDecorator` para eles.

Ao separar o armazenamento em cache do código de função principal, também mantemos o código principal mais simples.

Agora vamos entrar em detalhes sobre como ele funciona.

O resultado do `cachingDecorator (func)` é um "wrapper": `function (x)` que "envolve" a chamada de `func (x)` na lógica de cache:

!] [] (decorator-makecaching-wrapper.png)

Como podemos ver, o wrapper retorna o resultado de `func (x)` "como está". De um código externo, a função "lenta" envolvida ainda faz o mesmo. Acabou de ter um aspecto de cache adicionado ao seu comportamento.

Para resumir, existem vários benefícios de usar um `cachingDecorator 'separado em vez de alterar o código do próprio` lento':

- O `cachingDecorator` é reutilizável. Podemos aplicá-lo a outra função.
- A lógica de cache é separada, não aumentou a complexidade do "lento" (se houvesse algum).
- Podemos combinar vários decoradores se necessário (outros decoradores seguirão).


## Usando "func.call" para o contexto

O decorador de cache mencionado acima não é adequado para trabalhar com métodos de objeto.

Por exemplo, no código abaixo `user.format ()` pára de funcionar após a decoração:

`` `js run
// vamos fazer o cache do worker.slow
deixa worker = {
() {
retornar 1;
},

lento (x) {
// na verdade, pode haver uma tarefa pesada de CPU pesada aqui
alerta ("Chamado com" + x);
retornar x * this.someMethod (); // (*)
}
};

// mesmo código que antes
função cachingDecorator (func) {
Deixe o cache = novo Mapa ();
função de retorno (x) {
se (cache.has (x)) {
retornar cache.get (x);
}
*! *
Deixe resultado = func (x); // (**)
* /! *
cache.set (x, resultado);
resultado de retorno;
};
}

alerta (worker.slow (1)); // o método original funciona

worker.slow = cachingDecorator (worker.slow); // agora torná-lo em cache

*! *
alerta (worker.slow (2)); // Whoops! Erro: Não é possível ler a propriedade 'someMethod' de indefinido
* /! *
`` `

O erro ocorre na linha `(*)` que tenta acessar `this.someMethod` e falha. Você pode ver por quê?

A razão é que o wrapper chama a função original como `func (x)` na linha `(**)`. E, quando chamado assim, a função recebe `this = undefined`.

Observaríamos um sintoma semelhante se tentássemos correr:

`` `js
deixe func = worker.slow;
func (2);
`` `

Assim, o invólucro passa a chamada para o método original, mas sem o contexto `this`. Daí o erro.

Vamos consertar isso.

Existe um método de função incorporado especial [func.call (context, ... args)] (mdn: js / Function / call) que permite chamar uma função explicitamente definindo `this`.

A sintaxe é:

`` `js
func.call (context, arg1, arg2, ...)
`` `

Ele é executado `func` fornecendo o primeiro argumento como` this`, e o próximo como os argumentos.

Para simplificar, essas duas chamadas fazem quase o mesmo:
`` `js
func (1, 2, 3);
func.call (obj, 1, 2, 3)
`` `

Ambos chamam de `func` com argumentos` 1`, `2` e` 3`. A única diferença é que `func.call` também define` this` para `obj`.

Como exemplo, no código abaixo chamamos `sayHi` no contexto de diferentes objetos:` sayHi.call (user) `executa` sayHi` fornecendo `this = user`, e a próxima linha define` this = admin`:

`` `js run
função sayHi () {
alerta (this.name);
}

deixe o usuário = {nome: "João"};
Deixe admin = {name: "Admin"};

// use chamada para passar objetos diferentes como "este"
sayHi.call (usuário); // John
sayHi.call (admin); // Admin
`` `

E aqui usamos `call` para chamar` say` com o contexto e frase dada:


`` `js run
função dizer (frase) {
alerta (this.name + ':' + frase);
}

deixe o usuário = {nome: "João"};

// o usuário se torna isso, e "Hello" se torna o primeiro argumento
say.call (usuário, "Olá"); // John: Olá
`` `


No nosso caso, podemos usar `call` no wrapper para passar o contexto para a função original:


`` `js run
deixa worker = {
() {
retornar 1;
},

lento (x) {
alerta ("Chamado com" + x);
retornar x * this.someMethod (); // (*)
}
};

função cachingDecorator (func) {
Deixe o cache = novo Mapa ();
função de retorno (x) {
se (cache.has (x)) {
retornar cache.get (x);
}
*! *
Deixe resultado = func.call (isto, x); // "this" foi passado corretamente agora
* /! *
cache.set (x, resultado);
resultado de retorno;
};
}

worker.slow = cachingDecorator (worker.slow); // agora torná-lo em cache

alerta (worker.slow (2)); // trabalho
alerta (worker.slow (2)); // funciona, não chama o original (em cache)
`` `

Agora tudo está bem.

Para deixar tudo claro, vejamos mais profundamente como "isso" é passado:

1. Depois da decoração `worker.slow` é agora a função wrapper` (x) {...} `.
2. Então, quando `worker.slow (2)` for executado, o wrapper recebe `2` como argumento e` this = worker` (é o objeto antes do ponto).
3. Dentro do invólucro, assumindo que o resultado ainda não está em cache, `func.call (this, x)` passa o `` atual '(`= worker`) e o argumento atual (` = 2) para o método original .

## Indo multi-argumento com "func.apply"

Agora vamos tornar o "cachingDecorator" ainda mais universal. Até agora, funcionava apenas com funções de argumento único.

Agora, como armazenar em cache o método `worker.slow` multi-argumento?

`` `js
deixa worker = {
lento (min, max) {
retornar min + max; // assustador CPU-hogger é assumido
}
};

// deve lembrar chamadas do mesmo argumento
worker.slow = cachingDecorator (worker.slow);
`` `

Temos duas tarefas para resolver aqui.

Primeiro é como usar ambos os argumentos `min` e` max` para a chave no mapa `cache`. Anteriormente, para um único argumento `x`, poderíamos apenas 'cache.set (x, result)` para salvar o resultado e `cache.get (x)` para recuperá-lo. Mas agora precisamos lembrar o resultado para uma * combinação de argumentos * `(min, max)`. O "Mapa" nativo toma um valor único apenas como a chave.

Existem muitas soluções possíveis:

1. Implementar uma nova estrutura de dados semelhante a um mapa (ou usar um terceiro) que seja mais versátil e permita múltiplas chaves.
2. Use mapas aninhados: `cache.set (min)` será um `Map` que armazena o par` (max, result) `. Então, podemos obter `result` como` cache.get (min) .get (max) `.
3. Junte dois valores em um. Em nosso caso particular, podemos usar apenas uma string `" min, max "` como a tecla `Map`. Para a flexibilidade, podemos permitir fornecer uma * função de hashing * para o decorador, que sabe como fazer um valor de muitos.


Para muitas aplicações práticas, a 3ª variante é boa o suficiente, então ficamos com ela.

A segunda tarefa a resolver é como passar muitos argumentos para `func`. Atualmente, o wrapper `function (x)` assume um único argumento, e `func.call (this, x)` o passa.

Aqui podemos usar outro método incorporado [func.apply] (mdn: js / Function / apply).

A sintaxe é:

`` `js
func.apply (context, args)
`` `

Ele executa a configuração `func` 'this = context` e usando um array-like object` args` como a lista de argumentos.


Por exemplo, essas duas chamadas são quase iguais:

`` `js
func (1, 2, 3);
func.apply (contexto, [1, 2, 3])
`` `

Ambos funcionam `func` dando argumentos` 1,2,3`. Mas `apply` também define` this = context`.

Por exemplo, aqui `say` é chamado com` this = user` e `messageData` como uma lista de argumentos:

`` `js run
função dizer (tempo, frase) {
alerta (`[$ {time}] $ {this.name}: $ {frase}`);
}

deixe o usuário = {nome: "João"};

deixe messageData = ['10: 00 ',' Olá ']; // tornar-se tempo e frase

*! *
// o usuário se torna isso, messageData é passado como uma lista de argumentos (tempo, frase)
say.apply (user, messageData); // [10:00] John: Olá (isto = usuário)
* /! *
`` `

A única diferença de sintaxe entre `call` e` apply` é que `call` espera uma lista de argumentos, enquanto que` apply` leva um objeto semelhante a uma matriz com eles.

Já conhecemos o operador de propagação `...` do capítulo <info: rest-parameters-spread-operator> que pode passar uma matriz (ou qualquer iterável) como uma lista de argumentos. Então, se usarmos isso com `call`, podemos alcançar quase o mesmo que` apply`.

Essas duas chamadas são quase equivalentes:

`` `js
Deixe args = [1, 2, 3];

*! *
func.call (contexto, ... args); // passa uma matriz como lista com operador de propagação
func.apply (context, args); // é o mesmo que usar aplicar
* /! *
`` `

Se olharmos mais de perto, há uma pequena diferença entre esses usos de `call` e` apply`.

- O operador de propagação `...` permite passar * iterable * `args` como a lista para" ligar ".
- O `apply` aceita apenas * array-like *` args`.

Então, essas chamadas se complementam. Onde esperamos um iterable, `call` funciona, onde esperamos um array-like,` apply` works.

E se `args` é tanto iterable quanto array-like, como uma matriz real, então, tecnicamente, podemos usar qualquer um deles, mas o` apply` provavelmente será mais rápido, porque é uma única operação. A maioria dos motores JavaScript optimizados internamente é melhor que um par `call + spread`.

Um dos usos mais importantes do `apply` está passando a chamada para outra função, como esta:

`` `js
Deixe wrapper = function () {
retornar outra função. Aplicar (isto, argumentos);
};
`` `

Isso é chamado * encaminhamento de chamadas *. O `wrapper` passa tudo o que é: o contexto` this` e os argumentos para 'anotherFunction` e retorna seu resultado.

Quando um código externo chama esse "wrapper", é indistinguível da chamada da função original.

Agora vamos assar tudo no mais poderoso `cachingDecorator`:

`` `js run
deixa worker = {
lento (min, max) {
alerta (`Chamado com $ {min}, $ {max}`);
retornar min + max;
}
};

função cachingDecorator (func, hash) {
Deixe o cache = novo Mapa ();
função de retorno () {
*! *
Deixe key = hash (argumentos); // (*)
* /! *
se (cache.has (chave)) {
retornar cache.get (chave);
}

*! *
Deixe resultado = func.apply (isto, argumentos); // (**)
* /! *

cache.set (chave, resultado);
resultado de retorno;
};
}

hash de função (args) {
retornar args [0] + ',' + args [1];
}

worker.slow = cachingDecorator (worker.slow, hash);

alerta (worker.slow (3, 5)); // trabalho
alerta ("Novamente" + worker.slow (3, 5)); // mesmo (em cache)
`` `

Agora, o wrapper opera com qualquer número de argumentos.

Há duas mudanças:

- Na linha `(*)` ele chama `hash` para criar uma única chave de` argumentos '. Aqui, usamos uma função simples de "união" que transforma os argumentos `(3, 5)` na chave `" 3,5 "`. Casos mais complexos podem exigir outras funções de hashing.
- Então `(**)` usa `func.apply` para passar o contexto e todos os argumentos que o wrapper obteve (independentemente de quantos) para a função original.


# Empréstimo de um método [# método-empréstimo]

Agora vamos fazer mais uma melhoria menor na função hashing:

`` `js
hash de função (args) {
retornar args [0] + ',' + args [1];
}
`` `

A partir de agora, funciona apenas em dois argumentos. Seria melhor se pudesse colar qualquer número de "args".

A solução natural seria usar o método [arr.join] (mdn: js / Array / join):

`` `js
hash de função (args) {
retornar args.join ();
}
`` `

... Infelizmente, isso não vai funcionar. Como estamos chamando de `hash (argumentos)` e `argumentos 'o objeto é tanto iterável como array-like, mas não uma matriz real.

Então, chamar `join` nela falharia, como podemos ver abaixo:

`` `js run
função hash () {
*! *
alerta (arguments.join ()); // Erro: arguments.join não é uma função
* /! *
}

hash (1, 2);
`` `

Ainda assim, há uma maneira fácil de usar a junção de matrizes:

`` `js run
função hash () {
*! *
alerta ([] .join.call (argumentos)); // 1,2
* /! *
}

hash (1, 2);
`` `

O truque é chamado * método de empréstimo *.

Tomamos (emprestar) um método de junção de uma matriz regular `[] .join`. E use `[] .join.call` para executá-lo no contexto de` argumentos '.

Por que isso funciona?

Isso porque o algoritmo interno do método nativo `arr.join (glue)` é muito simples.

Tirado da especificação quase "como está":

1. Deixe `cola 'ser o primeiro argumento ou, se não houver argumentos, então uma vírgula` "," `.
2. Deixe `result` ser uma string vazia.
3. Anexe `this [0]` para `result`.
4. Anexe `cola 'e` este [1] `.
5. Anexe `cola 'e` este [2] `.
6. Faça isso até que os itens `this.length` sejam colados.
7. Retorne `resultado '.

Então, tecnicamente, leva `this` e junta` this [0] `,` this [1] `... etc juntos. É intencionalmente escrito de uma maneira que permite qualquer array-como `this` (não uma coincidência, muitos métodos seguem esta prática). É por isso que também funciona com `this = arguments`.

## Resumo

* Decorador * é um invólucro em torno de uma função que altera seu comportamento. O trabalho principal ainda é realizado pela função.

Em geral, é seguro substituir uma função ou um método por um decorado, exceto por uma pequena coisa. Se a função original tivesse propriedades nele, como `func.calledCount` ou qualquer outra coisa, então o decorado não os fornecerá. Porque isso é um invólucro. Portanto, é preciso ter cuidado se alguém os usar. Alguns decoradores fornecem suas próprias propriedades.

Os decoradores podem ser vistos como "recursos" ou "aspectos" que podem ser adicionados a uma função. Podemos adicionar um ou adicionar muitos. E tudo isso sem alterar seu código!

Para implementar `cachingDecorator`, estudamos métodos:

- [func.call (context, arg1, arg2 ...)] (mdn: js / Function / call) - chama `func` com contexto e argumentos dados.
- [func.apply (context, args)] (mdn: js / Function / apply) - chama `func` passando` context` como `this` e array-like` args` em uma lista de argumentos.

O * encaminhamento de chamadas genérico * geralmente é feito com `apply`:

`` `js
Deixe wrapper = function () {
return original.apply (this, arguments);
}
`` `

Nós também vimos um exemplo de * método de empréstimo * quando tomamos um método de um objeto e `chamamos" isso no contexto de outro objeto. É comum tomar métodos de matrizes e aplicá-los aos argumentos. A alternativa é usar o objeto de parâmetros de descanso que é uma matriz real.


Há muitos decoradores lá na natureza. Verifique se você conseguiu resolver as tarefas deste capítulo.
