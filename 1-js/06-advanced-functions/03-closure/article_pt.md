
# Fecho

O JavaScript é uma linguagem muito orientada a funções. Dá muita liberdade. Uma função pode ser criada em um momento, depois copiada para outra variável ou passada como argumento para outra função e é chamada de um lugar totalmente diferente depois.

Sabemos que uma função pode acessar variáveis ​​fora dela. E esse recurso é usado com bastante freqüência.

Mas o que acontece quando as variáveis ​​externas mudam? Uma função obtém um valor mais recente ou o que existia quando a função foi criada?

Além disso, o que acontece quando uma função viaja para outro local do código e é chamado a partir daí - isso obtém acesso a variáveis ​​externas no novo local?

Diferentes idiomas se comportam de forma diferente aqui, neste capítulo nós cobrimos JavaScript.

[cortar]

## Algumas perguntas

Vamos formular duas perguntas para a semente, e depois estudar a mecânica interna peça por peça, de modo que você possa responder essas questões e outras mais complexas no futuro.

1. A função `sayHi` usa uma variável externa` name`. Quando a função é executada, qual valor destes dois irá usar?

`` `js
Deixe o nome = "John";

função sayHi () {
alerta ("Olá", + nome);
}

nome = "Pete";

*! *
diga oi(); // o que mostrará: "John" ou "Pete"?
* /! *
`` `

Tais situações são comuns no desenvolvimento do navegador e do servidor. Uma função pode ser programada para ser executada mais tarde do que é criada, por exemplo após uma ação do usuário ou uma solicitação de rede.

Então, a questão é: ele pega as últimas mudanças?


2. A função `makeWorker` faz outra função e retorna. Essa nova função pode ser chamada de outro lugar. Será que terá acesso a variáveis ​​externas a partir do seu local de criação ou o local de invocação ou talvez ambos?

`` `js
function make worker () {
let name = "Pete";

função de retorno () {
alerta (nome);
};
}

Deixe o nome = "John";

// crie uma função
deixe work = make worker ();

// chame-o
*! *
trabalhos(); // o que isso vai mostrar? "Pete" (nome onde foi criado) ou "John" (nome onde é chamado)?
* /! *
`` `


## Lexical Environment

Para entender o que está acontecendo, vamos primeiro discutir o que uma "variável" tecnicamente é.

Em JavaScript, cada função de execução, bloco de código e o script como um todo têm um objeto associado chamado * Ambiente Lexical *.

O objeto de ambiente Lexical consiste em duas partes:

