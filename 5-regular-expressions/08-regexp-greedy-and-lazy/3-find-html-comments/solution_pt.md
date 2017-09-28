Precisamos encontrar o começo do comentário `match: <! -`, então tudo até o final de `match: ->`.

A primeira idéia poderia ser "padrão: <! -. *? ->` - o quantificador preguiçoso faz com que o ponto pare direito antes da "partida": -> `.

Mas um ponto em Javascript significa "qualquer símbolo, exceto a nova linha". Portanto, os comentários multilinha não serão encontrados.

Podemos usar `padrão: [\ s \ S]` em vez do ponto para combinar "qualquer coisa":

`` `js run
deixe reg = / <! - [\ s \ S] *? -> / g;

Deixe str = `... <! - My - comment
teste -> ... <! ----> ..
`;

alerta (str.match (reg)); // '<! - My - comment \ n test ->', '<! ---->'
```
