A solução usando um loop:

`` `js run
função sumTo (n) {
deixe soma = 0;
para (vamos i = 1; i <= n; i ++) {
soma + = i;
}
Soma de retorno;
}

alerta (Sumter (100));
`` `

A solução usando a recursão:

`` `js run
função sumTo (n) {
se (n == 1) retornar 1;
retornar n + sumTo (n - 1);
}

alerta (Sumter (100));
`` `

A solução usando a fórmula: `sumTo (n) = n * (n + 1) / 2`:

`` `js run
função sumTo (n) {
retornar n * (n + 1) / 2;
}

alerta (Sumter (100));
`` `

P.S. Naturalmente, a fórmula é a solução mais rápida. Ele usa apenas 3 operações para qualquer número `n`. A matemática ajuda!

A variante do loop é a segunda em termos de velocidade. Tanto na variante recursiva quanto no loop, somamos os mesmos números. Mas a recursão envolve chamadas aninhadas e gerenciamento de pilha de execução. Isso também requer recursos, então é mais lento.

P.P.S. O padrão descreve uma otimização de "chamada de cauda": se a chamada recursiva é a última na função (como no `sumTo` acima), então a função externa não precisará retomar a execução e não precisamos lembrar seu contexto de execução. Nesse caso, `sumTo (100000)` é contável. Mas se o seu mecanismo de JavaScript não o suportar, haverá um erro: o tamanho máximo da pilha excedido, porque geralmente há uma limitação no tamanho total da pilha.
