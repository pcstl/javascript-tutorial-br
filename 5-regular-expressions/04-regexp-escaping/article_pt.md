
# Escapando, caracteres especiais

Como já vimos, uma barra invertida `" \ "` é usada para denotar classes de personagens. Então é um personagem especial.

Há outros caracteres especiais também, que têm um significado especial em uma regex. Eles são usados ​​para fazer buscas mais poderosas.

Aqui está uma lista completa deles: `padrão: [\ ^ $. | ? * + () `.

Não tente lembrar disso - quando lidamos com cada um deles separadamente, você o conhecerá de forma automática de forma automática.

## Escapando

Para usar um caractere especial como um regular, antecipe-o com uma barra invertida.

Isso também é chamado de "escapar de um personagem".

Por exemplo, precisamos encontrar um padrão de ponto: '.' `. Em uma expressão regular, um ponto significa "qualquer personagem, exceto uma nova linha", então, se realmente queremos dizer "um ponto", vamos colocar uma barra invertida antes disso: `padrão: \.`.

`` `js run
alerta ("Capítulo 5.1" .match (/ \ d \. \ d /)); // 5.1
```

Parênteses também são caracteres especiais, então, se os queremos, devemos usar `pattern: \ (`. O exemplo abaixo procura uma string `" g () "`:

`` `js run
alerta ("function g ()". match (/ g \ (\) /)); // "g ()"
```

Se estamos procurando uma barra invertida `\`, então devemos dobrá-la:

`` `js run
alerta ("1 \ 2" .match (/ \\ /)); // '\'
```

## A slash

O símbolo slash `'/'` não é um caractere especial, mas em JavaScript é usado para abrir e fechar o regexp: `pattern: / ... pattern ... /`, então devemos escapar disso também.

Aqui está o que procura uma slash `'/'` parece:

`` `js run
alerta ("/".match(/\//)); // '/'
```

Por outro lado, as novas sintaxes `new RegExp` não requerem escapar dela:

`` `js run
alerta ("/".match(new RegExp (" / "))); // '/'
```

## new RegExp

Se estamos criando uma expressão regular com `new RegExp`, então precisamos fazer mais alguma escapatória.

Por exemplo, considere isso:

`` `js run
deixe reg = novo RegExp ("\ d \. \ d");

alerta ("Capítulo 5.1" .match (reg)); // nulo
```

Não funciona, mas por quê?

O motivo é a seqüência de regras de escape. Olhe aqui:

`` `js run
alerta ("\ d \. \ d"); // d.d
```

As barras invertidas são usadas para escapar dentro de uma string e caracteres especiais específicos de string como `\ n`. As citações "consumir" e interpretá-las, por exemplo:

- `\ n` - torna-se um caractere de nova linha,
- `\ u1234` - torna-se o caractere Unicode com esse código,
- ... E quando não há significado especial: como `\ d` ou` \ z`, a barra invertida é simplesmente removida.

Então, a chamada para 'novo RegExp' obtém uma string sem barras invertidas.

Para corrigi-lo, precisamos duplicar as barras invertidas, porque as citações transformam `\\` em `\`:

`` `js run
*!*
Deixe regStr = "\\ d \\. \\ d";
*/!*
alert (vivo); // \ d \. \ d (corrigir agora)

note reg = new RegExp (regStr);

alerta ("Capítulo 5.1" .match (reg)); // 5.1
```
