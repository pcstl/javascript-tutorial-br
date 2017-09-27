importância: 3

---

# Explique o valor de "isto"

No código abaixo, pretendemos chamar o método `user.go ()` 4 vezes seguidas.

Mas as chamadas `(1)` e `(2)` funcionam de forma diferente de `(3)` e `(4)`. Por quê?

`` `js run no-embellecer
deixe obj, método;

obj = {
go: function () {alert (this); }
};

obj.go (); // (1) [object Object]

(obj.go) (); // (2) [object Object]

(método = obj.go) (); // (3) indefinido

(obj.go || obj.stop) (); // (4) indefinido
`` `

