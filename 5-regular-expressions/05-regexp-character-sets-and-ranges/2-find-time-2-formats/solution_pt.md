Resposta: `padrão: \ d \ d [-:] \ d \ d`.

`` `js run
Deixe reg = / \ d \ d [-:] \ d \ d / g;
alerta ("Café da manhã às 09:00. Jantar em 21-30" .match (reg)); // 09:00, 21-30
```

Observe que o padrão do dash: '-' `tem um significado especial entre colchetes, mas apenas entre outros caracteres, não quando é no início ou no final, então não precisamos escapar dele.
