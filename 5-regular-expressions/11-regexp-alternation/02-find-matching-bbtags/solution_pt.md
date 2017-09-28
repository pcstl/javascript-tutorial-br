
A tag de abertura é `padrão: \ [(b | url | quote) \]`.

Então, encontre tudo até a etiqueta de fechamento - vamos representar o padrão padrão: [\ s \ S] *? `Para combinar qualquer caractere incluindo a linha nova e, em seguida, uma referência reversa para a tag de fechamento.

O padrão completo: `padrão: \ [(b | url | quote) \] [\ s \ S] *? \ [/ \ 1 \]`.

Em ação:

`` `js run
Deixe reg = / \ [(b | url | quote) \] [\ s \ S] *? \ [\ / \ 1 \] / g;

deixe str = `
[b] olá! [/ b]
[citar]
[url] http://google.com [/ url]
[/citar]
`;

alerta (str.match (reg)); // [b] hello! [/ b], [quote] [url] http://google.com [/ url] [/ quote]
```

Por favor, note que tivemos que escapar de uma barra para a tag de fechamento `padrão: [/ \ 1]`, porque normalmente a barra fecha o padrão.