1. * Registro de ambiente * - um objeto que possui todas as variáveis ​​locais como suas propriedades (e algumas outras informações, como o valor de `isto).
2. Uma referência ao * ambiente lexical externo *, geralmente o associado ao código léxicamente logo a fora dele (fora dos suportes de figura atual).

Então, uma "variável" é apenas uma propriedade do objeto interno especial, Environment Record. "Para obter ou alterar uma variável" significa "obter ou alterar a propriedade desse objeto".

Por exemplo, neste código simples, há apenas um ambiente Lexical:

! [ambiente lexical] (lexical-environment-global.png)

Este é um chamado ambiente léxico global, associado ao script completo. Para navegadores, todas as tags `<script>` compartilham o mesmo ambiente global.

Na figura acima, o retângulo significa Registro de ambiente (armazenamento variável) e a seta significa a referência externa. O ambiente Lexical global não tem um externo, então esse é "nulo".

Aqui está a imagem maior de como as variáveis ​​`let` funcionam:

! [ambiente lexical] (lexical-environment-global-2.png)

Retângulos no lado direito demonstram como o Ambiente Lexical global muda durante a execução:

1. Quando o script é iniciado, o Ambiente Lexical está vazio.
2. Aparece a definição `let phrase`. Agora, inicialmente não tem valor, então "indefinido" é armazenado.
3. "fase" é atribuído.
4. `frase` refere-se a um novo valor.

Tudo parece simples por enquanto, certo?

Para resumir:

- Uma variável é uma propriedade de um objeto interno especial, associado ao bloco / função / script atualmente em execução.
- Trabalhar com variáveis ​​realmente está trabalhando com as propriedades desse objeto.

### Declaração de função

Declarações de função são especiais. Ao contrário das variáveis ​​`let`, elas são processadas não quando a execução as atinge, mas quando um ambiente Lexical é criado. Para o ambiente Lexical global, significa o momento em que o script é iniciado.

... E é por isso que podemos chamar uma declaração de função antes de ser definida.

O código abaixo demonstra que o ambiente Lexical não está vazio desde o início. Tem "dizer", porque essa é uma declaração de função. E mais tarde fica `frase ', declarada com` let`:

! [ambiente lexical] (lexical-environment-global-3.png)


### Ambiente Lexical interno e externo

Durante a chamada `say ()` usa uma variável externa, então vejamos os detalhes do que está acontecendo.

Primeiro, quando uma função é executada, uma nova função O ambiente Lexical é criado automaticamente. Essa é uma regra geral para todas as funções. Esse ambiente Lexical é usado para armazenar variáveis ​​locais e parâmetros da chamada.

<! -
`` `js
Deixe a frase = "Olá";

função diga (nome) {
alerta (`$ {frase}, $ {name}`);
}

diga ("John"); // Olá john
`` `
->

Aqui está a imagem de Ambientes Lexicos quando a execução está dentro de `say (" John ")`, na linha rotulada com uma seta:

! [ambiente lexical] (lexical-environment-simple.png)

Durante a chamada de função, temos dois Ambientes Lexicos: o interno (para a chamada de função) e o externo (global):

- O ambiente Lexical interno corresponde à execução atual de `say`. Tem uma única variável: `name`, o argumento da função. Chamamos `say (" John ")`, então o valor de `name` é` "John" `.
- O ambiente Lexical externo é o ambiente Lexical global.

O ambiente Lexical interno tem a referência `externa 'ao externo.

** Quando um código quer acessar uma variável - primeiro é procurado no ambiente Lexical interno, então no exterior, então o mais externo e assim por diante até o final da corrente. **

Se uma variável não for encontrada em qualquer lugar, isso é um erro no modo estrito. Sem `uso estrito ', uma tarefa para uma variável indefinida cria uma nova variável global, para compatibilidade com versões anteriores.

Vamos ver como a pesquisa prossegue no nosso exemplo:

- Quando o `alerta` dentro de` dizer 'quer acessar `nome`, ele o encontra imediatamente na função Ambiente Lexicônico.
- Quando deseja acessar `frase`, então não existe` frase` localmente, então segue a referência 'externa' e acha globalmente.

! [pesquisa do ambiente lexical] (lexical-environment-simple-lookup.png)

Agora, podemos dar a resposta à primeira pergunta de semente desde o início do capítulo.

** Uma função obtém as variáveis ​​externas como são agora, os valores mais recentes. **

Isso é por causa do mecanismo descrito. Os valores das variáveis ​​antigas não são salvos em qualquer lugar. Quando uma função os quer, ele tira os valores atuais do seu próprio ou de um ambiente Lexical externo.

Então, a resposta para a primeira pergunta é 'Pete`:

`` `js run
Deixe o nome = "John";

função sayHi () {
alerta ("Olá", + nome);
}

nome = "Pete"; // (*)

*! *
diga oi(); // Pete
* /! *
`` `


O fluxo de execução do código acima:

1. O ambiente Lexical global tem `name:" John "`.
2. Na linha `(*)` a variável global é alterada, agora tem `name:" Pete "`.
3. Quando a função `say ()`, é executada e leva `name` de fora. Aqui é a partir do ambiente Lexical global onde já é "Pete" `.


`` `smart header =" One call - one Lexical Environment "
Observe que uma nova função O ambiente lexicônico é criado cada vez que uma função é executada.

E se uma função é chamada várias vezes, então cada invocação terá seu próprio ambiente Lexical, com variáveis ​​locais e parâmetros específicos para essa mesma execução.
`` `

`` `smart header =" O ambiente Lexical é um objeto de especificação "
"Ambiente Lexical" é um objeto de especificação. Não podemos obter este objeto em nosso código e manipulá-lo diretamente. Os mecanismos de JavaScript também podem otimizar, descartar variáveis ​​que não são usadas para economizar memória e executar outros truques internos, mas o comportamento visível deve ser conforme descrito.
`` `


## Funções aninhadas

Uma função é chamada de "aninhada" quando é criada dentro de outra função.

Tecnicamente, isso é facilmente possível.

Podemos usá-lo para organizar o código, assim:

`` `js
função sayHiBye (firstName, lastName) {

// função aninhada auxiliar para usar abaixo
function getFullName () {
return firstName + "" + lastName;
}


alerta ("Bye", + getFullName ());

}
`` `

Aqui, a função * * aninhada * getFullName () `é feita por conveniência. Pode acessar as variáveis ​​externas e, assim, pode retornar o nome completo.

O que é mais interessante, uma função aninhada pode ser retornada: como uma propriedade de um novo objeto (se a função externa cria um objeto com métodos) ou como resultado por si só. E depois usou outro lugar. Não importa onde, ele ainda mantém o acesso às mesmas variáveis ​​externas.

Um exemplo com a função do construtor (veja o capítulo <info: constructor-new>):

`` `js run
// função do construtor retorna um novo objeto
função Usuário (nome) {

// o método do objeto é criado como uma função aninhada
this.sayHi = function () {
alerta (nome);
};
}

Deixe usuário = novo Usuário ("João");
user.sayHi (); // o código do método tem acesso ao "nome" externo
`` `

Um exemplo com a devolução de uma função:

`` `js run
função makeCounter () {
Deixe contagem = 0;

função de retorno () {
contagem de retorno ++; // tem acesso ao contador externo
};
}

let counter = makeCounter ();

alerta (contador ()); // 0
alerta (contador ()); // 1
alerta (contador ()); // 2
`` `

Vamos continuar com o exemplo do `makeCounter`. Ele cria a função "contador" que retorna o próximo número em cada invocação. Apesar de ser simples, variantes ligeiramente modificadas desse código têm usos práticos, por exemplo, como um [gerador de números de pseudorandom] (https://en.wikipedia.org/wiki/Pseudorandom_number_generator) e muito mais. Então, o exemplo não é bastante artificial.

Como o contador trabalha internamente?

Quando a função interna é executada, a variável em `count ++` é pesquisada de dentro para fora. Para o exemplo acima, a ordem será:

!] [] (lexical-search-order.png)

1. Os locais da função aninhada.
2. As variáveis ​​da função externa.
3. ... E até chegar a globais.

Nesse exemplo, `count` é encontrado na etapa` 2`. Quando uma variável externa é modificada, ela é alterada onde ela é encontrada. Então `count ++` encontra a variável externa e aumenta no ambiente Lexical onde pertence. Como se tivéssemos "deixado contar = 1".

Aqui estão duas perguntas para você:

1. Podemos de alguma forma redefinir o `counter` do código que não pertence ao` makeCounter`? Por exemplo. depois das chamadas `alert` no exemplo acima.
2. Se chamamos `makeCounter ()` várias vezes - ele retorna muitas funções `counter`. Eles são independentes ou compartilham o mesmo "conde"?

Tente respondê-los antes de continuar lendo.

...Você terminou?

Ok, aqui vamos com as respostas.

1. Não há como chegar. O `counter` é uma variável de função local, não podemos acessá-lo de fora.
2. Para cada chamada para `makeCounter ()` uma nova função, o ambiente Lexical é criado, com seu próprio 'contador'. Assim, as funções `counter` resultantes são independentes.

Aqui está a demonstração:

`` `js run
função makeCounter () {
Deixe contagem = 0;
função de retorno () {
contagem de retorno ++;
};
}

Deixe counter1 = makeCounter ();
Deixe counter2 = makeCounter ();

alerta (counter1 ()); // 0
alerta (counter1 ()); // 1

alerta (contador 2.0 (independente)
`` `


Provavelmente, a situação com variáveis ​​externas é bastante clara para você a partir de agora. Mas, em situações mais complexas, pode ser necessária uma compreensão mais profunda dos internals. Então, vamos em frente.

## Ambientes detalhados

Agora, como você entende como os fechamentos funcionam em geral, podemos finalmente descer para as porcas e parafusos.

Veja o que está acontecendo no exemplo `makeCounter` passo a passo, siga-o para se certificar de que você entende tudo. Observe a propriedade adicional `[[Environment]]` que não cobrimos ainda.

1. Quando o script acabou de começar, existe apenas um ambiente Lexical global:

!] [] (lexenv-anested-makecounter-1.png)

