# Quantificadores +, *,? e n}

Digamos que temos uma seqüência de caracteres como `+7 (903) -123-45-67` e queremos encontrar todos os números nela. Mas, ao contrário de antes, estamos interessados ​​em não dígitos, mas números completos: `7, 903, 123, 45, 67`.

Um número é uma seqüência de 1 ou mais dígitos `\ d`. O instrumento para dizer o quanto precisamos é chamado de * quantificadores *.

## Quantidade {n}

O quantificador mais óbvio é um número em citações de figuras: `pattern: {n}`. Um quantificador é colocado após um personagem (ou uma classe de personagem e assim por diante) e especifica exatamente quanto nós precisamos.

Também tem formas avançadas, aqui vamos com exemplos:

Contagem exata: `{5}`
: `padrão: \ d {5}` denota exatamente 5 dígitos, o mesmo que `padrão: \ d \ d \ d \ d \ d`.

O exemplo abaixo procura um número de 5 dígitos:

`` `js run
alerta ("eu tenho 12345 anos" .match (/ \ d {5} /)); // "12345"
```

Podemos adicionar `\ b` para excluir números mais longos:` padrão: \ b \ d {5} \ b`.

A contagem de-para: `{3,5}`
: Para encontrar números de 3 a 5 dígitos, podemos colocar os limites em parênteses de figura: `padrão: \ d {3,5}`

`` `js run
alerta ("Eu não tenho 12 anos, mas 1234 anos" .match (/ \ d {3,5} /)); // "1234"
```

Podemos omitir o limite superior. Em seguida, um padrão regexp `: \ d {3,}` procura por números de `3 e mais dígitos:

`` `js run
alerta ("Eu não tenho 12 anos, mas 345678 anos" .match (/ \ d {3,} /)); // "345678"
```

No caso da seqüência de caracteres `+7 (903) -123-45-67, precisamos de números: um ou mais dígitos em uma linha. Isso é `padrão: \ d {1,}`:

`` `js run
deixe str = "+7 (903) -123-45-67";

Deixe números = str.match (/ \ d {1,} / g);

alerta (números); // 7,903,123,45,67
```

## Shorthands

Os quantificadores mais frequentemente necessários têm shorthands:

`+`
: Significa "um ou mais", o mesmo que `{1,}`.

Por exemplo, `padrão: \ d +` procura por números:

`` `js run
deixe str = "+7 (903) -123-45-67";

alerta (str.match (/ \ d + / g)); // 7,903,123,45,67
```

`?`
: Significa "zero ou one", o mesmo que `{0,1}`. Em outras palavras, torna o símbolo opcional.

Por exemplo, o padrão `pattern: ou? R` procura por 'match: o` seguido de zero ou de um` match: u` e, em seguida, `match: r`.

Então, pode encontrar `match: or` na palavra` subject: color` e `match: our` in` subject: colour`:

`` `js run
Deixe str = "Devo escrever cor ou cor?";

alerta (str.match (/ color? r / g)); // cor, cor
```

`*`
: Significa "zero ou mais", o mesmo que `{0,}`. Ou seja, o personagem pode repetir qualquer momento ou estar ausente.

O exemplo abaixo procura um dígito seguido de qualquer número de zeros:

`` `js run
alerta ("100 10 1" .match (/ \ d0 * / g)); // 100, 10, 1
```

Compare com `'+'` (um ou mais):

`` `js run
alerta ("100 10 1" .match (/ \ d0 + / g)); // 100, 10
```

## Mais exemplos

Quantificadores são usados ​​com muita frequência. Eles são um dos principais "blocos de construção" para expressões regulares complexas, então vejamos mais exemplos.

Regex "fração decimal" (um número com um ponto flutuante): `padrão: \ d + \. \ D +`
: Em ação:
`` `js run
alerta ("0 1 12.345 7890" .match (/ \ d + \. \ d + / g)); // 12.345
```

Regex "abre HTML-tag sem atributos", como `<span>` ou `<p>`: `padrão: / <[a-z] +> / i`
: Em ação:

`` `js run
alerta ("<body> ... </ body>". match (/ <[a-z] +> / gi)); // <body>
```

Procuramos um padrão de personagem: '<' `seguido de uma ou mais letras inglesas, e depois` pattern: '>' `.

Regex "abre HTML-tag sem atributos" (melhorado): `padrão: / <[a za-z0-9] *> / i`
: Melhor regex: de acordo com o padrão, o nome da tag HTML pode ter um dígito em qualquer posição, exceto o primeiro, como `<h1>`.

`` `js run
alerta ("<h1> Olá! </ h1>". corresponda (/ <[a-z] [a-z0-9] *> / gi)); // <h1>
```

Regexp "abrir ou fechar HTML-tag sem atributos": `padrão: / <\ /? [A-z] [a-z0-9] *> / i`
: Nós adicionamos um padrão slash opcional: /? `Antes da tag. Tive que escapar com uma barra invertida, caso contrário, o JavaScript pensaria que é o fim do padrão.

`` `js run
alerta ("<h1> Olá! </ h1>". corresponda (/ <\ /? [a-z] [a-z0-9] *> / gi)); // <h1>, </ h1>
```

`` `cabeçalho inteligente =" Mais preciso significa mais complexo "
Podemos ver uma regra comum nestes exemplos: quanto mais precisa é a expressão regular - mais longa e mais complexa é.

Por exemplo, as tags HTML podem usar uma regex mais simples: `pattern: <\ w +>`.

Porque `pattern: \ w 'significa qualquer letra em inglês ou um dígito ou`' _'`, o regexp também corresponde a não-tags, por exemplo `match: <_>`. Mas é muito mais simples que "padrão: <[a-z] [a-z0-9] *>`.

Estamos bem com `pattern: <\ w +>` ou precisamos de `pattern: <[a-z] [a-z0-9] *>`?

Na vida real, ambas as variantes são aceitáveis. Depende de quão tolerante possamos ser para fósforos "extras" e se é difícil ou não filtrá-los por outros meios.
```
