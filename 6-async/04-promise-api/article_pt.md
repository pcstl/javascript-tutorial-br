# Promise API

Existem 4 métodos estáticos na classe `Promise`. Vamos abordar rapidamente seus casos de uso aqui.

## Promise.resolve

A sintaxe:

`` `js
deixe prometer = Promise.resolve (value);
```

Retorna uma promessa resolvida com o "valor" dado.

Igual a:

`` `js
deixe prometer = nova Promise (resolve => resolve (value));
```

O método é usado quando já temos um valor, mas queremos que ele seja "embrulhado" em uma promessa.

Por exemplo, a função `loadCached` abaixo obtém o` url` e lembra o resultado, de modo que futuras chamadas no mesmo URL retornam imediatamente:

`` `js
função loadCached (url) {
Deixe cache = loadCached.cache || (loadCached.cache = new Map ());

se (cache.has (url)) {
*!*
Retornar Promise.resolve (cache.get (url)); // (*)
*/!*
}

retorno fetch (url)
.then (response => response.text ())
.then (text => {
cache [url] = texto;
retornar texto;
});
}
```

Podemos usar `loadCached (url) .then (...)`, porque a função é garantida para retornar uma promessa. Essa é a finalidade `Promise.resolve` na linha` (*) `: assegura a interface unificada. Nós sempre podemos usar `.then` depois de` loadCached`.

## Promise.reject

A sintaxe:

`` `js
deixe prometer = Promise.reject (erro);
```

Crie uma promessa rejeitada com o `error`.

Igual a:

`` `js
deixe prometer = nova Promessa ((resolver, rejeitar) => rejeitar (erro));
```

Cobri-lo aqui para a completude, raramente usado em código real.

## Promise.all

O método para executar muitas promessas em paralelo e aguardar até que todas elas estejam prontas.

A sintaxe é:

`` `js
deixe prometer = Promise.all (iterable);
```

É preciso um objeto `iterável 'com promessas, tecnicamente pode ser qualquer iterável, mas geralmente é uma matriz e retorna uma nova promessa. A nova promessa resolve com quando todos eles são resolvidos e tem uma série de seus resultados.

Por exemplo, o `Promise.all` abaixo se instala após 3 segundos, e então o resultado é uma matriz` [1, 2, 3] `:

`` `js run
Promise.all ([
nova Promessa ((resolver, rejeitar) => setTimeout (() => resolver (1), 3000)), // 1
nova Promessa ((resolva, rejeite) => setTimeout (() => resolve (2), 2000)), // 2
nova Promessa ((resolver, rejeitar) => setTimeout (() => resolver (3), 1000)) // 3
]). então (alerta); // 1,2,3 quando as promessas estão prontas: cada promessa contribui com um membro da matriz
```

Observe que a ordem relativa é a mesma. Embora a primeira promessa demore o tempo mais longo para resolver, ainda é o primeiro na série de resultados.

Um truque comum é mapear uma série de dados de trabalho em uma série de promessas e, em seguida, envolver isso em `Promise.all`.

Por exemplo, se possuímos uma variedade de URLs, podemos buscá-los assim:

`` `js run
Deixe urls = [
'https://api.github.com/users/iliakan',
'https://api.github.com/users/remy',
'https://api.github.com/users/jeresig'
];

// mapa cada url para a busca da promessa (URL do github)
Deixe pedidos = urls.map (url => fetch (url));

// Promise.all espera até que todos os trabalhos sejam resolvidos
Promise.all (pedidos)
.then (respostas => answers.forEach (
response => alert (`$ {response.url}: $ {response.status}`)
));
```

Um exemplo mais real da vida com a obtenção de informações do usuário para uma série de usuários de github por seus nomes (ou podemos buscar uma série de produtos por seus ids, a lógica é a mesma):

`` `js run
Nomes tardios = ['Dinastia', 'Romance', 'Jaresig'];

Deixe requests = names.map (name => fetch (`https://api.github.com/users/$ {name}`));

