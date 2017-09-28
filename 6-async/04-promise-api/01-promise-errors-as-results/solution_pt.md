A solução é realmente muito simples.

Dê uma olhada neste:

`` `js
Promise.all (
fetch('https://api.github.com/users/iliakan'),
buscar ('https://api.github.com/users/remy'),
buscar ('http: // no-such-url')
)
```

Aqui temos uma série de promessas `fetch (...)` que vão para `Promise.all`.

Não podemos mudar o modo `Promise.all` funciona: se detectar um erro, ele rejeita com ele. Portanto, precisamos evitar qualquer erro. Em vez disso, se ocorrer um erro `fetch`, precisamos tratá-lo como um resultado" normal ".

Veja como:

`` `js
Promise.all (
buscar ('https://api.github.com/users/iliakan') .catch (err => err),
buscar ('https://api.github.com/users/remy') .catch (err => err),
buscar ('http: // no-such-url') .catch (err => err)
)
```

Em outras palavras, o `.catch` toma um erro para todas as promessas e retorna normalmente. Com as regras de como as promessas funcionam, se um manipulador `.then / catch 'retornar um valor (não importa se é um objeto de erro ou outra coisa), a execução continua o fluxo" normal ".

Então, o `.catch` retorna o erro como um resultado" normal "para o` Promise.all` externo.

Este código:
`` `js
Promise.all (
urls.map (url => fetch (url))
)
```

Pode ser reescrito como:

`` `js
Promise.all (
urls.map (url => fetch (url) .catch (err => err))
)
```
