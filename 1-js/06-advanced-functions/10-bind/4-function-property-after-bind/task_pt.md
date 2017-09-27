importância: 5

---

# Função de propriedade depois de ligar

Existe um valor na propriedade de uma função. Isso mudará após `bind`? Por que, elaborado?

`` `js run
função sayHi () {
alerta (this.name);
}
sayHi.test = 5;

*! *
deixe bound = sayHi.bind ({
Nome: "John"
});

alerta (bound.test); // qual será o resultado? porque?
* /! *
`` `

