
# Function object, NFE

Como já sabemos, as funções em JavaScript são valores.

Todo o valor em JavaScript tem o tipo. Que tipo de valor é uma função?

Em JavaScript, uma função é um objeto.

Uma boa maneira de imaginar funções é como "objetos de ação" chamáveis. Nós não podemos apenas chamá-los, mas também tratá-los como objetos: adicionar / remover propriedades, passar por referência etc.


## A propriedade "name"

Os objetos de função contêm algumas propriedades às vezes utilizáveis.

Por exemplo, um nome de função está acessível como a propriedade "name":

`` `js run
função sayHi () {
alerta ("Olá");
}

alerta (sayHi.name); // diga oi
`` `

O que é mais engraçado, a lógica de atribuição de nomes é inteligente. Ele também adere o nome certo para função que é usada em tarefas:

`` `js run
deixe sayHi = function () {
alerta ("Olá");
}

alerta (sayHi.name); // sayHi (works!)
`` `

Também funciona se a atribuição for feita através de um valor padrão:

`` `js run
função f (sayHi = function () {}) {
alerta (sayHi.name); // sayHi (works!)
}

f ();
`` `

Na especificação, esse recurso é chamado de "nome contextual". Se a função não fornecer um, então, em uma tarefa, é descoberto no contexto.

Métodos de objetos também têm nomes:

`` `js run
Deixe o usuário = {

SayHi () {
// ...
},

sayBye: function () {
// ...
}

}

alerta (user.sayHi.name); // diga oi
alerta (user.sayBye.name); // diga tchau
`` `

Ainda não há mágica. Há casos em que não há como descobrir o nome certo.

Então está vazio, como aqui:

`` `js
// função criada dentro da matriz
Deixe arr = [function () {}];

alerta (arr [0] .name); // <string vazio>
// o motor não tem como configurar o nome certo, então não há nenhum
`` `

Na prática, a maioria das funções tem um nome.

## A propriedade "length"

Existe outra propriedade interna "comprimento" que retorna o número de parâmetros de função, por exemplo:

`` `js run
função f1 (a) {}
função f2 (a, b) {}
Funcione muitos (a, b, ... mais) {}

alerta (f1.length); // 1
alerta (f2.length); // 2
alerta (many.length); // 2
`` `

Aqui podemos ver que os parâmetros de descanso não são contados.

A propriedade `length` às vezes é usada para introspecção em funções que operam em outras funções.

Por exemplo, no código abaixo, a função `ask` aceita uma 'pergunta' a perguntar e um número arbitrário de funções` handler` para chamar.

Quando um usuário responde, ele chama os manipuladores. Podemos passar dois tipos de manipuladores:

- Uma função de argumento zero, então é exigido apenas uma resposta positiva.
- Uma função com argumentos, então é chamada em qualquer caso e recebe a resposta.

A idéia é que temos uma sintaxe simples de manipulador de argumentos para casos positivos (variante mais freqüente), mas também permite a manipuladores universais.

Para chamar `handlers` da maneira correta, examinamos a propriedade` length`:

`` `js run
função perguntar (pergunta, ... manipuladores) {
let is Sim = confirmar (pergunta);

para (deixe o manipulador de manipuladores) {
se (handler.length == 0) {
Se (is Sim) manipulador ();
} outro {
manipulador (isSUes);
}
}

}

// para resposta positiva, ambos os manipuladores são chamados
// para resposta negativa, apenas o segundo
pergunte ("Pergunta?", () => alerta ('Você disse que sim'), resultado => alerta (resultado));
`` `

Este é um caso particular do chamado [polimorfismo] (https://en.wikipedia.org/wiki/Polymorphism_ (computer_science)) - tratando os argumentos de forma diferente, dependendo do tipo ou, em nosso caso, dependendo do `length` . A idéia é utilizada em bibliotecas de JavaScript.

## Propriedades personalizadas

Nós também podemos adicionar propriedades próprias.

Aqui, adicionamos a propriedade `counter` para rastrear a contagem total de chamadas:

`` `js run
função sayHi () {
alerta ("Olá");

*! *
// vamos contar quantas vezes corremos
sayHi.counter ++;
* /! *
}
sayHi.counter = 0; // valor inicial

diga oi(); // Oi
diga oi(); // Oi

alerta ('Chamado $ {sayHi.counter} times`); // Chamado 2 vezes
`` `

`` `warn header =" Uma propriedade não é uma variável "
Uma propriedade atribuída a uma função como `sayHi.counter = 0` não * define * uma variável local` contador 'dentro dela. Em outras palavras, uma propriedade `counter` e uma variável` let counter` são duas coisas não relacionadas.

Podemos tratar uma função como um objeto, armazenar propriedades nele, mas isso não tem efeito em sua execução. As variáveis ​​nunca usam propriedades de função e vice-versa. Estas são apenas palavras paralelas.
`` `

As propriedades das funções podem substituir o fechamento às vezes. Por exemplo, podemos reescrever o exemplo do contador do capítulo <info: closing> para usar uma propriedade de função:

`` `js run
função makeCounter () {
// ao invés de:
// let count = 0

contador de função () {
retornar counter.count ++;
};

counter.count = 0;

contador de retorno;
}

let counter = makeCounter ();
alerta (contador ()); // 0
alerta (contador ()); // 1
`` `

O `count` agora está armazenado na função diretamente, não em seu ambiente Lexical externo.

É pior ou melhor do que usar o fechamento?

A principal diferença é que se o valor de `count` morar em uma variável externa, um código externo não consegue acessar. Somente funções aninhadas podem modificá-lo. E se for obrigado a funcionar, então é possível:

