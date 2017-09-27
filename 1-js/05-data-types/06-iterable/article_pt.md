
# Iterables

* Iterable * objetos é uma generalização de arrays. Esse é um conceito que permite tornar qualquer objeto utilizável em um loop `for..of`.

Arrays por si só são iteráveis. Mas não só arrays. As cordas também são iteráveis ​​e muitos outros objetos internos.

Iterables são amplamente utilizados pelo JavaScript principal. Como veremos, muitos operadores e métodos internos dependem deles.

[cortar]

## Symbol.iterator

Podemos facilmente entender o conceito de iterables fazendo um dos nossos.

Por exemplo, temos um objeto, não é uma matriz, mas parece adequado para `for..of`.

Como um objeto `range` que representa um intervalo de números:

`` `js
deixe range = {
de: 1,
para: 5
};

// Queremos que o for..of funcione:
// para (deixe um numero de alcance) ... num = 1,2,3,4,5
`` `

Para tornar o `range` iterable (e, portanto, permitir` for..of` work), precisamos adicionar um método ao objeto chamado `Symbol.iterator` (um símbolo embutido especial apenas para isso).

- Quando `for..of` começa, ele chama esse método (ou erros se não for encontrado).
- O método deve retornar um * iterador * - um objeto com o método `próximo`.
- Quando `for..of` quer o próximo valor, ele chama` next () `nesse objeto.
- O resultado de `next ()` deve ter o formulário `{done: Boolean, value: any}`, onde `done = true` significa que a iteração está concluída, caso contrário` value` deve ser o novo valor.

Aqui está a implementação completa para `range`:

`` `js run
deixe range = {
de: 1,
para: 5
};

// 1. chamar para ... inicialmente chama isso
intervalo [Symbol.iterator] = function () {

// 2. ... retorna o iterador:
Retorna {
atual: this.from,
último: this.to,

// 3. next () é chamado em cada iteração pelo loop for..of
Próximo() {
// 4. deve retornar o valor como um objeto {done: .., value: ...}
se (this.current <= this.last) {
return {done: false, value: this.current ++};
} outro {
return {done: true};
}
}
};
};

// agora funciona
para (deixe um numero de alcance) {
alerta (num); // 1, então 2, 3, 4, 5
}
`` `

Há uma importante separação de preocupações neste código:

- O `range` em si não possui o método` next () `.
- Em vez disso, outro objeto, um chamado "iterador", é criado pela chamada para `range [Symbol.iterator] ()`, e lida com a iteração.

Portanto, o objeto do iterador é separado do objeto que itera.

Tecnicamente, podemos fundê-los e usar o `range` em si mesmo como o iterador para tornar o código mais simples.

Como isso:

`` `js run
deixe range = {
de: 1,
para: 5,

[Symbol.iterator] () {
this.current = this.from;
devolva isso;
},

Próximo() {
se (this.current <= this.to) {
return {done: false, value: this.current ++};
} outro {
return {done: true};
}
}
};

para (deixe um numero de alcance) {
alerta (num); // 1, então 2, 3, 4, 5
}
`` `

Agora `range [Symbol.iterator] ()` retorna o objeto `range` em si: ele possui o método` next () `necessário e lembra o progresso atual da iteração em` this.current`. Às vezes, também está bem. A desvantagem é que agora é impossível ter dois "for..of" loops executando sobre o objeto simultaneamente: eles vão compartilhar o estado da iteração, porque há apenas um iterador - o próprio objeto.

`` `cabeçalho inteligente =" iteradores infinitos "
Os iteradores infinitos também são possíveis. Por exemplo, o `range` torna-se infinito para` range.to = Infinity`. Ou podemos fazer um objeto iterável que gere uma seqüência infinita de números pseudorandom. Também pode ser útil.

Não há limitações no `próximo ', ele pode retornar mais e mais valores, isso é normal.

Claro, o "for..of" loop sobre um iterável seria infinito. Mas podemos sempre pará-lo usando `break`.
`` `


## String é iterable

As matrizes e as cadeias de caracteres são as mais utilizadas em scripts incorporados.

Para uma string, `for..of` faz um loop sobre seus caracteres:

`` `js run
para (deixe char de "teste") {
alerta (char); // t, então e, então s, então t
}
`` `

E funciona corretamente com pares de substituição!

`` `js run
deixe str = '𝒳😂';
para (deixe char of str) {
alerta (char); // 𝒳, e depois 😂
}
`` `

## Chamando um iterador explicitamente

Normalmente, os internos de iterables estão escondidos do código externo. Há um ciclo para ... do que, isso funciona, é tudo o que precisa saber.

Mas, para entender as coisas um pouco mais profundamente, vejamos como criar um iterador explicitamente.

Vamos iterar sobre uma seqüência de caracteres da mesma forma como `for..of`, mas com chamadas diretas. Este código obtém um iterador de string e o chama "manualmente":

`` `js run
Deixe str = "Olá";

// faz o mesmo que
// para (let char of str) alert (char);

deixe iterator = str [Symbol.iterator] ();

