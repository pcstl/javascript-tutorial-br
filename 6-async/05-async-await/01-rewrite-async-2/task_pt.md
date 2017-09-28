
# Reescreva "rethrow" assíncrono / aguardo

Abaixo, você pode encontrar o exemplo "retomar" do capítulo <info: promessa-encadeamento>. Reescreva-o usando `async / await` em vez de` .then / catch`.

E se livrar da recursão em favor de um loop em `demoGithubUser`: com` async / await` que se torna fácil de fazer.

`` `js run
classe HttpError extends Error {
construtor (resposta) {
super (`$ {response.status} para $ {response.url}`);
this.name = 'HttpError';
this.response = response;
}
}

função loadJson (url) {
retorno fetch (url)
.then (response => {
se (response.status == 200) {
return response.json ();
} outro {
lance o novo HttpError (resposta);
}
})
}

// Solicita um nome de usuário até que o github retorna um usuário válido
function demoGithubUser () {
let name = prompt ("Digite um nome?", "iliakan");

return loadJson (`https://api.github.com/users/$ {name}`)
.then (user => {
alerta (`Nome completo: $ {user.name} .`);
retornar usuário;
})
.catch (err => {
se (erro instância de HttpError && err.response.status == 404) {
alerta ("Nenhum usuário desse tipo, por favor entre novamente.");
retornar demoGithubUser ();
} outro {
errar;
}
});
}

demoGithubUser ();
```
