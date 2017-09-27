Isso é porque `i` nunca seria igual a` 10`.

Execute-o para ver os valores * reais * de `i`:

`` `js run
deixe i = 0;
enquanto (i <11) {
i + = 0,2;
se (i> 9.8 && i <10.2) alert (i);
}
`` `

Nenhum deles é exatamente `10 '.

Tais coisas acontecem devido às perdas de precisão ao adicionar frações como `0.2`.

Conclusão: evadir verificações de igualdade ao trabalhar com frações decimais.
