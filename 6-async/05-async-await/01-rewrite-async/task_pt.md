
# Reescreva usando async / await

Reescreva um dos exemplos do capítulo <info: promessa de encadeamento> usando `async / await` em vez de` .then / catch`:

`` `js run
função loadJson (url) {
retorno fetch (url)
.then (response => {
se (response.status == 200) {
return response.json ();
} outro {
lança um novo erro (response.status);
}
})
}

loadJson ('no-such-user.json') // (3)
.catch (alerta); // Erro 404
```
