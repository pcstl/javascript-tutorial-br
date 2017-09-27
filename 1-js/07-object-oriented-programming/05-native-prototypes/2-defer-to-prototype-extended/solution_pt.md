

`` `js run
Function.prototype.defer = function (ms) {
deixe f = isso;
função de retorno (... args) {
setTimeout (() => f.apply (this, args), ms);
}
};

// Confira
função f (a, b) {
alerta (a + b);
}

f.defer (1000) (1, 2); // mostra 3 após 1 segundo
`` `
