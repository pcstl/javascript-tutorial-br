# Quantificadores ávidos e preguiçosos

Os quantificadores são muito simples desde a primeira vista, mas na verdade eles podem ser complicados.

Devemos entender como a pesquisa funciona muito bem se pretendemos procurar algo mais complexo do que o padrão: / \ d + / `.

[cortar]

Vamos dar a seguinte tarefa como um exemplo.

Nós temos um texto e precisamos substituir todas as cotações `" ... "` com guillemet marks: `« ... »`. Eles são preferidos para a tipografia em muitos países.

Por exemplo: `" Olá, o mundo "deve se tornar` "Olá, mundo" `.

Alguns países preferem as citações "Witam, świat!" (Polonês) ou mesmo 「你好, 世界」 `(chinês). Para locais diferentes, podemos escolher substituições diferentes, mas tudo funciona da mesma forma, então vamos começar com `« ... »`.

Para fazer substituições, primeiro precisamos encontrar todas as substrings citadas.

A expressão regular pode ser assim: `pattern: /". + "/ G`. Ou seja: procuramos uma citação seguida de um ou mais caracteres e, em seguida, uma outra citação.

... Mas se tentarmos aplicá-lo, mesmo em um caso tão simples ...

`` `js run
deixe reg = /".+"/g;

Deixe str = 'a "bruxa" e sua "vassoura" é uma ";

alerta (str.match (reg)); // "bruxa" e sua "vassoura"
```

... Podemos ver que isso não funciona como previsto!

Em vez de encontrar dois jogos, combine: "witch" `e` match: "vassoura", encontra um: `match:" witch "e sua" vassoura ".

Isso pode ser descrito como "a ganância é a causa de todo o mal".

## Pesquisa gananciosa

Para encontrar uma correspondência, o mecanismo de expressão regular usa o seguinte algoritmo:

- Para cada posição na corda
- Combine o padrão nessa posição.
- Se não houver correspondência, vá para a próxima posição.

Essas palavras comuns não tornam óbvio por que o regexp falha, então vamos elaborar como a pesquisa funciona para o padrão padrão: ". +" `.

1. O primeiro caractere de padrão é um padrão de citações: "`.

O mecanismo de expressão regular tenta encontrá-lo na posição zero da seqüência de origem `assunto: uma" bruxa "e sua" vassoura "é uma", mas há `assunto: a` lá, então não há nenhuma combinação.

Então, ele avança: vai para as próximas posições na cadeia de origem e tenta encontrar o primeiro caractere do padrão lá e, finalmente, encontra a citação na 3ª posição:

!] [] (witch_greedy1.png)

2. A citação é detectada e, em seguida, o mecanismo tenta encontrar uma correspondência para o resto do padrão. Ele tenta ver se o resto da seqüência de assunto está em conformidade com `pattern:. +" `.

Em nosso caso, o próximo caractere padrão é `pattern: .` (um ponto). Ele denota "qualquer caractere, exceto uma nova linha", então a próxima letra de string `corresponde: 'w'` se encaixa:

! [] (witch_greedy2.png)

3. Então o ponto se repete devido ao padrão de quantificador:. + `. O motor de expressão regular constrói a partida tomando personagens um a um enquanto é possível.

... Quando se torna impossível? Todos os caracteres correspondem ao ponto, então ele só pára quando ele chega ao fim da seqüência de caracteres:

!] [] (witch_greedy3.png)

4. Agora o motor terminou de repetir para `pattern:. +` E tenta encontrar o próximo caractere do padrão. É a frase "padrão": ``. Mas há um problema: a sequência terminou, não há mais caracteres!

O motor de expressão regular entende que demorou muito "padrão:. +` E começa a * retroceder *.

Em outras palavras, encurta a correspondência para o quantificador por um caractere:

!] [] (witch_greedy4.png)

Agora, assume que o `pattern:. +` Termina um caractere antes do fim e tenta combinar o resto do padrão dessa posição.

Se houvesse uma citação lá, então esse seria o fim, mas o último personagem é `subject: 'e'`, então não há correspondência.

5. ... Então, o motor diminui o número de repetições de `pattern:. +` Por mais um caractere:

!] [] (witch_greedy5.png)

A cotação `pattern: '' '` does not match `subject:' n'`.

6. O motor mantém backtracking: diminui a contagem de repetição para `pattern: '.'` Até que o resto do padrão (no nosso caso `pattern: '' '`) corresponde:

!] [] (witch_greedy6.png)

7. A partida está completa.

8. Então, a primeira partida é `match:" witch "e sua" vassoura "`. A busca adicional começa onde a primeira partida termina, mas não há mais citações no restante da string `subject: is one`, então não há mais resultados.

Provavelmente não é o que esperávamos, mas é assim que funciona.

** No modo ganancioso (por padrão), o quantificador é repetido o menor tempo possível. **

O mecanismo regex tenta buscar tantos caracteres quanto possível por `pattern:. +`, E depois encurta um por um.

