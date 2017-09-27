Não podemos "substituir" o primeiro personagem, porque as cadeias em JavaScript são imutáveis.

Mas podemos criar uma nova string com base no existente, com o primeiro personagem em maiúscula:

`` `js
Deixe newStr = str [0] .toUpperCase () + str.slice (1);
`` `

Há um pequeno problema. Se `str` estiver vazio, então` str [0] `é indefinido, então vamos ter um erro.

Existem duas variantes aqui:

1. Use `str.charAt (0)`, pois sempre retorna uma string (talvez vazia).
2. Adicione um teste para uma string vazia.

Aqui está a segunda variante:

`` `js run
função ucFirst (str) {
se (! str) retornar str;

retornar str [0] .toUpperCase () + str.slice (1);
}

alerta (ucFirst ("john")); // John
`` `

