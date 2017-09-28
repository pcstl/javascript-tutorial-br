# Backreferências: \ n e $ n

Os grupos de captura podem ser acessados ​​não apenas no resultado, mas na cadeia de substituição e no padrão também.

[cortar]

## Grupo em substituição: $ n

Quando estamos usando o método `replace`, podemos acessar n-th grupo na seqüência de substituição usando` $ n`.

Por exemplo:

`` `js run
let name = "John Smith";

name = name.replace (/ (\ w +) (\ w +) / i, *! * "$ 2, $ 1" * /! *);
alerta (nome); // Smith, John
```

Aqui, o "padrão: $ 1" na seqüência de substituição significa "substituir o conteúdo do primeiro grupo aqui" e "padrão: $ 2" significa "substituir o segundo grupo aqui".

Referenciar um grupo na seqüência de substituição nos permite reutilizar o texto existente durante a substituição.

## Grupo no padrão: \ n

Um grupo pode ser referenciado no padrão usando `\ n`.

Para deixar as coisas claras vamos considerar uma tarefa. Precisamos encontrar uma seqüência citada: ou um assunto único: "..." ou um assunto duplo: "..." - ambas as variantes precisam corresponder.

Como procurá-los?

Podemos colocar dois tipos de cotações no padrão: `padrão: ['"] (. *?) [' "]`. Isso encontra strings como `match:" ... "` e `match: '...'`, mas dá correspondências incorretas quando uma citação aparece dentro de outra, como a string `subject:" Ela é a única! " :

`` `js run
Deixe str = "Ele disse: \" Ela é a única! \ ".";

deixe reg = /['"](.*?)['"]/g;

// O resultado não é o que esperamos
alerta (str.match (reg)); // "Ela"
```

Como podemos ver, o padrão encontrou uma frase de abertura `match:" `, então o texto é consumado preguiçosamente até a outra cotação` match: '`, que fecha a partida.

Para se certificar de que o padrão procura a citação de fechamento exatamente o mesmo que a abertura, vamos fazer um grupo e usar a referência:

`` `js run
Deixe str = "Ele disse: \" Ela é a única! \ ".";

Deixe reg = /(['"])(.*?)\1/g;

alerta (str.match (reg)); // "Ela é a única!"
```

Agora tudo está correto! O motor de expressão regular encontra o primeiro padrão de citações: (['"])` e lembra o conteúdo do `padrão: (...)`, esse é o primeiro grupo de captura.

Além disso, no padrão "padrão: \ 1" significa "encontrar o mesmo texto que no primeiro grupo".

Observe:

- Para fazer referência a um grupo dentro de uma seqüência de substituição - usamos `$ 1`, enquanto no padrão - uma barra invertida` \ 1`.
- Se usarmos ``: `no grupo, não podemos fazer referência a ele. Os grupos que estão excluídos da captura de `(?: ...)` não são lembrados pelo motor.
