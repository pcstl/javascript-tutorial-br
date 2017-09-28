
# Círculo animado com retorno de chamada

Na tarefa <info: task / animate-circle> é mostrado um círculo de animação crescente.

Agora, digamos que não precisamos apenas de um círculo, mas para mostrar uma mensagem dentro dele. A mensagem deve aparecer * após * a animação está completa (o círculo está totalmente crescido), caso contrário ficaria feio.

Na solução da tarefa, a função `showCircle (cx, cy, radius) 'desenha o círculo, mas não dá forma de rastrear quando está pronto.

Adicione um argumento de retorno de chamada: `showCircle (cx, cy, radius, callback)` para ser chamado quando a animação for concluída. O `callback` deve receber o círculo` <div> `como um argumento.

Aqui está o exemplo:

`` `js
showCircle (150, 150, 100, div => {
div.classList.add ('message-ball');
div.append ("Olá, mundo!");
});
```

Demo:

[iframe src = "solução" altura = 260]

Tome a solução da tarefa <info: task / animate-circle> como base.