Para a nossa tarefa, queremos outra coisa. É para isso que é o modo de quantificador preguiçoso.

## Modo preguiçoso

O modo preguiçoso de quantificador é um oposto ao modo ganancioso. Isso significa: "repete um número mínimo de vezes".

Podemos habilitá-lo colocando um ponto de interrogação `padrão: '?'` Após o quantificador, para que ele se torne "padrão: *?` Ou "padrão: +?" Ou mesmo "padrão: ??` para `padrão: ' ? ''.

Para tornar as coisas claras: geralmente um ponto de interrogação `padrão:?` É um quantificador por si só (zero ou um), mas se adicionado * após outro quantificador (ou mesmo em si) * obtém outro significado - ele troca o modo de correspondência de ganancioso para preguiçoso.

O padrão regexp `: /". +? "/ G` funciona como pretendido: ele encontra` match: "witch" `e` match: "vassoura" `:

`` `js run
deixe reg = /".+?"/g;

Deixe str = 'a "bruxa" e sua "vassoura" é uma ";

alerta (str.match (reg)); // bruxa, vassoura
```

Para entender claramente a mudança, vamos rastrear a pesquisa passo a passo.

1. O primeiro passo é o mesmo: ele encontra o padrão de início `padrão: '' '` na 3ª posição:

!] [] (witch_greedy1.png)

2. O próximo passo também é semelhante: o motor encontra uma correspondência para o padrão ponto `: '.'`:

! [] (witch_greedy2.png)

3. E agora a pesquisa é diferente. Como temos um modo preguiçoso para o `padrão: +?`, O mecanismo não tenta combinar um ponto mais uma vez, mas pára e tenta combinar o resto do padrão padrão: '' '' agora mesmo:

!] [] (witch_lazy3.png)

Se houvesse uma citação lá, então a pesquisa terminaria, mas há `` i'`, então não há correspondência.
4. Então o motor de expressão regular aumenta o número de repetições para o ponto e tenta mais uma vez:

!] [] (witch_lazy4.png)

Falha novamente. Então o número de repetições é aumentado repetidas vezes ...
5. ... Até a correspondência para o resto do padrão é encontrada:

! [] (witch_lazy5.png)

6. A próxima pesquisa começa a partir do final da partida atual e produz mais um resultado:

!] [] (witch_lazy6.png)

Neste exemplo, vimos como funciona o modo preguiçoso para `pattern: +?`. Padrão dos quantificadores: +? `E` padrão: ?? `funcionam da maneira similar - o motor regexp aumenta o número de repetições somente se o resto do padrão não puder corresponder na posição dada.

** A preguiça só é habilitada para o quantificador com `?`. **

Outros quantificadores permanecem gananciosos.

Por exemplo:

`` `js run
alerta ("123 456" .match (/ \ d + \ d +? / g)); // 123 4
```

1. O padrão `padrão: \ d +` tenta combinar tantos números quanto possível (modo ganancioso), então ele encontra `match: 123` e pára, porque o próximo caractere é um padrão do espaço: ''`.
2. Então há um espaço no padrão, ele coincide.
3. Então há `pattern: \ d +?`. O quantificador está no modo preguiçoso, por isso encontra um "match": 4 e tenta verificar se o resto do padrão corresponde a partir dele.

... Mas não há nada no padrão após `pattern: \ d +?`.

O modo preguiçoso não repete nada sem necessidade. O padrão terminou, então terminamos. Nós temos uma correspondência `match: 123 4`.
4. A próxima pesquisa começa a partir do caractere '5'.

`` `cabeçalho inteligente =" otimizações "
Os motores modernos de expressão regular podem otimizar os algoritmos internos para funcionarem mais rapidamente. Então eles podem funcionar um pouco diferente do algoritmo descrito.

Mas para entender como as expressões regulares funcionam e para construir expressões regulares, não precisamos saber disso. Eles são usados ​​apenas internamente para otimizar as coisas.

As expressões regulares complexas são difíceis de otimizar, então a pesquisa pode funcionar exatamente como descrito também.
```

## Abordagem alternativa

Com regexps, muitas vezes é mais uma maneira de fazer o mesmo.

No nosso caso, podemos encontrar cordas citadas sem modo preguiçoso usando o padrão regex `:" [^ "] +" `:

`` `js run
deixe reg = / "[^"] + "/ g;

Deixe str = 'a "bruxa" e sua "vassoura" é uma ";

alerta (str.match (reg)); // bruxa, vassoura
```

O padrão regexp `:" [^ "] +" `dá resultados corretos, porque ele procura um padrão de citações: '' '' seguido de um ou mais padrões sem citações ': [^"] `, e depois o citações de encerramento.

Quando o motor regex procura por "padrão: [^"] + 'ele pára as repetições quando ele atende a citação de fechamento, e acabamos.

Por favor, note que esta lógica não substitui os quantificadores preguiçosos!

