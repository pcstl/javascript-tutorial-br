# Variáveis

Na maioria das vezes, uma aplicação JavaScript precisa trabalhar com informações. Aqui estão 2 exemplos:
1. Uma loja on-line - as informações podem incluir produtos vendidos e um carrinho de compras.
2. Um aplicativo de bate-papo - as informações podem incluir usuários, mensagens e muito mais.

As variáveis ​​são usadas para armazenar essas informações.

[cortar]

## Uma variável

A [variável] (https://en.wikipedia.org/wiki/Variable_ (computer_science)) é um "armazenamento nomeado" para dados. Podemos usar variáveis ​​para armazenar guloseimas, visitantes e outros dados.

Para criar uma variável em JavaScript, precisamos usar a palavra-chave `let`.

A declaração abaixo cria (em outras palavras: * declara * ou * define *) uma variável com o nome "mensagem":

`` `js
deixar mensagem;
`` `

Agora podemos inserir alguns dados usando o operador de atribuição `=`:

`` `js
deixar mensagem;

*! *
mensagem = 'Olá'; // armazene a string
* /! *
`` `

A cadeia agora é salva na área de memória associada à variável. Podemos acessá-lo usando o nome da variável:

`` `js run
deixar mensagem;
mensagem = 'Olá!';

*! *
alerta (mensagem); // mostra o conteúdo variável
* /! *
`` `

Para ser conciso, podemos unir a declaração e atribuição de variáveis ​​em uma única linha:

`` `js run
Deixe a mensagem = 'Olá!'; // define a variável e atribui o valor

alerta (mensagem); // Olá!
`` `

Também podemos declarar múltiplas variáveis ​​em uma linha:

`` `js no-embellecer
Deixe o usuário = 'João', idade = 25, mensagem = 'Olá';
`` `

Isso pode parecer mais curto, mas não é recomendado. Por uma questão de melhor legibilidade, use uma única linha por variável.

A variante multilinha é um pouco mais longa, mas mais fácil de ler:

`` `js
Deixe o usuário = 'João';
deixe idade = 25;
Deixe a mensagem = 'Olá';
`` `

Algumas pessoas também escrevem muitas variáveis ​​como essa:
`` `js no-embellecer
Deixe o usuário = 'John',
idade = 25,
mensagem = 'Olá';
`` `

... Ou mesmo no estilo "vírgula primeiro":

`` `js no-embellecer
Deixe o usuário = 'John'
, idade = 25
, mensagem = 'Olá';
`` `

Tecnicamente, todas essas variantes fazem o mesmo. Então, é uma questão de gosto pessoal e estética.


`` `` smart header = "` var` em vez de `let`"
Nos scripts mais antigos, você também pode encontrar outra palavra-chave: `var` em vez de` let`:

`` `js
*! * var * /! * message = 'Olá';
`` `

A palavra-chave `var` é * quase * o mesmo que` let`. Ele também declara uma variável, mas de uma forma ligeiramente diferente, da "velha escola".

Há diferenças sutis entre `let` e` var`, mas ainda não são importantes para nós. Vamos abordá-los em detalhes mais adiante, no capítulo <info: var>.
`` ``

## Uma analogia da vida real

Podemos facilmente entender o conceito de uma "variável" se a imaginarmos como uma "caixa" para dados, com um adesivo de nome exclusivo.

Por exemplo, a variável `mensagem` pode ser imaginada como uma caixa chamada` `mensagem '' com o valor '' Olá! '' Nela:

!] [variable.png]

Podemos colocar qualquer valor na caixa.

Também podemos mudar isso. O valor pode ser alterado quantas vezes for necessário:

`` `js run
deixar mensagem;

mensagem = 'Olá!';

mensagem = 'Mundo!'; // valor alterado

alerta (mensagem);
`` `

Quando o valor é alterado, os dados antigos são removidos da variável:

!] [] (variable-change.png)

Também podemos declarar duas variáveis ​​e copiar dados de um para o outro.

`` `js run
Deixe Hello = 'Hello world!';

deixar mensagem;

*! *
// copie 'Olá mundo' de Olá para a mensagem
mensagem = olá;
* /! *

// agora duas variáveis ​​possuem os mesmos dados

alerta (mensagem); // Olá Mundo!
`` `

