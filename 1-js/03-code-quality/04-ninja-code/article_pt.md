# Código Ninja

Os ninjas do programador do passado usaram esses truques para que os mantenedores de código chorem. Os gurus de revisão de código os procuram em tarefas de teste. Os desenvolvedores novatos às vezes os usam melhor do que os ninjas programadores.

Leia-os cuidadosamente e descubra quem você é - um ninja, um novato ou talvez um revisor de código?

[cortar]

`` `warn header =" Irony detectado "
Estas são regras de escrita do código ruim. Só ... Você sabe, algumas pessoas perdem o ponto.
`` `

## Brevity é a alma do sagacidade

Faça o código o mais curto possível. Mostre o quão inteligente você é.

Deixe características de linguagem sutil guiá-lo.

Por exemplo, dê uma olhada neste operador ternário `'?'`:

`` `js
// tirado de uma conhecida biblioteca de javascript
i = i? eu <0? Math.max (0, len + i): i: 0;
`` `

Legal certo? Se você escrever assim, o desenvolvedor que se deparar com esta linha e tenta entender qual é o valor de `i` vai ter um tempo feliz. Então venha até você, procurando uma resposta.

Diga-lhe que mais curto é sempre melhor. Iniciá-lo nos caminhos do ninja.

## Variáveis ​​de uma letra

`` `quote author =" Laozi (Tao Te Ching) "
O Dao se esconde em sem palavras. Apenas o Dao está bem iniciado e bem
completado.
`` `

Outra maneira de codificar mais rápido (e muito pior!) É usar nomes de variáveis ​​de uma única letra em todos os lugares. Como `a`,` b` ou `c`.

Uma variável curta desaparece no código como um ninja real na floresta. Ninguém poderá encontrá-lo usando "pesquisa" do editor. E mesmo que alguém o faça, ele não poderá "decifrar" o que significa "a" ou "b".

... Mas há uma exceção. Um ninja real nunca usará `i` como o contador em um loop` `para ''. Em qualquer lugar, mas não aqui. Olhe ao redor, há muitas letras mais exóticas. Por exemplo, `x` ou` y`.

Uma variável exótica como um contador de loop é especialmente legal se o corpo do loop for necessário de 1-2 páginas (torná-lo mais longo se você puder). Então, se alguém olha profundamente no loop, ele não poderá descobrir rapidamente que a variável chamada `x` é o contador de contornos.

## Use abreviaturas

Se as regras da equipe proíbem o uso de nomes de uma letra e vagos - encostam-nas, faça abreviaturas.

Como isso:

- `list` ->` lst`.
- `userAgent` ->` ua`.
- `browser` ->` best`.
- ... etc

Somente aquele com verdadeira intuição poderá entender esses nomes. Tente encurtar tudo. Apenas uma pessoa digna deve poder manter o desenvolvimento do seu código.

## Soar alto. Seja abstrato.

`` `quote author =" Laozi (Tao Te Ching) "
A ótima praça é sem canto
O grande navio é o último completo, <br>
A grande nota é um som rarificado, <br>
A imagem excelente não tem forma.
`` `

Ao escolher um nome, tente usar a palavra mais abstrata. Como `obj`,` data`, `value`,` item`, `elem` e assim por diante.

- ** O nome ideal para uma variável é `data`. ** Use-o em qualquer lugar que você puder. De fato, cada variável contém * dados *, certo?

... Mas o que fazer se "dados" já estiverem ocupados? Experimente o "valor", também é universal. Afinal, uma variável eventualmente recebe um * valor *.

- ** Nome uma variável por seu tipo: `str`,` num` ... **

Experimente. Um jovem ninja pode se perguntar - esses nomes tornam o código pior? Na verdade sim!

De um lado, o nome da variável ainda significa algo. Ele diz o que está dentro da variável: uma string, um número ou outra coisa. Mas quando um estranho tenta entender o código, ele ficará surpreso ao ver que na verdade não há nenhuma informação!

Na verdade, o tipo de valor é fácil de descobrir por depuração. Mas qual é o significado da variável? Qual cadeia / número armazena? Não há como descobrir sem uma boa meditação!

- ** ... Mas e se não houver mais nomes desse tipo? ** Apenas adicione um número: `data1, item2, elem5` ...

## Teste de atenção

Somente um programador verdadeiramente atento deve ser capaz de entender o código. Mas como verificar isso?

** Uma das formas - use nomes de variáveis ​​semelhantes, como `data` e` data`. **

Misture-os onde você puder.

Uma leitura rápida desse código torna-se impossível. E quando há um erro de digitação ... Ummm ... Estamos presos durante muito tempo para beber chá.


## Smart synonyms

`` `quote author =" Confucius "
O mais difícil de tudo é encontrar um gato preto em uma sala escura, especialmente se não houver gato.
`` `

O uso de * nomes similares * para * mesmas coisas * torna a vida mais interessante e mostra sua criatividade para o público.

Por exemplo, considere prefixos de função. Se uma função mostra uma mensagem na tela - comece com `display ...`, como `displayMessage`. E então, se outra função mostrar na tela algo mais, como um nome de usuário, comece com `show ...` (como `showName`).

Insinuando que existe uma diferença sutil entre essas funções, enquanto não existe.

Faça um pacto com outros ninjas da equipe: se John começar a "mostrar" funções com `display ...` em seu código, então Peter poderia usar `render ...` e Ann -` paint ... `. Observe quanto mais interessante e diversificado o código se tornou.

... E agora o hat trick!

Para duas funções com diferenças importantes - use o mesmo prefixo!

