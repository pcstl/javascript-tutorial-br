# Aulas de personagem

Considere uma tarefa prática - temos um número de telefone `" +7 (903) -123-45-67 "`, e precisamos encontrar todos os dígitos nessa corda. Outros personagens não nos interessam.

Uma classe de personagem é uma notação especial que corresponde a qualquer símbolo do conjunto.

[cortar]

Por exemplo, há uma classe de "dígito". Está escrito como `\ d`. Nós o colocamos no padrão, e durante a pesquisa, qualquer dígito coincide.

Por exemplo, o padrão regex `/ / d /` procura por um único dígito:

`` `js run
deixe str = "+7 (903) -123-45-67";

deixe reg = / \ d /;

alerta (str.match (reg)); // 7
```

O regexp não é global no exemplo acima, portanto, ele só procura a primeira partida.

Vamos adicionar a bandeira `g` para procurar todos os dígitos:

`` `js run
deixe str = "+7 (903) -123-45-67";

Deixe reg = / \ d / g;

alerta (str.match (reg)); // conjunto de correspondências: 7,9,0,3,1,2,3,4,5,6,7
```

## A maioria das classes usadas: \ d \ s \ w

Essa era uma classe de personagem para dígitos. Existem outras classes de personagens também.

Os mais utilizados são:

`\ d` (" d "é de" dígito ")
: Um dígito: um caractere de `0` para` 9`.

`\ s` (" s "é de" espaço ")
: Um símbolo de espaço: que inclui espaços, guias, linhas novas.

`\ w` (" w "é de" palavra ")
: Um personagem "mundano": uma letra de alfabeto inglês ou um dígito ou um sublinhado. Letras não inglesas (como cyrillic ou hindi) não pertencem a `\ w`.

Por exemplo, `padrão: \ d \ s \ w 'significa um dígito seguido por um caractere espacial seguido por um caractere de palavra, como` "1 Z" `.

Um regex pode conter símbolos regulares e classes de caracteres.

Por exemplo, `pattern: CSS \ d` corresponde a uma string` match: CSS` com um dígito depois:

`` `js run
Deixe str = "CSS4 é legal";
deixe reg = / CSS \ d /

alerta (str.match (reg)); // CSS4
```

Também podemos usar muitas classes de personagens:

`` `js run
alerta ("Eu amo o HTML5!". Corresponde (/ \ s \ w \ w \ w \ w \ d /)); // 'HTML5'
```

A correspondência (cada classe de personagem corresponde a um caractere de resultado):

! [] (love-html5-classes.png)

## Limite do Word: \ b

O limite de palavras `pattern: \ b` - é uma classe de caractere especial.

Não indica um personagem, mas sim um limite entre os personagens.

Por exemplo, `padrão: \ Java \ b` corresponde` match: Java` na string `subject: Hello, Java!`, Mas não no script `subject: Hello, JavaScript!`.

`` `js run
alerta ("Hello, Java!". match (/ \ bJava \ b /)); // Java
alerta ("Olá, JavaScript!". match (/ \ bJava \ b /)); // nulo
```

O limite tem "largura zero", em um sentido que normalmente uma classe de personagem significa um caractere no resultado (como um wordly ou um dígito), mas não neste caso.

O limite é um teste.

Quando o motor de expressões regulares está fazendo a busca, ele está se movendo ao longo da string na tentativa de encontrar a partida. Em cada posição de cadeia, tenta encontrar o padrão.

Quando o padrão contém `padrão: \ b`, ele testa que a posição na string se encaixa numa das condições:

- Início da seqüência de caracteres, e o primeiro caractere de string é `\ w`.
- String end, eo último caractere de string é `\ w`.
- Dentro da string: de um lado é `\ w`, do outro lado - não` \ w`.

Por exemplo, na string `subject: Hello, Java!` As seguintes posições correspondem `\ b`:

! [] (hello-java-boundaries.png)

Então, corresponde ao padrão: \ bHello \ b` e `pattern: \ bJava \ b`, mas não` pattern: \ bHell \ b` (porque não há limite de palavra após `l`) e não` Java! \ B` (porque o sinal de exclamação não é um personagem sinóptico, então não há limite de palavras depois).

`` `js run
alerta ("Hello, Java!". match (/ \ bHello \ b /)); // Olá
alerta ("Hello, Java!". match (/ \ Java \ b /)); // Java
alerta ("Olá, Java!". match (/ \ Hell \ b /)); // nulo
alerta ("Hello, Java!". match (/ \ Java! \ b /)); // nulo
```

Mais uma vez, vejamos que o `pattern: \ b` faz com que o mecanismo de pesquisa teste o limite, de modo que` pattern: Java \ b` encontre `match: Java` somente quando seguido por um limite de palavras, mas não adiciona um carta ao resultado.

