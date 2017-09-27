importância: 5

---

# Classe estende o objeto?

Como sabemos, todos os objetos normalmente herdam de Object.prototype e obtêm acesso a métodos de objeto "genéricos".

Como demonstrado aqui:

`` `js run
classe Rabbit {
construtor (nome) {
this.name = name;
}
}

deixe coelho = coelho novo ("Rab");

*! *
// o método hasOwnProperty é de Object.prototype
// coelho .__ proto__ === Object.prototype
alerta (rabbit.hasOwnProperty ('name')); // verdade
* /! *
`` `

Então, é correto dizer que "a classe Rabbit extends Object" `faz exatamente o mesmo que` "class Rabbit" `, ou não?

será que vai dar certo?

`` `js
Classe Rabbit extends Object {
construtor (nome) {
this.name = name;
}
}

deixe coelho = coelho novo ("Rab");

alerta (rabbit.hasOwnProperty ('name')); // verdade
`` `

Caso não consiga corrigir o código.
