# Métodos de RegEx e String

Existem dois conjuntos de métodos para lidar com expressões regulares.

1. Primeiro, as expressões regulares são objetos da classe incorporada [RegExp] (mdn: js / RegEx), que fornece muitos métodos.
2. Além disso, existem métodos em cadeias regulares que podem funcionar com a regex.

A estrutura é um pouco desarrumada, então primeiro consideramos métodos separadamente, e depois - receitas práticas para tarefas comuns.

[cortar]

## str.search (reg)

Já vimos esse método. Retorna a posição da primeira partida ou `-1` se nenhuma encontrada:

`` `js run
Deixe str = "Uma gota de tinta pode fazer um milhão pensar";

alerta (str.search (*! * / a / i * /! *)); // 0 (a primeira posição)
```

** A limitação importante: `search` sempre procura a primeira partida. **

Não podemos encontrar as próximas posições usando `search`, não há nenhuma sintaxe para isso. Mas existem outros métodos que podem.

## str.match (reg), nenhum sinalizador "g"

O comportamento do método `str.match` varia de acordo com o sinalizador` g`. Primeiro vamos ver o caso sem ele.

Então `str.match (reg)` procura apenas a primeira partida.

O resultado é uma matriz com essa combinação e propriedades adicionais:

- `index` - a posição da partida dentro da string,
- `input` - a string do assunto.

Por exemplo:

`` `js run
Deixe str = "Fama é a sede da juventude";

Deixe o resultado = str.match (*! * / fama / i * /! *);

alerta (resultado [0]); // Fama (a partida)
alerta (result.index); // 0 (na posição zero)
alerta (resultado.input); // "Fama é a sede da juventude" (a corda)
```

A matriz pode ter mais de um elemento.

** Se uma parte do padrão é delimitada por parênteses `(...)`, então ele se torna um elemento separado da matriz. **

Por exemplo:

`` `js run
Deixe str = "JavaScript é uma linguagem de programação";

Deixe resultado = str.match (*! * / JAVA (SCRIPT) / i * /! *);

alerta (resultado [0]); // JavaScript (toda a partida)
alerta (resultado [1]); // script (a parte da correspondência que corresponde aos parênteses)
alerta (result.index); // 0
alerta (resultado.input); // JavaScript é uma linguagem de programação
```

Devido ao indicador `i`, a pesquisa não é sensível a maiúsculas e minúsculas, então ele encontra` match: JavaScript`. A parte da partida que corresponde a `pattern: SCRIPT` torna-se um item de matriz separado.

Voltaremos a parênteses mais adiante no capítulo <info: regexp-groups>. Eles são ótimos para pesquisa e substituição.

## str.match (reg) com o sinalizador "g"

Quando há um sinalizador `" g "`, então `str.match` retorna uma matriz de todas as correspondências. Não há propriedades adicionais nessa matriz, e os parênteses não criam nenhum elemento.

Por exemplo:

`` `js run
Deixe str = "HO-Ho-ho!";

Deixe o resultado = str.match (*! * / ho / ig * /! *);

alerta (resultado); // HO, Ho, ho (todas as correspondências, sem maiúsculas e minúsculas)
```

Com parênteses, nada muda, aqui vamos:

`` `js run
Deixe str = "HO-Ho-ho!";

let result = str.match( *!*/h(o)/ig*/!* );

alerta (resultado); // HO, Ho, ho
```

Então, com a bandeira `g`, o` resultado 'é uma matriz simples de correspondências. Sem propriedades adicionais.

Se quisermos obter informações sobre posições de correspondência e usar parênteses, então devemos usar o método [RegExp # exec] (mdn: js / RegExp / exec) que iremos abordar abaixo.

`` `'' cabeçalho de aviso =" Se não houver correspondências, a chamada para 'corresponder' retorna 'nulo' "
Por favor, note que isso é importante. Se não houver correspondências, o resultado não é uma matriz vazia, mas "nulo".

Tenha isso em mente para evadir armadilhas como esta:

`` `js run
Deixe str = "Hey-hey-hey!";

alerta (str.match (/ ho / gi) .length); // erro! não há comprimento de nulo
```
````

