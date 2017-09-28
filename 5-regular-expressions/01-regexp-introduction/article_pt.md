# Padrões e sinalizadores

Expressões regulares é uma maneira poderosa de procurar e substituir dentro de uma string.

Em JavaScript, as expressões regulares são implementadas usando objetos de uma classe 'RegExp' incorporada e integradas com strings.

Observe que as expressões regulares variam entre as linguagens de programação. Neste tutorial, nos concentramos em JavaScript. Claro que há muito em comum, mas eles são um pouco diferentes em Perl, Ruby, PHP etc.

[cortar]

## Expressões regulares

Uma expressão regular (também "regexp", ou apenas "reg") consiste em um * padrão * e bandeiras * opcionais *.

Existem duas sintaxes para criar um objeto de expressão regular.

A sintaxe longa:

`` `js
regexp = novo RegExp ("padrão", "flags");
```

... E o curto, usando barras `" / "`:

`` `js
regex = / pattern /; // sem bandeiras
regexp = / pattern / gmi; // com bandeiras g, m e i (para ser coberto em breve)
```

Slashes `" / "` Diga ao JavaScript que estamos criando uma expressão regular. Eles desempenham o mesmo papel que as citações para as cordas.

## Uso

Para pesquisar dentro de uma string, podemos usar o método [search] (mdn: js / String / search).

Aqui está um exemplo:

`` `js run
Deixe str = "Eu amo o JavaScript!"; // pesquisará aqui

deixe regexp = / love /;
alerta (str.search (regexp)); // 2
```

O método `str.search` procura o padrão` pattern: / love / `e retorna a posição dentro da string. Como podemos imaginar, `pattern: / love /` é o padrão mais simples possível. O que faz é uma simples pesquisa de substring.

O código acima é o mesmo que:

`` `js run
Deixe str = "Eu amo o JavaScript!"; // pesquisará aqui

deixe substr = 'love';
alerta (str.search (substr)); // 2
```

Então, procurar por `pattern: / love /` é o mesmo que procurar por "love" `.

Mas é só por agora. Em breve, criaremos expressões regulares mais complexas com muita procura de mais poder.

`` `cabeçalho inteligente =" cores "
A partir daqui, o esquema de cores é:

- regex - `pattern: red`
- string (onde buscamos) - `subject: blue`
- resultado - `match: green`
```


`` `smart header =" Quando usar `new RegExp`?"
Normalmente usamos a sintaxe curta `/.../`. Mas não permite inserções de variáveis, por isso devemos conhecer a regexp exata no momento da redação do código.

Por outro lado, o `novo RegExp` permite construir um padrão dinamicamente a partir de uma string.

Então podemos descobrir o que precisamos procurar e criar 'novo RegExp' dele:

`` `js run
Deixe search = prompt ("O que você deseja pesquisar?", "love");
deixe regexp = novo RegExp (pesquisa);

// encontre o que o usuário quiser
alerta ("Eu amo JavaScript" .search (regexp));
```
````


## Bandeiras

Expressões regulares podem ter sinalizadores que afetam a pesquisa.

Existem apenas 5 deles em JavaScript:

`eu '
: Com este sinalizador, a pesquisa é sensível a maiúsculas e minúsculas: nenhuma diferença entre `A` e` a` (veja o exemplo abaixo).

`g`
: Com esta bandeira, a busca procura todas as correspondências, sem ela - apenas a primeira (veremos os usos no próximo capítulo).

`m`
: Modo multilinha (coberto no capítulo <info: regex-multiline>).

"você"
: Ativa o suporte completo do Unicode. A bandeira permite o processamento correto de pares de substituição. Mais sobre isso no capítulo <info: regexp-unicode>.

e
: Modo pegajoso (coberto no [próximo capítulo] (info: regexp-methods # y-flag))


## A bandeira "i"

A bandeira mais simples é `i`.

Um exemplo com ele:

`` `js run
Deixe str = "Eu amo o JavaScript!";

alerta (str.search (/ LOVE /)); // -1 (não encontrado)
alerta (str.search (/ LOVE / i)); // 2
```

1. A primeira pesquisa retorna `-1` (não encontrado), porque a pesquisa é sensível a maiúsculas e minúsculas por padrão.
2. Com a bandeira `padrão: / LOVE / i` a pesquisa encontrou` match: love` na posição 2.

Então, a bandeira `i` já faz expressões regulares mais poderosas do que uma simples pesquisa de substring. Mas há muito mais. Vamos cobrir outras bandeiras e recursos nos próximos capítulos.


## Resumo

- Uma expressão regular consiste em um padrão e bandeiras opcionais: `g`,` i`, `m`,` u`, `y`.
- Sem bandeiras e símbolos especiais que estudaremos mais tarde, a busca por uma regexp é a mesma que uma pesquisa de substring.
- O método `str.search (regexp)` retorna o índice onde a partida é encontrada ou `-1` se não houver correspondência.
