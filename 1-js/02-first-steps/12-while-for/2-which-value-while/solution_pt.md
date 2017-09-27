A tarefa demonstra como os formulários postfix / prefix podem levar a resultados diferentes quando usados ​​em comparações.

1. ** De 1 a 4 **

`` `js run
deixe i = 0;
enquanto (++ i <5) alerta (i);
`` `

O primeiro valor é `i = 1`, porque` ++ i` primeiro incrementa `i` e depois retorna o novo valor. Assim, a primeira comparação é `1 <5` e o` alerta` mostra `1`.

Em seguida, siga `2,3,4 ...` - os valores aparecem um após o outro. A comparação sempre usa o valor incrementado, porque `++` é antes da variável.

Finalmente, `i = 4` é incrementado para` 5`, a comparação `while (5 <5)` falha e o loop pára. Então `5 'não é mostrado.
2. ** De 1 a 5 **

`` `js run
deixe i = 0;
enquanto (i ++ <5) alerta (i);
`` `

O primeiro valor é novamente `i = 1`. O formulário postfix de `i ++` incrementa `i` e depois retorna o valor * antigo *, então a comparação` i ++ <5` usará `i = 0` (ao contrário de` ++ i <5`).

Mas a chamada de "alerta" é separada. É outra declaração que se executa após o incremento e a comparação. Então, ele recebe o atual `i = 1`.

Em seguida, siga `2,3,4 ...`

Vamos parar no `i = 4`. O formulário de prefixo `++ i` aumentaria e usaria` 5` na comparação. Mas aqui temos o formulário postfix `i ++`. Então ele incrementa `i` to` 5`, mas retorna o valor antigo. Portanto, a comparação é realmente `while (4 <5)` - true, e o controle continua para 'alert`.

O valor `i = 5` é o último, porque na próxima etapa` while (5 <5) `é falso.
