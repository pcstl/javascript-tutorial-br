importância: 4

---

# Carregar imagens visíveis

Digamos que temos um cliente de baixa velocidade e queremos salvar seu tráfego móvel.

Para esse efeito, decidimos não exibir imagens imediatamente, mas sim substituí-las por espaços reservados, assim:

`` `html
<img *! * src = "placeholder.svg" * /! * width = "128" height = "128" *! * data-src = "real.jpg" * /! *>
`` `

Então, inicialmente, todas as imagens são `placeholder.svg`. Quando a página se desloca para a posição onde o usuário pode ver a imagem - nós mudamos `src` para o de` data-src`, e assim a imagem é carregada.

Aqui está um exemplo no `iframe`:

[iframe src = "solução"]

Desloque-o para ver as imagens carregar "on-demand".

Requisitos:
- Quando a página é carregada, as imagens que estão na tela devem ser carregadas imediatamente, antes de qualquer rolagem.
- Algumas imagens podem ser regulares, sem `data-src`. O código não deve tocá-los.
- Uma vez que uma imagem é carregada, não deve mais recarregar quando deslocou para dentro / para fora.

P.S. Se você puder, faça uma solução mais avançada que "preload" imagens que são uma página abaixo / após a posição atual.

P.P.S. Somente roteio vertical deve ser manipulado, sem rolagem horizontal.
