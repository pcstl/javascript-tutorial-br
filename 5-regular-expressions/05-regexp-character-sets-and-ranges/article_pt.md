# Conjuntos e intervalos [...]

Vários caracteres ou classes de caracteres dentro de colchetes "[...]" significam "procurar qualquer caractere entre os dados".

[cortar]

## Sets

Por exemplo, `pattern: [eao]` significa qualquer um dos 3 caracteres: `'''''''''e'` ou` 'o'`.

Isso é chamado de * set *. Os conjuntos podem ser usados ​​em um regex juntamente com caracteres comuns:

`` `js run
// encontrar [t ou m], e depois "op"
alerta ("Mop top" .match (/ [tm] op / gi)); // "Mop", "top"
```

Tenha em atenção que, embora existam vários caracteres no conjunto, eles correspondem exatamente a um personagem na partida.

Assim, o exemplo acima não dá nenhuma correspondência:

`` `js run
// encontrar "V", então [o ou i], então "la"
alerta ("Voila" .match (/ V [oi] la /)); // nulo, sem correspondências
```

O padrão assume:

- `padrão: V`,
- então * um * das letras `padrão: [oi]`,
- então `pattern: la`.

Então haveria uma partida para `match: Vola` ou` match: Vila`.

## Gamas

Os colchetes também podem conter * intervalos de caracteres *.

Por exemplo, `padrão: [a-z]` é um caractere no intervalo de `a` para` z`, e `pattern: [0-5]` é um dígito de `0` para` 5`.

No exemplo abaixo, estamos procurando por "x" seguido de dois dígitos ou letras de `A` para` F`:

`` `js run
alerta ("Exception 0xAF" .match (/ x [0-9A-F] [0-9A-F] / g)); // xAF
```

Por favor, note que, na palavra "assunto: Exceção", existe uma subcadeia `assunto: xce`. Não corresponde ao padrão, porque as letras são minúsculas, enquanto no padrão set `: [0-9A-F]` são maiúsculas.

Se quisermos encontrá-lo também, podemos adicionar um intervalo `a-f`:` padrão: [0-9A-Fa-f] `. A bandeira `i` também permitiria letras minúsculas.

** As classes de caracteres são abreviaturas para certos conjuntos de caracteres. **

Por exemplo:

- ** \ d ** - é o mesmo que `pattern: [0-9]`,
- ** \ w ** - é o mesmo que `pattern: [a-zA-Z0-9_]`,
- ** \ s ** - é o mesmo que `pattern: [\ t \ n \ v \ f \ r]` mais alguns outros caracteres de espaço unicode.

Podemos usar classes de personagens dentro de [...] `também.

Por exemplo, queremos combinar todos os caracteres verbais ou um traço, para palavras como "vigésimo terceiro". Não podemos fazê-lo com `pattern: \ w +`, porque a classe `pattern: \ w` não inclui um dash. Mas podemos usar o `pattern: [\ w-]`.

Também podemos usar uma combinação de classes para cobrir todos os personagens possíveis, como 'padrão: [\ s \ S] `. Isso combina espaços ou não-espaços - qualquer personagem. Isso é mais largo do que um ponto `". "`, Porque o ponto coincide com qualquer caractere exceto uma nova linha.

## Excluindo intervalos

Além dos intervalos normais, existem intervalos "excluídos" que se parecem com o padrão: [^ ...] `.

Eles são denotados por um personagem de caixa '^' no início e combinam qualquer personagem *, exceto os dados *.

Por exemplo:

- `pattern: [^ aeyo]` - qualquer caractere exceto `'a'`,`' e'`, `'y'` ou`' o'`.
- `pattern: [^ 0-9]` - qualquer caractere, exceto um dígito, o mesmo que `\ D`.
- `padrão: [^ \ s]` - qualquer caractere não-espacial, o mesmo que `\ S`.

O exemplo abaixo procura por qualquer caractere, exceto letras, dígitos e espaços:

`` `js run
alert( "alice15@gmail.com".match(/[^\d\sA-Z]/gi) ); // @ and .
```

## Não escapar em [...]

Geralmente, quando queremos encontrar exatamente o caractere de ponto, precisamos escapar como "padrão: \.`. E se precisamos de uma barra invertida, usamos o `pattern: \\`.

Entre colchetes, a grande maioria dos caracteres especiais pode ser usada sem escapar:

- Um padrão de ponto: '.' `.
- Um padrão plus `: '+'`.
- Padrão dos parênteses: '()' `.
- Dash `padrão: '-'` no início ou no final (onde não define um intervalo).
- Um padrão de tapete: '^' 'se não no início (onde significa exclusão).
- E o suporte do quadrado de abertura `pattern: '['`.

Em outras palavras, todos os caracteres especiais são permitidos, exceto quando eles significam algo para colchetes.

Um ponto `". "'Dentro de colchetes significa apenas um ponto. O padrão padrão: [.,] `Procuraria um dos caracteres: um ponto ou uma vírgula.

No exemplo abaixo, o padrão regex `: [- (). ^ +]` Procura um dos caracteres `- (). ^ +`:

`` `js run
// Não é necessário escapar
Deixe reg = /[-(). ^+]/g;

alerta ("1 + 2 - 3" .match (reg)); // Corresponde +, -
```

... Mas se você decidir escapar deles "apenas no caso", então não haveria danos:

`` `js run
// Escapou tudo
Deixe reg = /[\-\(\)\.\^\+]/g;

alerta ("1 + 2 - 3" .match (reg)); // também funciona: +, -
```
