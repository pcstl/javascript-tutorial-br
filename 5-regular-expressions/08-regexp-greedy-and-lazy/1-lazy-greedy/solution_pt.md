
O resultado é: `match: 123 4`.

Primeiro, o padrão preguiçoso: \ d +? `Tenta tirar o mínimo de dígitos possível, mas tem que chegar ao espaço, então é preciso` match: 123`.

Então, o segundo `\ d +?` Leva apenas um dígito, porque isso é suficiente.
