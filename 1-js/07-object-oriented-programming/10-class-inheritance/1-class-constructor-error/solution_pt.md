Isso porque o construtor filho deve chamar `super ()`.

Aqui está o código corrigido:

`` `js run
classe Animal {

construtor (nome) {
this.name = name;
}

}

Classe Rabbit extends Animal {
construtor (nome) {
*! *
super (nome);
* /! *
this.created = Date.now ();
}
}

*! *
deixe coelho = coelho novo ("coelho branco"); // Certo, agora
* /! *
alerta (rabbit.name); // Coelho branco
`` `
