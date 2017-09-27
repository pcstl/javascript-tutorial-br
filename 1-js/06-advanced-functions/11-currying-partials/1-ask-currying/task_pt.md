importância: 5

---

# Aplicação parcial para login

A tarefa é uma variante pouco mais complexa de <info: task / question-use-bind>.

O objeto `usuário 'foi modificado. Agora em vez de duas funções `loginOk / loginFail`, ele possui uma única função` user.login (true / false) `.

O que passa `askPassword` no código abaixo, para que ele chame` user.login (true) `como` ok` e `user.login (fail)` como `fail`?

`` `js
função askPassword (ok, fail) {
Deixe password = prompt ("Password?", '');
se (senha == "rockstar") ok ();
else fail ();
}

Deixe o usuário = {
Nome: 'John',

login (resultado) {
alerta (this.name + (resultado? 'logado': 'falha ao logon'));
}
};

*! *
askPassword (?,?); //?
* /! *
`` `

Suas alterações devem apenas modificar o fragmento destacado.

