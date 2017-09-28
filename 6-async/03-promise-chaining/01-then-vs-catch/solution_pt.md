A resposta curta é: ** não, eles não são iguais **:

A diferença é que, se ocorrer um erro em `f1`, então ele é tratado por` .catch` aqui:

`` `js run
promessa
.then (f1)
.catch (f2);
```

...Mas não aqui:

`` `js run
promessa
.then (f1, f2);
```

Isso ocorre porque um erro é passado na cadeia, e na segunda parte do código não há nenhuma cadeia abaixo `f1`.

Em outras palavras, `.then` passa resultados / erros para o próximo` .then / catch`. Então, no primeiro exemplo, há um `catch` abaixo, e no segundo - não existe, então o erro não é processado.
