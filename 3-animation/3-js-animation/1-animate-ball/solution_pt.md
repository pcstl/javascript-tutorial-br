Para saltar, podemos usar a propriedade CSS `top` e` position: absolute` para a bola dentro do campo com `position: relative`.

A coordenada inferior do campo é `field.clientHeight`. Mas a propriedade `top` dá coordenadas para o topo da bola, a posição da borda é` field.clientHeight - ball.clientHeight`.

Então, animamos o `top` de` 0` para `field.clientHeight - ball.clientHeight`.

Agora, para obter o efeito "saltando", podemos usar a função de temporização `saltar 'no modo` easeOut`.

Aqui está o código final para a animação:

`` `js
Deixe para = field.clientHeight - ball.clientHeight;

animar ({
duração: 2000,
timing: makeEaseOut (rebote),
desenhar (progresso) {
ball.style.top = to * progresso + 'px'
}
});
```
