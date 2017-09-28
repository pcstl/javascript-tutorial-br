
# Promessa encadeamento

Voltemos ao problema mencionado no capítulo <info: callbacks>:

- Temos uma seqüência de tarefas assíncronas a serem feitas uma após a outra. Por exemplo, carregando scripts.
- Como codificá-lo bem?

As promessas fornecem algumas receitas para fazer isso.

[cortar]

Neste capítulo, cobrimos o encadeamento da promessa.

Parece assim:

`` `js run
nova Promessa (função (resolver, rejeitar) {

setTimeout (() => resolve (1), 1000); // (*)

}). então (função (resultado) {// (**)

alerta (resultado); // 1
resultado de retorno * 2;

}). então (função (resultado) {// (***)

alerta (resultado); // 2
resultado de retorno * 2;

}). então (function (result) {

alerta (resultado); // 4
resultado de retorno * 2;

});
```

A idéia é que o resultado seja passado através da cadeia de manipuladores `.then`.

Aqui o fluxo é:
1. A promessa inicial resolve em 1 segundo `(*)`,
2. Em seguida, o manipulador `.then` é chamado` (**) `.
3. O valor que ele retorna é passado para o próximo `.then` handler` (***) `
4. ... e assim por diante.

À medida que o resultado é passado ao longo da cadeia de manipuladores, podemos ver uma seqüência de chamadas de "alerta": `1` ->` 2` -> `4`.

! [] (promessa-então-chain.png)

Tudo funciona, porque uma chamada para "prometer". Depois, uma promessa é retomada, para que possamos chamar o próximo `.then`.

Quando um manipulador retorna um valor, ele se torna o resultado dessa promessa, então o próximo `.then` é chamado com ele.

Para tornar essas palavras mais claras, aqui está o início da cadeia:

`` `js run
nova Promessa (função (resolver, rejeitar) {

setTimeout (() => resolve (1), 1000);

}). então (function (result) {

alerta (resultado);
resultado de retorno * 2; // <- (1)

}) // <-- (2)
// .então…
```

O valor retornado por `.then` é uma promessa, é por isso que podemos adicionar outro` .then` em `(2)`. Quando o valor é retornado em `(1)`, essa promessa é resolvida, então o próximo manipulador corre com o valor.

Ao contrário do encadeamento, tecnicamente também podemos adicionar muitos `.then` a uma única promessa, como esta:

`` `js run
deixe prometer = nova Promessa (função (resolver, rejeitar) {
setTimeout (() => resolve (1), 1000);
});

promessa. (função (resultado) {
alerta (resultado); // 1
resultado de retorno * 2;
});

promessa. (função (resultado) {
alerta (resultado); // 1
resultado de retorno * 2;
});

promessa. (função (resultado) {
alerta (resultado); // 1
resultado de retorno * 2;
});
```

... Mas isso é uma coisa totalmente diferente. Aqui está a imagem (compare com o encadeamento acima):

! [] (promessa-então-muitos.png)

Todos `.then` na mesma promessa obtêm o mesmo resultado - o resultado dessa promessa. Então, no código acima, "alerta" mostra o mesmo: `1`. Não há resultado - passando entre eles.

Na prática, raramente precisamos de vários manipuladores para uma promessa. O encadeamento é usado com muita freqüência.

## Promessas de retorno

Normalmente, um valor retornado por um manipulador `.then` é imediatamente passado para o próximo manipulador. Mas há uma exceção.

Se o valor retornado for uma promessa, então a execução posterior será suspensa até que ela se ajuste. Depois disso, o resultado dessa promessa é dado ao próximo manipulador `.then`.

Por exemplo:

`` `js run
nova Promessa (função (resolver, rejeitar) {

setTimeout (() => resolve (1), 1000);

}). então (function (result) {

alerta (resultado); // 1

*!*
devolver nova Promessa ((resolver, rejeitar) => {// (*)
setTimeout (() => resolve (resultado * 2), 1000);
});
*/!*

}). então (função (resultado) {// (**)

alerta (resultado); // 2

devolver nova Promessa ((resolver, rejeitar) => {
setTimeout (() => resolve (resultado * 2), 1000);
});

}). então (function (result) {

alerta (resultado); // 4

});
```

