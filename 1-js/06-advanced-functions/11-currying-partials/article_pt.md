libs:
- Lodsh

---

# Currying e parciais

Até agora só estávamos falando sobre "isso". Agora vamos dar um passo adiante.

Nós podemos ligar não só `this`, mas também argumentos. Isso raramente é feito, mas às vezes pode ser útil.

[cortar]

A sintaxe completa de `bind`:

`` `js
deixe bound = func.bind (context, arg1, arg2, ...);
`` `

Ele permite vincular contexto como `this` e iniciar argumentos da função.

Por exemplo, temos uma função de multiplicação `mul (a, b)`:

`` `js
função matlab) {
retorno a * b;
}
`` `

Vamos usar `bind` para criar uma função` double` na base:

`` `js run
*! *
Deixe duplo = mul.bind (null, 2);
* /! *

alerta (duplo (3)); // = mul (2, 3) = 6
alerta (duplo (4)); // = mul (2, 4) = 8
alerta (duplo (5)); // = mul (2, 5) = 10
`` `

A chamada para `mul.bind (null, 2)` cria uma nova função `double` que passa chamadas para` mul`, corrigindo `null` como o contexto e` 2` como o primeiro argumento. Outros argumentos são passados ​​"tal como está".

Isso é chamado de [aplicação de função parcial] (https://en.wikipedia.org/wiki/Partial_application) - criamos uma nova função, corrigindo alguns parâmetros da existente.

Por favor, note que aqui, na verdade, não usamos `this` aqui. Mas `bind` exige isso, então devemos colocar algo como 'null`.

A função `triple` no código abaixo triplica o valor:

`` `js run
*! *
deixe triple = mul.bind (null, 3);
* /! *

alerta (triplo (3)); // = mul (3, 3) = 9
alerta (triplo (4)); // = mul (3, 4) = 12
alerta (triplo (5)); // = mul (3, 5) = 15
`` `

Por que geralmente fazemos uma função parcial?

Aqui, nosso benefício é que criamos uma função independente com um nome legível (`double`,` triple`). Podemos usá-lo e não escrever o primeiro argumento de todas as vezes, porque ele é corrigido com `bind`.

Em outros casos, a aplicação parcial é útil quando temos uma função muito genérica e queremos uma variante menos universal por conveniência.

Por exemplo, temos uma função `send (from, to, text)`. Então, dentro de um objeto de "usuário", podemos querer usar uma variante parcial: `sendTo (to, text)` que envia do usuário atual.

## Going partial without context

E se quisermos corrigir alguns argumentos, mas não vincular isso?

O `bind` nativo não permite isso. Não podemos simplesmente omitir o contexto e avançar para os argumentos.

Felizmente, uma função "parcial" para vinculação de argumentos apenas pode ser facilmente implementada.

Como isso:

`` `js run
*! *
função parcial (func, ... argsBound) {
função de retorno (... args) {// (*)
return func.call (this, ... argsBound, ... args);
}
}
* /! *

// Uso:
Deixe o usuário = {
primeiro nome: "John",
diga (tempo, frase) {
alerta (`[$ {time}] $ {this.firstName}: $ {frase}!`);
}
};

// adicione um método parcial que diz algo agora corrigindo o primeiro argumento
user.sayNow = parcial (user.say, new Date (). getHours () + ':' + new Date (). getMinutes ());

user.sayNow ("Olá");
// Algo como:
// [10:00] Olá, John!
`` `

O resultado de `partial (func [, arg1, arg2 ...]) 'call is a wrapper` (*) `que chama` func` com:
- O mesmo "isto", pois ele obtém (para "user.sayNow" chamar de "usuário")
- Então dá-lhe `... argsBound` - argumentos da chamada 'parcial' (` "10:00" `)
- Então dá-lhe `... args` - argumentos dados ao wrapper (` "Hello" `)

Então, é fácil fazê-lo com o operador de propagação, certo?

Além disso, há uma implementação [_.partial] (https://lodash.com/docs#partial) pronta da biblioteca lodash.

## Currying

Às vezes, as pessoas misturam o aplicativo de função parcial mencionado acima com outra coisa chamada "currying". Essa é outra técnica interessante de trabalhar com funções que apenas temos que mencionar aqui.

[Currying] (https://en.wikipedia.org/wiki/Currying) está traduzindo uma função de callable como `f (a, b, c)` em callable como `f (a) (b) (c)` .

Vamos fazer a função `curry` que executa currying para funções binárias. Em outras palavras, traduz `f (a, b)` em `f (a) (b)`:

`` `js run
*! *
função curry (func) {
função de retorno (a) {
função de retorno (b) {
return func (a, b);
};
};
}
* /! *

// uso
soma de função (a, b) {
Retornar a + b;
}

deixe transportado Sum = curry (soma);

alerta (transportado (1) (2)); // 3
`` `

Como você pode ver, a implementação é uma série de invólucros.

- O resultado de `curry (func)` é uma função do wrapper `(a)`.
- Quando é chamado como `sum (1)`, o argumento é salvo no Ambiente Lexical e um novo wrapper é retornado `function (b)`.
- Então `soma (1) (2)` finalmente chama `função (b)` fornecendo `2`, e passa a chamada para a" soma "multi-argumento original.

Implementações mais avançadas de curry como [_.curry] (https://lodash.com/docs#curry) da biblioteca lodash fazem algo mais sofisticado. Eles retornam um invólucro que permite que uma função seja chamada normalmente quando todos os argumentos são fornecidos * ou * retorna um parcial caso contrário.

`` `js
função curry (f) {
função de retorno (... args) {
// if args.length == f.length (tantos argumentos quanto f),
// passa a chamada para f
// de outra forma retorna uma função parcial que corrige args como os primeiros argumentos
};
}
`` `

# Currying? Pelo que?

O curling avançado permite que ambos mantenham a função normalmente chamada e obtenham partials facilmente. Para entender os benefícios, definitivamente precisamos de um exemplo digno da vida real.

Por exemplo, temos a função log 'log (data, importância, mensagem) `que formata e exibe as informações. Em projetos reais, essas funções também possuem muitos outros recursos úteis, como: enviá-lo pela rede ou filtragem:

`` `js
log de função (data, importância, mensagem) {
alerta (`[$ {date.getHours ()}: $ {date.getMinutes ()}] [$ {importância}] $ {message}`);
}
`` `

Vamos curry!

`` `js
log = _.curry (log);
`` `

Depois disso, `log` ainda funciona da maneira normal:

`` `js
log (nova data (), "DEBUG", "alguma depuração");
`` `

... Mas também pode ser chamado na forma de curry:

`` `js
log (nova Data ()) ("DEBUG") ("alguma depuração"); // log (a) (b) (c)
`` `

Vamos obter uma função de conveniência para os registros de hoje:

`` `js
// todayLog será o parcial do log com o primeiro argumento fixo
Deixe TodayLog = log (nova Data ());

// use-o
TodayLog ("INFO", "mensagem"); // [HH: mm] mensagem INFO
`` `

E agora uma função de conveniência para as mensagens de depuração de hoje:

`` `js
Deixe TodayDebug = todayLog ("DEBUG");

todayDebug ("mensagem"); // [HH: mm] mensagem DEBUG
`` `

Assim:
1. Não perdemos nada após o curry: o `log 'ainda é chamado normalmente.
2. Conseguimos gerar funções parciais que são convenientes em muitos casos.

## Implementação avançada de curry

Caso esteja interessado, aqui está a implementação de curry "avançada" que poderíamos usar acima.

`` `js run
função curry (func) {

função de retorno curried (... args) {
se (args.length> = func.length) {
return func.apply (this, args);
} outro {
função de retorno (... args2) {
retornar curried.apply (isto, args.concat (args2));
}
}
};

}

soma de função (a, b, c) {
Retornar a + b + c;
}

deixe curriedSum = curry (soma);

// ainda é chamado normalmente
alerta (CurrySum (1, 2, 3)); // 6

// obtenha o parcial com curry (1) e chame-o com outros 2 argumentos
alerta (curriedSum (1) (2,3)); // 6

// forma completa de curry
alerta (curriedSum (1) (2) (3)); // 6
`` `

O novo "curry" pode parecer complicado, mas na verdade é muito fácil de entender.

O resultado de `curry (func)` é o wrapper `curried` que se parece com isto:

`` `js
// func é a função para transformar
função curry (... args) {
se (args.length> = func.length) {// (1)
return func.apply (this, args);
} outro {
Passo de função de retorno (... args2) {// (2)
retornar curried.apply (isto, args.concat (args2));
}
}
};
`` `

Quando executámos, existem dois ramos:

1. Ligue agora: se a contagem de `args` passada for igual à que a função original tenha em sua definição (` func.length`) ou mais, então passe a chamada para ela.
2. Obter parcial: caso contrário, `func` ainda não foi chamado. Em vez disso, é devolvido outro "pass" de wrapper, que irá re-aplicar `curried` fornecendo argumentos anteriores junto com os novos. Então, em uma nova chamada, novamente, obteremos um novo argumento parcial (se não suficiente) ou, finalmente, o resultado.

Por exemplo, vamos ver o que acontece no caso de `sum (a, b, c)`. Três argumentos, então `sum.length = 3`.

Para a chamada `curry (1) (2) (3)`:

1. A primeira chamada `curried (1)` lembra `1` em seu ambiente Lexical e retorna um" pass "de wrapper.
2. O wrapper `pass` é chamado com` (2) `: é necessário args anteriores (` 1`), concatena-os com o que recebeu `(2)` e chama `curry (1, 2)` juntos .

Como a contagem do argumento é ainda inferior a 3, `curry` retorna` pass`.
3. O wrapper `pass` é chamado novamente com` (3) `, para a próxima chamada` pass (3) `leva args anterior (` 1`, `2`) e adiciona` 3` para eles, fazendo a chamada `curry (1, 2, 3)` - há argumentos '3', finalmente, eles são dados à função original.

Se isso ainda não for óbvio, apenas trate a seqüência de chamadas em sua mente ou no papel.

`` `cabeçalho inteligente = somente" funções de comprimento fixo "
O curry exige que a função tenha um número fixo conhecido de argumentos.
`` `

`` `cabeçalho inteligente =" Um pouco mais do que curtir "
Por definição, currying deve converter `sum (a, b, c)` em `sum (a) (b) (c)`.

Mas a maioria das implementações de currying em JavaScript são avançadas, conforme descrito: eles também mantêm a função desejável na variante de vários argumentos.
`` `

## Resumo

- Quando corrigimos alguns argumentos de uma função existente, a função resultante (menos universal) é chamada de * a parcial *. Podemos usar `bind` para obter um parcial, mas também existem outras formas.

Parciais são convenientes quando não queremos repetir o mesmo argumento uma e outra vez. Como se tivéssemos uma função `send (from, to)`, e `from` deveriam ser sempre iguais para nossa tarefa, podemos obter um parcial e continuar com isso.

- * Currying * é uma transformação que faz `f (a, b, c)` callable como `f (a) (b) (c)`. As implementações de JavaScript geralmente mantêm a função normalmente chamada e retornam a contagem de argumentos parcial se a contagem não for suficiente.

Currying é ótimo quando queremos partidas fáceis. Como vimos no exemplo de registro: a função universal `log (data, importância, mensagem)` após currying nos dá partials quando chamado com um argumento como `log (date)` ou dois argumentos `log (data, importância) `.
