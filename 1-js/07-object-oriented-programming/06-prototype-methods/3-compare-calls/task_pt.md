importância: 5

---

# A diferença entre chamadas

Vamos criar um novo objeto `rabbit`:

`` `js
função Coelho (nome) {
this.name = name;
}
Rabbit.prototype.sayHi = function () {
alerta (this.name);
};

Deixe o coelho = Coelho novo ("Coelho");
`` `

Essas chamadas fazem o mesmo ou não?

`` `js
coelho.sayHi ();
Rabbit.prototype.sayHi ();
Object.getPrototypeOf (coelho) .sayHi ();
coelho .__ proto __. sayHi ();
`` `
