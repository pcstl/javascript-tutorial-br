# Regex para cores HTML

Crie um regexp para pesquisar cores HTML escritas como '# ABCDEF`: primeiro `#` e depois 6 caracteres hexadecimais.

Um exemplo de uso:

`` `js
deixe reg = /... sua regexp ... /

Deixe str = "cor: # 121212; cor de fundo: # AA00ef bad-colors: f # fddee # fd2 # 12345678";

alerta (str.match (reg)) // # 121212, # AA00ef
```

P.S. Nesta tarefa, não precisamos de outros formatos de cores como `# 123` ou` rgb (1,2,3) `etc.
