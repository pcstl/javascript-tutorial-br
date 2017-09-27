

`` `js run demo
deixe calculadora = {
soma () {
devolva this.a + this.b;
},

eu {} {
devolva this.a * this.b;
},

ler() {
this.a = + prompt ('a?', 0);
this.b = + prompt ('b?', 0);
}
};

calculator.read ();
alerta (calculator.sum ());
alerta (calculator.mul ());
`` `