Promise.all (pedidos)
.then (respostas => {
// todas as respostas estão prontas, podemos mostrar códigos de status HTTP
para (deixe resposta das respostas) {
alerta (`$ {response.url}: $ {response.status}`); // mostra 200 para cada url
}

retornar respostas;
})
// mapear o mapa de respostas na matriz de response.json () para ler seu conteúdo
.then (respostas => Promise.all (answers.map (r => r.json ())))
// todas as respostas JSON são analisadas: "usuários" é a matriz delas
.then (users => users.forEach (user => alert (user.name)));
```

Se alguma das promessas for rejeitada, `Promise.all` rejeita imediatamente com esse erro.

Por exemplo:


`` `js run
Promise.all ([
nova Promessa ((resolver, rejeitar) => setTimeout (() => resolver (1), 1000)),
*!*
nova promessa ((resolver, rejeitar) => setTimeout (() => rejeitar (novo erro ("Whoops!")), 2000)),
*/!*
nova Promessa ((resolva, rejeite) => setTimeout (() => resolve (3), 3000))
]). catch (alerta); // Erro: Whoops!
```

Aqui a segunda promessa é rejeitada em dois segundos. Isso leva à rejeição imediata de `Promise.all`, então` .catch` executa: o erro de rejeição se torna o resultado de todo o `Promise.all`.

O detalhe importante é que as promessas não fornecem nenhuma maneira de "cancelar" ou "abortar" sua execução. Então, outras promessas continuam a ser executadas, e eventualmente se estabelecem, mas todos os seus resultados são ignorados.

Existem maneiras de evitar isso: podemos escrever código adicional para 'clearTimeout` (ou cancelar) as promessas em caso de erro, ou podemos criar erros como membros na matriz resultante (veja a tarefa abaixo deste capítulo sobre isso).

`` `` smart header = "` Promise.all (iterable) `permite itens não prometidos em` iterable` "
Normalmente, `Promise.all (iterable)` aceita uma série de promessas iteráveis ​​(na maioria dos casos). Mas se algum desses objetos não é uma promessa, está envolvido em `Promise.resolve`.

Por exemplo, aqui os resultados são `[1, 2, 3]`:

`` `js run
Promise.all ([
nova promessa ((resolver, rejeitar) => {
setTimeout (() => resolve (1), 1000)
}),
2, // tratado como Promise.resolve (2)
3 // tratado como Promise.resolve (3)
]). então (alerta); // 1, 2, 3
```

Então, somos capazes de passar valores não prometidos para `Promise.all` quando conveniente.

````

## Promise.race

Semelhante a `Promise.all` leva uma iterable de promessas, mas em vez de esperar por todas elas terminar - aguarda o primeiro resultado (ou erro) e continua com isso.

A sintaxe é:

`` `js
deixe prometer = Promise.race (iterable);
```

Por exemplo, aqui o resultado será `1`:

`` `js run
Promise.race ([
nova Promessa ((resolver, rejeitar) => setTimeout (() => resolver (1), 1000)),
nova promessa ((resolver, rejeitar) => setTimeout (() => rejeitar (novo erro ("Whoops!")), 2000)),
nova Promessa ((resolva, rejeite) => setTimeout (() => resolve (3), 3000))
]). então (alerta); // 1
```

Então, o primeiro resultado / erro se torna o resultado de todo o `Promise.race`. Após a primeira promessa estabelecida "ganha a corrida", todos os outros resultados / erros são ignorados.

## Resumo

Existem 4 métodos estáticos de classe `Promise`:

1. `Promise.resolve (value)` - faz uma promessa resolvida com o valor dado,
2. `Promise.reject (error)` - faz uma promessa rejeitada com o erro dado,
3. `Promise.all (promessas)` - aguarda todas as promessas para resolver e retorna uma série de seus resultados. Se alguma das promessas dadas rejeitar, então se torna o erro de `Promise.all`, e todos os outros resultados são ignorados.
4. `Promise.race (promessas)` - aguarda a primeira promessa de liquidação, e seu resultado / erro se torna o resultado.

Destes quatro, `Promise.all` é o mais comum na prática.
