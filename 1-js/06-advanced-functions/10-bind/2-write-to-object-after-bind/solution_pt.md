A resposta: 'nulo'.


`` `js run
função f () {
alerta (isto); // nulo
}

Deixe o usuário = {
g: f.bind (null)
};

user.g ();
`` `

O contexto de uma função vinculada é rígido. Não há como mudar isso.

Então, mesmo enquanto executamos `user.g ()`, a função original é chamada com `this = null`.
