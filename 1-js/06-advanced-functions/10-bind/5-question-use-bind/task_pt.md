importância: 5

---

# Pergunte a perder isso

A chamada para `askPassword ()` no código abaixo deve verificar a senha e depois chamar `user.loginOk / loginFail`, dependendo da resposta.

Mas isso leva a um erro. Por quê?

Corrija a linha realçada para que tudo comece a funcionar direito (outras linhas não devem ser alteradas).

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
askPassword (user.loginOk, user.loginFail);
* /! *
`` `


