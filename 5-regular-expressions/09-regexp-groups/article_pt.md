# Capturar grupos

Uma parte do padrão pode ser incluída entre parênteses `padrão: (...)`. Isso é chamado de "grupo de captura".

Isso tem dois efeitos:

1. Permite colocar uma parte da partida em um item separado da matriz ao usar os métodos [String # match] (mdn: js / String / match) ou [RegExp # exec] (mdn: / RegExp / exec).
2. Se colocarmos um quantificador após os parênteses, ele se aplica aos parênteses como um todo, e não ao último caractere.

[cortar]

## Exemplo

No exemplo abaixo, o padrão `pattern: (go) +` encontra um ou mais `match: 'go'`:

`` `js run
alerta ('Gogogo agora!'. match (/ (go) + / i)); // "Vai! Vai! Vai"
```

Sem parênteses, o padrão `padrão: / go + /` significa `subject: g`, seguido de` subject: o` repetido uma ou mais vezes. Por exemplo, `match: goooo` ou` match: gooooooooo`.

Parênteses agrupam a palavra "padrão: (ir)" juntos.

Vamos fazer algo mais complexo - um regex para combinar um e-mail.

Exemplos de e-mails:

```
my@mail.com
john.smith@site.com.uk
```

O padrão: `padrão: [-. \ W] + @ ([\ w -] + \.) + [\ W -] {2,20}`.

- A primeira parte antes `@` pode incluir caracteres verbais, um padrão de ponto e uma dash: [-. \ W] + `, como` match: john.smith`.
- Então 'padrão: @ `
- E depois o domínio. Pode ser um domínio de segundo nível `site.com` ou com subdomínios como` host.site.com.uk`. Podemos combiná-lo como "uma palavra seguida por um ponto" repetida uma ou mais vezes para subdomínios: `match: mail.` ou` match: site.com.` e, em seguida, "uma palavra" para a última parte: `match : .com` ou `match: .uk`.

A palavra seguida por um ponto é `padrão: (\ w + \.) +` (Repetido). A última palavra não deve ter um ponto no final, então é apenas `\ w {2,20}`. O padrão quantifier `: {2,20}` limita o comprimento, porque as zonas de domínio são como `.uk` ou` .com` ou `.museum`, mas não podem ter mais de 20 caracteres.

Portanto, o padrão de domínio é `padrão: (\ w + \.) + \ W {2,20}`. Agora, substituímos `\ w` por` [\ w-] `, porque os traços também são permitidos em domínios, e nós obtemos o resultado final.

Essa regexp não é perfeita, mas geralmente funciona. É curto e bom o suficiente para corrigir erros ou mistypes ocasionais.

Por exemplo, aqui podemos encontrar todos os e-mails na string:

`` `js run
Deixe reg = /[-.\w]+@([\w-]+\.)+[\w-]{2,20}/g;

alerta ("my@mail.com @ his@site.com.uk" .match (reg)); // my @ mail.com, seu @ site.com.uk
```


## Conteúdo de parênteses

Parênteses são numerados da esquerda para a direita. O mecanismo de pesquisa lembra o conteúdo de cada um e permite fazer referência no padrão ou na seqüência de substituição.

Por exemplo, podemos encontrar uma tag HTML usando padrão padrão (simplificado): <. *?> `. Normalmente, gostaríamos de fazer algo com o resultado depois.

Se incluímos os conteúdos internos de `<...>` em parênteses, então podemos acessá-lo assim:

`` `js run
Deixe str = '<h1> Olá, mundo! </ h1>';
deixe reg = /<(.*?)>/;

alerta (str.match (reg)); // Array: ["<h1>", "h1"]
```

A chamada para [String # match] (mdn: js / String / match) retorna grupos somente se o regexp não tiver um padrão `pattern: /.../ g`.

Se precisamos de todas as correspondências com seus grupos, podemos usar o método [RegExp # exec] (mdn: js / RegExp / exec) conforme descrito em <info: regexp-methods>:

`` `js run
Deixe str = '<h1> Olá, mundo! </ h1>';

// duas partidas: abrir as tags <h1> e fechar </ h1>
Deixe reg = /<(.*?)>/g;

deixar coincidir;

enquanto (match = reg.exec (str)) {
// primeiro mostra a partida: <h1>, h1
// então mostra a correspondência: </ h1>, / h1
alerta (correspondência);
}
```

Aqui temos duas partidas para `pattern: <(. *?)>`, Cada uma delas é uma matriz com a combinação completa e grupos.

## Grupos aninhados

Parênteses podem ser aninhados. Neste caso, a numeração também vai da esquerda para a direita.

Por exemplo, ao pesquisar uma tag em `subject: <span class =" my ">`, podemos estar interessados ​​em:

1. O conteúdo da etiqueta como um todo: `match: span class =" my "`.
2. O nome da etiqueta: `match: span`.
3. Os atributos da etiqueta: `match: class =" my "`.

Vamos adicionar parênteses para eles:

`` `js run
Deixe str = '<span class = "my">';

Deixe reg = / <(([a-z] +) \ s * ([^>] *))> /;

Deixe resultado = str.match (reg);
alerta (resultado); // <span class = "my"> span class = "my", span, class = "my"
```

Veja como se parecem os grupos:

! [] (regex-anested-groups.png)

No índice zero do `resultado 'está sempre a partida completa.

Em seguida, grupos, numerados da esquerda para a direita. O que abrir primeiro dá o primeiro grupo `result [1]`. Aqui inclui todo o conteúdo da tag.

Então, em `result [2]` vai o grupo da segunda abertura `padrão: (` até o padrão correspondente:) `- nome da etiqueta, então não agrupamos espaços, mas atributos de grupo para` resultado [3] `.

** Se um grupo for opcional e não existir na partida, o índice correspondente do `resultado 'está presente (e é igual a' indefinido '). **

