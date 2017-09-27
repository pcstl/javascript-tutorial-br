importância: 5

---

# Posicione a nota dentro (absoluto)

Estenda a tarefa anterior <info: task / position-at-absolute>: ensine a função `positionAt (âncora, posição, elem)` para inserir `elem` dentro da` âncora '.

Novos valores para `position`:

- `top-out`,` right-out`, `bottom-out` - funcionam da mesma forma que antes, eles inserem o` elem` over / right / under `anchor`.
- `top-in`,` right-in`, `bottom-in` - insira` element` dentro da `anchor`: coloque-o na borda superior / direita / inferior.

Por exemplo:

`` `js
// mostra a nota acima blockquote
positionAt (blockquote, "top-out", nota);

// mostra a nota dentro de blockquote, no topo
positionAt (blockquote, "top-in", nota);
`` `

O resultado:

[iframe src = "solução" height = "310" border = "1" link]

Como o código-fonte, tome a solução da tarefa <info: task / position-at-absolute>.
