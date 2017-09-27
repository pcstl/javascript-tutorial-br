

# Objeto Global

Quando o JavaScript foi criado, havia uma idéia de um "objeto global" que fornece todas as variáveis ​​e funções globais. Foi planejado que vários scripts no navegador usariam esse único objeto global e compartilhassem variáveis ​​através dele.

Desde então, o JavaScript evoluiu grandemente, e essa idéia de ligar código através de variáveis ​​globais tornou-se muito menos atraente. No JavaScript moderno, o conceito de módulos tomou seu lugar.

Mas o objeto global ainda permanece na especificação.

Em um navegador, ele é chamado de "janela", para Node.JS é "global", para outros ambientes pode ter outro nome.

Faz duas coisas:

1. Fornece acesso a funções e valores internos, definidos pela especificação e pelo ambiente.
Por exemplo, podemos chamar `alert` diretamente ou como método de` window`:

`` `js run


// o mesmo que

`` `

O mesmo se aplica a outros built-ins. Por exemplo. podemos usar `window.Array` em vez de` Array`.

2. Fornece acesso a declarações de função globais e variáveis ​​`var`. Podemos ler e escrevê-los usando suas propriedades, por exemplo:

<! - não é estrito mover variáveis ​​de eval ->
`` `js não confiável não execute uma atualização rigorosa
var phrase = "Hello";

função sayHi () {
alerta (frase);
}

// pode ler da janela
alerta (window.phrase); // Olá (var global)
alerta (window.sayHi); // função (declaração de função global)

// pode escrever na janela (cria uma nova variável global)
window.test = 5;


`` `

... Mas o objeto global não possui variáveis ​​declaradas com `let / const`!

`` `js não confiável não execute uma atualização rigorosa
*! * let * /! * user = "John";
alerta (usuário); // John

alerta (window.user); // indefinido, não tenha deixado
alerta ("usuário" na janela); // falso
`` `

`` `smart header =" O objeto global não é um registro ambiental global "
Nas versões do ECMAScript anteriores ao ES-2015, não havia variáveis ​​`let / const`, apenas` var`. E o objeto global foi usado como um Registro de Ambiente global (os termos eram um pouco diferentes, mas essa é a essência).

Mas a partir do ES-2015, essas entidades são divididas. Existe um ambiente léxico global com seu registro ambiental. E há um objeto global que fornece * algumas * das variáveis ​​globais.

Como uma diferença prática, as variáveis ​​globais `let / const` são definitivamente propriedades do Registro de ambiente global, mas elas não existem no objeto global.

Naturalmente, é porque a idéia de um objeto global como uma forma de acessar "todas as coisas globais" vem da antiguidade. Hoje em dia não é considerado uma coisa boa. Recursos de linguagem moderna como `let / const` não fazem amizades com ele, mas os antigos ainda são compatíveis.
`` `

## Usos de "janela"

Em ambientes do lado do servidor como Node.JS, o objeto `global` é usado excepcionalmente raramente. Provavelmente seria justo dizer "nunca".

No entanto, as "janelas" no navegador são algumas vezes utilizadas.

Normalmente, não é uma boa idéia usar isso, mas aqui estão alguns exemplos que você pode encontrar.

1. Para acessar exatamente a variável global se a função tiver o local com o mesmo nome.

`` `js não confiável não execute uma atualização rigorosa
var user = "Global";

função sayHi () {
var user = "Local";

*! *
alerta (window.user); // Global
* /! *
}

SayHi ();
`` `

Esse uso é uma solução alternativa. Seria melhor nomear as variáveis ​​de forma diferente, isso não exigirá uso para escrever o código dessa maneira. E note "var" `antes do` usuário '. O truque não funciona com as variáveis ​​`let`.

2. Para verificar se existe uma determinada variável global ou uma compilação.

Por exemplo, queremos verificar se existe uma função global `XMLHttpRequest`.

Não podemos escrever `if (XMLHttpRequest)`, porque se não houver `XMLHttpRequest`, haverá um erro (variável não definida).

Mas podemos lê-lo de `window.XMLHttpRequest`:

`` `js run
se (window.XMLHttpRequest) {
alerta ('XMLHttpRequest existe!')
}
`` `

Se não existe tal função global, então `window.XMLHttpRequest` é apenas uma propriedade de objeto não existente. Isso é 'indefinido', sem erro, então funciona.

Também podemos escrever o teste sem `janela ':

`` `js
se (typeof XMLHttpRequest == 'function') {
/ * existe uma função XMLHttpRequest? * /
}
`` `

Isso não usa `window`, mas é (teoricamente) menos confiável, porque` typeof` pode usar um XMLHttpRequest local, e queremos o global.


3. Para levar a variável da janela da direita. Esse é provavelmente o caso de uso mais válido.

Um navegador pode abrir várias janelas e guias. Uma janela também pode incorporar outra em `<iframe>`. Toda janela do navegador possui seu próprio objeto `window` e variáveis ​​globais. O JavaScript permite que janelas que vêm do mesmo site (mesmo protocolo, host, porta) para acessar variáveis ​​entre si.

Esse uso é um pouco além do nosso alcance por enquanto, mas parece:
`` `html run
<iframe src = "/" id = "iframe"> </ iframe>

<script>
alerta (innerWidth); // obter innerWidth propriedade da janela atual (apenas navegador)
alerta (Array); // Obter Array da janela atual (javascript core builtin)

// quando o iframe carrega ...
iframe.onload = function () {
// obtém a largura da janela do iframe
*! *
alerta (iframe.contentWindow.innerWidth);
* /! *
// obtenha o Array incorporado da janela do iframe
*! *
alerta (iframe.contentWindow.Array);
* /! *
};
</ script>
`` `

Aqui, os dois primeiros alertas usam a janela atual e as duas últimas levam variáveis ​​da janela `iframe`. Pode ser qualquer variável se `iframe` for originário do mesmo protocolo / host / porta.

## "this" e objeto global

Às vezes, o valor de `this` é exatamente o objeto global. Isso raramente é usado, mas alguns scripts dependem disso.

1. No navegador, o valor de `this` na área global é` window`:

`` `js run
// fora das funções
alerta (esta === janela); // verdade
`` `

Outros, ambientes não-browser, podem usar outro valor para `this` em tais casos.

2. Quando uma função com `this` é chamada no modo não-estrito, ele obtém o objeto global como` this`:
`` `js run no-strict
// não em modo estrito (!)
função f () {
alerta (isto); // [janela do objeto]
}

f (); // chamado sem um objeto
`` `

Por especificação, `this` neste caso deve ser o objeto global, mesmo em ambientes não-browser como Node.JS. Isso é para compatibilidade com scripts antigos, no modo estrito, "este" seria "indefinido".
