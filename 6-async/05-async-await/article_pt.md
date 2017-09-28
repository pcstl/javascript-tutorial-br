# Async / await

Existe uma sintaxe especial para trabalhar com promessas de forma mais confortável, chamada "async / await". É surpreendentemente fácil de entender e usar.

## Funções assíncronas

Vamos começar com a palavra-chave `async`. Pode ser colocado antes da função, assim:

`` `js
Função assíncrona f () {
retornar 1;
}
```

A palavra "assíncrono" antes de uma função significa uma coisa simples: uma função sempre retorna uma promessa. Se o código tiver `return <non-promise>` nele, o JavaScript automaticamente o envolve em uma promessa resolvida com esse valor.

Por exemplo, o código acima retorna uma promessa resolvida com o resultado de `1, vamos testá-lo:

`` `js run
Função assíncrona f () {
retornar 1;
}

f (). então (alerta); // 1
```

... Poderíamos devolver uma promessa explicitamente, isso seria o mesmo:

`` `js run
Função assíncrona f () {
Retornar Promise.resolve (1);
}

f (). então (alerta); // 1
```

Então, `async` garante que a função retorna uma promessa, envolve não promessas nele. Simples, certo? Mas não só isso. Há outra palavra-chave `await` que funciona apenas dentro das funções` async`, e é muito legal.

## Aguardam

A sintaxe:

`` `js
// funciona apenas dentro de funções assíncronas
Deixe valor = aguardar promessa;
```

A palavra-chave `await` faz com que JavaScript aguarde até que essa promessa resolva e retorna seu resultado.

Aqui é exemplo com uma promessa que resolve em 1 segundo:
`` `js run
Função assíncrona f () {

deixe prometer = nova Promessa ((resolver, rejeitar) => {
setTimeout (() => resolve ("done!"), 1000)
});

*!*
Deixe o resultado = aguarde promessa; // aguarde até que a promessa resolva (*)
*/!*

alerta (resultado); // "feito!"
}

f ();
```

A execução da função "pausa" na linha `(*)` e retoma-se quando a promessa se instala, com `result` tornando-se seu resultado. Então, o código acima mostra "feito!" em um segundo.

Vamos enfatizar: `await` literalmente faz JavaScript esperar até que a promessa se estabeleça, e depois continue com o resultado. Isso não custa a nenhum recurso de CPU, porque o motor pode fazer outros trabalhos enquanto isso: execute outros scripts, manipule eventos etc.

É apenas uma sintaxe mais elegante de obter o resultado da promessa do que `promise.then`, mais fácil de ler e escrever.

`` `` warn header = "Não é possível usar` wait 'em funções normais "
Se tentarmos usar `await` na função não-assíncrono, isso seria um erro de sintaxe:

`` `js run
função f () {
deixe prometer = Promise.resolve (1);
*!*
Deixe o resultado = aguarde promessa; // Erro de sintaxe
*/!*
}
```

Podemos obter esse erro no caso se esquecermos de colocar `async` antes de uma função. Como disse, `esperar 'só funciona dentro de" função assíncrona ".
````

Vamos levar o exemplo `showAvatar ()` do capítulo <info: promessa de encadeamento> e reescreva-o usando `async / await`:

1. Teremos que substituir as chamadas `.then` por` await`.
2. Também devemos tornar a função `async` para que eles funcionem.

`` `js run
função assíncrona showAvatar () {

// leia o nosso JSON
Deixe resposta = aguardar busca ('/ article / promise-chaining / user.json');
Deixe o usuário = aguarde response.json ();

// leia o usuário do github
Deixe githubResponse = aguardar fetch (`https://api.github.com/users/$ {user.name}`);
deixe githubUser = aguarde githubResponse.json ();

// mostra o avatar
deixe img = document.createElement ('img');
img.src = githubUser.avatar_url;
img.className = "promessa-avatar-exemplo";
document.body.append (img);

// aguarde 3 segundos
aguarde novas promessas ((resolver, rejeitar) => setTimeout (resolver, 3000));

img.remove ();

retornar githubUser;
}

showAvatar ();
```

Muito limpo e fácil de ler, certo? Muito melhor do que antes.

`` `` smart header = "` await` não funcionará no código de nível superior "
As pessoas que estão apenas começando a usar `await` tendem a esquecer isso, mas não podemos escrever` await` no código de nível superior. Isso não funcionaria:

`` `js run
// erro de sintaxe no código de nível superior
Deixe resposta = aguardar busca ('/ article / promise-chaining / user.json');
Deixe o usuário = aguarde response.json ();
```

Portanto, precisamos ter uma função assíncrono de wrapping para o código que aguarda. Assim como no exemplo acima.
````
`` `` smart header = "` await` aceita thenables "
Como `promise.then`,` await` permite usar objetos que podem ser selecionados (aqueles com um método "então" que pode ser chamado). Mais uma vez, a idéia é que um objeto de terceiros pode não ser uma promessa, mas compatível com a promessa: se ele for compatível com `.then`, basta usar com` await`.

Por exemplo, aqui `await` aceita` new Thenable (1) `:
`` `js run
classe Thenable {
constructor(num) {
this.num = num;
}
então (resolva, rejeite) {
alerta (resolver); // função () {código nativo}
// resolve com this.num * 2 após 1000ms
setTimeout (() => resolve (this.num * 2), 1000); // (*)
}
};

