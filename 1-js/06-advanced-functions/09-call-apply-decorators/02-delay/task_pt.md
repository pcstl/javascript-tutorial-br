importância: 5

---

# Decorador atrasado

Crie um delay de decorador `(f, ms)` que atrasa cada chamada de `f` por` ms 'milissegundos.

Por exemplo:

`` `js
função f (x) {
alerta (x);
}

// criar invólucros
deixe f1000 = atraso (f, 1000);
deixe f1500 = atraso (f, 1500);

f1000 ("teste"); // mostra "teste" após 1000ms
f1500 ("teste"); // mostra "teste" após 1500ms
`` `

Em outras palavras, `delay (f, ms)` retorna uma variante "delayed by` ms` "de` f`.

No código acima, `f` é uma função de um único argumento, mas sua solução deve passar todos os argumentos e o contexto` this`.
