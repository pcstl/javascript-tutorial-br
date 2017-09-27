# Parâmetros de descanso e operador de propagação

Muitas funções incorporadas do JavaScript suportam em número arbitrário de argumentos.

Por exemplo:

- `Math.max (arg1, arg2, ..., argN)` - retorna o maior dos argumentos.
- `Object.assign (dest, src1, ..., srcN)` - copia propriedades de `src1..N` para` dest`.
- ...e assim por diante.

Neste capítulo, veremos como fazer o mesmo. E, mais importante, como se sentir confortável trabalhando com tais funções e arrays.

[cortar]

## Restar parâmetros `...`

Uma função pode ser chamada com qualquer número de argumentos, independentemente de como está definido.

Como aqui:
`` `js run
soma de função (a, b) {
Retornar a + b;
}

alerta (soma (1, 2, 3, 4, 5));
`` `

Não haverá nenhum erro devido a argumentos "excessivos". Mas, é claro, no resultado, apenas os dois primeiros serão contados.

Os parâmetros restantes podem ser mencionados em uma definição de função com três pontos `...`. Eles literalmente significam: "reunir os parâmetros restantes em uma matriz".

Por exemplo, para reunir todos os argumentos em array `args`:

`` `js run
function sumAll (... args) {// args é o nome da matriz
deixe soma = 0;

para (arg de args) soma + = arg;

Soma de retorno;
}

alerta (sumAll (1)); // 1
alerta (sumAll (1, 2)); // 3
alerta (sumAll (1, 2, 3)); // 6
`` `

Podemos escolher obter os primeiros parâmetros como variáveis ​​e reunir apenas o resto.

Aqui, os dois primeiros argumentos variam e o restante passa a matriz de "títulos"

`` `js run
function showName (firstName, lastName, ... titles) {
alerta (firstName + '' + lastName); // Júlio César

// o resto entra em matrizes de títulos
// ie. titles = ["Consul", "Imperator"]
alerta (títulos [0]); // Cônsul
alerta (títulos [1]); // Imperator
alerta (títulos.length); // 2
}

showName ("Julius", "César", "Consul", "Imperator");
`` `

`` `` warn header = "Os parâmetros restantes devem estar no final"
Os demais parâmetros reúnem todos os argumentos restantes, então o seguinte não tem sentido:

`` `js
função f (arg1, ... rest, arg2) {// arg2 depois ... restante?
// erro
}
`` `

O "descanso" deve ser sempre o último.
`` ``

## A variável "argumentos"

Há também um objeto especial semelhante a uma matriz chamado `argumentos 'que contém todos os argumentos por seu índice.

Por exemplo:

`` `js run
função showName () {
alerta (arguments.length);
alerta (argumentos [0]);
alerta (argumentos [1]);

// é iterável
// para (arg of arguments) alert (arg);
}

// mostra: 2, Júlio, César
showName ("Julius", "César");

// mostra: 1, Ilya, indefinido (nenhum segundo argumento)
showName ("Ilya");
`` `

Nos tempos antigos, os parâmetros de descanso não existiam na linguagem e os "argumentos" eram a única maneira de obter todos os argumentos da função, independentemente do número total.

E ainda funciona, podemos usá-lo.

Mas a desvantagem é que, embora os "argumentos" sejam simples e iteráveis, não é uma matriz. Não suporta métodos de matriz, então não podemos dizer chamar `arguments.map (...)`.

Além disso, sempre tem todos os argumentos nele, não podemos capturá-los parcialmente, como fizemos com os parâmetros de descanso.

Então, quando precisamos desses recursos, os parâmetros de descanso são preferidos.

`` `` cabeçalho inteligente = "as funções de seta não possuem` \ "argumentos \" `"
Se acessar o objeto `argumentos 'de uma função de seta, ele os leva da função externa" normal ".

Aqui está um exemplo:

`` `js run
função f () {
Deixe showArg = () => alert (argumentos [0]);
showArg ();
}