Por exemplo, vamos considerar o padrão regexp `: a (z)? (C)?`. Ele procura por "um" "seguido opcionalmente por" z "" opcionalmente seguido por "c" `.

Se o executarmos na corda com uma única letra `subject: a`, então o resultado é:

`` `js run
Deixe match = 'a'.match (/ a (z)? (c)? /);

alerta (match.length); // 3
alerta (correspondência [0]); // a (jogo inteiro)
alerta (correspondência [1]); // Indefinido
alerta (correspondência [2]); // Indefinido
```

A matriz tem o comprimento de `3`, mas todos os grupos estão vazios.

E aqui está uma combinação mais complexa para o string `subject: ack`:

`` `js run
let match = 'ack'.match (/ a (z)? (c)? /)

alerta (match.length); // 3
alerta (correspondência [0]); // ac (jogo inteiro)
alerta (correspondência [1]); // indefinido, porque não há nada para (z)?
alerta (correspondência [2]); // c
```

O comprimento da matriz é permanente: `3`. Mas não há nada para o grupo `padrão: (z)?`, Então o resultado é `[" ac ", indefinido," c "]`.

## Grupos de não captura com?:

Às vezes, precisamos de parênteses para aplicar corretamente um quantificador, mas não queremos seu conteúdo na matriz.

Um grupo pode ser excluído adicionando `pattern:?:` No começo.

Por exemplo, se quisermos encontrar o `pattern: (go) +`, mas não quiser colocar, lembre-se do conteúdo (`go`) em um item separado da matriz, podemos escrever:` pattern :( ?: go) + `.

No exemplo abaixo, só recebemos o nome "John" como um membro separado da matriz `results`:

`` `js run
Deixe str = "Gogo John!";
*!*
// exclua o Gogo da captura
deixe reg = / (?: go) + (\ w +) / i;
*/!*

Deixe resultado = str.match (reg);

alerta (resultado.length); // 2
alerta (resultado [1]); // John
```
