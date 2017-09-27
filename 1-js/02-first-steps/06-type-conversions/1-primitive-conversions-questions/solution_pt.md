
`` `js no-embellecer
"" + 1 + 0 = "10" // (1)
"" - 1 + 0 = -1 // (2)
true + false = 1
6 / "3" = 2
"2" * "3" = 6
4 + 5 + "px" = "9px"
"$" + 4 + 5 = "$ 45"
"4" - 2 = 2
"4px" - 2 = NaN
7/0 = Infinito
"-9 \ n" + 5 = "-9 \ n5"
"-9 \ n" - 5 = -14
nulo + 1 = 1 // (3)
indefinido + 1 = NaN // (4)
`` `

1. A adição com uma string `" "+ 1" converte `1` para uma string:` "" + 1 = "1" `, e então temos" 1 "+ 0`, a mesma regra é aplicada.
2. A subscrição `" - "` (como a maioria das operações de matemática) só funciona com números, converte uma string vazia `` "` para `0`.
3. `null` torna-se` 0 'após a conversão numérica.
4. `undefined` torna-se 'NaN' após a conversão numérica.
