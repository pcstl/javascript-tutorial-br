# Promessa

Imagine que você é o melhor cantor, e os fãs pedem o próximo dia e a noite.

Para obter um alívio, você promete enviá-lo quando é publicado. Você dá a seus fãs uma lista. Eles podem preencher suas coordenadas, de modo que quando a música estiver disponível, todas as partes inscritas conseguem obtê-lo instantaneamente. E se algo estiver muito errado, para que a música não seja publicada, então eles também serão notificados.

Todos estão felizes: você, porque as pessoas não mais se aglomeram e os fãs, porque não perderão o single.

Essa foi uma analogia da vida real para coisas que muitas vezes temos na programação:

1. Um "código produtor" que faz algo e precisa de tempo. Por exemplo, ele carrega um script remoto. Esse é um "cantor".
2. Um "código consumidor" quer o resultado quando estiver pronto. Muitas funções podem precisar desse resultado. Estes são "fãs".
3. A * promessa * é um objeto JavaScript especial que os vincula. Essa é uma "lista". O código produtor cria e dá a todos, para que possam se inscrever para o resultado.

A analogia não é muito precisa, porque as promessas de JavaScript são mais complexas do que uma lista simples: possuem recursos e limitações adicionais. Mas ainda assim são parecidos.

A sintaxe do construtor para um objeto de promessa é:

`` `js
deixe prometer = nova Promessa (função (resolver, rejeitar) {
// executor (o código produtor, "cantor")
});
```

A função passada para `new Promise` é chamada de * executor *. Quando a promessa é criada, é chamada automaticamente. Ele contém o código produzido, que eventualmente deve terminar com um resultado. Em termos da analogia acima, o executor é um "cantor".

O objeto "promissor" resultante possui propriedades internas:

- `state` - inicialmente está" pendente ", depois muda para" cumprido "ou" rejeitado ",
- `result` - um valor arbitrário, inicialmente 'indefinido'.

Quando o executor terminar o trabalho, deve chamar um dos seguintes:

- `resolve (value)` - para indicar que o trabalho terminou com sucesso:
- define `estado` para` `preenchido '',
- define `result` para` value`.
- `rejeitar (erro)` - para indicar que ocorreu um erro:
- define `estado 'para` "rejeitado" `
- define `result` para` error`.

! [] (promessa-resolve-reject.png)

Aqui está um simples executor, para reunir isso todos juntos:

`` `js run
deixe prometer = nova Promessa (função (resolver, rejeitar) {
// a função é executada automaticamente quando a promessa é construída

alerta (resolver); // function () {[native code]}
alerta (rejeitar); // function () {[native code]}

// depois de um segundo sinal de que o trabalho é feito com o resultado "feito!"
setTimeout (() => *! * resolve ("done!") * /! *, 1000);
});
```

Podemos ver duas coisas executando o código acima:

1. O executor é chamado de forma automática e imediata (por "nova promessa").
2. O executor recebe dois argumentos: `resolve` e` reject` - estas funções vêm do mecanismo JavaScript. Não precisamos criá-los. Em vez disso, o executor deveria chamá-los quando estiver pronto.

Depois de um segundo de pensar, o executor chama `resolve (" done ")` para produzir o resultado:

! [] (promessa-resolução-1.png)

Esse foi um exemplo da "conclusão bem sucedida do trabalho".

E agora um exemplo onde o executor rejeita a promessa com um erro:

`` `js
deixe prometer = nova Promessa (função (resolver, rejeitar) {
// após 1 segundo sinal de que o trabalho está concluído com um erro
setTimeout (() => *! * rejeitar (novo erro ("Whoops!")) * /! *, 1000);
});
```

! [] (promessa-rejeição-1.png)

Para resumir, o executor deve fazer um trabalho (algo que leva tempo geralmente) e depois chamar `resolver 'ou` rejeitar' para alterar o estado do objeto de promessa correspondente.

A promessa que é resolvida ou rejeitada é chamada de "resolvida", em oposição a uma promessa "pendente".

`` `` cabeçalho inteligente = "Pode haver apenas um resultado ou um erro"
O executor deve chamar apenas um `resolve` ou` reject`. A mudança de estado da promessa é final.

