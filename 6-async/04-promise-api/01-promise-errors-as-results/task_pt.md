# Promise.-tolerante a falhas.

Gostaríamos de buscar múltiplos URLs em paralelo.

Aqui está o código para fazer isso:

`` `js run
Deixe urls = [
'https://api.github.com/users/iliakan',
'https://api.github.com/users/remy',
'https://api.github.com/users/jeresig'
];

Promise.all (urls.map (url => fetch (url)))
// para cada resposta mostra seu status
.then (respostas => {// (*)
para (deixe resposta das respostas) {
alerta (`$ {response.url}: $ {response.status}`);
}
));
```

O problema é que, se algum dos pedidos falhar, o `Promise.all` rejeita com o erro e perdemos os resultados de todos os outros pedidos.

Isso não é bom.

Modifique o código para que a matriz `respostas 'na linha` (*) `inclua os objetos de resposta para obtenções bem-sucedidas e objetos de erro para falhas.

Por exemplo, se um dos URLs for ruim, então deve ser como:

`` `js
Deixe urls = [
'https://api.github.com/users/iliakan',
'https://api.github.com/users/remy',
'http: // no-such-url'
];

Promise.all (...) // seu código para buscar URLs ...
// ... e passar erros de busca como membros da matriz resultante ...
.then (respostas => {
// 3 URLs => 3 membros da matriz
alerta (resposta [0] .status); // 200
alerta (resposta [1] .status); // 200
alerta (respostas [2]); // TypeError: falha ao buscar (o texto pode variar)
});
```

P.S. Nessa tarefa você não precisa carregar a resposta completa usando `response.text ()` ou `response.json ()`. Basta lidar com erros de busca do jeito certo.