enquanto (verdadeiro) {
Deixe resultado = iterator.next ();
se (result.done) quebrar;
alerta (resultado.valor); // emite caracteres um a um
}
`` `

Isso raramente é necessário, mas nos dá mais controle sobre o processo do que `for..of`. Por exemplo, podemos dividir o processo de iteração: iterar um pouco, depois parar, fazer outra coisa e, em seguida, continuar depois.

## Iterables e array-likes [# array-like]

Existem dois termos oficiais que parecem semelhantes, mas são muito diferentes. Certifique-se de compreendê-los bem para evitar a confusão.

- * Iterables * são objetos que implementam o método `Symbol.iterator`, conforme descrito acima.
- * Array-likes * são objetos que têm índices e `length`, então eles se parecem com arrays.

Naturalmente, essas propriedades podem se combinar. Por exemplo, as strings são ambas iteráveis ​​(`for..of` funciona nelas) e array-like (eles têm índices numéricos e` length`).

Mas um iterável pode não ser semelhante a uma matriz. E vice-versa, um array-like pode não ser iterable.

Por exemplo, o `range` no exemplo acima é iterable, mas não como array, porque não possui propriedades indexadas e` length`.

E aqui está o objeto que é array-like, mas não iterable:

`` `js run
Deixe arrayLike = {// tem índices e comprimento => array-like
0: "Olá",
1: "Mundo",
comprimento: 2
};

*! *
// Erro (sem Symbol.iterator)
para (deixe item de arrayLike) {}
* /! *
`` `

O que eles compartilham em comum - tanto iterables como array-likes geralmente são * não arrays *, eles não têm `push`,` pop` etc. Isso é bastante inconveniente se nós tivermos esse objeto e queremos trabalhar com ele como com uma matriz.

## Array.from

Existe um método universal [Array.from] (mdn: js / Array / from) que os reúne. É preciso um valor iterável ou semelhante a uma matriz e faz uma "matriz" real "" da mesma. Então, podemos chamar métodos de matrizes nela.

Por exemplo:

`` `js run
Deixe arrayLike = {
0: "Olá",
1: "Mundo",
comprimento: 2
};

*! *
Deixe arr = Array.from (arrayLike); // (*)
* /! *
alerta (arr.pop ()); // Mundo (método funciona)
`` `

`Array.from` na linha` (*) `pega o objeto, examina-o para ser um iterable ou array-like, então faz uma nova matriz e copia todos os itens.

O mesmo acontece com um iterável:

`` `js
// assumindo que o intervalo é tirado do exemplo acima
Deixe arr = Array.from (range);
alerta (arr); // 1,2,3,4,5 (array toString conversão funciona)
`` `

A sintaxe completa para `Array.from` permite fornecer uma função opcional de" mapeamento ":
`` `js
Array.from (obj [, mapFn, thisArg])
`` `

O segundo argumento `mapFn` deve ser a função a ser aplicada a cada elemento antes de adicionar à matriz, e` thisArg` permite configurar `this` para isso.

Por exemplo:

`` `js
// assumindo que o intervalo é tirado do exemplo acima

// quadrado de cada número
Deixe arr = Array.from (range, num => num * num);

alerta (arr); // 1,4,9,16,25
`` `

Aqui, usamos `Array.from` para transformar uma string em uma série de caracteres:

`` `js run
deixe str = '𝒳😂';

// divide str na matriz de caracteres
Deixe chars = Array.from (str);

alerta (caracteres [0]); // 𝒳
alerta (caracteres [1]); // 😂
alerta (chars.length); // 2
`` `

Ao contrário do `str.split`, ele depende da natureza iterável da string e assim, assim como` for..of`, funciona corretamente com pares de substituição.

Tecnicamente, aqui faz o mesmo que:

`` `js run
deixe str = '𝒳😂';

deixe chars = []; // Array.to interno faz o mesmo loop
para (deixe char of str) {
chars.push (char);
}

alerta (caracteres);
`` `

... Mas é mais curto.

Podemos até mesmo criar uma "fatia" de alerta de substituição:

`` `js run
Função fatia (str, start, end) {
retornar Array.from (str) .slice (start, end) .join ('');
}

Deixe str = '𝒳😂 𩷶';

alerta (fatia (str, 1, 3)); // 😂 𩷶

// método nativo não suporta pares de substituição
alerta (str.slice (1, 3)); // lixo (duas peças de diferentes pares de substituição)
`` `


## Resumo

Os objetos que podem ser usados ​​em `for..of` são chamados * iterable *.

- Tecnicamente, iterables deve implementar o método chamado `Symbol.iterator`.
- O resultado de `obj [Symbol.iterator]` é chamado de * iterator *. Ele lida com o processo de iteração adicional.
- Um iterador deve ter o método chamado `next ()` que retorna um objeto `{done: Boolean, value: any}`, aqui `done: true` denota o final da iteração, caso contrário, o` value` é o próximo valor.
- O método `Symbol.iterator` é chamado automaticamente por` for..of`, mas também podemos fazê-lo diretamente.
- Iteráveis ​​incorporados, como cordas ou arrays, também implementam `Symbol.iterator`.
- String iterator sabe sobre pares de substituição.


Os objetos que têm propriedades indexadas e `length` são chamados * array-like *. Tais objetos também podem ter outras propriedades e métodos, mas não possuem métodos internos de matrizes.

Se olharmos dentro da especificação - veremos que a maioria dos métodos internos assumem que eles funcionam com iterables ou array-likes em vez de matrizes "reais", porque isso é mais abstrato.

`Array.from (obj [, mapFn, thisArg])` faz uma "Array" real de um "obj" iterable ou similar a uma matriz, e então podemos usar métodos de matrizes nela. Os argumentos opcionais `mapFn` e` thisArg` permitem aplicar uma função a cada item.
