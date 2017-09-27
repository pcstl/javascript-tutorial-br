

`` `js run demo
função Calculadora () {

this.read = function () {
this.a = + prompt ('a?', 0);
this.b = + prompt ('b?', 0);
};

this.sum = function () {
devolva this.a + this.b;
};

this.mul = function () {
devolva this.a * this.b;
};
}

Deixe calculadora = calculadora nova ();
calculator.read ();

alerta ("Sum =" + calculator.sum ());
alerta ("Mul =" + calculator.mul ());
`` `
