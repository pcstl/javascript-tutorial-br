importância: 4

---

# Formate a data relativa

Escreva uma função `formatDate (data)` que deve formatar `data` da seguinte maneira:

- Se a partir da data da "data" for inferior a 1 segundo, então "agora".
- Caso contrário, se a partir da data da "data" for inferior a 1 minuto, então "não há".
- Caso contrário, se menos de uma hora, então "m min. Atrás".
- Caso contrário, a data completa no formato `" DD.MM.YY HH: mm "`. Isto é: `` dia.mão. Ano de horas: minutos "`, tudo em formato de 2 dígitos, e. `31.12.16 10: 00`.

Por exemplo:

`` `js
alerta (formatDate (nova data (nova data - 1))); // "agora mesmo"

alerta (formatDate (nova data (nova data - 30 * 1000))); // "30 seg. Ago"

alerta (formatDate (nova data (nova data - 5 * 60 * 1000))); // "5 min. Ago"

// data de ontem como 31.12.2016, 20:00
alerta (formatDate (nova data (nova data - 86400 * 1000)));
`` `