## str.split (regexp | substr, limit)

Divide a string usando o regex (ou uma substring) como um delimitador.

Nós já usamos `split` com strings, assim:

`` `js run
alerta ('12 -34-56 '.split (' - ')) // [12, 34, 56]
```

Mas também podemos transmitir uma expressão regular:

`` `js run
alerta ('12 -34-56'.split (/ - /)) // [12, 34, 56]
```

## str.replace (str | reg, str | func)

A faca do exército suíço para busca e substituição em cordas.

O uso mais simples - pesquisa e substitua uma substring, como esta:

`` `js run
// substitua um dash por um dois pontos
alerta ('12 -34-56 '.replace ("-", ":")) // 12: 34-56
```

Quando o primeiro argumento de `replace` é uma string, ele só procura a primeira partida.

Para encontrar todos os traços, precisamos usar não a string `" - "`, mas um padrão regex `: / - / g`, com um sinalizador obrigatório` g`

`` `js run
// substituir todos os traços por dois pontos
alerta ('12 -34-56'.replace (*! * / - / g * /! *, ":")) // 12:34:56
```

O segundo argumento é uma string de substituição.

Podemos usar caracteres especiais nela:

| Símbolo | Inserções |
|--------|--------|
|`$$`|`"$"` |
| `$ &` | o jogo inteiro |
| <code> $ `</ code> | uma parte da string antes da partida |
| `$ '` | uma parte da string após a partida |
| `$ n` | if` n` é um número de 1-2 dígitos, então significa o conteúdo de n-th parênteses contando a esquerda para a direita |

Por exemplo, vamos usar `$ &` para substituir todas as entradas de `" John "` por `" Mr.John "`:

`` `js run
Deixe str = "John Doe, John Smith e John Bull".

// por cada John - substitua-o pelo Sr. e então por John
alerta (str.replace (/ John / g, 'Mr. $ &'));
// "Sr. John Doe, Sr. John Smith e Sr. John Bull.";
```

Os parênteses são muito frequentemente usados ​​junto com `$ 1`,` $ 2`, assim:

`` `js run
Deixe str = "John Smith";

alerta (str.replace (/ (John) (Smith) /, '$ 2, $ 1')) // Smith, John
```

** Para situações que requerem substituições "inteligentes", o segundo argumento pode ser uma função. **

Será chamado para cada partida, e seu resultado será inserido como uma substituição.

Por exemplo:

`` `js run
deixe i = 0;

// substitua cada "ho" pelo resultado da função
alerta ("HO-Ho-ho" .replace (/ ho / gi, function () {
return ++ i;
})); // 1-2-3
```

No exemplo acima, a função retorna o próximo número sempre, mas geralmente o resultado é baseado na partida.

A função é chamada com argumentos `func (str, p1, p2, ..., pn, offset, s)`:

1. `str` - a partida,
2. `p1, p2, ..., pn` - conteúdo dos parênteses (se houver),
3. `offset` - posição do jogo,
4. `s` - a string de origem.

Se não houver parênteses no regexp, a função sempre tem 3 argumentos: `func (str, offset, s)`.

Vamos usá-lo para mostrar informações completas sobre partidas:

`` `js run
// mostra e substitui todas as partidas
substituto da função (str, offset, s) {
alerta (`Found $ {str} na posição $ {offset} na string $ {s}`);
retornar str.toLowerCase ();
}

Deixe o resultado = "HO-Ho-ho" .replace (/ ho / gi, substituto);
alerta ('Resultado:' + resultado); // Resultado: ho-ho-ho

// mostra cada partida:
// encontrou HO na posição 0 na cadeia HO-Ho-ho
// Encontrado Ho na posição 3 na corda HO-Ho-ho
// Encontrado na posição 6 na corda HO-Ho-ho
```

No exemplo abaixo, existem dois parênteses, então `` substituto 'é chamado com 5 argumentos: `str` é a partida completa, então parênteses, e depois` offset` e `s`:

