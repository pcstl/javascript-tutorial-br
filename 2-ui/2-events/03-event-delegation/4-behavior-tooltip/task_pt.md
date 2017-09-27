importância: 5

---

# Comportamento de dica de ferramenta

Crie o código JS para o comportamento da dica de ferramenta.

Quando um mouse passa por um elemento com `data-tooltip`, a dica de ferramenta deve aparecer sobre ele, e quando ele desaparecer, esconder.

Um exemplo de HTML anotado:
`` `html
<button data-tooltip = "a dica de ferramenta é mais longa do que o elemento"> botão curto </ button>
<button data-tooltip = "HTML <br> tooltip"> Mais um botão </ button>
`` `

Deve funcionar assim:

[iframe src = "solução" height = 200 border = 1]

Nesta tarefa, assumimos que todos os elementos com `data-tooltip` têm apenas texto dentro. Não há tags aninhadas.

Detalhes:

- A dica de ferramenta não deve cruzar as arestas da janela. Normalmente, ele deve estar acima do elemento, mas se o elemento estiver na parte superior da página e não há espaço para a dica de ferramenta, abaixo abaixo.
- A dica de ferramenta é fornecida no atributo `data-tooltip`. Pode ser HTML arbitrário.

Você precisará de dois eventos aqui:
- `mouseover` desencadeia quando um ponteiro vem sobre um elemento.
- `mouseout` dispara quando um ponteiro deixa um elemento.

Use a delegação de eventos: configure dois manipuladores no `documento 'para rastrear todos os" overs "e" outs "de elementos com` data-tooltip` e gerencie dicas de ferramentas a partir daí.

Depois que o comportamento é implementado, mesmo as pessoas que não estão familiarizadas com o JavaScript podem adicionar elementos anotados.

P.S. Para manter as coisas simples e simples: apenas uma dica de ferramenta pode aparecer de cada vez.
