# Problema infinito de retrocesso

Algumas expressões regulares parecem simples, mas podem executar muito tempo e até "pendurar" o mecanismo de JavaScript.

Mais cedo ou mais, a maioria dos desenvolvedores ocasionalmente enfrenta esse comportamento.

A situação típica - uma expressão regular funciona bem às vezes, mas, para certas cadeias, ela "trava" consumindo 100% da CPU.

Isso pode até ser uma vulnerabilidade. Por exemplo, se o JavaScript estiver no servidor e ele usa expressões regulares para processar dados do usuário, essa entrada pode causar negação de serviço. O autor viu pessoalmente e relatou tais vulnerabilidades, mesmo para programas bem conhecidos e amplamente utilizados.

Portanto, o problema definitivamente vale a pena lidar.

[cortar]

## Exemplo

O plano será assim:

1. Primeiro, vemos o problema como pode ocorrer.
2. Então simplificamos a situação e veremos por que ela ocorre.
3. Então nós corrigimos isso.

Por exemplo, considere pesquisar tags em HTML.

Queremos encontrar todas as tags, com ou sem atributos - como `assunto: <a href="..." class="doc" ...>`. Precisamos que o regexp funcione de forma confiável, porque o HTML vem da internet e pode ser confuso.

Em particular, precisamos que combine tags como `<a test="<>" href = "#"> `- com` <`e`> `em atributos. Isso é permitido pelo [padrão HTML] (https://html.spec.whatwg.org/multipage/syntax.html#syntax-attributes).

Agora podemos ver que uma regexp simples como `padrão: <[^>] +>` não funciona, porque ele pára no primeiro `>`, e precisamos ignorar `<>` dentro de um atributo.

`` `js run
// a partida não atinge o final da tag - errado!
alerta ('<a test="<> "href =" # ">'. match (/ <[^>] +> /)); // <a test="<>
```

Precisamos de toda a tag.

Para lidar corretamente com tais situações, precisamos de uma expressão regular mais complexa. Ele terá a forma `pattern: <tag (key = value) *>`.

Na linguagem regex que é: `padrão: <\ w + (\ s * \ w + = (\ w + |" [^ "] *") \ s *) *> `:

1. `padrão: <\ w +` - é o início da tag,
2. `padrão: (\ s * \ w + = (\ w + |" [^ "] *") \ s *) * `- é um número arbitrário de pares` word = value`, onde o valor pode ser uma palavra `padrão: \ w +` ou uma seqüência citada `padrão:" [^ "] *" `.

Isso ainda não oferece suporte a poucos detalhes da gramática HTML, por exemplo, seqüências de caracteres em citações "únicas", mas podem ser adicionadas mais tarde, de modo que seja um pouco próximo da vida real. Por enquanto queremos que o regexp seja simples.

Vamos tentar em ação:

`` `js run
Deixe reg = / <\ w + (\ s * \ w + = (\ w + | "[^"] * ") \ s *) *> / g;

Deixe str = '... <a test="<> "href =" # "> ... <b> ...';

alerta (str.match (reg)); // <a test="<> "href =" # ">, <b>
```

Ótimo, funciona! Ele encontrou a marca longa `match: <a test="<>" href = "#"> `e a curta 'match: <b>`.

Agora vamos ver o problema.

Se você executar o exemplo abaixo, pode pendurar o navegador (ou o que o motor de JavaScript executa):

`` `js run
Deixe reg = / <\ w + (\ s * \ w + = (\ w + | "[^"] * ") \ s *) *> / g;

deixe str = `<tag a = b a = b a = b a = b a = b a = b a = b a = b
a = b a = b a = b a = b a = b a = b a = b a = b a = b a = b a = b a = b a = b ";

*!*
// A pesquisa demorará muito tempo
alerta (str.match (reg));
*/!*
```

Alguns motores regexp podem lidar com essa pesquisa, mas a maioria deles não.

Qual é o problema? Por que uma expressão regular simples em uma pequena seqüência "trava"?

Vamos simplificar a situação, removendo a etiqueta e as cordas citadas.

Aqui olhamos apenas atributos:

`` `js run
// apenas procura por atributos delimitados por espaço
Deixe reg = / <(\ s * \ w + = \ w + \ s *) *> / g;

Deixe str = `<a = b a = b a = b a = b a = b a = b a = b a = b
a = b a = b a = b a = b a = b a = b a = b a = b a = b a = b a = b a = b a = b ";

*!*
// a busca demorará muito tempo
alerta (str.match (reg));
*/!*
```

O mesmo problema persiste.

Aqui terminamos a demo do problema e começamos a olhar para o que está acontecendo e por que ele trava.

## Backtracking

Para tornar um exemplo ainda mais simples, vamos considerar `padrão: (\ d +) * $`.

Essa expressão regular também tem o mesmo problema. Na maioria dos motores regexp, a pesquisa leva muito tempo (cuidado - pode travar):

`` `js run
alerta ('12345678901234567890123456789123456789z'.match (/ (\ d +) * $ /));
```

Então, o que há de errado com o regexp?

Primeiro, pode-se notar que o regexp é um pouco estranho. O padrão de quantificador: * `parece estranho. Se queremos um número, podemos usar `pattern: \ d + $`.

De fato, o regexp é artificial. Mas o motivo pelo qual é lento é o mesmo que os que vimos acima. Então, vamos entender isso, e depois retornar aos exemplos da vida real.

O que acontece durante a pesquisa do `padrão: (\ d +) * $` na linha `subject: 123456789z`?

1. Primeiro, o motor regex tenta encontrar um número `padrão: \ d +`. O padrão plus `: +` é ganancioso por padrão, portanto, ele consome todos os dígitos:

```
\ d + .......
(123456789) de
```
2. Então ele tenta aplicar o começo ao redor do padrão dos parênteses: (\ d +) * `, mas não há mais dígitos, então a estrela não dá nada.

Em seguida, o padrão tem o padrão de cadeia `padrão: $`, e no texto temos `subject: z`.

```
X
\ d + ........ $
(123456789) de
```

Nenhuma partida!
3. Não há correspondência, então o padrão quantificador ganancioso: + `diminui a contagem de repetições (backtracks).

Agora `\ d +` não é todos os dígitos, mas todos exceto o último:
```
\ d + .......
(12345678) 9Z
```
4. Agora, o mecanismo tenta continuar a busca a partir da nova posição (`9`).

O padrão start `: (\ d +) *` agora pode ser aplicado - dá o número `match: 9`:

```

\ D + ....... \ d +
(12345678) (9)
```

O mecanismo tenta combinar `$` novamente, mas falha, porque atende `subject: z`:

```
X
\ D + ....... \ d +
(12345678) (9)
```

Não há correspondência, então o motor continuará retrocedendo.
5. Agora, o primeiro número `padrão: \ d +` terá 7 dígitos, eo restante da seqüência `subject: 89` se torna o segundo` pattern: \ d + `:

```
X
\ D + ...... \ d +
(1234567) (89)
```

... Ainda não corresponde ao `padrão: $`.

O mecanismo de busca retrocede novamente. Backtracking geralmente funciona assim: o último quantificador ganancioso diminui o número de repetições até que ele possa. Em seguida, o quantificador ganancioso anterior diminui, e assim por diante. No nosso caso, o último quantificador ganancioso é o segundo `padrão: \ d +`, do `assunto: 89` para` subject: 8` e, em seguida, a estrela leva `subject: 9`:

```
X
\ D + ...... \ d + \ d +
(1234567) (8) (9)
```
6. ... Fail novamente. O segundo e o terceiro `padrão: \ d +` voltou para o final, então o primeiro quantificador encurta a correspondência para `subject: 123456` e a estrela leva o resto:

```
X
\ D + ....... \ d +
(123456) (789) a partir de
```

Novamente, não há correspondência. O processo se repete: o último quantificador ganancioso libera um caractere (`9`):

```
X
\ d + ..... \ d + \ d +
(123456) (78) (9)
```
7. ... E assim por diante.

O motor de expressão regular passa por todas as combinações de `123456789` e suas subsequências. Há muitos deles, é por isso que demora tanto tempo.

Um cara inteligente pode dizer aqui: "Retroceder? Vamos ligar o modo preguiçoso - e não mais voltar atrás!".

Vamos substituir `padrão: \ d +` com `padrão: \ d +?` E veja se ele funciona (cuidado, pode pendurar o navegador)

`` `js run
// sloooooowwwwww
alerta ('12345678901234567890123456789123456789z'.match (/ (\ d +?) * $ /));
```

Não, não.

Os quantificadores preguiçosos realmente fazem o mesmo, mas na ordem inversa. Basta pensar em como o mecanismo de pesquisa funcionaria neste caso.

Alguns motores de expressão regular têm checagens complicadas para detectar backtracking infinito ou outros meios para trabalhar em torno deles, mas não há solução universal.

No exemplo acima, quando buscamos `padrão: <(\ s * \ w + = \ w + \ s *) *>` na string `subject: <a = ba = ba = ba = b` - a coisa semelhante acontece.

A string não tem `>` no final, então a partida é impossível, mas o motor regexp não sabe sobre isso. A pesquisa retrocede tentando diferentes combinações de 'padrão: (\ s * \ w + = \ w + \ s *) `:

```
(a = b a = b a = b) (a = b)
(a = b a = b) (a = b a = b)
...
```

## Como consertar?

O problema - muitas variantes na retrocesso, mesmo que não as precisemos.

Por exemplo, no padrão `padrão: (\ d +) * $` nós (pessoas) podemos ver facilmente que o padrão: (\ d +) `não precisa voltar atrás.

Diminuir a contagem de 'padrão: \ d + `não pode ajudar a encontrar uma correspondência, não há dúvida entre estes dois:

```
\ d + ........
(123456789) de

\ D + ... \ d + ....
(1234) (56789) com
```

Voltemos a mais exemplos da vida real: `padrão: <(\ s * \ w + = \ w + \ s *) *>`. Nós queremos que ele encontre pares `name = value` (tanto quanto ele pode). Não há necessidade de voltar atrás aqui.

Em outras palavras, se encontrou vários pares `name = value` e então não conseguiu encontrar`> `, então não há necessidade de diminuir a contagem de repetições. Mesmo que combinemos um par menos, não nos dará o fechamento de `>`:

Os motores regexp modernos suportam os chamados quantificadores "possessivos" para isso. Eles são como gananciosos, mas não retrocedem de forma alguma. Muito simples, eles capturam o que podem, e a pesquisa continua. Há também outra ferramenta chamada "grupos atômicos" que proíbem a retrocesso entre parênteses.

Infelizmente, mas ambos esses recursos não são suportados pelo JavaScript.

Embora possamos obter um efeito semelhante usando o lookahead. Há mais sobre a relação entre quantificadores possessivos e lookahead em artigos [Regex: Emulate Atomic Grouping (e Quantitadores Possessivos) com LookAhead] (http://instanceof.me/post/52245507631/regex-emulate-atomic-grouping-with-lookahead ) e [Mimicking Atomic Groups] (http://blog.stevenlevithan.com/archives/mimic-atomic-groups).

O padrão para tirar tantas repetições quanto possível sem backtracking é: `padrão: (? = (A +)) \ 1`.

Em outras palavras, o lookahead `pattern:? =` Procura o padrão de contagem máxima: a + `da posição atual. E então eles são "consumidos no resultado" pelo backreference `pattern: \ 1`.

Não haverá backtracking, porque lookahead não retrocede. Se encontrou 5 vezes do `padrão: a +` e a correspondência adicional falhou, então não volta para 4.

Vamos corrigir o regexp para uma tag com atributos desde o início do capítulo `pattern: <\ w + (\ s * \ w + = (\ w + |" [^ "] *") \ s *) *> `. Nós ' Use o lookahead para evitar backtracking dos pares `name = value`:

`` `js run
// regex para pesquisar nome = valor
deixe attrReg = / (\ s * \ w + = (\ w + | "[^"] * ") \ s *) /

// use-o dentro da regex para tag
Deixe reg = novo RegExp ('<\\ w + (? = (' + attrReg.source + '*)) \\ 1>', 'g');

deixe bom = '... <a test="<> "href =" # "> ... <b> ...';

let bad = `<tag a = b a = b a = b a = b a = b a = b a = b a = b
a = b a = b a = b a = b a = b a = b a = b a = b a = b a = b a = b a = b a = b ";

alerta (good.match (reg)); // <a test="<> "href =" # ">, <b>
alerta (bad.match (reg)); // nulo (sem resultados, rápido!)
```

Ótimo, funciona! Encontramos um jogo de tag longo: <a test="<> "href =" # ">` e um pequeno `match: <b>` e não pendurava o motor.

Observe a propriedade `attrReg.source`. Os objetos `RegExp` fornecem acesso a sua string de origem nela. Isso é conveniente quando queremos inserir um regexp em outro.
