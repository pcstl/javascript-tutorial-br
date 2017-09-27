
1. Vamos adicionar `__proto__`:

`` `js run
deixe head = {
óculos: 1
};

deixe table = {
caneta: 3,
__proto__: head
};

deixe bed = {
folha: 1,
travesseiro: 2,
__proto__: tabela
};

deixe os bolsos = {
dinheiro: 2000,
__proto__: cama
};

alerta (pockets.pen); // 3
alerta (bed.glasses); // 1
alerta (table.money); // Indefinido
`` `

2. Nos motores modernos, desempenho-sábio, não há diferença se nós tomamos uma propriedade de um objeto ou seu protótipo. Eles lembram onde a propriedade foi encontrada e reutilizá-la no próximo pedido.

Por exemplo, para `pockets.glasses`, eles se lembram de onde eles encontraram` glasses` (na `head`), e a próxima vez procurará aqui. Eles também são inteligentes o suficiente para atualizar caches internos se algo mudar, para que a otimização seja segura.