Função assíncrona f () {
// aguarda 1 segundo, então o resultado se torna 2
Deixe o resultado = aguarde novo Apontável (1);
alerta (resultado);
}

f ();
```

Se `await` obtiver um objeto não prometido com` .then`, ele chama aquele método que fornece funções nativas `resolve`,` reject` como argumentos. Então, "esperar" espera até que uma delas seja chamada (no exemplo acima, acontece na linha `(*)`) e depois prossegue com o resultado.
````

`` `` cabeçalho inteligente = "métodos assíncronos"
Um método de classe também pode ser assíncrono, basta colocar `async` antes dele.

Como aqui:

`` `js run
classe garçom {
*!*
async wait () {
*/!*
aguarde Promise.resolve (1);
}
}

novo garçom ()
.esperar()
.then (alerta); // 1
```
O significado é o mesmo: garante que o valor retornado seja uma promessa e permita que "aguarde".

````
## Gerenciamento de erros

Se uma promessa resolva normalmente, então "esperar promessa" retorna o resultado. Mas, no caso de uma rejeição, ele lança o erro, apenas se houvesse uma instrução `throw` naquela linha.

Este código:

`` `js
Função assíncrona f () {
*!*
aguarde Promise.reject (novo erro ("Whoops!"));
*/!*
}
```

... É o mesmo que isso:

`` `js
Função assíncrona f () {
*!*
lança um novo erro ("Whoops!");
*/!*
}
```

Em situações reais, a promessa pode levar algum tempo antes de rejeitar. Então, 'aguardar' vai aguardar e, em seguida, lançar um erro.

Podemos pegar esse erro usando `try..catch`, da mesma forma que um" throw "regular:

`` `js run
Função assíncrona f () {

experimentar {
Deixe resposta = aguardar busca ('http: // no-such-url');
} catch (err) {
*!*
alerta (erro); // TypeError: falha ao buscar
*/!*
}
}

f ();
```

Em caso de erro, o controle salta para o bloco `catch`. Também podemos envolver várias linhas:

`` `js run
Função assíncrona f () {

experimentar {
Deixe resposta = aguardar busca ('/ no-user-here');
Deixe o usuário = aguarde response.json ();
} catch (err) {
// captura erros tanto em busca como em resposta.json
alerta (erro);
}
}

f ();
```

Se não tivermos `try..catch`, a promessa gerada pela chamada da função async` f () `será rejeitada. Podemos anexar `.catch` para lidar com isso:

`` `js run
Função assíncrona f () {
Deixe resposta = aguardar busca ('http: // no-such-url');
}

// f () torna-se uma promessa rejeitada
*!*
f (). catch (alerta); // TypeError: não conseguiu buscar // (*)
*/!*
```

Se nos esquecemos de adicionar `.catch` aí, obtemos um erro de promessa não controlado (e podemos vê-lo no console). Podemos pegar esses erros usando um manipulador de eventos global conforme descrito no capítulo <info: promessa de encadeamento>.


`` `smart header =" `async / await` e` promise.then / catch` "
Quando usamos `async / await`, raramente precisamos` .then`, porque `await` nos lida com a espera. E podemos usar um `try..catch` normal em vez de` .catch`. Isso geralmente é (nem sempre) mais conveniente.

Mas no nível superior do código, quando estamos fora de qualquer função "assíncica", somos sintaticamente incapazes de usar `await`, então é uma prática normal adicionar` .then / catch` para lidar com o resultado final ou erros de queda.

Como na linha `(*)` do exemplo acima.
```

`` `` smart header = "` async / await` funciona bem com `Promise.all`"
Quando precisamos esperar várias promessas, podemos envolvê-las em `Promise.all` e depois` await`:

`` `js
// aguarde a série de resultados
Deixe resultados = aguarde Promise.all ([
buscar (url1),
buscar (url2),
...
]);
```

Em caso de erro, ele se propaga como de costume: da promessa falhada de `Promise.all`, e então se torna uma exceção que podemos capturar usando` try..catch` em torno da chamada.

````

## Resumo

A palavra-chave `async` antes de uma função tem dois efeitos:

1. Faz com que sempre devolva uma promessa.
2. Permite usar `await` nela.

A palavra-chave "aguarda" antes de uma promessa faz com que JavaScript aguarde até que essa promessa se estabeleça e, em seguida:

1. Se for um erro, a exceção é gerada, da mesma forma que se o "erro de lance" fosse chamado nesse mesmo lugar.
2. Caso contrário, ele retorna o resultado, para que possamos atribuí-lo a um valor.

Juntos, eles fornecem uma ótima estrutura para escrever código assíncrono que é fácil de ler e de escrever.

Com `async / await`, raramente precisamos escrever` promise.then / catch`, mas ainda não devemos esquecer que eles são baseados em promessas, porque às vezes (por exemplo, no âmbito mais externo), temos que usar esses métodos. Também `Promise.all` é uma boa coisa para esperar muitas tarefas simultaneamente.
