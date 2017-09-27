
`` `js run demo
deixe num;

Faz {
num = prompt ("Digite um número maior que 100?", 0);
} enquanto (num <= 100 && num);
`` `

O loop `do..while` repete enquanto ambos os verificações são verdadeiros:

1. O cheque para `num <= 100` - ou seja, o valor introduzido ainda não é maior do que` 100`.
2. O check `&& num` é falso quando` num` é `null` ou uma string vazia. Então, o loop `while 'pára também.

P.S. Se `num` for` null`, então `num <= 100` é` true`, então, sem a 2ª verificação, o loop não parará se o usuário clicar em CANCELAR. São necessárias duas verificações.
