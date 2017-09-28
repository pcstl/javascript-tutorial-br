# Encontre etiquetas HTML

Crie uma expressão regular para encontrar todas as tags HTML (abrindo e fechando) com seus atributos.

Um exemplo de uso:

`` `js run
Deixe reg = / your regexp / g;

Deixe str = '<> <a href="/"> <input type = "radio" verificado> <b>';

alerta (str.match (reg)); // '<a href="/">', '<input type = "radio" marcado>', '<b>'
```

Vamos supor que não pode conter `<` e `>` dentro (entre citações também), isso simplifica as coisas um pouco.