Naquele momento inicial, há apenas função `MakeCounter`, porque é uma Declaração de Função. Ainda não correu.

Todas as funções "no nascimento" recebem uma propriedade oculta `[[Ambiente]]` com a referência ao ambiente Lexical de sua criação. Ainda não falamos sobre isso, mas, tecnicamente, é assim que a função sabe onde foi feita.

Aqui, `makeCounter` é criado no ambiente Lexical global, então` [[Environment]] `mantém a referência a ele.

Em outras palavras, uma função é "impressa" com uma referência ao ambiente Lexical onde nasceu. E `[[Ambiente]]` é a propriedade da função oculta que possui essa referência.

2. Então o código é executado e a chamada para `makeCounter ()` é executada. Aqui está a imagem no momento em que a execução está na primeira linha dentro de `makeCounter ()`:

!] [] (lexenv-anested-makecounter-2.png)

No momento da chamada de `makeCounter ()`, o Ambiente Lexical é criado, para manter suas variáveis ​​e argumentos.

Como todos os ambientes Lexical, ele armazena duas coisas:
1. Um registro de ambiente com variáveis ​​locais. No nosso caso, `count` é a única variável local (aparece nele quando a linha com` let count` é executada).
2. A referência lexical externa, que é definida como `[[Ambiente]]` da função. Aqui [[Ambiente]] `de` makeCounter` faz referência ao ambiente Lexical global.

Então, agora temos dois Ambientes Lexicos: o primeiro é global, o segundo é para o atual chamado `makeCounter`, com a referência externa ao global.

3. Durante a execução do `makeCounter ()`, uma pequena função aninhada é criada.

Não importa se a função é criada usando Function Declaration ou Function Expression. Todas as funções recebem a propriedade `[[Environment]]` que faz referência ao ambiente Lexical onde foram feitas. Então, essa nova e minúscula função aninhada também o obtém.

Para a nossa nova função aninhada, o valor de [[Ambiente]] é o atual Ambiente Lexico de `makeCounter ()` (onde nasceu):

!] [] (lexenv-anested-makecounter-3.png)

Observe que, nesta etapa, a função interna foi criada, mas ainda não foi chamada. O código dentro de `function () {return count ++; } `não está funcionando, nós vamos devolvê-lo.

4. À medida que a execução continua, a chamada para `makeCounter ()` termina e o resultado (a pequena função aninhada) é atribuído à variável global `counter`:

!] [] (lexenv-anested-makecounter-4.png)

Essa função possui apenas uma linha: `return count ++`, que será executada quando a executemos.

5. Quando o `counter ()` é chamado, um ambiente "Lexical" vazio é criado para ele. Não tem variáveis ​​locais por si só. Mas o `[[Ambiente]]` de `counter` é usado como referência externa para ele, então ele tem acesso às variáveis ​​da chamada` makeCounter () `anterior, onde foi criado:

!] [] (lexenv-anested-makecounter-5.png)

Agora, se ele acessa uma variável, primeiro busca seu próprio ambiente Lexical (vazio), depois o Ambiente Lexical da antiga chamada `makeCounter ()`, então a global.

Quando ele procura o `count`, ele o encontra entre as variáveis` makeCounter`, no ambiente Lexical externo mais próximo.

Observe como o gerenciamento de memória funciona aqui. Quando a chamada `makeCounter ()` terminou há algum tempo, o seu Ambiente Lexical foi retido na memória, porque existe uma função aninhada com `[[Environment]]` referenciando-o.

Geralmente, um objeto de ambiente Lexical vive enquanto houver uma função que possa usá-lo. E quando não há, está desmarcada.

6. A chamada para `counter ()` não só retorna o valor de `count`, mas também o aumenta. Observe que a modificação é feita "no lugar". O valor de `count` é modificado exatamente no ambiente onde foi encontrado.

!] [] (lexenv-anested-makecounter-6.png)

Então, retornamos ao passo anterior com a única mudança - o novo valor de `count`. As seguintes chamadas, todas, fazem o mesmo.

7. As invocações `counter ()` em segundo lugar fazem o mesmo.

A resposta para a segunda pergunta de sementes desde o início do capítulo agora deve ser óbvia.

A função `work ()` no código abaixo usa o 'nome' do local de sua origem através da referência de campo lexical externo:

!] [] (lexenv-anested-work.png)

Então, o resultado é "Pete" aqui.

... Mas se não houvesse `let name` em` makeWorker () `, então a pesquisa iria para fora e levaria a variável global como podemos ver da cadeia acima. Nesse caso, seria "John" `.

