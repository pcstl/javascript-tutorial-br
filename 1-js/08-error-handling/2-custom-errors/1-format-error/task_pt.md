importância: 5

---

# Herdar de SyntaxError

Crie uma classe `FormatError` que herda da classe incorporada` SyntaxError`.

Ele deve suportar as propriedades `message`,` name` e `stack`.

Exemplo de uso:

`` `js
Deixe err = new FormatError ("erro de formatação");

alerta (err.message); // erro de formatação
alerta (err.name); // FormatError
alerta (err.stack); // pilha

alerta (erro instanceof FormatError); // verdade
alerta (instância err de SyntaxError); // true (porque herda do SyntaxError)
`` `
