# Encontre o tempo como hh: mm ou hh-mm

O tempo pode estar no formato `horas: minutos 'ou` horas-minutos'. Ambas as horas e os minutos têm 2 dígitos: `09: 00` ou` 21-30`.

Escreva um regex para encontrar tempo:

`` `js
Deixe reg = / your regexp / g;
alerta ("Café da manhã às 09:00. Jantar em 21-30" .match (reg)); // 09:00, 21-30
```

P.S. Nesta tarefa, assumimos que o tempo está sempre correto, não há necessidade de filtrar linhas erradas como "45:67". Mais tarde, lidaremos com isso também.
