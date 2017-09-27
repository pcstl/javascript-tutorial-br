# Comparações

Muitos operadores de comparação que conhecemos de matemática:

- Maior / menor que: <code> a & gt; b </ code>, <code> a & lt; b </ code>.
- Maior / menor ou igual: <code> a & gt; = b </ code>, <code> a & lt; = b </ code>.
- A verificação de igualdade é escrita como 'a == b` (observe o sinal de equação dupla `' = '`. Um único símbolo `a = b' significaria uma atribuição).
- Não é igual. Em matemática, a notação é <code> & ne; </ code>, em JavaScript está escrito como uma atribuição com um sinal de exclamação antes dele: <code> a! = B </ code>.

[cortar]

## Boolean é o resultado

Assim como todos os outros operadores, uma comparação retorna um valor. O valor é do tipo booleano.

- `true` - significa" sim "," correto "ou" a verdade ".
- `falso '- significa" não "," errado "ou" mentira ".

Por exemplo:

`` `js run
alerta (2> 1); // verdadeiro (correto)
alerta (2 == 1); // falso (errado)
alerta (2! = 1); // verdadeiro (correto)
`` `

Um resultado de comparação pode ser atribuído a uma variável, assim como qualquer valor:

`` `js run
Deixe resultado = 5> 4; // atribuir o resultado da comparação
alerta (resultado); // verdade
`` `

## Comparação de cordas

Para ver qual seqüência de caracteres é maior do que a outra, o chamado "dicionário" ou "lexicográfico" é usado.

Em outras palavras, as cordas são comparadas letra a letra.

Por exemplo:

`` `js run
alerta ('Z'> 'A'); // verdade
alerta ('Glow'> 'Glee'); // verdade
alerta ('Bee'> 'Be'); // verdade
`` `

O algoritmo para comparar duas cordas é simples:

1. Compare os primeiros caracteres de ambas as cordas.
2. Se o primeiro for maior (ou menos), a primeira seqüência é maior (ou menor) do que a segunda. Foram realizadas.
3. Caso contrário, se os primeiros caracteres forem iguais, compare os segundos caracteres da mesma maneira.
4. Repita até o final de qualquer string.
5. Se ambas as cordas terminaram simultaneamente, elas são iguais. Caso contrário, a cadeia mais longa é maior.

No exemplo acima, a comparação `'Z'> 'A'` obtém o resultado no primeiro passo.

Strings `" Glow "` e `" Glee "são comparados por personagem:

1. `G` é o mesmo que` G`.
2. `l` é o mesmo que` l`.
3. `o` é maior que` e`. Pare aqui. A primeira string é maior.

`` `smart header =" Não é um dicionário real, mas uma ordem Unicode "
O algoritmo de comparação fornecido acima é aproximadamente equivalente ao usado nos dicionários de livros ou nas listas telefônicas. Mas não é exatamente o mesmo.

Por exemplo, o caso é importante. Uma letra maiúscula `" A "` não é igual à minúscula `" a "`. Qual é o maior? Na verdade, o minúsculo `` a '' é. Por quê? Como o caractere em minúscula possui um índice maior na tabela de codificação interna (Unicode). Voltaremos a detalhes específicos e consequências no capítulo <info: string>.
`` `

## Comparação de diferentes tipos

Quando os valores comparados pertencem a diferentes tipos, eles são convertidos em números.

Por exemplo:

`` `js run
alerta ('2'> 1); // true, string '2' torna-se um número 2
alerta ('01' == 1); // true, string '01' torna-se um número 1
`` `

Para valores booleanos, `true` se torna` 1` e `false` se torna` 0`, é por isso que:

`` `js run
alerta (verdadeiro == 1); // verdade
alerta (falso == 0); // verdade
`` `

`` `` cabeçalho inteligente = "Uma consequência engraçada"
É possível que ao mesmo tempo:

- Dois valores são iguais.
- Um deles é "verdadeiro" como booleano e o outro é "falso" como booleano.

Por exemplo:

`` `js run
deixe a = 0;
alerta (booleano (a)); // falso

deixe b = "0";
alerta (booleano (b)); // verdade

alerta (a == b); // verdade!
`` `

Do ponto de vista de JavaScript, isso é bastante normal. Uma verificação de igualdade converte-se usando a conversão numérica (daí `" 0 "` torna-se '0'), enquanto a conversão 'Booleana' usa outro conjunto de regras.
`` ``

## Estratégia de igualdade

Uma verificação de igualdade regular `" == "` tem um problema. Não pode diferir `0` de` false`:

`` `js run
alerta (0 == falso); // verdade
`` `

O mesmo com uma string vazia:

