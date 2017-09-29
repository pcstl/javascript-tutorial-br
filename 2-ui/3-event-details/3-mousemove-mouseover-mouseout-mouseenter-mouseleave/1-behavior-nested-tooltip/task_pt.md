importância: 5

---

# Comportamento de dica de ferramenta melhorado

Escreva o JavaScript que mostra uma dica de ferramenta sobre um elemento com o atributo `data-tooltip`.

Isso é como a tarefa <info: task / behavior-tooltip>, mas aqui os elementos anotados podem ser aninhados. A ferramenta mais aninhada é mostrada.

Por exemplo:

`` `html
<div data-tooltip = "Aqui - é o interior da casa" id = "house">
<div data-tooltip = "Aqui - é o telhado" id = "roof"> </ div>
...
<a href="https://en.wikipedia.org/wiki/The_Three_Little_Pigs" data-tooltip="Leia em ...."> Passeio sobre mim </a>
</ div>
`` `

O resultado no iframe:

[iframe src = "solução" height = 300 border = 1]

P.S. Dica: apenas uma dica de ferramenta pode aparecer ao mesmo tempo.
