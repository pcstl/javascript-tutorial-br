A resposta: `1`, e depois` indefinido '.

`` `js run

`` `

A chamada para `alert` retorna` undefined` (ele apenas mostra uma mensagem, então não há retorno significativo).

Por isso, `&&` avalia o operando esquerdo (saídas `1`) e pára imediatamente, porque` indefinido 'é um valor falso. E `&&` procura um valor de falsy e o retorna, então está feito.

