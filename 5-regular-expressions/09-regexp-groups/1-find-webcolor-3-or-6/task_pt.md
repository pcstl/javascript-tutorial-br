# Encontre cores no formato #abc ou #abcdef

Escreva um regexp que corresponda cores no formato `# abc` ou` # abcdef`. Isto é: `#` seguido por 3 ou 6 dígitos hexadimais.

Exemplo de uso:
`` `js
Deixe reg = / your regexp / g;

Deixe str = "cor: # 3f3; background-color: # AA00ef; e: #abcd";

alerta (str.match (reg)); // # 3f3 # AA0ef
```

P.S. Deve ser exatamente 3 ou 6 dígitos hexadecimais: valores como `# abcd` não devem corresponder.
