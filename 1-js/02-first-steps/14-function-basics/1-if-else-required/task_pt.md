importância: 4

---

# O "else" é necessário?

A seguinte função retorna `true` se o parâmetro` age` for maior que `18`.

Caso contrário, solicita uma confirmação e retorna o resultado:

`` `js
função checkAge (age) {
se (idade> 18) {
retornar verdadeiro;
*! *
} outro {
// ...
Retornar confirmar ('Os pais te permitiram?');
}
* /! *
}
`` `

A função funcionará de forma diferente se o `else` for removido?

`` `js
função checkAge (age) {
se (idade> 18) {
retornar verdadeiro;
}
*! *
// ...
Retornar confirmar ('Os pais te permitiram?');
* /! *
}
`` `

Existe alguma diferença no comportamento dessas duas variantes?