`` `cabeçalho inteligente =" idiomas funcionais "
Pode ser interessante saber que existem linguagens de programação [funcional] (https://en.wikipedia.org/wiki/Functional_programming) que proíbem a alteração de um valor variável. Por exemplo, [Scala] (http://www.scala-lang.org/) ou [Erlang] (http://www.erlang.org/).

Em tais linguagens, uma vez que o valor é armazenado "na caixa", está lá para sempre. Se precisarmos armazenar outra coisa, o idioma nos obriga a criar uma nova caixa (declare uma nova variável). Não podemos reutilizar o antigo.

Embora pareça um pouco estranho à primeira vista, essas linguagens são bastante capazes de um desenvolvimento sério. Mais do que isso, existem áreas como cálculos paralelos, onde esta limitação confere certos benefícios. Estudar tal linguagem (mesmo que não planeja usá-lo em breve) é recomendado para ampliar a mente.
`` `

## Nomenclatura variável [# nome variável]

Existem duas limitações para um nome de variável em JavaScript:

1. O nome deve conter apenas letras, dígitos, símbolos `$` e `_`.
2. O primeiro personagem não deve ser um dígito.

Nomes válidos, por exemplo:

`` `js
deixe UserName;
deixe test123;
`` `

Quando o nome contém várias palavras, [camelCase] ​​(https://en.wikipedia.org/wiki/CamelCase) é comumente usado. Isto é: as palavras vão um após o outro, cada palavra começa com uma letra maiúscula: `myVeryLongName`.

O que é interessante - o sinal de dólar '' $ '' e o sublinhado '' _'` também podem ser usados ​​em nomes. São símbolos regulares, como letras, sem qualquer significado especial.

Estes nomes são válidos:

`` `js não são confiáveis
Deixe $ = 1; // declarou uma variável com o nome "$"
Deixe _ = 2; // e agora uma variável com o nome "_"

alerta ($ + _); // 3
`` `

Exemplos de nomes de variáveis ​​incorretas:

`` `js no-embellecer
deixe 1a; // não pode começar com um dígito

deixe meu nome; // um hífen '-' não é permitido no nome
`` `

`` `cabeçalho inteligente =" Caso importante "
Variáveis ​​denominadas `apple` e` AppLE` - são duas variáveis ​​diferentes.
`` `

`` `` cabeçalho inteligente = "letras não inglesas são permitidas, mas não recomendadas"
É possível usar qualquer idioma, incluindo letras cirílicas ou mesmo hieróglifos, como este:

`` `js
Deixe o nome = '...';
deixe-me = '...';
`` `

Tecnicamente, não há nenhum erro aqui, tais nomes são permitidos, mas existe uma tradição internacional de usar o inglês em nomes de variáveis. Mesmo que estivéssemos escrevendo um pequeno roteiro, pode ter uma longa vida à frente. Pessoas de outros países podem precisar lê-lo algum tempo.
`` ``

`` `` warn header = "Reserved names"
Existe uma lista de palavras reservadas, que não podem ser usadas como nomes de variáveis, porque elas são usadas pelo próprio idioma.

Por exemplo, as palavras `let`,` class`, `return`,` function` são reservadas.

O código abaixo dá um erro de sintaxe:

`` `js run no-embellecer
deixe let = 5; // não pode nomear uma variável "let", erro!
deixe retornar = 5; // também não pode nomeá-lo "return", erro!
`` `
`` ``

`` `` '' '' '' '' '' '' '' '' '' '' '' '' '' '' '' "

Normalmente, precisamos definir uma variável antes de usá-la. Mas nos velhos tempos, era tecnicamente possível criar uma variável por mera atribuição do valor, sem "deixar". Isso ainda funciona agora se não colocarmos `use strict`. O comportamento é mantido para compatibilidade com scripts antigos.

`` `js run no-strict
// nota: nenhum "uso estrito" neste exemplo

num = 5; // a variável "num" é criada se não existisse

alerta (num); // 5
`` `

Isso é uma prática ruim, ele dá um erro no modo estrito:

`` `js não são confiáveis
"uso estrito";

*! *
num = 5; // erro: num não está definido
* /! *
`` `

`` ``

## Constantes

Para declarar uma variável constante (não variável), pode-se usar `const` em vez de` let`:

`` `js
const myBirthday = '18 .04.1982 ';
`` `

As variáveis ​​declaradas usando `const` são chamadas de" constantes ". Eles não podem ser alterados. Uma tentativa de fazê-lo causaria um erro:

`` `js run
const myBirthday = '18 .04.1982 ';

meu aniversário = '01 .01.2001 '; // erro, não pode atribuir a constante!
`` `

Quando um programador tem certeza de que a variável nunca deve mudar, ele pode usar `const` para garantir isso, e também para mostrar claramente esse fato a todos.


### Constantes em maiúsculas

Existe uma prática generalizada para usar constantes como alias para valores difíceis de lembrar que são conhecidos antes da execução.

Essas constantes são nomeadas usando letras maiúsculas e sublinhados.

Como isso:

`` `js run
const COLOR_RED = "# F00";
const COLOR_GREEN = "# 0F0";
const COLOR_BLUE = "# 00F";
const COLOR_ORANGE = "# FF7F00";

// ... quando precisamos escolher uma cor
Deixe color = COLOR_ORANGE;
alerta (cor); // # FF7F00
`` `

Benefícios:

- `COLOR_ORANGE` é muito mais fácil de lembrar do que` "# FF7F00" `.
- É muito mais fácil de digitar em "FF7F00" do que no `COLOR_ORANGE`.
- Ao ler o código, `COLOR_ORANGE` é muito mais significativo do que` # FF7F00`.

Quando devemos usar maiúsculas para uma constante, e quando devemos nomeá-las normalmente? Vamos deixar isso claro.

Ser um "constante" significa que o valor nunca muda. Mas há constantes que são conhecidas antes da execução (como um valor hexadecimal para vermelho), e há aqueles que são * calculados * em tempo de execução, durante a execução, mas não mudam após a atribuição.

Por exemplo:
`` `js
const pageLoadTime = / * tempo tomado por uma página da Web para carregar * /;
`` `

O valor de `pageLoadTime` não é conhecido antes da carga da página, então é chamado normalmente. Mas ainda é uma constante, porque não muda após a atribuição.

Em outras palavras, as constantes com nome de capital são usadas apenas como alias para valores "codificados".

## Nome das coisas corretas

Falando sobre variáveis, há uma coisa mais importante.

Nomeie as variáveis ​​sensivelmente. Tome tempo para pensar se necessário.

A nomeação variável é uma das habilidades mais importantes e complexas na programação. Um rápido olhar sobre nomes de variáveis ​​pode revelar qual código é escrito por um iniciante e que por um desenvolvedor experiente.

Em um projeto real, a maior parte do tempo é gasto na modificação e extensão da base de código existente, ao invés de escrever algo completamente separado do zero. E quando retornamos ao código depois de algum tempo de fazer outra coisa, é muito mais fácil encontrar informações bem rotuladas. Ou, em outras palavras, quando as variáveis ​​têm bons nomes.

Por favor, passe algum tempo pensando sobre o nome certo para uma variável antes de declarar. Isso vai te pagar muito.

Algumas regras de boa-a-seguir são:

- Use nomes legíveis para humanos como `userName` ou` shoppingCart`.
- Fique longe de abreviaturas ou nomes curtos como `a`,` b`, `c`, a menos que você realmente saiba o que está fazendo.
- Faça o nome maximamente descritivo e conciso. Exemplos de nomes incorretos são `data` e` value`. Esse nome não diz nada. É apenas bom usá-los se for excepcionalmente óbvio a partir do contexto quais dados ou valores são significados.
- Concorda em termos dentro de sua equipe e em sua própria mente. Se um visitante do site é chamado de "usuário", devemos nomear variáveis ​​relacionadas como `currentUser` ou` newUser`, mas não `currentVisitor` ou` newManInTown`.

Soa simples? Na verdade, é, mas a criação de bons nomes descritivos e concisos na prática não é. Vá em frente.

`` `cabeçalho inteligente =" Reutilizar ou criar? "
E a última nota. Existem alguns programadores preguiçosos que, ao invés de declarar uma nova variável, tendem a reutilizar os existentes.

Como resultado, a variável é como uma caixa onde as pessoas lançam coisas diferentes sem alterar o adesivo. O que está dentro dele agora? Quem sabe ... Precisamos nos aproximar e verificar.

Esse programador economiza um pouco na declaração de variáveis, mas perde dez vezes mais na depuração do código.

Uma variável extra é boa, não má.

Modern minigos e navegadores JavaScript otimizam o código com bastante força, portanto, não criará problemas de desempenho. O uso de variáveis ​​diferentes para valores diferentes pode até ajudar o motor a otimizar.
`` `

## Resumo

Podemos declarar variáveis ​​para armazenar dados. Isso pode ser feito usando `var` ou` let` ou `const`.

- `let` - é uma declaração de variável moderna. O código deve estar no modo estrito para usar `let` no Chrome (V8).
- `var` - é uma declaração de variável da velha escola. Normalmente, não o usamos, mas abordaremos diferenças sutis do `let` no capítulo <info: var>, apenas no caso de você precisar deles.
- `const` - é como` let`, mas o valor da variável não pode ser alterado.

As variáveis ​​devem ser nomeadas de forma a que possamos entender facilmente o que está dentro.
