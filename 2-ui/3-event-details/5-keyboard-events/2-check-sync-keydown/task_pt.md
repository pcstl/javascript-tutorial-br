importância: 5

---

# Teclas rápidas estendidas

Crie uma função `runOnKeys (func, code1, code2, ... code_n)` que executa `func` na pressão simultânea de teclas com códigos` code1`, `code2`, ...,` code_n`.

Por exemplo, o código abaixo mostra `alert` quando` "Q" `e` "W" `são pressionados juntos (em qualquer idioma, com ou sem CapsLock)

`` `js no-embellecer
runOnKeys (

"KeyQ",
"KeyW"
);
`` `

[demo src = "solução"]
