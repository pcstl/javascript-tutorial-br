importância: 4

---

# Constante maiúscula?

Examine o seguinte código:

`` `js
const birthday = '18 .04.1982 ';

const age = someCode (aniversário);
`` `

Aqui temos uma data constante de "aniversário" e a "idade" é calculada a partir de "aniversario" com a ajuda de algum código (não é fornecido para breu, e porque os detalhes não são importantes aqui).

Seria correto usar maiúsculas e minúsculas para `birthday`? Para `age`? Ou mesmo para ambos?

`` `js
const BIRTHDAY = '18 .04.1982 '; // fazer maiúsculas?

const AGE = someCode (BIRTHDAY); // fazer maiúsculas?
`` `