Todas as outras chamadas de `resolve` e` reject` são ignoradas:

`` `js
deixe prometer = nova Promessa (função (resolver, rejeitar) {
resolver ("feito");

rejeitar (novo erro ("...")); // ignorado
setTimeout (() => resolve ("...")); // ignorado
});
```

A idéia é que um trabalho feito pelo executor pode ter apenas um resultado ou um erro. Na programação, existem outras estruturas de dados que permitem muitos resultados "fluídos", por exemplo, fluxos e filas. Eles têm suas próprias vantagens e desvantagens versus promessas. Eles não são suportados pelo núcleo de JavaScript e carecem de certos recursos de linguagem que as promessas fornecem, não os cobrimos aqui para nos concentrar em promessas.

Além disso, se chamamos `resolver / rejeitar 'com mais de um argumento - apenas o primeiro argumento é usado, os próximos são ignorados.
````

`` `smart header =" Rejeitar com `Error` objects"
Tecnicamente, podemos chamar "rejeitar" (como "resolver") com qualquer tipo de argumento. Mas é recomendado usar objetos `Error` em` reject` (ou herdar deles). O raciocínio para isso se tornará óbvio em breve.
```

`` `` cabeçalho inteligente = "Resolver / rejeitar pode ser imediato"
Na prática, um executor geralmente faz algo de forma assíncrona e chama `resolver / rejeitar 'depois de algum tempo, mas não precisa. Podemos chamar `resolve` ou` reject` imediatamente, assim:

`` `js
deixe prometer = nova Promessa (função (resolver, rejeitar) {
resolver (123); // dê imediatamente o resultado: 123
});
```

Por exemplo, acontece quando começamos a fazer um trabalho e depois percebemos que tudo já foi feito. Tecnicamente, está bem: temos uma promessa resolvida no momento.
````

`` `cabeçalho inteligente =" O `estado 'e` resultado "são internos"
As propriedades `estado 'e` resultado' de um objeto promessa são internas. Não podemos acessá-los diretamente do nosso código, mas podemos usar métodos `.then / catch` para isso, eles são descritos abaixo.
```

## Consumidores: ".then" e ".catch"

Um objeto de promessa serve como um link entre o código de produção (executor) e as funções de consumo - aqueles que desejam receber o resultado / erro. As funções de consumo podem ser registradas usando métodos `promise.then` e` promise.catch`.


A sintaxe de `.then` é:

`` `js
promessa.
função (resultado) {/ * lidar com um resultado bem sucedido * /},
função (erro) {/ * lidar com um erro * /}
);
```

O argumento da primeira função é executado quando a promessa é resolvida e obtém o resultado, e a segunda - quando é rejeitada e obtém o erro.

Por exemplo:

`` `js run
deixe prometer = nova Promessa (função (resolver, rejeitar) {
setTimeout (() => resolve ("done!"), 1000);
});

// resolve executa a primeira função em .then
promessa.
*!*
resultado => alerta (resultado), // mostra "feito!" depois de 1 segundo
*/!*
erro => alerta (erro) // não é executado
);
```

Em caso de rejeição:

`` `js run
deixe prometer = nova Promessa (função (resolver, rejeitar) {
setTimeout (() => rejeitar (novo erro ("Whoops!")), 1000);
});

// rejeitar executa a segunda função em .then
promessa.
resultado => alerta (resultado), // não é executado
*!*
error => alert (error) // mostra "Error: Whoops!" depois de 1 segundo
*/!*
);
```

Se nos interessamos apenas em conclusões bem-sucedidas, podemos fornecer apenas um argumento para `.then`:

`` `js run
deixa promessa = nova Promise (resolve => {
setTimeout (() => resolve ("done!"), 1000);
});

*!*
promessa. (alerta); // mostra "feito!" depois de 1 segundo
*/!*
```

Se nos interessamos apenas nos erros, podemos usar `.then (null, function)` ou um "alias" para ele: `.catch (function)`


`` `js run
deixe prometer = nova Promessa ((resolver, rejeitar) => {
setTimeout (() => rejeitar (novo erro ("Whoops!")), 1000);
});

