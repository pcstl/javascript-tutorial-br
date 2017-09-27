importância: 5

---

# Onde escreve?

Nós temos "coelhos" herdando de "animais".

Se chamamos `rabbit.eat ()`, qual objeto recebe a propriedade `full`:` animal` ou `rabbit`?

`` `js
deixe animal = {
comer () {
this.full = true;
}
};

deixe coelho = {
__proto__: animal
};

rabbit.eat ();
`` `
