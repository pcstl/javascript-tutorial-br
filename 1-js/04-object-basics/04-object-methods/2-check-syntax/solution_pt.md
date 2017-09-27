**Erro**!

Tente:

`` `js run
Deixe o usuário = {
Nome: "John",
go: function () {alert (this.name)}
}

(user.go) () // erro!
`` `

A mensagem de erro na maioria dos navegadores não dá a entender o que deu errado.

** O erro aparece porque um ponto-e-vírgula está faltando após `user = {...}`. **

O JavaScript não assume um ponto e vírgula antes de um suporte `(user.go) ()`, então ele lê o código como:

`` `js no-embellecer
deixe user = {go: ...} (user.go) ()
`` `

Então, também podemos ver que essa expressão conjunta é sintaticamente uma chamada do objeto `{go: ...}` como uma função com o argumento `(user.go)`. E isso também acontece na mesma linha com `let user`, então o objeto` user` ainda não foi definido, daí o erro.

Se inserimos o ponto-e-vírgula, tudo está bem:

`` `js run
Deixe o usuário = {
Nome: "John",
go: function () {alert (this.name)}
} *! *; * /! *

(user.go) () // John
`` `

Por favor, note que os suportes em torno de `(user.go)` não fazem nada aqui. Geralmente eles configuram a ordem das operações, mas aqui o ponto `.` funciona primeiro de qualquer maneira, então não há efeito. Apenas importa o ponto-e-vírgula.






