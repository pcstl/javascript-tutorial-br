Um número hexadecimal de dois dígitos é `padrão: [0-9a-f] {2}` (assumindo que o sinalizador `pattern: i` está habilitado).

Precisamos do número `NN` e, em seguida,`: NN` repetido 5 vezes (mais números);

O regexp é: `padrão: [0-9a-f] {2} (: [0-9a-f] {2}) {5}`

Agora vamos mostrar que a partida deve capturar todo o texto: comece no início e termine no final. Isso é feito envolvendo o padrão em `pattern: ^ ... $`.

Finalmente:

`` `js run
deixe reg = / ^ [0-9a-fA-F] {2} (: [0-9a-fA-F] {2}) {5} $ / i;

alerta (reg.test ('01: 32: 54: 67: 89: AB ')); // verdade

alerta (reg.test ('0132546789AB')); // falso (sem dois pontos)

alerta (reg.test ('01: 32: 54: 67: 89 ')); // falso (5 números, necessidade 6)

alerta (reg.test ('01: 32: 54: 67: 89: ZZ ')) // falso (ZZ no final)
```
