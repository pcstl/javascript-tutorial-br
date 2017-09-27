

`` `js no-embellecer
5> 4 → true
"maçã"> "abacaxi" → falso
"2"> "12" → verdadeiro
indefinido == nulo → verdadeiro
indefinido === nulo → falso
null == "\ n0 \ n" → falso
null === + "\ n0 \ n" → falso
`` `

Alguns dos motivos:

1. Obviamente, é verdade.
2. Comparação do dicionário, portanto, falso.
3. Novamente, a comparação do dicionário, o primeiro char de `" 2 "` é maior do que o primeiro char de `" 1 "`.
4. Os valores `null` e` indefinidos` são iguais apenas.
5. A igualdade restrita é rigorosa. Diferentes tipos de ambos os lados levam a falso.
6. Veja (4).
7. Igualdade estrita de diferentes tipos.