Aqui, o primeiro `.then` mostra` 1` retorna `new Promise (...)` na linha `(*)`. Depois de um segundo, ele resolve, e o resultado (o argumento de `resolver ', aqui é' resultado * 2 ') é transmitido ao manipulador do segundo` .then` na linha `(**)`. Mostra `2 'e faz o mesmo.

Então, a saída é novamente 1 -> 2> 4, mas agora com 1 segundo de atraso entre as chamadas de "alerta".

Retornar promessas nos permite construir cadeias de ações assíncronas.

## Exemplo: loadScript

Vamos usar esse recurso com `loadScript` para carregar scripts um a um, em sequência:

`` `js run
loadScript ("/ article / promise-chaining / one.js")
.then (function (script) {
retornar loadScript ("/ article / promise-chaining / two.js");
})
.then (function (script) {
retornar loadScript ("/ article / promise-chaining / three.js");
})
.then (function (script) {
// usa funções declaradas em scripts
// para mostrar que eles realmente carregaram
1();
dois();
três();
});
```

Aqui cada chamada `loadScript 'retorna uma promessa, e o próximo` .then` é executado quando ele resolve. Em seguida, inicia o carregamento do próximo script. Portanto, os scripts são carregados um após o outro.

Podemos adicionar mais ações assíncronas à cadeia. Por favor, note que o código ainda é "plano", ele cresce, não para a direita. Não há sinais de "pirâmide de doom".

Por favor, note que, tecnicamente, também é possível escrever `.then` diretamente após cada promessa, sem devolvê-las, assim:

`` `js run
loadScript ("/ article / promise-chaining / one.js"). então (function (script1) {
loadScript ("/ article / promise-chaining / two.js"). então (function (script2) {
loadScript ("/ article / promise-chaining / three.js"). então (function (script3) {
// esta função tem acesso às variáveis ​​script1, script2 e script3
1();
dois();
três();
});
});
});
```

Este código faz o mesmo: carrega 3 scripts em sequência. Mas "cresce para a direita". Então, temos o mesmo problema que com os callbacks. Use encadeamento (promessas de retorno de `.then`) para evadê-lo.

Às vezes, é bom escrever `.then` diretamente, porque a função aninhada tem acesso ao escopo externo (aqui o retorno de chamada mais aninhado tem acesso a todas as variáveis` scriptX`), mas isso é uma exceção e não uma regra.


`` `` cabeçalho inteligente = "Thenables"
Para ser preciso, `.then` pode retornar um objeto arbitrário" então ", e será tratado da mesma maneira que uma promessa.

Um objeto "então" é qualquer objeto com um método `.then`.

A ideia é que as bibliotecas de terceiros podem implementar objetos próprios "promissores". Eles podem ter um conjunto extenso de métodos, mas também ser compatíveis com promessas nativas, porque implementam `.then`.

Aqui está um exemplo de um objeto possível:

`` `js run
classe Thenable {
constructor(num) {
this.num = num;
}
então (resolva, rejeite) {
alerta (resolver); // função () {código nativo}
// resolve com this.num * 2 após o 1 segundo
setTimeout (() => resolve (this.num * 2), 1000); // (**)
}
}

nova Promise (resolve => resolve (1))
.then (result => {
Retornar novo Thenable (resultado); // (*)
})
.then (alerta); // mostra 2 após 1000ms
```

O JavaScript verifica o objeto retornado pelo manipulador `.then` na linha` (*) `: se ele possui um método chamado chamado` then`, então ele chama aquele método que fornece funções nativas `resolve`,` reject` como argumentos (semelhante para o executor) e espera até que um deles seja chamado. No exemplo acima, `resolve (2)` é chamado após 1 segundo `(**)`. Então o resultado é passado mais para baixo da cadeia.

Esse recurso permite integrar objetos personalizados com cadeias de promessas sem ter que herdar de `Promise`.
````


## Mais grande exemplo: buscar

Na frente, as promessas de programação são freqüentemente usadas para pedidos de rede. Então vamos ver um exemplo extenso disso.

Usaremos o método [fetch] (mdn: api / WindowOrWorkerGlobalScope / fetch) para carregar as informações sobre o usuário a partir do servidor remoto. O método é bastante complexo, tem muitos parâmetros opcionais, mas o uso básico é bastante simples:

`` `js
deixe prometer = buscar (url);
```

Isso faz uma solicitação de rede para o `url` e retorna uma promessa. A promessa resolve-se com um objeto `response` quando o servidor remoto responde com cabeçalhos, mas * antes que a resposta completa seja baixada *.

Para ler a resposta completa, devemos chamar um método `response.text ()`: ele retorna uma promessa que se resolve quando o texto completo é baixado do servidor remoto, com esse texto como resultado.

O código abaixo faz uma solicitação para `user.json` e carrega o texto do servidor:

`` `js run
buscar ('/ article / promise-chaining / user.json')
//. Então abaixo é executado quando o servidor remoto responde
.then (function (response) {
// response.text () retorna uma nova promessa que resolve com o texto de resposta completo
// quando terminamos de fazer o download
retornar response.text ();
})
.then (function (text) {
// ... e aqui está o conteúdo do arquivo remoto
alerta (texto); // {"nome": "iliakan", isAdmin: true}
});
```

Há também um método `response.json ()` que lê os dados remotos e o analisa como JSON. Em nosso caso, isso é ainda mais conveniente, então vamos mudar para ele.

Também usamos as funções de seta por brevidade:

`` `js run
// o mesmo que acima, mas response.json () analisando o conteúdo remoto como JSON
buscar ('/ article / promise-chaining / user.json')
.then (response => response.json ())
.then (user => alert (user.name)); // iliakan
```

Agora vamos fazer algo com o usuário carregado.

Por exemplo, podemos fazer mais um pedido para o github, carregar o perfil do usuário e mostrar o avatar:

`` `js run
// Faça um pedido para user.json
buscar ('/ article / promise-chaining / user.json')
// Carregue como json
.then (response => response.json ())
// Faça um pedido para github
.then (user => fetch (`https://api.github.com/users/$ {user.name}`))
// Carrega a resposta como json
.then (response => response.json ())
// Mostra a imagem do avatar (githubUser.avatar_url) por 3 segundos (talvez o anime)
.then (githubUser => {
deixe img = document.createElement ('img');
img.src = githubUser.avatar_url;
img.className = "promessa-avatar-exemplo";
document.body.append (img);

setTimeout (() => img.remove (), 3000); // (*)
});
```

O código funciona, veja comentários sobre os detalhes, mas deve ser bastante auto-descritivo. Embora, haja um problema potencial nele, um erro típico daqueles que começam a usar promessas.

Olhe para a linha `(*)`: como podemos fazer algo * depois * o avatar terminou de mostrar e é removido? Por exemplo, gostaríamos de mostrar um formulário para editar esse usuário ou qualquer outra coisa. A partir de agora, não há como chegar.

Para tornar a cadeia extensível, precisamos devolver uma promessa que se resolve quando o avatar terminar.

Como isso:

`` `js run
buscar ('/ article / promise-chaining / user.json')
.then (response => response.json ())
.then (user => fetch (`https://api.github.com/users/$ {user.name}`))
.then (response => response.json ())
*!*
.then (githubUser => new Promise (função (resolver, rejeitar) {
*/!*
deixe img = document.createElement ('img');
img.src = githubUser.avatar_url;
img.className = "promessa-avatar-exemplo";
document.body.append (img);

setTimeout (() => {
img.remove ();
*!*
resolver (githubUser);
*/!*
}, 3000);
}))
// dispara após 3 segundos
.then (githubUser => alert (`Terminado mostrando $ {githubUser.name}`));
```

Agora, logo após o `setTimeout` é executado` img.remove () `, ele chama` resolve (githubUser) `, passando o controle para o` .then` seguinte na cadeia e passando para frente os dados do usuário.

Como regra geral, uma ação assíncrona deve sempre retornar uma promessa.

Isso possibilita planejar ações depois disso. Mesmo que não tenhamos planejado estender a cadeia agora, talvez precisemos dela mais tarde.

Finalmente, podemos dividir o código em funções reutilizáveis:

`` `js run
função loadJson (url) {
retorno fetch (url)
.then (response => response.json ());
}

função loadGithubUser (nome) {
retorno de retorno (`https://api.github.com/users/$ {name}`)
.then (response => response.json ());
}

função showAvatar (githubUser) {
Retornar nova Promessa (função (resolver, rejeitar) {
deixe img = document.createElement ('img');
img.src = githubUser.avatar_url;
img.className = "promessa-avatar-exemplo";
document.body.append (img);

setTimeout (() => {
img.remove ();
resolver (githubUser);
}, 3000);
});
}

// Usa-os:
loadJson ('/ article / promise-chaining / user.json')
.then (user => loadGithubUser (user.name))
.then (showAvatar)
.then (githubUser => alert (`Terminado mostrando $ {githubUser.name}`));
// ...
```

## Gerenciamento de erros

As ações assíncronas às vezes podem falhar: em caso de erro, as promessas correspondentes são rejeitadas. Por exemplo, `fetch` falha se o servidor remoto não estiver disponível. Podemos usar `.catch` para lidar com erros (rejeições).

Promise chaining é ótimo nesse aspecto. Quando uma promessa rejeita, o controle salta para o manipulador de rejeição mais próximo da cadeia. Isso é muito conveniente na prática.

Por exemplo, no código abaixo, o URL está errado (nenhum servidor desse tipo) e `.catch` lida com o erro:

`` `js run
*!*
fetch('https://no-such-server.blabla') // rejects
*/!*
.then (response => response.json ())
.catch (err => alert (err)) // TypeError: falha ao buscar (o texto pode variar)
```

Ou, talvez, tudo esteja certo com o servidor, mas a resposta não é um JSON válido:

`` `js run
fetch ('/') // fetch funciona bem agora, o servidor responde com sucesso
*!*
.then (response => response.json ()) // rejeita: a página é HTML, não um json válido
*/!*
.catch (err => alert (err)) // SyntaxError: token inesperado <em JSON na posição 0
```


No exemplo abaixo, adicionamos `.catch` para lidar com todos os erros na cadeia de carregamento e exibição do avatar:

`` `js run
buscar ('/ article / promise-chaining / user.json')
.then (response => response.json ())
.then (user => fetch (`https://api.github.com/users/$ {user.name}`))
.then (response => response.json ())
.then (githubUser => new Promise (função (resolver, rejeitar) {
deixe img = document.createElement ('img');
img.src = githubUser.avatar_url;
img.className = "promessa-avatar-exemplo";
document.body.append (img);

setTimeout (() => {
img.remove ();
resolver (githubUser);
}, 3000);
}))
.catch (error => alert (error.message));
```

Aqui `.catch` não se desencadeia, porque não há erros. Mas se alguma das promessas acima rejeitar, então seria executado.

## Implicit try..catch

O código dos executores do executor e da promessa tem uma "tentativa de" tentativa "invisível" em torno disso. Se ocorrer um erro, é pego e tratado como uma rejeição.

Por exemplo, este código:

`` `js run
nova Promessa (função (resolver, rejeitar) {
*!*
lança um novo erro ("Whoops!");
*/!*
}). catch (alerta); // Erro: Whoops!
```

... Funciona da mesma forma que isso:

`` `js run
nova Promessa (função (resolver, rejeitar) {
*!*
rejeitar (novo erro ("Whoops!"));
*/!*
}). catch (alerta); // Erro: Whoops!
```

O "tentativa de" tentativa "invisível" em torno do executor automaticamente capta o erro e trata-o como uma rejeição.

Isso não é só no executor, mas também nos manipuladores. Se "lançarmos" dentro do manipulador .then`, isso significa uma promessa rejeitada, então o controle salta para o manipulador de erros mais próximo.

Aqui está um exemplo:

`` `js run
nova Promessa (função (resolver, rejeitar) {
resolve("ok");
}). então (function (result) {
*!*
lança um novo erro ("Whoops!"); // rejeita a promessa
*/!*
}). catch (alerta); // Erro: Whoops!
```

Isso não é só para `throw`, mas para qualquer erro, incluindo erros de programação também:

`` `js run
nova Promessa (função (resolver, rejeitar) {
resolve("ok");
}). então (function (result) {
*!*
blabla(); // no such function
*/!*
}). catch (alerta); // ReferenceError: blabla não está definido
```

Como efeito colateral, o `.catch final 'não só capta rejeições explícitas, mas também erros ocasionais nos manipuladores acima.

## Rethrowing

Como já percebemos, `.catch` comporta-se como` try..catch`. Podemos ter tantos `.then` como queremos e, em seguida, usar um` .catch 'único no final para lidar com erros em todos eles.

Em um `try..catch` regular, podemos analisar o erro e talvez reenviá-lo se não pudermos lidar. O mesmo é possível para promessas. Se "lançarmos" dentro de `.catch`, então o controle vai para o próximo manipulador de erros mais próximo. E se lidar com o erro e terminar normalmente, ele continua com o manipulador de sucesso `.then`.

No exemplo abaixo, o `.catch 'lida com êxito com o erro:
`` `js run
// a execução: catch -> then
nova Promessa (função (resolver, rejeitar) {

lança um novo erro ("Whoops!");

}). catch (função (erro) {

alerta ("O erro é tratado, continue normalmente");

}). então (() => alerta ("Next handler bem sucedido é executado"));
```

Aqui, o bloco `.catch` termina normalmente. Então, o próximo processador bem sucedido é chamado. Ou poderia retornar algo, seria o mesmo.

... E aqui o bloco `.catch` analisa o erro e o joga novamente:

`` `js run
// a execução: catch -> catch -> then
nova Promessa (função (resolver, rejeitar) {

lança um novo erro ("Whoops!");

}). catch (função (erro) {// (*)

se (erro instância de URIError) {
// lidar com isso
} outro {
alerta ("Não consigo lidar com esse erro");

*!*
lança erro; // lançando este ou outro erro pula para a próxima captura
*/!*
}

}). então (function () {
/ * nunca corre aqui * /
}). catch (erro => {// (**)

alerta (`O erro desconhecido ocorreu: $ {error}`);
// não retorna nada => a execução vai do modo normal

});
```

O manipulador `(*)` pega o erro e simplesmente não consegue lidar com isso, porque não é "URIError", então ele o joga novamente. Então, a execução salta para o próximo `.catch` na cadeia` (**) `.

Na seção abaixo, veremos um exemplo prático de retomar.

## Fetch error handling example

Vamos melhorar o tratamento de erros para o exemplo de carregamento do usuário.

A promessa retornada por [fetch] (mdn: api / WindowOrWorkerGlobalScope / fetch) rejeita quando é impossível fazer uma solicitação. Por exemplo, um servidor remoto não está disponível ou o URL está mal formado. Mas se o servidor remoto responde com o erro 404, ou mesmo com o erro 500, então é considerada uma resposta válida.

E se o servidor retornar uma página que não seja JSON com erro 500 na linha `(*)`? E se não houver tal usuário, e github retorna uma página com erro 404 em `(**)`?

`` `js run
fetch ('no-such-user.json') // (*)
.then (response => response.json ())
.then (user => fetch (`https://api.github.com/users/$ {user.name}`)) // (**)
.then (response => response.json ())
.catch (alerta); // SyntaxError: token inesperado <em JSON na posição 0
// ...
```


A partir de agora, o código tenta carregar a resposta como JSON, independentemente do quê e morra com um erro de sintaxe. Você pode ver isso executando o exemplo acima, pois o arquivo `no-such-user.json` não existe.

Isso não é bom, porque o erro simplesmente cai na cadeia, sem detalhes: o que falhou e onde.

Então, vamos adicionar mais um passo: devemos verificar a propriedade `response.status` que possui status HTTP e, se não for 200, então, lance um erro.

`` `js run
classe HttpError extends Error {// (1)
construtor (resposta) {
super (`$ {response.status} para $ {response.url}`);
this.name = 'HttpError';
this.response = response;
}
}

função loadJson (url) {// (2)
retorno fetch (url)
.then (response => {
se (response.status == 200) {
return response.json ();
} outro {
lance o novo HttpError (resposta);
}
})
}

loadJson ('no-such-user.json') // (3)
.catch (alerta); // HttpError: 404 para ... / no-such-user.json
```

1. Criamos uma classe personalizada para erros HTTP para distingui-los de outros tipos de erros. Além disso, a nova classe possui um construtor que aceita o objeto `response` e ​​o salva no erro. Portanto, o código de manipulação de erros poderá acessá-lo.
2. Em seguida, colocamos o código de solicitação e de tratamento de erros em uma função que busca o `url` * e * trata qualquer status não-200 como um erro. Isso é conveniente, porque muitas vezes precisamos dessa lógica.
3. Agora, 'alerta' mostra uma mensagem melhor.

A grande coisa sobre ter nossa própria classe para erros é que podemos verificar isso com facilidade no código de manipulação de erros.

Por exemplo, podemos fazer um pedido e, se tivermos 404 - peça ao usuário para modificar as informações.

O código abaixo carrega um usuário com o nome dado do github. Se não existe tal usuário, então ele pede o nome correto:

`` `js run
function demoGithubUser () {
let name = prompt ("Digite um nome?", "iliakan");

return loadJson (`https://api.github.com/users/$ {name}`)
.then (user => {
alerta (`Nome completo: $ {user.name} .`); // (1)
retornar usuário;
})
.catch (err => {
*!*
se (erro instância de HttpError && err.response.status == 404) {// (2)
*/!*
alerta ("Nenhum usuário desse tipo, por favor entre novamente.");
retornar demoGithubUser ();
} outro {
errar;
}
});
}

demoGithubUser ();
```

Aqui:

1. Se `loadJson` retornar um objeto de usuário válido, então o nome é mostrado` (1) `, e o usuário é retornado, para que possamos adicionar mais ações relacionadas ao usuário na cadeia. Nesse caso, o `.catch 'abaixo é ignorado, tudo é muito simples e fino.
2. Caso contrário, em caso de erro, verificamos na linha `(2)`. Somente se for realmente o erro HTTP, e o status é 404 (Não encontrado), pedimos ao usuário para voltar a inserir. Para outros erros - não sabemos como lidar, então nós apenas os reencontremos.

## Rejeições não controladas

O que acontece quando um erro não é tratado? Por exemplo, após a retomada, como no exemplo acima. Ou se esquecemos de anexar um manipulador de erros ao final da cadeia, como aqui:

`` `js não é confiável executar atualizar
nova Promessa (function () {
noSuchFunction (); // Erro aqui (sem essa função)
});
```

Ou aqui:

`` `js não é confiável executar atualizar
nova Promessa (function () {
lança um novo erro ("Whoops!");
}). então (function () {
// ...alguma coisa...
}). então (function () {
// ...algo mais...
}). então (function () {
// ... mas nenhuma captura depois disso!
});
```

Em teoria, nada deve acontecer. Em caso de erro, o estado de promessa é "rejeitado", e a execução deve passar para o manipulador de rejeição mais próximo. Mas não há tal manipulador nos exemplos acima. Então, o erro fica "preso".

Na prática, isso significa que o código é ruim. Na verdade, como é que não há tratamento de erros?

A maioria dos mecanismos JavaScript rastreia tais situações e gera um erro global nesse caso. Podemos vê-lo no console.

No navegador podemos buscá-lo usando o evento `unhandledrejection`:

`` `js run
*!*
window.addEventListener ('unhandledrejection', function (event) {
// o objeto de evento tem duas propriedades especiais:
alerta (evento.promise); // [Promessa objeto] - a promessa que gerou o erro
alerta (event.reason); // Erro: Whoops! - o objeto de erro não tratado
});
*/!*

nova Promessa (function () {
lança um novo erro ("Whoops!");
}); // nenhuma captura para lidar com o erro
```

O evento é a parte do [padrão HTML] (https://html.spec.whatwg.org/multipage/webappapis.html#unhandled-promise-rejections). Agora, se ocorrer um erro e não há `.catch`, o manipulador` unhandledrejection` desencadeia: o objeto `event` tem a informação sobre o erro, para que possamos fazer algo com ele.

Normalmente, esses erros são irrecuperáveis, portanto, nossa melhor saída é informar o usuário sobre o problema e, provavelmente, informar sobre o incidente no servidor.

Em ambientes que não são do navegador, como o Node.JS, existem outras formas similares de rastrear erros não processados.

## Resumo

Para resumir, `.then / catch (handler)` retorna uma nova promessa que muda dependendo do manipulador:

1. Se retornar um valor ou terminar sem um `return` (o mesmo que` return undefined`), então a nova promessa será resolvida e o manipulador de resolução mais próximo (o primeiro argumento de `.then`) é chamado com esse valor .
2. Se ele lança um erro, então a nova promessa é rejeitada e o manipulador de rejeição mais próximo (segundo argumento de `.then` ou` .catch`) é chamado com ele.
3. Se retornar uma promessa, o JavaScript aguarda até que ele se ajuste e, em seguida, age em seu resultado da mesma maneira.

A imagem de como a promessa retornada por `.then / catch` muda:

! [] (promessa-manipulador-variantes.png)

A imagem menor de como os manipuladores são chamados:

! [] (promessa-manipulador-variantes-2.png)

Nos exemplos de manipulação de erros acima do `.catch` foi sempre o último na cadeia. Na prática, porém, nem toda cadeia de promessas tem um `.catch`. Assim como o código comum nem sempre está envolvido no `try..catch`.

Devemos colocar `.catch` exatamente nos locais onde queremos lidar com erros e saber como lidar com eles. Usar classes de erro personalizadas pode ajudar a analisar erros e repensar aqueles que não podemos manipular.

Para erros que estão fora do nosso âmbito, devemos ter o manipulador de eventos `unhandledrejection` (para navegadores e análogos para outros ambientes). Tais erros desconhecidos geralmente são irrecuperáveis, então tudo o que devemos fazer é informar o usuário e, provavelmente, informar o nosso servidor sobre o incidente.
