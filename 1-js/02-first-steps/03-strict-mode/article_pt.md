# O modo moderno, "use rigoroso"

Durante muito tempo, o JavaScript estava evoluindo sem problemas de compatibilidade. Novos recursos foram adicionados ao idioma, mas a funcionalidade antiga não mudou.

Isso teve o benefício de nunca quebrar o código existente. Mas a desvantagem foi que qualquer erro ou uma decisão imperfeita tomada pelos criadores de JavaScript ficaram presos na língua para sempre.

Foi assim até 2009, quando o ECMAScript 5 (ES5) apareceu. Ele adicionou novos recursos ao idioma e modificou alguns dos existentes. Para manter o código antigo funcionando, a maioria das modificações está desativada por padrão. É preciso habilitá-los explicitamente com uma diretiva especial `" use strict "`.

[cortar]

## "use strict"

A diretiva parece uma string: `" use strict "` ou `'use strict'`. Quando está localizado na parte superior do script, o script inteiro funciona da maneira "moderna".

Por exemplo

`` `js
"uso estrito";

// este código funciona da maneira moderna
...
`` `

Nós aprenderemos funções (uma maneira de agrupar comandos) em breve.

Olhando para a frente, vamos apenas notar que "usar" "rígido" pode ser colocado no início de uma função (a maioria dos tipos de funções) ao invés de todo o script. Em seguida, o modo estrito é habilitado somente nessa função. Mas geralmente as pessoas usam isso para todo o script.


`` `` warn header = "Certifique-se de que \" use strict \ "esteja no topo"
Certifique-se de que `" use strict "` esteja na parte superior do script, caso contrário, o modo estrito pode não estar ativado.

Não existe um modo estrito aqui:

`` `js no-strict
alerta ("algum código");
// "uso estrito" abaixo é ignorado, deve estar no topo

"uso estrito";

// modo estrito não está ativado
`` `

Somente os comentários podem aparecer acima `" use strict "`.
`` ``

`` `warn header =" Não há como cancelar `use strict`"
Não há nenhuma diretiva "sem uso estrito" ou similar, que retornaria o comportamento antigo.

Uma vez que entramos no modo estrito, não há retorno.
`` `

## Sempre "use estrito"

As diferenças de "usar estrito" versus o modo "padrão" ainda estão cobertas.

Nos próximos capítulos, à medida que aprendemos recursos de linguagem, vamos fazer anotações sobre as diferenças do modo estrito. Por sorte, não há muitos. E eles realmente tornam nossa vida melhor.

Neste ponto, é suficiente saber sobre isso em geral:

1. A diretiva `" use strict "` muda o motor para o modo "moderno", alterando o comportamento de alguns recursos internos. Veremos os detalhes enquanto estudamos.
2. O modo estrito é habilitado por `" use strict "` no topo. Além disso, existem vários recursos de linguagem como "classes" e "módulos" que permitem o modo estrito automaticamente.
3. O modo estrito é suportado por todos os navegadores modernos.
4. Recomenda-se sempre iniciar scripts com `" use strict "`. Todos os exemplos neste tutorial assumem assim, a menos que (muito raramente) especifique o contrário.
