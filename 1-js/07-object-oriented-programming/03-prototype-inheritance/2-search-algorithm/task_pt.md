importância: 5

---

Algoritmo # Searching

A tarefa tem duas partes.

Temos um objeto:

`` `js
deixe head = {
óculos: 1
};

deixe table = {
caneta: 3
};

deixe bed = {
folha: 1,
travesseiro: 2
};

deixe os bolsos = {
dinheiro: 2000
};
`` `

1. Use `__proto__` para atribuir protótipos de forma que qualquer pesquisa de propriedade siga o caminho:` pockets` -> `bed` ->` table` -> `head`. Por exemplo, `pockets.pen` deve ser` 3` (encontrado na `tabela`) e` bed.glasses` deve ser `1` (encontrado em` head`).
2. Responda a pergunta: é mais rápido obter `glasses` como` pocket.glasses` ou `head.glasses`? Benchmark, se necessário.
