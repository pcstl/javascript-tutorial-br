importância: 5

---

# Calculadora de depósitos

Crie uma interface que permita inserir uma soma do depósito bancário e porcentagem, e então calcula quanto será após determinados períodos de tempo.

Aqui está a demonstração:

[iframe src = "solução" height = "350" border = "1"]

Qualquer alteração de entrada deve ser processada imediatamente.

A fórmula é:
`` `js
// inicial: a soma inicial de dinheiro
// interesse: e. 0,05 significa 5% ao ano
// anos: quantos anos esperar?
let result = Math.round (inicial * (1 + interesse * anos));
`` `
