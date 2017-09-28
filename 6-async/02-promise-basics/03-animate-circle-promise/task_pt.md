
# Círculo animado com promessa

Reescreva a função `showCircle` na solução da tarefa <info: task / animate-circle-callback> para que ela retorne uma promessa em vez de aceitar um retorno de chamada.

O novo uso:

`` `js
showCircle (150, 150, 100) .then (div => {
div.classList.add ('message-ball');
div.append ("Olá, mundo!");
});
```

Tome a solução da tarefa <info: task / animate-circle-callback> como base.
