Uma regex para procurar cor de 3 dígitos `# abc`:` padrão: / # [a-f0-9] {3} / i`.

Podemos adicionar exatamente mais 3 dígitos hexadecimais opcionais. Não precisamos de mais ou menos. Ou nós os temos ou nós não.

A maneira mais simples de adicioná-los - é anexar ao regex: `padrão: / # [a-f0-9] {3} ([a-f0-9] {3})? / I`

Podemos fazê-lo de forma mais inteligente através de: `padrão: / # ([a-f0-9] {3}) {1,2} / i`.

Aqui, o padrão regexp `: [a-f0-9] {3}` está entre parênteses para aplicar o padrão quantificador `` `{1,2}` como um todo.

Em ação:

`` `js run
deixe reg = / # ([a-f0-9] {3}) {1,2} / gi;

Deixe str = "cor: # 3f3; background-color: # AA00ef; e: #abcd";

alerta (str.match (reg)); // # 3f3 # AA0ef #abc
```

Há um problema menor aqui: o padrão encontrado `match: # abc` em` subject: # abcd`. Para evitar que possamos adicionar `pattern: \ b` ao final:

`` `js run
deixe reg = / # ([a-f0-9] {3}) {1,2} \ b / gi;

Deixe str = "cor: # 3f3; background-color: # AA00ef; e: #abcd";

alerta (str.match (reg)); // # 3f3 # AA0ef
```
