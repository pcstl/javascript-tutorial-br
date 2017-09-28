# Erro no setTimeout

Como você acha, o `.catch` gatilho? Explique sua resposta?

`` `js
nova Promessa (função (resolver, rejeitar) {
setTimeout (() => {
lança um novo erro ("Whoops!");
}, 1000);
}). catch (alerta);
```