Por exemplo, a função `printPage (página)` usará uma impressora. E a função `printText (texto)` colocará o texto na tela. Deixe um leitor desconhecido pensar bem sobre a função denominada `printMessage`:" Onde coloca a mensagem? Para uma impressora ou na tela? ". Para que ele realmente brilhe, `printMessage (mensagem)` deve emiti-lo na nova janela!

## Reutilizar nomes

`` `quote author =" Laozi (Tao Te Ching) "
Uma vez que o todo está dividido, as partes <br>
precisa de nomes.
Já existem nomes suficientes.
É preciso saber quando parar.
`` `

Adicione uma nova variável somente quando absolutamente necessário.

Em vez disso, reutilize os nomes existentes. Basta escrever novos valores neles.

Em uma função tente usar apenas variáveis ​​passadas como parâmetros.

Isso tornaria realmente difícil identificar o que é exatamente na variável * agora *. E também de onde vem. Uma pessoa com intuição fraca teria que analisar o código linha por linha e acompanhar as mudanças através de cada ramo de código.

** Uma variante avançada da abordagem é secretamente (!) Substituir o valor por algo semelhante no meio de um loop ou uma função. **

Por exemplo:

`` `js
função ninjaFunction (elemento) {
// 20 linhas de código que trabalham com elem

elemento = clone (elemento);

// mais 20 linhas, agora trabalhando com o clone do elem!
}
`` `

Um programador companheiro que quer trabalhar com `elem` na segunda metade da função ficará surpreso ... Somente durante a depuração, depois de examinar o código, ele descobrirá que ele está trabalhando com um clone!

Deadly effective mesmo contra um ninja experiente. Visto regularmente no código.

## Submissões de diversão

Coloque underscores `_` e` __` antes dos nomes das variáveis. Como `_name` ou` __value`. Seria ótimo se você soubesse seu significado. Ou, melhor, adicione-os apenas por diversão, sem um significado particular. Ou diferentes significados em diferentes lugares.

Você mata dois coelhos com um tiro. Primeiro, o código torna-se mais longo e menos legível, e o segundo, um colaborador pode passar muito tempo tentando descobrir o que os sublinhados significam.

Um ninja inteligente coloca underscores em um ponto do código e evade-os em outros lugares. Isso torna o código ainda mais frágil e aumenta a probabilidade de erros futuros.

## Mostre seu amor

Deixe todos verem quão magníficas são suas entidades! Nomes como `superElement`,` megaFrame` e `niceItem` definitivamente iluminarão um leitor.

Na verdade, de uma mão, algo está escrito: "super ...", "mega", "bom". Mas, por outro lado, não traz detalhes. Um leitor pode decidir procurar um significado oculto e meditar por uma hora ou duas.

## Sobreposição de variáveis ​​externas

`` `quote author =" GU case yin Z i "
Quando na luz, não pode ver nada na escuridão.
Quando na escuridão, pode ver tudo na luz.
`` `

Use os mesmos nomes para variáveis ​​dentro e fora de uma função. Tão simples. Não é necessário nenhum esforço.

`` `js
Deixe *! * user * /! * = authenticateUser ();

função render () {
deixe *! * user * /! * = anotherValue ();
...
... muitas linhas ...
...
... // <- um programador quer trabalhar com o usuário aqui e ...
...
}
`` `

Um programador que salta dentro do `render` provavelmente não perceberá que há um` usuário 'local que soa o externo.

Então ele tentará trabalhar com `usuário 'assumindo que é a variável externa, o resultado de` authenticateUser () `... A armadilha é suspensa! Olá, depurador ...


## Efeitos secundários em todos os lugares!

Existem funções que parecem não mudar nada. Como `isReady ()`, `checkPermission ()`, `findTags ()` ... Eles são assumidos para realizar cálculos, encontrar e retornar os dados, sem alterar nada fora deles. Em outras palavras, sem "efeitos colaterais".

** Um truque realmente bonito é adicionar uma ação "útil" a eles, além da tarefa principal. **

A expressão de surpresa atordoada no rosto de seu colega quando ele vê uma função chamada `is..`,` check ...` ou `find ...` mudando algo - definitivamente irá ampliar seus limites de razão.

** Outra maneira de surpreender é retornar um resultado não padrão. **

Mostre seu pensamento original! Deixe a chamada de `checkPermission` não retornar` true / false`, mas um objeto complexo com os resultados do cheque.

Os desenvolvedores que tentam escrever `if (checkPermission (..))`, vão se perguntar por que isso não funciona. Diga-lhes: "Leia os documentos!". E dê este artigo.


## Funções poderosas!

`` `quote author =" Laozi (Tao Te Ching) "
O grande Tao flui em todos os lugares, <br>
tanto para a esquerda como para a direita.
`` `

Não limite a função pelo que está escrito em seu nome. Seja mais amplo.

Por exemplo, uma função `validateEmail (email)` poderia (além de verificar o email para a correção) mostrar uma mensagem de erro e pedir para voltar a inserir o e-mail.

As ações adicionais não devem ser óbvias a partir do nome da função. Um verdadeiro codificador ninja os tornará óbvios do código também.

** A união de várias ações em uma protege seu código de reutilização. **

Imagine, outro desenvolvedor só quer verificar o e-mail e não enviar qualquer mensagem. Sua função `validateEmail (email)` que faz ambos não será adequado a ele. Então ele não vai quebrar sua meditação perguntando nada sobre isso.

## Resumo

Todos os "conselhos" acima são do código real ... Às vezes, escrito por desenvolvedores experientes. Talvez ainda mais experiente do que você é;)

- Siga alguns deles, e seu código ficará cheio de surpresas.
- Siga muitos deles, e seu código se tornará verdadeiramente seu, ninguém gostaria de mudá-lo.
- Siga tudo, e seu código se tornará uma lição valiosa para os jovens desenvolvedores que procuram esclarecimentos.
