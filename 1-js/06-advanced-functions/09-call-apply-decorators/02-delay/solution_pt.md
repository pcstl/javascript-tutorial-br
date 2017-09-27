A solução:

`` `js
atraso de função (f, ms) {

função de retorno () {
setTimeout (() => f.apply (this, arguments), ms);
};

}
`` `

Observe como uma função de seta é usada aqui. Como sabemos, as funções de seta não possuem "isso" e "argumento", então `f.apply (this, arguments)` leva `this` e` arguments` do wrapper.

Se nós passarmos uma função regular, `setTimeout` chamaria isso sem argumentos e` this = window` (no navegador), então precisamos escrever um pouco mais de código para passá-los do wrapper:

`` `js
atraso de função (f, ms) {

// adicionou variáveis ​​para passar este e argumentos do wrapper dentro setTimeout
função de retorno (... args) {
deixe salvado isto = isto;
setTimeout (function () {
f.apply (savedThis, args);
}, Senhora);
};

}
`` `