`` `smart header =" Closures "
Existe um termo geral de programação "encerramento", que os desenvolvedores geralmente devem saber.

Um [fechamento] (https://en.wikipedia.org/wiki/Closure_ (computer_programming)) é uma função que lembra suas variáveis ​​externas e pode acessá-las. Em algumas línguas, isso não é possível, ou uma função deve ser escrita de forma especial para que isso aconteça. Mas, como explicado acima, em JavaScript, todas as funções são naturalmente fechadas (há apenas uma exclusão, para ser abordada em <info: new-function>).

Isto é: eles se lembram automaticamente de onde eles são criados usando uma propriedade [[Ambiente]] `escondida e todos podem acessar variáveis ​​externas.

Quando em uma entrevista, um desenvolvedor frontend obtém uma pergunta sobre "o que é um encerramento?", Uma resposta válida seria uma definição do encerramento e uma explicação de que todas as funções em JavaScript são fechamentos e, talvez, mais algumas palavras sobre detalhes técnicos: [[Ambiente]] `propriedade e como os ambientes Lexical funcionam.
`` `

## Code blocks and loops, IIFE

Os exemplos acima concentraram-se em funções. Mas os ambientes Lexical também existem para blocos de código `{...}`.

Eles são criados quando um bloco de código é executado e contém variáveis ​​locais de bloco. Aqui estão alguns exemplos.