*!*
// .catch (f) é o mesmo que promise.then (null, f)
promise.catch (alerta); // mostra "Error: Whoops!" depois de 1 segundo
*/!*
```

A chamada `.catch (f)` é um análogo completo de `.then (null, f)`, é apenas uma taquigrafia.

`` `` cabeçalho inteligente = "Em promessas estabelecidas", então, é executado imediatamente "
Se uma promessa estiver pendente, os manipuladores `.th / catch` aguardam o resultado. Caso contrário, se uma promessa já se estabeleceu, eles executam imediatamente:

`` `js run
// uma promessa imediatamente resolvida
deixa promessa = nova Promise (resolve => resolve ("done!"));

promessa. (alerta); // feito! (mostra agora)
```

Isso é útil para trabalhos que às vezes podem exigir tempo e às vezes terminam imediatamente. O manipulador é garantido para funcionar em ambos os casos.
````

`` `` smart header = "Os manipuladores de` .then / catch` são sempre assíncronos "
Para ser ainda mais preciso, quando o manipulador `.th / catch` deve ser executado, primeiro entra em uma fila interna. O mecanismo de JavaScript leva os manipuladores da fila e é executado quando o código atual termina, semelhante ao `setTimeout (..., 0)`.

Em outras palavras, quando `.then (handler)` vai disparar, ele faz algo como `setTimeout (handler, 0)` em vez disso.

No exemplo abaixo, a promessa é imediatamente resolvida, então `.then (alert)` dispara agora: a chamada `alert` está em fila e é executada imediatamente após o código terminar.

`` `js run
// uma promessa imediatamente resolvida
deixa promessa = nova Promise (resolve => resolve ("done!"));

promessa. (alerta); // feito! (logo após o código atual terminar)

alerta ("código concluído"); // este alerta mostra primeiro
```

Então, o código após `.then` sempre é executado antes do manipulador (mesmo no caso de uma promessa pré-resolvida). Normalmente isso não é importante, em alguns cenários podem ser importantes.
````

Agora, vejamos mais exemplos práticos de como promessas podem nos ajudar na escrita de código assíncrono.

## Exemplo: loadScript

Temos a função `loadScript 'para carregar um script do capítulo anterior.

Aqui está a variante baseada em chamada de retorno, apenas para lembrá-la:

`` `js
função loadScript (src, callback) {
let script = document.createElement ('script');
script.src = src;

script.onload = () => retorno de chamada (nulo, script);
script.onerror = () => retorno de chamada (novo erro (`erro de carga do script` + src));

document.head.append (script);
}
```

Vamos reescrevê-lo usando promessas.

A nova função `loadScript` não exigirá uma chamada de retorno. Em vez disso, ele criará e retornará um objeto de promessa que se liquidará quando o carregamento estiver completo. O código externo pode adicionar manipuladores usando `.then`:

`` `js run
função loadScript (src) {
Retornar nova Promessa (função (resolver, rejeitar) {
let script = document.createElement ('script');
script.src = src;

script.onload = () => resolve (script);
script.onerror = () => rejeitar (novo erro ("Erro de carga do script:" + src));

document.head.append (script);
});
}
```

Uso:

`` `js run
Promessa tardia = Loadscript ("Http: //cdnjscloudflare.com/aazax/libs/loadshjs/3.2.0/loadsh.");

promessa.
script => alert (`$ {script.src} está carregado!`),
error => alert (`Erro: $ {error.message}`)
);

promise.then (script => alert ('Mais um manipulador para fazer outra coisa!'));
```

Podemos ver imediatamente alguns benefícios sobre a sintaxe baseada em chamada de retorno:

`` `compare minus =" Callbacks "plus =" Promessas "
- Devemos ter uma função de "callback" pronta ao chamar `loadScript`. Em outras palavras, devemos saber o que fazer com o resultado * antes * chamado loadScript é chamado.
- Pode haver apenas um retorno de chamada.
+ As promessas nos permitem codificar as coisas na ordem natural. Primeiro, executamos `loadScript` e` .then` escrevem o que fazer com o resultado.
+ Podemos chamar `.then` de uma promessa quantas vezes quisermos, a qualquer momento depois.
```

Assim, as promessas já nos dão um melhor fluxo de código e flexibilidade. Mas há mais. Veremos isso nos próximos capítulos.
