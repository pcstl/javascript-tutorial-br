
As notas estão abaixo do código:

`` `js run
função assíncrono loadJson (url) {// (1)
Deixe resposta = aguardar busca (url); // (2)

se (response.status == 200) {
Deixe json = aguarde response.json (); // (3)
retornar json;
}

lança um novo erro (response.status);
}

loadJson ('no-such-user.json')
.catch (alerta); // Erro: 404 (4)
```

Notas:

1. A função `loadUrl` torna-se" assíncrono ".
2. Todos os `.then` dentro são substituídos por` await`.
3. Podemos "retornar response.json ()` ao invés de esperá-lo, assim:

`` `js
se (response.status == 200) {
return response.json (); // (3)
}
```

Então, o código externo teria que esperar por essa promessa de resolver. No nosso caso, não importa.
4. O erro lançado de `loadJson` é tratado por` .catch`. Não podemos usar `aguardar loadJson (...)` lá, porque não estamos em uma função 'async`.
