
Tente executá-lo:

`` `js run
Deixe str = "Olá";

str.test = 5; // (*)

alerta (str.test);
`` `

Pode haver dois tipos de resultado:
1. `indefinido '
2. Um erro.

Por quê? Vamos reproduzir o que está acontecendo na linha `(*)`:

1. Quando uma propriedade de `str` é acessada, um" objeto wrapper "é criado.
2. A operação com a propriedade é realizada nela. Então, o objeto obtém a propriedade `test`.
3. A operação termina e o "objeto wrapper" desaparece.

Então, na última linha, `str` não possui vestígios da propriedade. Um novo objeto wrapper para cada operação de objeto em uma string.

Entretanto, alguns navegadores podem decidir limitar ainda mais o programador e não permitem atribuir propriedades a primitivas. É por isso que, na prática, também podemos ver erros na linha `(*)`. No entanto, está um pouco mais longe da especificação.

** Este exemplo mostra claramente que as primitivas não são objetos. **

Eles simplesmente não podem armazenar dados.

Todas as operações de propriedade / método são realizadas com a ajuda de objetos temporários.