É simplesmente diferente. Há momentos em que precisamos de um ou outro.

Vejamos mais um exemplo em que os quantificadores preguiçosos falham e esta variante funciona corretamente.

Por exemplo, queremos encontrar links do formulário `<a href="..."> class="doc">`, com qualquer `href`.

Qual expressão regular usar?

A primeira idéia pode ser: `pattern: / <a href=".*" class="doc"> / g`.

Vamos verificá-lo:
`` `js run
Deixe str = '... <a href="link" class="doc"> ...';
Deixe reg = / <a href=".*" class="doc"> / g;

// Trabalho!
alerta (str.match (reg)); // <a href="link" class="doc">
```

... Mas e se houver muitos links no texto?

`` `js run
Deixe str = '... <a href="link1" class="doc"> ... <a href="link2" class="doc"> ...';
Deixe reg = / <a href=".*" class="doc"> / g;

// Whoops! Dois links em uma partida!
alerta (str.match (reg)); // <a href="link1" class="doc"> ... <a href="link2" class="doc">
```

Agora, o resultado é errado pelo mesmo motivo que o nosso exemplo de "bruxas". O padrão de quantificação:. * `Levou muitos caracteres.

A partida parece assim:

`` `html
<a href=" ........................" class="doc">
<a href="link1" class="doc"> ... <a href="link2" class="doc">
```

Vamos modificar o padrão fazendo o padrão quantificador:. *? `Preguiçoso:

`` `js run
Deixe str = '... <a href="link1" class="doc"> ... <a href="link2" class="doc"> ...';
deixe reg = / <a href = ". *?" class = "doc"> / g;

// Trabalho!
alerta (str.match (reg)); // <a href="link1" class="doc">, <a href="link2" class="doc">
```

Agora funciona, existem duas partidas:

`` `html
<a href="....." class="doc"> <a href="....." class="doc">
<a href="link1" class="doc"> ... <a href="link2" class="doc">
```

Por que isso funciona - deve ser óbvio após todas as explicações acima. Então não vamos parar nos detalhes, mas tente mais um texto:

`` `js run
Deixe str = '... <a href="link1" class="wrong"> ... <p style = "" class = "doc"> ...';
deixe reg = / <a href = ". *?" class = "doc"> / g;

// Correspondência errada!
alerta (str.match (reg)); // <a href="link1" class="wrong"> ... <p style = "" class = "doc">
```

Podemos ver que o regexp não corresponde apenas a um link, mas também um monte de texto depois, incluindo `<p ...>`.

Por que acontece?

1. Primeiro, o regex encontra um link de partida `match: <a href =" `.

2. Então ele procura o `pattern:. *?`, Pegamos um personagem, depois verifique se há uma correspondência para o resto do padrão e, em seguida, pegue outro ...

O padrão de quantificação:. *? `Consome caracteres até que ele atenda` match: class = "doc"> `.

... E onde pode encontrá-lo? Se olharmos para o texto, podemos ver que o único "match: class =" doc ">` está além do link, na tag `<p>`.

3. Então temos uma partida:

`` `html
<a href=" ......................" class="doc">
<a href="link1" class="wrong"> ... <p style = "" class = "doc">
```

Então, a preguiça não funcionou para nós aqui.

Precisamos do padrão para procurar `<a href="...algo..." class="doc">`, mas as variantes gananciosas e preguiçosas têm problemas.

A variante correta seria: `pattern: href =" [^ "] *" `. Tomará todos os caracteres dentro do atributo` href` até a citação mais próxima, exatamente o que precisamos.

Um exemplo de trabalho:

`` `js run
Deixe str1 = '... <a href="link1" class="wrong"> ... <p style = "" class = "doc"> ...';
Deixe str2 = '... <a href="link1" class="doc"> ... <a href="link2" class="doc"> ...';
Deixe reg = / <a href="[^"]*" class="doc"> / g;

// Trabalho!
alerta (str1.match (reg)); // nulo, sem correspondências, está correto
alerta (str2.match (reg)); // <a href="link1" class="doc">, <a href="link2" class="doc">
```

## Resumo

Quantificadores têm dois modos de trabalho:

Ávido
: Por padrão, o mecanismo de expressão regular tenta repetir o quantificador quantas vezes for possível. Por exemplo, `padrão: \ d +` consome todos os dígitos possíveis. Quando torna-se impossível consumir mais (não mais dígitos ou fim de string), continua a combinar o resto do padrão. Se não houver correspondência, diminui o número de repetições (backtracks) e tenta novamente.

Preguiçoso
: Ativado pelo ponto de interrogação `padrão:? 'Após o quantificador. O mecanismo regexp tenta combinar o resto do padrão antes de cada repetição do quantificador.

Como vimos, o modo preguiçoso não é uma "panacea" da busca gananciosa. Uma alternativa é uma busca gananciosa "ajustada", com exclusões. Em breve, veremos mais exemplos disso.
