

1. Use uma função de wrapper, uma seta para ser concisa:

`` `js
askPassword (() => user.login (true), () => user.login (falso));
`` `

Agora, ele recebe `usuário 'de variáveis ​​externas e executa o modo normal.

2. Ou crie uma função parcial de `user.login` que use` user` como contexto e tenha o primeiro argumento correto:


`` `js
askPassword (user.login.bind (usuário, verdadeiro), user.login.bind (usuário, falso));
`` `
