# Alternação (OR) |

A alternância é o termo em expressão regular que é realmente um "OR" simples.

Em uma expressão regular é denotada com um caractere de linha vertical `padrão: |`.

[cortar]

Por exemplo, precisamos encontrar linguagens de programação: HTML, PHP, Java ou JavaScript.

O regex correspondente: `padrão: html | php | javascript)?`.

Um exemplo de uso:

`` `js run
Deixe reg = / html | php | css | java (script)? / gi;

Deixe str = "Primeiro HTML apareceu, então CSS e, em seguida, JavaScript";

alerta (str.match (reg)); // 'HTML', 'CSS', 'JavaScript'
```

Já conhecemos uma coisa semelhante - colchetes. Eles permitem escolher entre vários caracteres, por exemplo `padrão: gr [ae] y` corresponde` match: grey` ou `match: grey`.

A alteração funciona não em um nível de personagem, mas no nível de expressão. Um padrão regex `: A | B | C 'significa uma das expressões` A`, `B` ou` C`.

Por exemplo:

- `padrão: gr (a | e) y 'significa exatamente o mesmo que` pattern: gr [ae] y`.
- `pattern: gra | ey` significa" gra "ou" ey ".

Para separar uma parte do padrão de alternância, geralmente o encerramos entre parênteses, como esta: `pattern: before (XXX | YYY) after`.

## Regex por tempo

Nos capítulos anteriores, havia uma tarefa para criar um regex para o tempo de busca na forma `hh: mm`, por exemplo` 12: 00`. Mas um simples padrão: \ d \ d: \ d \ d é muito vago. Aceita "25: 99" como o tempo.

Como podemos fazer um melhor?

Podemos aplicar uma correspondência mais cuidadosa:

- O primeiro dígito deve ser "0" ou "1" seguido de qualquer dígito.
- Ou `2 'seguido de` padrão: [0-3] `

Como um regexp: `padrão: [01] \ d | 2 [0-3]`.

Então podemos adicionar um dois pontos e a parte dos minutos.

Os minutos devem ser de `0 'para' 59 ', na linguagem regex que significa o primeiro dígito` padrão: [0-5] `seguido de qualquer outro dígito` \ d`.

Vamos colá-los juntos no padrão: `padrão: [01] \ d | 2 [0-3]: [0-5] \ d`.

Estamos quase concluídos, mas há um problema. A alternância `|` está entre o `padrão: [01] \ d` e` padrão: 2 [0-3]: [0-5] \ d`. Isso é errado, porque ele irá corresponder ao padrão esquerdo ou direito:


`` `js run
Deixe reg = / [01] \ d | 2 [0-3]: [0-5] \ d / g;

alerta ("12" .match (reg)); // 12 (correspondente [01] \ d)
```

Isso é bastante óbvio, mas ainda é um erro freqüente ao começar a trabalhar com expressões regulares.

Precisamos adicionar parênteses para aplicar a alternância exatamente às horas: `[01] \ d` OU` 2 [0-3] `.

A variante correta:

`` `js run
Deixe reg = / ([01] \ d | 2 [0-3]): [0-5] \ d / g;

alerta ("00:00 10:10 23:59 25:99 1: 2" .match (reg)); // 00: 00,10: 10,23: 59
```