Geralmente, usamos `\ b` para encontrar palavras inglesas autônomas. Então, se quisermos "Java" `idioma, então, padrão: \ bJava \ b` encontra exatamente uma palavra autônoma e ignora quando é parte de` "JavaScript" `.

Outro exemplo: um padrão regexp `: \ b \ d \ d \ b` procura números autônomos de dois dígitos. Por outras palavras, exige que antes e depois do `padrão: \ d \ d` deve ser um símbolo diferente de` \ w` (ou início / fim da string).

`` `js run
alerta ("1 23 456 78" .match (/ \ b \ d \ d \ b / g)); // 23,78
```

`` `warn header =" O limite do Word não funciona para alfabetos que não são ingleses "
A verificação de limite de palavras `\ b` testa um limite entre` \ w` e outra coisa. Mas `\ w 'significa uma letra em inglês (ou um dígito ou um sublinhado), então o teste não funcionará para outros personagens (como cyrillic ou hieroglyphs).
```


## Reverse classes

Para cada classe de personagem existe uma "classe reversa", denotada com a mesma letra, mas em maiúscula.

O "reverso" significa que corresponde a todos os outros caracteres, por exemplo:

`\ D`
: Não-dígito: qualquer caractere exceto `\ d`, por exemplo, uma letra.

`\ S`
: Não espaço: qualquer caractere exceto `\ s`, por exemplo, uma letra.

`\ W`
: Caracteres não-verbais: qualquer coisa, exceto `\ w`.

`\ B`
: Não-limite: um teste de reversão para `\ b`.

No começo do capítulo, vimos como obter todos os dígitos do telefone `subject: +7 (903) -123-45-67`. Vamos obter um número de telefone "puro" da string:

`` `js run
deixe str = "+7 (903) -123-45-67";

alerta (str.match (/ \ d / g). jooin ('')); // 79031234567
```

Uma maneira alternativa seria encontrar não-dígitos e removê-los da string:


`` `js run
deixe str = "+7 (903) -123-45-67";

alerta (str.replace (/ \ D / g, "")); // 79031234567
```

## Espaços são caracteres comuns

Por favor, note que as expressões regulares podem incluir espaços. São tratados como personagens comuns.

Normalmente, prestamos pouca atenção aos espaços. Para nós, strings `subject: 1-5` e` subject: 1 - 5` são quase idênticos.

Mas se uma regex não leva espaços, ganhou o trabalho.

Vamos tentar encontrar dígitos separados por um traço:

`` `js run
alerta ("1 - 5" .match (/ \ d- \ d /)); // nulo, sem partida!
```

Aqui nós corrigimos isso adicionando espaços no regexp:

`` `js run
alerta ("1 - 5" .match (/ \ d - \ d /)); // 1 - 5, agora funciona
```

Claro, apenas são necessários espaços se os buscarmos. Espaços extras (assim como outros caracteres extras) podem impedir uma combinação:

`` `js run
alerta ("1-5" .match (/ \ d - \ d /)); // nulo, porque a string 1-5 não tem espaços
```

Em outras palavras, em uma expressão regular, todos os personagens são importantes. Espaços também.

## Um ponto é qualquer personagem

O ponto `". "` É uma classe de personagem especial que corresponde a * qualquer caractere exceto uma nova linha *.

Por exemplo:

`` `js run
alerta ("Z" .match (/./)); // Z
```

Ou no meio de uma regex:

`` `js run
deixe reg = /CS.4/;

alerta ("CSS4" .match (reg)); // CSS4
alerta ("CS-4" .match (reg)); // CS-4
alerta ("CS 4" .match (reg)); // CS 4 (o espaço também é um caractere)
```

Observe que o ponto significa "qualquer caractere", mas não a "ausência de um personagem". Deve haver um personagem para combiná-lo:

`` `js run
alerta ("CS4" .match (/CS.4/)); // nulo, sem correspondência porque não há personagem para o ponto
```


## Resumo

Cobrimos aulas de personagens:

- `\ d` - dígitos.
- `\ D` - não dígitos.
- `\ s` - símbolos de espaço, guias, linhas novas.
- `\ S` - todos menos` \ s`.
- `\ w` - Letras inglesas, dígitos, sublinhado` '_'`.
- `\ W` - todos menos` \ w`.
- `'.'` - qualquer personagem, exceto uma nova linha.

Se quisermos procurar um personagem que tenha um significado especial como uma barra invertida ou um ponto, então devemos escapar com uma barra invertida: `padrão: \.`

Observe que uma regex também pode conter caracteres especiais de cadeia, como uma nova linha `\ and`. Não há conflito com as classes de personagens, porque outras letras são usadas para elas.