## E se

No exemplo abaixo, quando a execução entra no bloco `if`, o novo ambiente Lexical" if-only "é criado para ele:

<! -
`` `js run
Deixe a frase = "Olá";

se for verdade) {
Deixe o usuário = "John";

alerta (`$ {frase}, $ {usuário}`); // Olá john
}

alerta (usuário); // Erro, não consigo ver essa variável!
`` `
->

!] [] (lexenv-if.png)

O novo ambiente Lexical obtém o anexo como a referência externa, portanto, a frase pode ser encontrada. Mas todas as variáveis ​​e expressões de função declaradas dentro de `if` residem nesse ambiente Lexical e não podem ser vistas de fora.

Por exemplo, depois que `if` termina, o 'alerta' abaixo não verá o` usuário ', daí o erro.

## Por enquanto

Para um loop, cada execução possui um ambiente Lexical separado. Se a variável for declarada em `for`, então também é local para esse ambiente Lexical:

`` `js run
para (vamos i = 0; i <10; i ++) {
// Cada ciclo possui seu próprio ambiente Lexical
// {Eu valorizo}
}

alerta (i); // Erro, nenhuma variável desse tipo
`` `

Essa é realmente uma exceção, porque "deixei" estar visualmente fora do `{...}`. Mas, de fato, cada rodada do loop possui seu próprio ambiente Lexical com o `i` atual nele.

Após o loop, `i` não está visível.

### Blocos de código

Também podemos usar um bloco de código "nulo" `{...}` para isolar variáveis ​​em um "escopo local".

Por exemplo, em um navegador da Web, todos os scripts compartilham a mesma área global. Então, se criarmos uma variável global em um script, ele fica disponível para outros. Mas isso se torna uma fonte de conflitos se dois scripts usam o mesmo nome de variável e se substituem.

Isso pode acontecer se o nome da variável for uma palavra generalizada e os autores dos roteiros não se conhecem.

Se quisermos evadir isso, podemos usar um bloco de código para isolar todo o script ou uma área nele:

`` `js run
{
// faça algum trabalho com variáveis ​​locais que não devem ser vistas fora

Deixe a mensagem = "Olá";

alerta (mensagem); // Olá
}

alerta (mensagem); // Erro: a mensagem não está definida
`` `

O código fora do bloco (ou dentro de outro script) não vê variáveis ​​nele, porque um bloco de código possui seu próprio ambiente Lexical.

### IIFE

Em scripts antigos, pode-se encontrar as chamadas "expressões de função invocadas imediatamente" (abreviado como IIFE) usado para este propósito.

Eles se parecem com isto:

`` `js run
(function () {

Deixe a mensagem = "Olá";

alerta (mensagem); // Olá

}) ();
`` `

Aqui, uma expressão de função é criada e imediatamente chamada. Então o código é executado agora e tem suas próprias variáveis ​​particulares.

A expressão de função é enrolada com parênteses `(função {...})`, porque quando o JavaScript atende a "função" no fluxo de código principal, ele entende como um começo de Declaração de Função. Mas uma Declaração de Função deve ter um nome, então haverá um erro:

`` `js run
// Erro: token inesperado (
function () {// <- JavaScript não consegue encontrar o nome da função, atende (e dá erro

Deixe a mensagem = "Olá";

alerta (mensagem); // Olá

} ();
`` `

Podemos dizer "tudo bem, deixe-o ser Declaração de Função, vamos adicionar um nome", mas não funcionará. O JavaScript não permite que declarações de função sejam chamadas imediatamente:

`` `js run
// erro de sintaxe devido a suportes abaixo
function go () {

} (); // <- não pode chamar declaração de função imediatamente
`` `

... Portanto, os suportes são necessários para mostrar JavaScript que a função é criada no contexto de outra expressão e, portanto, é uma expressão de função. Não precisa de nenhum nome e pode ser chamado imediatamente.

Existem outras maneiras de dizer ao JavaScript que queremos dizer Expressão Funcional:

`` `js run
// Formas de criar IIFE

