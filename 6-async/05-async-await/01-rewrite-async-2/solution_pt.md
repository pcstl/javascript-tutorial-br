
Não há truques aqui. Basta substituir `.catch` por` try ... catch` dentro de `demoGithubUser` e adicionar` async / await` quando necessário:

`` `js run
classe HttpError extends Error {
construtor (resposta) {
super (`$ {response.status} para $ {response.url}`);
this.name = 'HttpError';
this.response = response;
}
}

função assíncrona loadJson (url) {
Deixe resposta = aguardar busca (url);
se (response.status == 200) {
return response.json ();
} outro {
lance o novo HttpError (resposta);
}
}

// Solicita um nome de usuário até que o github retorna um usuário válido
função assíncrona demoGithubUser () {

deixar o usuário;
enquanto (verdadeiro) {
let name = prompt ("Digite um nome?", "iliakan");

experimentar {
usuário = aguarde loadJson (`https://api.github.com/users/$ {name}`);
pausa; // nenhum erro, loop de saída
} catch (err) {
se (erro instância de HttpError && err.response.status == 404) {
// loop continua após o alerta
alerta ("Nenhum usuário desse tipo, por favor entre novamente.");
} outro {
// erro desconhecido, retomar
errar;
}
}
}


alerta (`Nome completo: $ {user.name} .`);
retornar usuário;
}

demoGithubUser ();
```
