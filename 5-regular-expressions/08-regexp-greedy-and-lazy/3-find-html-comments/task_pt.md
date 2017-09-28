# Encontre comentários em HTML

Encontre todos os comentários HTML no texto:

`` `js
Deixe reg = / your regexp / g;

Deixe str = `... <! - My - comment
teste -> ... <! ----> ..
`;

alerta (str.match (reg)); // '<! - My - comment \ n test ->', '<! ---->'
```
