# Cordas

Em JavaScript, os dados de texto são armazenados como strings. Não há nenhum tipo separado para um único personagem.

O formato interno para strings é sempre [UTF-16] (https://en.wikipedia.org/wiki/UTF-16), não está vinculado à codificação da página.

[cortar]

## Citações

Vamos lembrar os tipos de cotações.

As cordas podem ser incluídas em citações simples, citações duplas ou backticks:

`` `js
Deixe single = 'single-coted';
Deixe duplo = "duplo-citado";

Deixe backticks = `backticks`;
`` `

As cotações simples e duplas são essencialmente as mesmas. Os backticks no entanto nos permitem incorporar qualquer expressão na string, incluindo chamadas de função:

`` `js run
soma de função (a, b) {
Retornar a + b;
}

alerta (`1 + 2 = $ {sum (1, 2)}.`); // 1 + 2 = 3.
`` `

Outra vantagem de usar backticks é que eles permitem que uma string abranja várias linhas:

`` `js run
Deixe guestList = `Guests:
* John
* Pete
* Mary
`;

alerta (guestList); // uma lista de convidados, múltiplas linhas
`` `

Se tentarmos usar aspas simples ou duplas da mesma maneira, haverá um erro:
`` `js run
Deixe guestList = "Convidados: // Erro: token inesperado ILEGAL
* John";
`` `

As citações simples e duplas provêm dos tempos antigos de criação de idiomas quando a necessidade de seqüências de caracteres multilinha não foi levada em consideração. Os Backticks apareceram muito mais tarde e, portanto, são mais versáteis.

Os Backticks também nos permitem especificar uma "função de modelo" antes do primeiro backtick. A sintaxe é: <code> func`string` </ code>. A função `func` é chamada automaticamente, recebe a string e as expressões incorporadas e pode processá-las. Você pode ler mais sobre isso nos [docs] (mdn: JavaScript / Reference / Template_literals # Tagged_template_literals). Isso é chamado de "modelos marcados". Esse recurso torna mais fácil o uso de strings em modelos personalizados ou outras funcionalidades, mas raramente é usado.


## Caracteres especiais

Ainda é possível criar cadeias multilinhas com aspas simples usando um chamado "caractere de nova linha", escrito como `\ n`, que denota uma quebra de linha:

`` `js run
deixe guestList = "Convidados: \ n * John \ n * Pete \ n * Mary";

alerta (guestList); // uma lista multilinha de convidados
`` `

Por exemplo, essas duas linhas descrevem o mesmo:

`` `js run


// duas linhas usando uma linha nova e backticks normais
alerta (`Olá
Mundo ');
`` `

Existem outros caracteres "especiais" menos comuns também. Aqui está a lista:

| Caráter | Descrição |
| ----------- | ------------- |
| `\ b` | Backspace |
| `\ f` | Alimentação de formulário |
| `\ n` | Nova linha |
| `\ r` | Retorno de carro |
| `\ t` | Tab |
| `\ uNNNN` | Um símbolo unicode com o código hexadecimal` NNNN`, por exemplo `\ u00A9` - é um unicode para o símbolo de copyright` © `. Deve ser exatamente 4 dígitos hexadecimais. |
| `\ u {NNNNNNNN}` | Alguns caracteres raros são codificados com dois símbolos unicode, levando até 4 bytes. Este longo unicode requer aparelhos em torno dele.

Exemplos com unicode:

`` `js run
alerta ("\ u00A9"); // ©
alerta ("\ u {20331}"); // 佫, um hieróglifo chinês raro (unicode longo)
alerta ("\ u {1F60D}"); // 😍, um símbolo de rosto sorridente (outro longo unicode)
`` `

Todos os caracteres especiais começam com um caractere de barra invertida `\`. É também chamado de "personagem de escape".

Também usamos isso se quisermos inserir uma citação na string.

Por exemplo:

`` `js run
alerta ('I *! * \' * /! * m the Walrus! '); // *! * Eu sou * /! * The Walrus!
`` `

Como você pode ver, temos que antecipar a cotação interna pela barra invertida `\ '`, porque, de outra forma, indicaria o fim da string.

Claro, isso refere-se apenas às citações que são as mesmas que as que estão incluídas. Então, como uma solução mais elegante, poderíamos alternar entre aspas duplas ou vice-versa em vez disso:

`` `js run
alerta ('Eu sou o Morsa!'); // Eu sou o Walrus!
`` `

Observe que a barra invertida `\` serve para a leitura correta da string por JavaScript, então desaparece. A string na memória não tem `\`. Você pode ver claramente isso em `alert` dos exemplos acima.

Mas e se precisarmos mostrar uma barra invertida real \ `dentro da string?

Isso é possível, mas precisamos duplicá-lo como `\\`:

`` `js run
alerta (`A barra invertida: \\`); // A barra invertida: \
`` `

## comprimento da corda


A propriedade `length` possui o comprimento da string:

`` `js run
alerta (`My \ n`.length); // 3
`` `

Note que `\ n` é um único caractere" especial ", então o comprimento é realmente` 3 '.

`` `warn header =" `length` é uma propriedade"
As pessoas com um fundo em algumas outras línguas, por vezes, são maiúsculas e minúsculas, chamando `str.length ()` ao invés de apenas `str.length`. Isso não funciona.

Observe que `str.length` é uma propriedade numérica, não uma função. Não há necessidade de adicionar colchetes depois.
`` `

## Acessando personagens

Para obter um personagem na posição `pos`, use colchetes` [pos] `ou ligue para o método [str.charAt (pos)] (mdn: js / String / charAt). O primeiro personagem começa a partir da posição zero:

`` `js run
Deixe str = `Hello`;

// o primeiro personagem
alerta (str [0]); // H
alerta (str.charAt (0)); // H

// o último personagem
alerta (str [str.length - 1]); // o
`` `

Os colchetes são uma maneira moderna de obter um personagem, enquanto o `charAt` existe principalmente por motivos históricos.

A única diferença entre eles é que se nenhum caracter é encontrado, `[]` retorna `undefined`, e` charAt` retorna uma string vazia:

`` `js run
Deixe str = `Hello`;

alerta (str [1000]); // Indefinido
alerta (str.charAt (1000)); // '' (uma string vazia)
`` `

Podemos também iterar sobre caracteres usando `for..of`:

`` `js run
para (deixe char de "Olá") {
alerta (char); // H, e, l, l, o (char se torna "H", então "e", então "l" etc.)
}
`` `

## As cordas são imutáveis

As strings não podem ser alteradas em JavaScript. É impossível mudar um personagem.

Vamos tentar para mostrar que não funciona:

`` `js run
Deixe str = 'Hi';

str [0] = 'h'; // erro
alerta (str [0]); // não funciona
`` `

A solução usual é criar uma nova string inteira e atribuí-la a `str` em vez da antiga.

Por exemplo:

`` `js run
Deixe str = 'Hi';

str = 'h' + str [1]; // substitua a string

alerta (str); // Oi
`` `

Nas seções a seguir, veremos mais exemplos disso.

## Mudando o caso

Métodos [toLowerCase ()] (mdn: js / String / toLowerCase) e [toUpperCase ()] (mdn: js / String / toUpperCase) alteram o caso:

`` `js run
alerta ('Interface'.toUpperCase ()); // INTERFACE
alerta ('Interface'.toLowerCase ()); // interface
`` `

Ou, se queremos um único caractere em minúscula:

`` `js
alerta ('Interface' [0] .toLowerCase ()); // 'Eu'
`` `

## Procurando por uma substring

Existem várias maneiras de procurar uma substring dentro de uma string.

### str.indexOf

O primeiro método é [str.indexOf (substr, pos)] (mdn: js / String / indexOf).

Ele procura o `substr` em` str`, a partir da posição dada `pos`, e retorna a posição onde a partida foi encontrada ou` -1` se nada puder ser encontrado.

Por exemplo:

`` `js run
Deixe str = 'Widget with id';

alerta (str.indexOf ('Widget')); // 0, porque 'Widget' é encontrado no início
alerta (str.indexOf ('widget')); // -1, não encontrado, a pesquisa diferencia maiúsculas de minúsculas

alerta (str.indexOf ("id")); // 1, "id" é encontrado na posição 1 (..widget with id)
`` `

O segundo parâmetro opcional permite pesquisar a partir da posição dada.

Por exemplo, a primeira ocorrência de `" id "` está na posição `1`. Para procurar a próxima ocorrência, vamos começar a busca na posição `2`:

`` `js run
Deixe str = 'Widget with id';

alerta (str.indexOf ('id', 2)) // 12
`` `


Se estamos interessados ​​em todas as ocorrências, podemos executar `indexOf` em um loop. Cada nova chamada é feita com a posição após a partida anterior:


`` `js run
Deixe str = "tão sly como uma raposa, tão forte como um boi";

deixe target = 'as'; // vamos procurá-lo

deixe pos = 0;
enquanto (verdadeiro) {
Deixe FoundPos = str.indexOf (target, pos);
se (foundPos == -1) quebrar;

alerta ('Encontrado em $ {foundPos} `);
pos = foundPos + 1; // continue a busca da próxima posição
}
`` `

O mesmo algoritmo pode ser definido mais curto:

`` `js run
Deixe str = "tão esperto como uma raposa, tão forte como um boi";
deixe target = "as";

*! *
deixe pos = -1;
enquanto ((pos = str.indexOf (target, pos + 1))! = -1) {
alerta (pos);
}
* /! *
`` `

`` `smart header =" `str.lastIndexOf (pos)` "
Existe também um método semelhante [str.lastIndexOf (pos)] (mdn: js / String / lastIndexOf) que busca do final de uma string para o seu início.

Ele listaria as ocorrências na ordem inversa.
`` `

Existe um ligeiro inconveniente com `indexOf` no teste` if`. Não podemos colocá-lo no `if` como este:

`` `js run
Deixe str = "Widget with id";

se (str.indexOf ("Widget")) {
alerta ("Encontramos"); // não funciona!
}
`` `

O `alert` no exemplo acima não mostra porque` str.indexOf ("Widget") `retorna` 0` (o que significa que encontrou a partida na posição inicial). Certo, mas `if` considera` 0` como "falso".

Então, devemos realmente verificar por "1", assim:

`` `js run
Deixe str = "Widget with id";

*! *
se (str.indexOf ("Widget")! = -1) {
* /! *
alerta ("Encontramos"); // funciona agora!
}
`` `

`` `` cabeçalho inteligente = "O truque NOT bit a bit"
Um dos truques antigos usados ​​aqui é o operador [bitwise NOT] (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators#Bitwise_NOT) `~`. Ele converte o número em um inteiro de 32 bits (remove a parte decimal se existir) e, em seguida, investe todos os bits na sua representação binária.

Para números inteiros de 32 bits, a chamada `~ n 'significa exatamente o mesmo que` - (n + 1) `(devido ao formato IEEE-754).

Por exemplo:

`` `js run
alerta (~ 2); // -3, o mesmo que - (2 + 1)
alerta (~ 1); // -2, o mesmo que - (1 + 1)
alerta (~ 0); // -1, o mesmo que - (0 + 1)
*! *
alerta (~ -1); // 0, o mesmo que - (- 1 + 1)
* /! *
`` `

Como podemos ver, `~ n` é zero somente se` n == -1`.

Então, o teste `if (~ str.indexOf (" ... ")) é verdade que o resultado de` indexOf` não é `-1`. Em outras palavras, quando há uma partida.

As pessoas usam isso para encurtar as verificações `indexOf`:

`` `js run
Deixe str = "Widget";

se (~ str.indexOf ("Widget")) {
alerta ('Found it!'); // trabalho
}
`` `

Geralmente, não é recomendável usar os recursos de linguagem de uma forma não óbvia, mas esse truque particular é amplamente utilizado em código antigo, então devemos entender isso.

Basta lembrar: `if (~ str.indexOf (...))` lê como "se encontrado".
`` ``

### includes, startsWith, endsWith

O método mais moderno [str.includes (substr, pos)] (mdn: js / String / includes) retorna `true / false` dependendo se` str` contém `substr` dentro.

É a escolha certa se precisarmos testar a partida, mas não precisamos de sua posição:

`` `js run
alerta ("Widget with id" .includes ("Widget")); // verdade


`` `

O segundo argumento opcional de `str.includes` é a posição para começar a pesquisar a partir de:

`` `js run
alerta ("Midget") inclui ("id")); // verdade
alerta ("Midget") inclui ("id", 3)); // falso, da posição 3 não há "id"
`` `

Os métodos [str.startsWith] (mdn: js / String / startsWith) e [str.endsWith] (mdn: js / String / endsWith) fazem exatamente o que eles dizem:

`` `js run
alerta ("Widget" .startsWith ("Wid")); // verdadeiro, "Widget" começa com "Wid"
alerta ("Widget" .endsWith ("get")); // verdadeiro, "Widget" termina com "get"
`` `

## Obtendo uma substring

Existem 3 métodos em JavaScript para obter uma substring: `substring`,` substr` e `slice`.

`str.slice (start [, end])`
: Retorna a parte da string do `start` para (mas não incluindo)` end`.

Por exemplo:

`` `js run
Deixe str = "stringify";
alerta (str.slice (0,5)); // 'string', então substring de 0 a 5 (não incluindo 5)
alerta (str.slice (0,1)); // 's', de 0 a 1, mas não incluindo 1, então apenas personagem em 0
`` `

Se não houver um segundo argumento, então `slice` vai até o final da string:

`` `js run
Deixe str = "st *! * ringify * /! *";
alerta (str.slice (2)); // ringify, da 2ª posição até o final
`` `

Valores negativos para `start / end` também são possíveis. Eles significam que a posição é contada a partir da extremidade da corda:

`` `js run
Deixe str = "strin *! * gif * /! * y";

// começa na 4ª posição da direita, termina na 1ª da direita
alerta (str.slice (-4, -1)); // gif
`` `


`str.substring (start [, end])`
: Retorna a parte da string * entre * `start` e` end`.

Isso é quase o mesmo que `slice`, mas permite que` start` seja maior que `end`.

Por exemplo:


`` `js run
Deixe str = "stringify";

// são iguais para substring
alerta (str.substring (2, 6)); // "toque"
alerta (str.substring (6, 2)); // "toque"

// ... mas não para fatia:
alerta (str.slice (2, 6)); // "anel" (o mesmo)
alerta (str.slice (6, 2)); // "" (uma string vazia)

`` `

Os argumentos negativos são (ao contrário da fatia) não suportados, são tratados como "0".


`str.substr (start [, length])`
: Retorna a parte da string de `start`, com o` length` fornecido.

Em contraste com os métodos anteriores, este nos permite especificar o `comprimento 'em vez da posição final:

`` `js run
Deixe str = "stringify";
alerta (str.substr (2, 4)); // toque, a partir da 2ª posição, obtenha 4 caracteres
`` `

O primeiro argumento pode ser negativo, para contar desde o final:

`` `js run
Deixe str = "strin *! * gi * /! * fy";
alerta (str.substr (-4, 2)); // gi, da 4ª posição, obtenha 2 caracteres
`` `

Vamos recapitular estes métodos para evitar qualquer confusão:

| método | seleciona ... | Negativos |
| -------- | ----------- | ----------- |
| `fatia (começo, fim)` | de `start` para` end` | permite negativos |
| `substring (start e end)` | entre `start` e` end` | valores negativos significam "0" |
| `substr (start, length)` | do `começo ', pegue os caracteres` length` | permite `start` negativo


`` `cabeçalho inteligente =" qual escolher? "
Todos eles podem fazer o trabalho. Formalmente, `substr` tem uma pequena desvantagem: é descrito não na especificação core JavaScript, mas no Anexo B, que abrange os recursos do navegador que existem principalmente por motivos históricos. Assim, ambientes que não sejam do navegador podem não suportá-lo. Mas, na prática, funciona em todos os lugares.

O autor encontra-se usando "fatia" quase que o tempo todo.
`` `

## Comparando strings

Como sabemos do capítulo <info: comparison>, as cordas são comparadas caracteres por caracteres em ordem alfabética.

Embora, haja algumas estranhezas.

1. Uma letra minúscula é sempre maior do que a maiúscula:

`` `js run
alerta ('a'> 'Z'); // verdade
`` `

2. As letras com marcas diacríticas estão "fora de ordem":

`` `js run
alerta ('Österreich'> 'Zelândia'); // verdade
`` `

Isso pode levar a resultados estranhos se classificarmos esses nomes de países. Normalmente, as pessoas esperariam que a "Zelândia" venha depois da "Österreich" na lista.

Para entender o que acontece, vamos rever a representação interna de strings em JavaScript.

Todas as seqüências de caracteres são codificadas usando [UTF-16] (https://en.wikipedia.org/wiki/UTF-16). Isto é: cada caractere tem um código numérico correspondente. Existem métodos especiais que permitem obter o personagem do código e de volta.

`str.codePointAt (pos)`
: Retorna o código para o personagem na posição `pos`:

`` `js run
// diferentes letras de caso têm códigos diferentes
alerta ("z" .codePointAt (0)); // 122
alerta ("Z" .codePointAt (0)); // 90
`` `

`String.fromCodePoint (código)`
: Cria um caractere por seu "código" numérico

`` `js run
alerta (String.fromCodePoint (90)); // Z
`` `

Nós também podemos adicionar caracteres unicode por seus códigos usando `\ u` seguido pelo código hexadecimal:

`` `js run
// 90 é 5a em sistema hexadecimal
alerta ('\ u005a'); // Z
`` `

Agora, vejamos os caracteres com os códigos `65..220` (o alfabeto latino e um pouco mais) fazendo uma série delas:

`` `js run
deixe str = '';

para (vamos i = 65; i <= 220; i ++) {
str + = String.fromCodePoint (i);
}
alerta (str);
// ABCDEFGHIJKLMNOPQRSTUVWXYZ [\] ^ _ `abcdefghijklmnopqrstuvwxyz {|} ~ ???????????
// ¡¢ £ ¤ ¥ |§¨ © ª «¬®¯ ° ± ²³'μ¶ · ¸¹º» ¼½¾¿ÀÁÂÃÄÅÆÇÈÉÊËÌÍÎÏÐÑÒÓÔÕÖ × ØÙÚÛÜ
`` `

Vejo? Os personagens capitais são primeiro, depois alguns personagens especiais e, em seguida, caracteres em minúsculas.

Agora, torna-se óbvio porque `a> Z`.

Os caracteres são comparados pelo seu código numérico. O código maior significa que o personagem é maior. O código para `a` (97) é maior do que o código para` Z` (90).

- Todas as letras minúsculas vão após letras maiúsculas porque seus códigos são maiores.
- Algumas letras como "Ö" aparecem do alfabeto principal. Aqui, o código é maior que qualquer coisa de `a` para` z`.


### Comparações corretas

O algoritmo "correto" para fazer comparações de cordas é mais complexo do que parece, porque os alfabetos são diferentes para diferentes idiomas. A mesma carta pode estar localizada de forma diferente em diferentes alfabetos.

Então, o navegador precisa conhecer o idioma para comparar.

Felizmente, todos os navegadores modernos (IE10- requerem a biblioteca adicional [Intl.JS] (https://github.com/andyearnshaw/Intl.js/)) que suportam o padrão de internacionalização [ECMA 402] (http: //www.ecma) -international.org/ecma-402/1.0/ECMA-402.pdf).

Ele fornece um método especial para comparar strings em diferentes idiomas, seguindo suas regras.

A chamada [str.localeCompare (str2)] (mdn: js / String / localeCompare):

- Retorna `1` se` str` for maior que `str2 'de acordo com as regras de idioma.
- Retorna `-1` se` str` for menor que `str2`.
- Retorna `0` se forem iguais.

Por exemplo:

`` `js run
alerta ('Österreich'.localeCompare (' Zealand ')); // -1
`` `

Este método tem dois argumentos adicionais especificados em [documentação] (mdn: js / String / localeCompare), que permitem especificar o idioma (por padrão, tirado do ambiente) e configurar regras adicionais como a sensibilidade do caso ou deve " "` e `" são tratados como os mesmos etc.

## Internals, Unicode

`` `warn header =" Advanced knowledge "
A seção aprofunda-se em cadeias internas. Este conhecimento será útil para você se você planeja lidar com emoji, matemática rara de personagens de hieróglifos ou outros símbolos raros.

Você pode ignorar a seção se você não planeja apoiá-los.
`` `

### Pares substitutivos

A maioria dos símbolos tem um código de 2 bytes. Letras na maioria das línguas europeias, números e até mesmo hieróglifos, têm uma representação de 2 bytes.

Mas 2 bytes apenas permitem 65536 combinações e isso não é suficiente para todos os símbolos possíveis. Símbolos tão raros são codificados com um par de caracteres de 2 bytes chamados de "par substituto".

O comprimento desses símbolos é `2`:

`` `js run
alerta ('𝒳'.length); // 2, MATEMÁTICA SCRIPT CAPITAL X
alerta ('😂'.length); // 2, FACE COM LARANJAS DE ALEGRIA
alerta ('𩷶' .length); // 2, um hieróglifo chinês raro
`` `

Observe que os pares de substituição não existiram no momento em que o JavaScript foi criado e, portanto, não são processados ​​corretamente pelo idioma!

Na verdade, temos um único símbolo em cada uma das strings acima, mas o `length` mostra um comprimento de` 2`.

`String.fromCodePoint` e` str.codePointAt` são poucos métodos raros que lidam com pares de substituição. Eles apareceram recentemente na língua. Antes deles, havia apenas [String.fromCharCode] (mdn: js / String / fromCharCode) e [str.charCodeAt] (mdn: js / String / charCodeAt). Esses métodos são realmente os mesmos que `fromCodePoint / codePointAt`, mas não funcionam com pares de substituição.

Mas, por exemplo, obter um símbolo pode ser complicado, porque os pares substituídos são tratados como dois caracteres:

`` `js run
alerta ('𝒳' [0]); // símbolos estranhos ...
alerta ('𝒳' [1]); // ... peças do par substituto
`` `

Observe que os pedaços do par substituto não têm significado sem o outro. Portanto, os alertas no exemplo acima exibem o lixo.

Tecnicamente, os pares de substituição também são detectáveis ​​por seus códigos: se um personagem tiver o código no intervalo de "0xd800..0xdbff", então é a primeira parte do par substituto. O próximo caractere (segunda parte) deve ter o código no intervalo `0xdc00..0xdfff`. Esses intervalos são reservados exclusivamente para pares de substituição pelo padrão.

No caso acima:

`` `js run
// charCodeAt não é compatível entre os parceiros, por isso dá códigos para peças

alerta ('𝒳'.charCodeAt (0) .toString (16)); // d835, entre 0xd800 e 0xdbff
alerta ('𝒳'.charCodeAt (1) .toString (16)); // dcb3, entre 0xdc00 e 0xdfff
`` `

Você encontrará mais maneiras de lidar com pares de substituição no capítulo <info: iterable>. Provavelmente há bibliotecas especiais para isso também, mas nada famoso o suficiente para sugerir aqui.

### Marcas diacríticas e normalização

Em muitos idiomas existem símbolos que são compostos do caractere de base com uma marca acima / abaixo dele.

Por exemplo, a letra `a` pode ser o caractere de base para:` àáâäãåā`. O caractere "composto" mais comum possui seu próprio código na tabela UTF-16. Mas nem todos eles, porque existem muitas combinações possíveis.

Para suportar composições arbitrárias, o UTF-16 nos permite usar vários caracteres unicode. O personagem base e um ou vários caracteres "marca" que o "decoram".

Por exemplo, se tivermos `S` seguido pelo caractere especial" ponto acima "(código` \ u0307`), ele é mostrado como Ṡ.

`` `js run
alerta ('S \ u0307'); // Ṡ
`` `

Se precisarmos de uma marca adicional acima da letra (ou abaixo dela) - não há problema, basta adicionar o caracter de marca necessário.

Por exemplo, se acrescentarmos um caractere "ponto abaixo" (código `\ u0323`), então teremos" S com pontos acima e abaixo ":` Ṩ`.

Por exemplo:

`` `js run
alerta ('S \ u0307 \ u0323'); // Ṩ
`` `

Isso proporciona grande flexibilidade, mas também um problema interessante: dois personagens podem parecer visualmente o mesmo, mas ser representados com diferentes composições unicode.

Por exemplo:

`` `js run
alerta ('S \ u0307 \ u0323'); // Ṩ, S + ponto acima + ponto abaixo
alerta ('S \ u0323 \ u0307'); // Ṩ, S + ponto abaixo + ponto acima

alerta ('S \ u0307 \ u0323' == 'S \ u0323 \ u0307'); // falso
`` `

Para resolver isso, existe um algoritmo de "normalização unicode" que traz cada string para a única forma "normal".

É implementado por [str.normalize ()] (mdn: js / String / normalize).

`` `js run
alerta ("S \ u0307 \ u0323" .normalize () == "S \ u0323 \ u0307" .normalize ()); // verdade
`` `

É divertido que, na nossa situação, `normalize ()` reúne uma sequência de 3 caracteres para um: `\ u1e68` (S com dois pontos).

`` `js run
alerta ("S \ u0307 \ u0323" .normalize (). length); // 1

alerta ("S \ u0307 \ u0323" .normalize () == "\ u1e68"); // verdade
`` `

Na realidade, nem sempre é esse o caso. O motivo é que o símbolo `Ṩ` é" comum o suficiente ", então os criadores UTF-16 o incluíram na tabela principal e lhe deram o código.

Se você quiser saber mais sobre regras e variantes de normalização - elas são descritas no apêndice do padrão Unicode: [Formulários de Normalização Unicode] (http://www.unicode.org/reports/tr15/), mas para a maioria das práticas os fins da informação desta seção são suficientes.


## Resumo

- Existem 3 tipos de cotações. Os backticks permitem que uma string abranja várias linhas e incorpore expressões.
- As strings em JavaScript são codificadas usando UTF-16.
- Podemos usar caracteres especiais como `\ n` e inserir letras pelo seu unicode usando` \ u ... `.
- Para obter um personagem, use: `[]`.
- Para obter uma substring, use: `slice` ou` substring`.
- Para letras minúsculas / maiúsculas, use: `toLowerCase / toUpperCase`.
- Para procurar uma substring, use: `indexOf`, ou` includes / startsWith / endsWith` para verificações simples.
- Para comparar strings de acordo com o idioma, use: `localeCompare`, caso contrário, eles são comparados por códigos de personagem.

Existem vários outros métodos úteis em strings:

- `str.trim ()` - remove ("trims") espaços do início e fim da string.
- `str.repeat (n)` - repete a string `n` vezes.
- ...e mais. Veja o [manual] (mdn: js / String) para obter detalhes.

As strings também possuem métodos para fazer pesquisa / substituição com expressões regulares. Mas esse tópico merece um capítulo separado, então retornaremos a isso mais tarde.
