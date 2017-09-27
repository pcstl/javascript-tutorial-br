`` `js
função throttle (func, ms) {

deixe isThrottled = false,
saveArg
salvo isto;

função wrapper () {

se (isThrottled) {// (2)
savedArgs = argumento;
savedThis = this;
Retorna;
}

func.apply (isto, argumentos); // (1)

isThrottled = true;

setTimeout (function () {
isThrottled = false; // (3)
se (savedArgs) {
wrapper.apply (savedThis, savedArgs);
savedArgs = savedThis = zero;
}
}, Senhora);
}

envolver o retorno;
}
`` `

Uma chamada para `throttle (func, ms)` retorna `wrapper`.

1. Durante a primeira chamada, o `wrapper` simplesmente executa` func` e define o estado de cooldown (`isThrottled = true`).
2. Neste estado, todas as chamadas memorizadas em `savedArgs / savedThis`. Observe que o contexto e os argumentos são igualmente importantes e devem ser memorizados. Precisamos deles simultaneamente para reproduzir a chamada.
3. ... Depois, após o "ms" milissegundos passar, os disparadores 'setTimeout`. O estado de cooldown é removido (`isThrottled = false`). E se tivéssemos ignorado as chamadas, o "wrapper" é executado com os últimos argumentos e contexto memorizados.

A terceira etapa não funciona `func`, mas` wrapper`, porque não só precisamos executar `func`, mas novamente inserir o estado de cooldown e configurar o tempo limite para reiniciá-lo.