`` `js run
substituição da função (str, nome, sobrenome, offset, s) {
// nome é o primeiro parêntese, o sobrenome é o segundo
retornar sobrenome + "," + nome;
}

Deixe str = "John Smith";

alerta (str.replace (/ (John) (Smith) /, substituto)) // Smith, John
```

Usar uma função nos dá o poder de substituição final, pois obtém todas as informações sobre a partida, tem acesso a variáveis ​​externas e pode fazer tudo.

## regexp.test (str)

Vamos passar para os métodos da classe `RegExp`, que são chamativos nos próprios regexps.

O método `test` procura qualquer correspondência e retorna` true / false` se ele encontrou.

Então, é basicamente o mesmo que `str.search (reg)! = -1`, por exemplo:

`` `js run
Deixe str = "Eu amo o JavaScript";

// estes dois testes fazem o mesmo
alerta (*! * / love / i * /! *. test (str)); // verdade
alerta (str.search (*! * / love / i * /! *)! = -1); // verdade
```

Um exemplo com a resposta negativa:

`` `js run
Deixe str = "Bla-bla-bla";

alerta (*! * / love / i * /! *. test (str)); // falso
alerta (str.search (*! * / love / i * /! *)! = -1); // falso
```

## regexp.exec (str)

Já vimos esses métodos de busca:

- `search` - procura a posição da partida,
- `match` - se não houver nenhuma bandeira` g`, retorna a primeira partida com parênteses,
- `match` - se houver uma bandeira` g` - retorna todas as correspondências, sem separar parênteses.

O método `regexp.exec` é um pouco mais difícil de usar, mas permite pesquisar todas as correspondências com parênteses e posições.

Ele se comporta de forma diferente, dependendo se o regexp tem o sinalizador `g`.

- Se não houver `g`, então` regexp.exec (str) `retorna a primeira correspondência, exatamente como` str.match (reg) `.
- Se houver `g`, então` regexp.exec (str) `retorna a primeira partida e * lembra * a posição após ela na propriedade` regexp.lastIndex`. A próxima chamada começa a pesquisar a partir do `regexp.lastIndex` e retorna a próxima partida. Se não houver mais correspondências, então `regexp.exec` retorna` null` e `regexp.lastIndex` está definido para` 0`.

Como podemos ver, o método não nos dá nada se o usarmos sem a bandeira `g`, porque` str.match` faz exatamente o mesmo.

Mas a bandeira `g` permite obter todas as correspondências com suas posições e grupos de parênteses.

Aqui está o exemplo de como as chamadas `regex.exec` subseqüentes retornam correspondem uma a uma:

`` `js run
Deixe str = "Muito sobre o JavaScript em https://javascript.info";

deixe regexp = / JAVA (SCRIPT) / ig;

*!*
// Procure a primeira partida
*/!*
___ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _.え ぇ c (str);
Oh rt (___ 0 ___ 0)); // ___ ___ ___ ___ 0
Oh, sim (1) "; // sc
alerta (matchOne.index); // 12 (a posição da partida)
alerta (matchOne.input); // o mesmo que str

alerta (regexp.lastIndex); // 22 (a posição após a partida)

*!*
// Procure a segunda partida
*/!*
Deixe matchTwo = regexp.exec (str); // continue procurando de regexp.lastIndex
alerta (matchTwo [0]); // javascript
alerta (matchTwo [1]); // roteiro
alerta (matchTwo.index); // 34 (a posição da partida)
alerta (matchTwo.input); // o mesmo que str

alerta (regexp.lastIndex); // 44 (a posição após a partida)

*!*
// Procure a terceira partida
*/!*
deixe matchThree = regexp.exec (str); // continue procurando de regexp.lastIndex
alerta (matchThree); // nulo (sem correspondência)

alerta (regexp.lastIndex); // 0 (reiniciar)
```

Como podemos ver, cada chamada `regexp.exec` retorna a correspondência em um" formato completo ": como uma matriz com parênteses, propriedades de" índice "e" entrada ".

O principal caso de uso para `regex.exec` é encontrar todas as correspondências em um loop:

`` `js run
Deixe str = 'Muito sobre JavaScript em https://javascript.info';

deixe regexp = / javascript / ig;

Deixe o resultado;

enquanto (resultado = regexp.exec (str)) {
alerta ('Encontrado $ {resultado [0]} em $ {result.index} `);
}
```

O loop continua até `regexp.exec` retornar` null` que significa "não mais combina".

`` `` cabeçalho inteligente = "Pesquisar a partir da posição dada"
Podemos forçar `regex.exec` a começar a procurar a partir da posição determinada definindo` last Index` manualmente:

`` `js run
Deixe str = 'Muito sobre JavaScript em https://javascript.info';

deixe regexp = / javascript / ig;
regexp.lastIndex = 30;

alerta (regexp.exec (str) .index); // 34, a pesquisa começa a partir da 30ª posição
```
````

## A bandeira "y" [# y-flag]

O sinalizador `y` significa que a pesquisa deve encontrar uma correspondência exatamente na posição especificada pela propriedade` regexp.lastIndex` e somente aí.

Em outras palavras, normalmente a pesquisa é feita em toda a cadeia: `pattern: / javascript /` procura por "javascript" em toda a cadeia.

Mas quando um regexp tem o sinalizador 'y`, então ele só procura a correspondência na posição especificada em `regexp.lastIndex` (` 0` por padrão).

Por exemplo:

`` `js run
Deixe str = "Eu amo o JavaScript!";

deixe reg = / javascript / iy;

alerta (reg.lastIndex); // 0 (padrão)
alerta (str.match (reg)); // nulo, não encontrado na posição 0

reg.lastIndex = 7;
alerta (str.match (reg)); // JavaScript (certo, essa palavra começa na posição 7)

// para qualquer outro reg.lastIndex, o resultado é nulo
```

O padrão regexp `: / javascript / iy` só pode ser encontrado se configurarmos` reg.lastIndex = 7`, porque devido ao sinalizador 'y`, o motor tenta apenas encontrá-lo no único local dentro de uma string - a partir do posição `reg.lastIndex`.

Então, qual é o objetivo? Onde aplicamos isso?

A razão é a performance.

O sinalizador `y` funciona muito bem para analisadores - programas que precisam" ler "o texto e construir estrutura de sintaxe na memória ou executar ações dele. Para isso, movemos o texto e aplicamos expressões regulares para ver o que temos a seguir: uma string? Um número? Algo mais?

A bandeira `y` permite aplicar uma expressão regular (ou muitos deles um a um) exatamente na posição determinada e quando entendemos o que está lá, podemos seguir em frente, passo a passo examinando o texto.

Sem a bandeira, o motor regexp sempre procura até o final do texto, o que leva tempo, especialmente se o texto for grande. Portanto, nosso analisador seria muito lento. A bandeira `y` é exatamente a coisa certa aqui.

## Resumo, receitas

Os métodos tornam-se muito mais fáceis de entender se os separarmos pelo seu uso em tarefas da vida real.

Para procurar apenas a primeira partida:
: - Encontre a posição da primeira partida - `str.search (reg)`.
- Encontre a partida completa - `str.match (reg)`.
- Verifique se há uma partida - `regexp.test (str)`.
- Encontre a correspondência a partir da posição dada - `regexp.exec (str)`, configure `regexp.lastIndex` para posicionar.

Para procurar todas as partidas:
: - Uma matriz de correspondências - `str.match (reg)`, o regexp com o sinalizador `g`.
- Obter todas as combinações com informações completas sobre cada um - `regexp.exec (str)` com o sinalizador `g` no loop.

Para pesquisar e substituir:
: - Substitua por outra string ou resultado de função - `str.replace (reg, str | func)`

Para dividir a string:
: - `str.split (str | reg)`

Nós também cobrimos duas bandeiras:

- O sinalizador `g` para encontrar todas as correspondências (pesquisa global),
- O sinalizador 'y` para procurar exatamente a posição dada dentro do texto.

Agora, conhecemos os métodos e podemos usar expressões regulares. Mas precisamos aprender sua sintaxe, então vamos seguir em frente.
