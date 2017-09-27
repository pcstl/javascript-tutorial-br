
Aqui estão as explicações.

1. Essa é uma chamada de método de objeto regular.

2. O mesmo, os suportes não alteram a ordem das operações aqui, o ponto é o primeiro de qualquer maneira.

3. Aqui temos uma chamada mais complexa `(expression) .method ()`. A chamada funciona como se estivesse dividida em duas linhas:

`` `js no-embellecer
f = obj.go; // calcula a expressão
f (); // ligue para o que temos
`` `

Aqui `f ()` é executado como uma função, sem `this`.

4. A coisa semelhante a `(3)`, à esquerda do ponto `.` temos uma expressão.

Para explicar o comportamento de `(3)` e `(4)` precisamos lembrar que os acessadores de propriedades (ponto ou colchetes) retornam um valor do Tipo de Referência.

Qualquer operação nele, exceto uma chamada de método (como atribuição `=` ou `||`) transforma-lo em um valor comum, que não contém a informação que permite configurar `this`.

