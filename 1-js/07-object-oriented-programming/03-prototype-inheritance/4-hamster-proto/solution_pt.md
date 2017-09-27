Vejamos atentamente o que está acontecendo na chamada `speedy.eat (" apple ")`.

1. O método `speedy.eat` é encontrado no protótipo (` = hamster`), então executado com `this = speedy` (o objeto antes do ponto).

2. Então `this.stomach.push ()` precisa encontrar a propriedade `stomach` e chamar` push` nele. Procura `estômago 'em` this` (`= speedy`), mas nada encontrado.

3. Então segue a cadeia do protótipo e encontra `estômago 'no` hamster`.

4. Então ele chama `push`, adicionando a comida no * estômago do protótipo *.

Então, todos os hamsters compartilham um único estômago!

Toda vez que o estômago é retirado do protótipo, o `stomach.push` o modifica" no lugar ".

Por favor note que tal coisa não acontece no caso de uma tarefa simples `this.stomach =`:

`` `js run
deixe hamster = {
estômago: [],

comer alimentos) {
*! *
// atribua a this.stomach em vez de this.stomach.push
this.stomach = [comida];
* /! *
}
};

deixe speedy = {
__proto__: hamster
};

deixe preguiçoso = {
__proto__: hamster
};

// Speedy encontrou a comida
speedyat ("maçã");
alerta (speed.stomach); // maçã

// O estômago preguiçoso está vazio
alerta (preguiçoso); // <nada>
`` `

Agora tudo funciona bem, porque `this.stomach =` não realiza uma pesquisa de `estômago '. O valor é escrito diretamente no objeto `this`.

Também podemos evitar totalmente o problema, certificando-se de que cada hamster tenha seu próprio estômago:

`` `js run
deixe hamster = {
estômago: [],

comer alimentos) {
this.stomach.push (comida);
}
};

deixe speedy = {
__proto__: hamster,
*! *
estômago: []
* /! *
};

deixe preguiçoso = {
__proto__: hamster,
*! *
estômago: []
* /! *
};

// Speedy encontrou a comida
speedyat ("maçã");
alerta (speed.stomach); // maçã

// O estômago preguiçoso está vazio
alerta (preguiçoso); // <nada>
`` `

Como uma solução comum, todas as propriedades que descrevem o estado de um objeto específico, como "estômago" acima, geralmente são escritas nesse objeto. Isso evita tais problemas.
