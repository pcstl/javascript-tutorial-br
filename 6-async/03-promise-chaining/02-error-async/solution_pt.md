A resposta é: ** não, não será **:

`` `js run
nova Promessa (função (resolver, rejeitar) {
setTimeout (() => {
lança um novo erro ("Whoops!");
}, 1000);
}). catch (alerta);
```

Conforme dito no capítulo, há um "try.catch" implícito em torno do código de função. Portanto, todos os erros síncronos são tratados.

Mas aqui o erro é gerado não enquanto o executor está em execução, mas depois. Então, a promessa não pode lidar com isso.
