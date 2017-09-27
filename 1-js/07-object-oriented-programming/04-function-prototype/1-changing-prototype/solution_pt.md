
Respostas:

1. `true`.

A atribuição de `Rabbit.prototype` configura` [[Prototype]] `para novos objetos, mas não afeta os existentes.

2. `falso '.

Os objetos são atribuídos por referência. O objeto de `Rabbit.prototype` não está duplicado, ainda é que um único objeto é referenciado por` Rabbit.prototype` e pelo `[[Prototype]]` `` rabbit`.

Então, quando mudamos seu conteúdo através de uma referência, é visível através do outro.

3. `true`.

Todas as operações `delete` são aplicadas diretamente ao objeto. Aqui, `delete rabbit.eats` tenta remover a propriedade` eats` de `rabbit`, mas não a possui. Portanto, a operação não terá nenhum efeito.

4. `indefinido '.

A propriedade `eats` é excluída do protótipo, não existe mais.
