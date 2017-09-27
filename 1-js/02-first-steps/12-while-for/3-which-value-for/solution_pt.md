** A resposta: de `0 'a` 4 em ambos os casos. **

`` `js run
para (deixe i = 0; i <5; ++ i) alert (i);

para (deixe i = 0; i <5; i ++) alert (i);
`` `

Isso pode ser facilmente deduzido do algoritmo de `for`:

1. Execute uma vez `i = 0` antes de tudo (começar).
2. Verifique a condição `i <5`
3. Se `true` - execute o loop body` alert (i) `e, em seguida,` i ++ `

O incremento `i ++` é separado da verificação de condição (2). Essa é apenas uma outra declaração.

O valor retornado pelo incremento não é usado aqui, então não há diferença entre `i ++` e `++ i`.