f (1); // 1
`` `
Como nos lembramos, as funções de seta não têm o seu próprio `this`. Agora, sabemos que eles também não têm o objeto especial de "argumentos".

`` ``

## Spread operator [# spread-operator]

Acabamos de ver como obter uma matriz a partir da lista de parâmetros.

Mas às vezes precisamos fazer exatamente o contrário.

Por exemplo, existe uma função embutida [Math.max] (mdn: js / Math / max) que retorna o maior número da lista:

`` `js run
alerta (Math.max (3, 5, 1)); // 5
`` `

Agora, digamos que temos uma matriz `[3, 5, 1]`. Como chamar `Math.max` com ele?

Passar "como ele" não funcionará, porque `Math.max` espera uma lista de argumentos numéricos, e não uma única matriz:

`` `js run
deixe arr = [3, 5, 1];

*! *
alerta (Math.max (arr)); // NaN
* /! *
`` `

... E certamente não podemos listar itens manualmente no código `Math.max (arg [0], arg [1], arg [2])`, porque podemos ter certeza de quanto tempo existem. À medida que nosso script é executado, pode haver muitos, ou pode haver nenhum. Também isso seria feio.

* Espalhe o operador * no resgate. Parece semelhante aos parâmetros de descanso, também usando `...`, mas faz todo o contrário.

Quando `... arr` é usado na chamada de função, ele" expande "um objeto iterável` arr` na lista de argumentos.

Para `Math.max`:

`` `js run
deixe arr = [3, 5, 1];

alerta (Math.max (... arr)); // 5 (spread transforma matriz em uma lista de argumentos)
`` `

Também podemos passar vários iterables desta forma:

`` `js run
Deixe arr1 = [1, -2, 3, 4];
Deixe arr2 = [8, 3, -8, 1];

alerta (Math.max (... arr1, ... arr2)); // 8
`` `

... E mesmo combinar o operador de propagação com valores normais:


`` `js run
Deixe arr1 = [1, -2, 3, 4];
Deixe arr2 = [8, 3, -8, 1];

alerta (Math.max (1, ... arr1, 2, ... arr2, 25)); // 25
`` `

O operador de spread também pode ser usado para mesclar arrays:

`` `js run
deixe arr = [3, 5, 1];
Deixe arr2 = [8, 9, 15];

*! *
Deixe mesclado = [0, ... arr, 2, ... arr2];
* /! *

alerta (mesclado); // 0,3,5,1,2,8,9,15 (0, então arr, depois 2, depois arr2)
`` `

Nos exemplos acima, usamos uma matriz para demonstrar o operador de propagação, mas qualquer iterável fará.

Por exemplo, aqui usamos o operador de propagação para transformar a cadeia em matriz de caracteres:

`` `js run
Deixe str = "Olá";

alerta ([... str]); // Olá
`` `

O operador de propagação internamente usa iteradores para reunir elementos, da mesma forma que `for..of` faz.

Então, para uma string, `for..of` retorna caracteres e` ... str` torna-se `" h "," e "," l "," l "," o "`. A lista de caracteres é passada para o inicializador de matriz `[... str]`.

Para esta tarefa particular, também poderíamos usar `Array.from`, porque converte uma iterável (como uma string) em uma matriz:

`` `js run
Deixe str = "Olá";

// Array.from converte uma iterável em uma matriz
alerta (Array.from (str)); // Olá
`` `

O resultado é o mesmo que `[... str]`.

Mas há uma diferença sutil entre `Array.from (obj)` e `[... obj]`:

- `Array.from` opera em array-likes e iterables.
- O operador de propagação opera apenas em iterables.

Então, para a tarefa de transformar algo em uma matriz, o `Array.from` parece mais universal.


## Resumo

Quando vemos `" ... "no código, são parâmetros de descanso ou o operador de propagação.

Existe uma maneira fácil de distinguir entre eles:

- Quando `...` está no final dos parâmetros de função, é "parâmetros de descanso" e reúne o resto da lista na matriz.
- Quando `...` ocorre em uma chamada de função ou similar, é chamado de "operador de propagação" e expande uma matriz na lista.

Use padrões:

- Os parâmetros de restrição são usados ​​para criar funções que aceitam qualquer número de argumentos.
- O operador de propagação é usado para passar uma matriz para funções que normalmente requerem uma lista de muitos argumentos.

Juntos, eles ajudam a viajar entre uma lista e uma série de parâmetros com facilidade.

Todos os argumentos de uma chamada de função também estão disponíveis em "argumentos" de estilo antigo: array-like iterable object.
