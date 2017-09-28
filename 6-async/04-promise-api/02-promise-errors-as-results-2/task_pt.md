# Recuperação tolerante a falhas com JSON

Melhore a solução da tarefa anterior <info: task / promise-errors-as-results>. Agora não precisamos apenas chamar `fetch`, mas carregar os objetos JSON de URLs fornecidos.

Aqui está o código de exemplo para fazer isso:

`` `js run
Deixe urls = [
'https://api.github.com/users/iliakan',
'https://api.github.com/users/remy',
'https://api.github.com/users/jeresig'
];

// fazer solicitações de busca
Promise.all (urls.map (url => fetch (url)))
// mapeia cada resposta para response.json ()
.then (answers => Promise.all (
response.map (r => r.json ())
))
// mostra o nome de cada usuário
.then (users => {// (*)
para (deixe o usuário dos usuários) {
alerta (user.name);
}
});
```

O problema é que, se algum dos pedidos falhar, o `Promise.all` rejeita com o erro e perdemos os resultados de todos os outros pedidos. Portanto, o código acima não é tolerante a falhas, assim como aquele na tarefa anterior.

Modifique o código para que a matriz na linha `(*)` inclua JSON analisado para solicitações bem-sucedidas e erro para erros.

Observe que o erro pode ocorrer tanto no `fetch` (se a solicitação da rede falhar) quanto no` response.json () `(se a resposta for inválida JSON). Em ambos os casos, o erro deve se tornar um membro do objeto de resultados.

A caixa de areia tem ambos os casos.