`` `js run
alerta ('' == falso); // verdade
`` `

Isso ocorre porque operandos de diferentes tipos são convertidos para um número pelo operador de igualdade `==`. Uma string vazia, assim como `falso ', torna-se um zero.

O que fazer se quisermos diferenciar `0 'de` falso'?

** Um operador de igualdade rigoroso `===` verifica a igualdade sem conversão de tipo. **

Em outras palavras, se `a` e` b` forem de tipos diferentes, então `a === b` retornará imediatamente` falso 'sem tentar convertê-los.

Vamos tentar:

`` `js run
alerta (0 === falso); // falso, porque os tipos são diferentes
`` `

Existe também um operador "estrito sem igualdade" `! ==`, como uma analogia para `! =`.

O operador de verificação de igualdade rigorosa é um pouco mais longo para escrever, mas torna óbvio o que está acontecendo e deixa menos espaço para erros.

## Comparação com nulo e indefinido

Vamos ver mais casos de ponta.

Existe um comportamento não intuitivo quando `null` ou` indefinido 'são comparados com outros valores.


Para uma verificação de igualdade rigorosa `===`
: Estes valores são diferentes, porque cada um deles pertence a um tipo separado dele próprio.

`` `js run
alerta (null === indefinido); // falso
`` `

Para uma verificação não-estrita `==`
: Existe uma regra especial. Estes dois são um "casal doce": eles se igualam (no sentido de "=="), mas não qualquer outro valor.

`` `js run
alerta (null == indefinido); // verdade
`` `

Para matemática e outras comparações `<> <=> =`
: Os valores `null / undefined` são convertidos para um número:` null` torna-se `0`, enquanto` undefined` se torna `NaN`.

Agora vejamos coisas engraçadas que acontecem quando aplicamos essas regras. E, o que é mais importante, como não cair em uma armadilha com esses recursos.

### Resultado estranho: null vs 0

Vamos comparar `null` com um zero:

`` `js run
alerta (null> 0); // (1) falso
alerta (null == 0); // (2) falso
alerta (null> = 0); // (3) *! * True * /! *
`` `

Sim, matematicamente, isso é estranho. O último resultado afirma que "null" é maior ou igual a zero ". Então uma das comparações acima deve estar correta, mas ambas são falsas.

A razão é que uma verificação de igualdade `==` e as comparações `> <> = <=` funcionam de forma diferente. Comparações convertem `null` para um número, portanto, trate-o como` 0`. É por isso que (3) `null> = 0` é verdadeiro e (1)` null> 0` é falso.

Por outro lado, a verificação de igualdade `==` para `undefined` e` null` funciona pela regra, sem conversões. Eles se igualam e não equivalem a qualquer outra coisa. É por isso que (2) `null == 0` é falso.

### Um incomparável indefinido

O valor "indefinido" não deve participar em comparações:

`` `js run
alerta (indefinido> 0); // falso (1)
alerta (indefinido <0); // falso (2)
alerta (indefinido == 0); // falso (3)
`` `

Por que não gosta tanto de um zero? Sempre falso!

Nós obtivemos esses resultados porque:

- Comparações `(1)` e `(2)` return `false` porque` undefined` é convertido em `NaN`. E `NaN` é um valor numérico especial que retorna` falso 'para todas as comparações.
- A verificação de igualdade `(3)` retorna `falso ', porque` indefinido' é igual a 'nulo' e nenhum outro valor.

### Evade problemas

Por que observamos esses exemplos? Devemos lembrar essas peculiaridades o tempo todo? Bem, na verdade não. Na verdade, essas coisas difíceis serão gradualmente familiarizadas ao longo do tempo, mas há uma maneira sólida de evadir quaisquer problemas com elas.

Apenas trate qualquer comparação com `undefined / null`, exceto a igualdade rigorosa ===` com cuidado excepcional.

Não use comparações `> => <<=` com uma variável que pode ser "nula / indefinida", a menos que você esteja realmente certo do que está fazendo. Se uma variável pode ter esses valores, verifique-os separadamente.

## Resumo

- Os operadores de comparação retornam um valor lógico.
- As cordas são comparadas letra a letra no pedido "dicionário".
- Quando os valores de diferentes tipos são comparados, eles são convertidos em números (com exclusão de uma verificação de igualdade rigorosa).
- Valores `null` e` undefined` iguais `==` uns aos outros e não são iguais a nenhum outro valor.
- Tenha cuidado ao usar comparações como `>` ou `<` com variáveis ​​que ocasionalmente podem ser 'nulas / indefinidas'. Fazer uma verificação separada para `null / undefined` é uma boa ideia.
