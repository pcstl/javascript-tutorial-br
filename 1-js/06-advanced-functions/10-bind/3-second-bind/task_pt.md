importância: 5

---

# Second bind

Podemos alterar `this` por ligação adicional?

Qual será o resultado?

`` `js no-embellecer
função f () {
alerta (this.name);
}

f = f.bind ({name: "John"}) .bind ({name: "Ann"});

f ();
`` `

