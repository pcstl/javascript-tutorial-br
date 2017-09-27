importância: 5

---

# Erro ao criar uma instância

Aqui está o código com `Rabbit` que se estende` Animal`.

Infelizmente, os objetos `Rabbit` não podem ser criados. O que está errado? Consertá-lo.
`` `js run
classe Animal {

construtor (nome) {
this.name = name;
}

}

Classe Rabbit extends Animal {
construtor (nome) {
this.name = name;
this.created = Date.now ();
}
}

*! *
deixe coelho = coelho novo ("coelho branco"); // Erro: isso não está definido
* /! *
alerta (rabbit.name);
`` `