(function () {
alerta ("Suportes em torno da função");
} *! *) * /! * ();

(function () {
alerta ("Brackets around the whole thing");
} () *! *) * /! *;

*! *! * /! * function () {
alerta ("O operador bitwise NOT inicia a expressão");
} ();

*! * + * /! * function () {
alerta ("Unary plus inicia a expressão");
} ();
`` `

Em todos os casos acima, declaramos uma Expressão de Função e executá-la imediatamente.

## Coleta de lixo

Objetos de ambiente Lexical sobre os quais estamos falando são sujeitos às mesmas regras de gerenciamento de memória que valores comuns.

- Geralmente, o ambiente Lexical é limpo após a execução da função. Por exemplo:

`` `js
função f () {
Deixe value1 = 123;
deixe valor2 = 456;
}

f ();
`` `

Aqui, dois valores são tecnicamente as propriedades do ambiente Lexical. Mas depois de `f ()` terminar, o ambiente Lexical torna-se inacessível, então ele é excluído da memória.

- ... Mas se existe uma função aninhada que ainda é alcançável após o final de `f`, a respectiva referência` [[Environment]] `também mantém o ambiente lexical externo:

`` `js
função f () {
Deixe valor = 123;

função g () {alerta (valor); }

*! *
retornar g;
* /! *
}

deixe g = f (); // g é acessível e mantém o ambiente lexical externo na memória
`` `

- Observe que se `f ()` for chamado muitas vezes e as funções resultantes forem salvas, os objetos correspondentes do ambiente Lexical também serão mantidos na memória. Todos os 3 deles no código abaixo:

`` `js
função f () {
Deixe value = Math.random ();

função de retorno () {alert (value); };
}

// 3 funções em matriz, cada uma delas liga ao ambiente Lexical
// da correspondente f () executar
// THE LE
deixe arr = [f (), f (), f ()];
`` `

- Um objeto de ambiente Lexical morre quando se torna inacessível. Isto é: quando não há funções aninhadas que a referenciem. No código abaixo, após `g` se tornar inacessível, o` valor` também é limpo da memória;

`` `js
função f () {
Deixe valor = 123;

função g () {alerta (valor); }

retornar g;
}

deixe g = f (); // enquanto g está vivo
// o ambiente Lexical correspondente mora

g = nulo; // ... e agora a memória é limpa
`` `

### Otimizações da vida real

Como vimos, em teoria, enquanto uma função está viva, todas as variáveis ​​externas também são mantidas.

Mas na prática, os motores JavaScript tentam otimizar isso. Eles analisam o uso variável e, se é fácil ver que uma variável externa não é usada - ela é removida.

** Um efeito secundário importante no V8 (Chrome, Opera) é que essa variável ficará indisponível na depuração. **

Tente executar o exemplo abaixo com as Ferramentas de desenvolvedor abertas no Chrome.

Quando faz uma pausa, no tipo de console `alert (value)`.

`` `js run
função f () {
Deixe value = Math.random ();

função g () {
depurador; // no console: digite o alerta (valor); Nenhuma dessas variáveis!
}

retornar g;
}

deixe g = f ();
g ();
`` `

Como você pode ver - não existe essa variável! Em teoria, deve ser acessível, mas o motor otimizou.

Isso pode levar a problemas de depuração engraçados (se não tão demorado). Um deles - podemos ver uma mesma variável externa em vez do esperado:

`` `js run global
Deixe value = "Surprise!";

função f () {
Deixe valor = "o valor mais próximo";

função g () {
depurador; // no console: digite o alerta (valor); Surpresa!
}

retornar g;
}

deixe g = f ();
g ();
`` `

`` `warn header =" Veja você! "
Esse recurso do V8 é bom de saber. Se você estiver depurando com o Chrome / Opera, mais cedo ou mais tarde você irá encontrá-lo.

Isso não é um bug de depurador, mas uma característica especial do V8. Talvez seja alterado algum tempo.
Você sempre pode verificar se executa exemplos nesta página.
`` `
