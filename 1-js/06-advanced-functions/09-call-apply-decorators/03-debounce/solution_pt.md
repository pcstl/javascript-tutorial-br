

`` `js run no-embellecer
função debounce (f, ms) {

deixe isCooldown = false;

função de retorno () {
se (isCooldown) retornar;

f.apply (isto, argumentos);

isCooldown = true;

setTimeout (() => isCooldown = false, ms);
};

}
`` `

A chamada para `debounce` retorna um wrapper. Pode haver dois estados:

- `isCooldown = false` - pronto para executar.
- `isCooldown = true` - aguardando o tempo limite.

Na primeira chamada `isCooldown` é falsy, então a chamada prossegue e o estado muda para` true`.

Enquanto `isCooldown` é verdadeiro, todas as outras chamadas são ignoradas.

Então, `setTimeout` reverte para` falso 'após o atraso dado.
