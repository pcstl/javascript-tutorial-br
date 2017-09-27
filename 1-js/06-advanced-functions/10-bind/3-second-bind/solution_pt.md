A resposta: ** João **.

`` `js run no-embellecer
função f () {
alerta (this.name);
}

f = f.bind ({name: "John"}) .bind ({name: "Pete"});

f (); // John
`` `

O objeto exótico [função vinculada] (https://tc39.github.io/ecma262/#sec-bound-function-exotic-objects) retornado por `f.bind (...)` lembra o contexto (e argumentos se fornecido) apenas no horário de criação.

Uma função não pode ser re-limitada.