`` `js run
função makeCounter () {

contador de função () {
retornar counter.count ++;
};

counter.count = 0;

contador de retorno;
}

let counter = makeCounter ();

*! *
counter.count = 10;
alerta (contador ()); // 10
* /! *
`` `

Então depende dos nossos objetivos qual variante escolher.

## Named Function Expression

Expressão de Função Nomeada ou, em breve, NFE, é um termo para Expressões de Função que possuem um nome.

Por exemplo, vamos tomar uma Expressão de Função comum:

`` `js
deixe sayHi = function (who) {
alerta (`Hello, $ {who}`);
};
`` `

... E adicione um nome para ele:

`` `js
vamos dizerHi = função *! * func * /! * (who) {
alerta (`Hello, $ {who}`);
};
`` `

Fizemos algo sã aqui? Qual é o papel desse nome adicional `` func ''?

Primeiro, notemos que ainda temos uma expressão de função. Adicionar o nome `` func '' após a 'função' não fez uma Declaração de Função, porque ainda é criada como parte de uma expressão de atribuição.

Adicionar esse nome também não quebrou nada.

A função ainda está disponível como `sayHi ()`:

`` `js run
vamos dizerHi = função *! * func * /! * (who) {
alerta (`Hello, $ {who}`);
};

sayHi ("John"); // Olá john
`` `

Existem duas coisas especiais sobre o nome `func`:

1. Permite referenciar a função por dentro de si.
2. Não é visível fora da função.

Por exemplo, a função `sayHi` abaixo re-chama-se com` "Guest" `se não` who` for fornecido:

`` `js run
vamos dizerHi = função *! * func * /! * (who) {
se (quem) {
alerta (`Hello, $ {who}`);
} outro {
*! *
func ("Convidado"); // use func para re-chamar-se
* /! *
}
};

diga oi(); // Olá, convidado

// Mas isso não vai funcionar:
func (); // Erro, func não está definido (não visível fora da função)
`` `

Por que usamos `func`? Talvez apenas use `sayHi` para a chamada aninhada?


Na verdade, na maioria dos casos podemos:

`` `js
deixe sayHi = function (who) {
se (quem) {
alerta (`Hello, $ {who}`);
} outro {
*! *
sayHi ("Convidado");
* /! *
}
};
`` `

O problema com esse código é que o valor de `sayHi` pode mudar. A função pode ir para outra variável, e o código começará a dar erros:

`` `js run
deixe sayHi = function (who) {
se (quem) {
alerta (`Hello, $ {who}`);
} outro {
*! *
sayHi ("Convidado"); // Erro: sayHi não é uma função
* /! *
}
};

deixe bem-vinda = sayHi;
sayHi = null;

bem vinda(); // Erro, a chamada aninhada sayHi não funciona mais!
`` `

Isso ocorre porque a função leva `sayHi` do seu ambiente lexical externo. Não há "sayHi" local, então a variável externa é usada. E no momento da chamada que o exterior `sayHi` é 'null`.

O nome opcional que podemos colocar na expressão de função é exatamente para resolver este tipo de problemas.

Vamos usá-lo para corrigir o código:

`` `js run
vamos dizerHi = função *! * func * /! * (who) {
se (quem) {
alerta (`Hello, $ {who}`);
} outro {
*! *
func ("Convidado"); // Agora tudo bem
* /! *
}
};

deixe bem-vinda = sayHi;
sayHi = null;

bem vinda(); // Olá, convidado (chamada aninhada funciona)
`` `

Agora funciona, porque o nome `` func '' é função-local. Não é retirado do exterior (e não está visível lá). A especificação garante que sempre faz referência à função atual.

O código externo ainda tem variável `sayHi` ou` welcome` mais tarde. E `func` é um" nome de função interna ", como ele se chama em particular.

`` `cabeçalho inteligente =" Não há tal coisa para Declaração de Função "
O recurso "nome interno" descrito aqui está disponível apenas para Expressões de Função, e não para Declarações de Função. Para declarações de função, não há nenhuma possibilidade de sintaxe para adicionar um nome "interno" mais um.

Às vezes, quando precisamos de um nome interno confiável, é motivo para reescrever uma declaração de função para o formulário de expressão de função com nome.
`` `

## Resumo

As funções são objetos.

Aqui cobrimos suas propriedades:

- `name` - o nome da função. Existe não apenas quando indicado na definição da função, mas também para atribuições e propriedades do objeto.
- `length` - o número de argumentos na definição da função. Os parâmetros de repouso não são contados.

Se a função for declarada como uma Expressão de Função (não no fluxo do código principal), e ela carrega o nome, então é chamada Expressão de Função Nomeada. O nome pode ser usado dentro para se referir, para chamadas recursivas ou tais.

Além disso, as funções podem ter propriedades adicionais. Muitas bibliotecas de JavaScript bem conhecidas fazem um ótimo uso desse recurso.

Eles criam uma função "principal" e atribuem-lhe muitas outras funções "auxiliares". Por exemplo, a biblioteca [jquery] (https://jquery.com) cria uma função chamada `$`. A biblioteca [lodash] (https://lodash.com) cria uma função `_`. E depois acrescenta `_.clone`,` _.keyBy` e outras propriedades para (veja [docs] (https://lodash.com/docs) quando quiser saber mais sobre eles). Na verdade, eles fazem isso para poluir menos o espaço global, de modo que uma única biblioteca ofereça apenas uma variável global. Isso reduz a possibilidade de possíveis conflitos de nomeação.

Assim, uma função pode fazer um trabalho útil por si só e também ter um monte de outras funcionalidades em propriedades.
