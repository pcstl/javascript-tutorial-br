
O erro ocorre porque `ask` obtém funções` loginOk / loginFail` sem o objeto.

Quando os chama, eles naturalmente assumem "isto = indefinido".

Vamos "ligar" o contexto:

`` `js run
função askPassword (ok, fail) {
Deixe password = prompt ("Password?", '');
se (senha == "rockstar") ok ();
else fail ();
}

Deixe o usuário = {
Nome: 'John',

loginOk () {
alerta (`$ {this.name} logado ');
},

loginFail () {
alerta (`$ {this.name} falha ao fazer logon ');
},

};

*! *
askPassword (user.loginOk.bind (usuário), user.loginFail.bind (usuário));
* /! *
`` `

Agora funciona.

Uma solução alternativa poderia ser:
`` `js
// ...
askPassword (() => user.loginOk (), () => user.loginFail ());
`` `

Normalmente, isso também funciona, mas pode falhar em situações mais complexas, onde o "usuário" tem a chance de ser substituído entre os momentos de perguntar e executar `() => user.loginOk ()`.


