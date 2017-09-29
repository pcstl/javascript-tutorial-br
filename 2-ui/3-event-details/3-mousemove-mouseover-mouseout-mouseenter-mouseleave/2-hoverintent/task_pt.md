importância: 5

---

# Dica de ferramenta "inteligente"

Escreva uma função que mostra uma dica de ferramenta sobre um elemento somente se o visitante mover o mouse * sobre ele *, mas não * através dele *.

Em outras palavras, se o visitante mover o mouse no elemento e parar - mostre a dica de ferramenta. E se ele simplesmente movia o mouse rapidamente, então não precisava, quem quer piscar extra?

Tecnicamente, podemos medir a velocidade do mouse sobre o elemento e, se for lento, assumimos que ele vem "sobre o elemento" e mostra a dica de ferramenta, se for rápido - então ignoramos.

Faça um objeto universal `new HoverIntent (options)` para ele. Com `options`:

- `elem` - elemento para rastrear.
- `over` - uma função para chamar se o mouse está movendo lentamente o elemento.
- `out` - uma função para chamar quando o mouse sai do elemento (se for chamado 'over`).

Um exemplo de usar esse objeto para a dica de ferramenta:

`` `js
// uma amostra de dicas de ferramentas
deixe tooltip = document.createElement ('div');
tooltip.className = "tooltip";
tooltip.innerHTML = "Tooltip";

// o objeto rastreará o mouse e ligará / desligará
novo HoverIntent ({
itens
sobre() {
tooltip.style.left = elem.getBoundingClientRect (). left + 'px';
tooltip.style.top = elem.getBoundingClientRect (). bottom + 5 + 'px';
document.body.append (tooltip);
},
Fora() {
tooltip.remove ();
}
});
`` `

A demo:

[iframe src = "solução" height = 140]

Se você mover o mouse sobre o "relógio" rápido, então nada acontece, e se você fizer isso lento ou parar com eles, então haverá uma dica de ferramenta.

Observe: a dica de ferramenta não "pisca" quando o cursor se move entre os subelementos do relógio.
