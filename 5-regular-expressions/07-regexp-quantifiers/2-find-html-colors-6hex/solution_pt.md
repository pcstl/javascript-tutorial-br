Precisamos procurar `#` seguido de 6 caracteres hexadecimais.

Um caractere hexadecimal pode ser descrito como "padrão: [0-9a-fA-F]`. Ou se usarmos a bandeira `i`, então apenas ': [0-9a-f]`.

Então, podemos procurar 6 deles usando o padrão quantificador: {6} `.

Como resultado, temos o regexp: `pattern: / # [a-f0-9] {6} / gi`.

`` `js run
deixe reg = / # [a-f0-9] {6} / gi;

Deixe str = "cor: # 121212; cor de fundo: # AA00ef bad-colors: f # fddee # fd2"

alerta (str.match (reg)); // # 121212, # AA00ef
```

O problema é que ele encontra a cor em seqüências mais longas:

`` `js run
alerta ("# 12345678" .match (/ # [a-f0-9] {6} / gi)) // # 12345678
```

Para consertar isso, podemos adicionar `pattern: \ b` ao final:

`` `js run
// cor
alerta ("# 123456" .match (/ # [a-f0-9] {6} \ b / gi)); // # 123456

// não é uma cor
alerta ("# 12345678" .match (/ # [a-f0-9] {6} \ b / gi)); // nulo
```
