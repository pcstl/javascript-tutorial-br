Na tarefa <info: task / animate-ball>, tivemos apenas uma propriedade para animar. Agora precisamos de mais um: `elem.style.left`.

A coordenada horizontal muda por outra lei: não "rebote", mas gradualmente aumenta a mudança da bola para a direita.

Podemos escrever mais um "animar" para isso.

Como a função de tempo, poderíamos usar `linear ', mas algo como` makeEaseOut (quad)' parece muito melhor.

O código:

`` `js
Deixe height = field.clientHeight - ball.clientHeight;
deixe largura = 100;

// animate top (saltando)
animar ({
duração: 2000,
timing: makeEaseOut (rebote),
desenhar: função (progresso) {
ball.style.top = altura * progresso + 'px'
}
});

// animar a esquerda (mover para a direita)
animar ({
duração: 2000,
timing: makeEaseOut (quad),
desenhar: função (progresso) {
ball.style.left = largura * progresso + "px"
}
});
```
