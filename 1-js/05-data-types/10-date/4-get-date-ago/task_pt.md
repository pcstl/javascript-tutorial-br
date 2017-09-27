importância: 4

---

# Qual dia do mês foi há muitos dias?

Crie uma função `getDateAgo (data e dias) para retornar o dia do mês 'dias' da 'data'.

Por exemplo, se hoje for 20, então `getDateAgo (new Date (), 1)` deve ser 19 e `getDateAgo (new Date (), 2)` deve ser 18.

Deve também funcionar de forma confiável em meses / anos:

`` `js
Deixe data = nova data (2015, 0, 2);

alerta (getDateAgo (data, 1)); // 1, (1 de janeiro de 2015)
alerta (getDateAgo (data, 2)); // 31, (31 de dezembro de 2014)
alerta (getDateAgo (data, 365)); // 2, (2 de janeiro de 2014)
`` `

P.S. A função não deve modificar a data prevista.
