# Seqüência de caracteres ^ e termine $

O padrão do tapete: padrão '^' `e dólar: os caracteres '$' 'têm significado especial em uma regex. Eles são chamados de "âncoras".

[cortar]

O padrão do tapete: ^ `coincide no início do texto e o dólar padrão: $` - no final.

Por exemplo, vamos testar se o texto começa com `Mary`:

`` `js run
Deixe str1 = 'Mary tinha um pouco de cordeiro, o velo era branco como a neve';
Deixe str2 = 'Em todo lugar que Mary foi, a lâmpada estava certa de ir';

alerta (/^Mary/.test(str1)); // verdade
alerta (/^Mary/.test(str2)); // falso
```

O padrão `padrão: ^ Mary` significa:" a corda começa e depois Maria ".

Agora vamos testar se o texto termina com um e-mail.

Para combinar um e-mail, podemos usar um padrão regexp `: [-. \ W] + @ ([\ w -] + \.) + [\ W -] {2,20}`. Não é perfeito, mas principalmente funciona.

Para testar se a cadeia termina com o email, vamos adicionar `pattern: $` ao padrão:

`` `js run
Deixe reg = /[-.\w]+@([\w-]+\.)+[\w-]{2,20}$g;

Deixe str1 = 'Meu e-mail é mail@site.com';
Deixe str2 = 'Em todo lugar que Mary foi, a lâmpada estava certa de ir';

alerta (reg.test (str1)); // verdade
alerta (reg.test (str2)); // falso
```

Podemos usar ambas as âncoras para verificar se a corda segue exatamente o padrão. Isso geralmente é usado para validação.

Por exemplo, queremos verificar que `str` é exatamente uma cor na forma` # `mais 6 dígitos hexadecimais. O padrão para a cor é `padrão: # [0-9a-f] {6}`.

Para verificar se a * string inteira * corresponde exatamente, adicionamos `pattern: ^ ... $`:

`` `js run
Deixe str = "#abcdef";

alerta (/^#[0-9a-f]{6}$/i.test(str)); // verdade
```

O motor regex procura o início do texto, então a cor e depois o texto termina. Apenas o que precisamos.

`` `cabeçalho inteligente =" âncoras têm zero comprimento "
As âncoras simples como `\ b` são testes. Eles têm zero de largura.

Em outras palavras, eles não combinam um personagem, mas sim forçam o motor regexp a verificar a condição (texto inicial / final).
```

O comportamento das âncoras muda se houver um padrão de bandeira: m` (modo multilinha). Nós exploraremos isso no próximo capítulo.
