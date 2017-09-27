importância: 5

---

# Traduz a borda-largura esquerda para borderLeftWidth

Escreva a função `camelize (str)` que altera as palavras separadas do tablão como "my-short-string" em "myShortString" com camel.

Ou seja: remove todos os traços, cada palavra depois do traço se torna maiúscula.

Exemplos:

`` `js
camelize ("background-color") == 'backgroundColor';
camelize ("list-style-image") == 'listStyleImage';
camelize ("- webkit-transition") == 'WebkitTransition';
`` `

P.S. Dica: use `split` para dividir a string em uma matriz, transformá-la e` join` back.
