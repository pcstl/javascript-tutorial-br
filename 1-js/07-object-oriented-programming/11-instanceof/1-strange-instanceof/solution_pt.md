Sim, parece estranho.

Mas `instanceof` não se preocupa com a função, mas sim com o` protótipo ', que combina com a cadeia do protótipo.

E aqui `a .__ proto__ == B.prototype`, então` instanceof` retorna `true`.

Assim, pela lógica de `instanceof`, o` prototype` realmente define o tipo, e não a função do construtor.
