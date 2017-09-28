
Solução:

`` `js run
deixe reg = /\.{3,}/g;
alerta ("Olá! ... Como vai? .....". match (reg)); // ..., .....
```

Tenha em atenção que o ponto é um caractere especial, por isso temos que escapar e inserir como `\ .`.
