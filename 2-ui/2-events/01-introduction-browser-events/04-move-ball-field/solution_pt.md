
Primeiro precisamos escolher um método de posicionamento da bola.

Não podemos usar `position: fixed` para isso, porque deslocando a página moveria a bola do campo.

Então, devemos usar `position: absolute` e, para tornar o posicionamento realmente sólido, faça o` field` em si mesmo.

Então a bola será posicionada relativamente ao campo:

`` `css
#field {
largura: 200px;
altura: 150px;
posição: relativa;
}

#ball {
posição: absoluta;
esquerda: 0; / * relativo ao antepassado posicionado mais próximo (campo) * /
topo: 0;
transição: 1s tudo; / * Animação CSS para esquerda / superior faz a bola voar * /
}
`` `

Em seguida, precisamos atribuir a correta `ball.style.position.left / top`. Eles contêm coordenadas relativas ao campo agora.

Aqui está a foto:

!] [] (move-ball-coords.png)

Nós temos `event.clientX / clientY` - coordenadas relativas à janela do clique.

Para obter a coordenada do lado esquerdo do campo clicando no campo, podemos subtrair a margem esquerda do campo e a largura da borda:

`` `js
deixe left = event.clientX - fieldInnerCoords.left - field.clientLeft;
`` `

Normalmente, `ball.style.position.left` significa a" margem esquerda do elemento "(a bola). Então, se atribuirmos que `saiu ', então a borda da bola ficaria sob o cursor do mouse.

Precisamos mover a bola a meio largura para a esquerda e a meia altura para torná-la central.

Então, o "esquerdo" final seria:

`` `js
deixe left = event.clientX - fieldInnerCoords.left - field.clientLeft - ball.offsetWidth / 2;
`` `

A coordenada vertical é calculada usando a mesma lógica.

Observe que a largura / altura da bola deve ser conhecida no momento em que acessamos `ball.offsetWidth`. Deve ser especificado em HTML ou CSS.
