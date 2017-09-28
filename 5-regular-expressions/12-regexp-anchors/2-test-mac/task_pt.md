# Verifique o endereço MAC

[MAC-address] (https://en.wikipedia.org/wiki/MAC_address) de uma interface de rede consiste em 6 números hexadecimais de dois dígitos separados por dois pontos.

Por exemplo: `subject: '01: 32: 54: 67: 89: AB'`.

Escreva um regexp que verifique se uma string é MAC-address.

Uso:
`` `js
Deixe reg = / your regexp /;

alerta (reg.test ('01: 32: 54: 67: 89: AB ')); // verdade

alerta (reg.test ('0132546789AB')); // falso (sem dois pontos)

alerta (reg.test ('01: 32: 54: 67: 89 ')); // falso (5 números, deve ser 6)

alerta (reg.test ('01: 32: 54: 67: 89: ZZ ')) // falso (ZZ ad the end)
```
