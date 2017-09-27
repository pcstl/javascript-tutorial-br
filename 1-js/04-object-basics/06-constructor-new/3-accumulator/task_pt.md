importância: 5

---

# Criar novo acumulador

Crie uma função de construtor `Accumulator (startingValue)`.

O objeto que ele cria deve:

- Armazena o "valor atual" na propriedade `valor`. O valor inicial é definido como o argumento do construtor `startingValue`.
- O método `read ()` deve usar `prompt` para ler um novo número e adicioná-lo ao` valor`.

Em outras palavras, a propriedade `value` é a soma de todos os valores inseridos pelo usuário com o valor inicial` startingValue`.

Aqui está a demo do código:

`` `js
deixe acumulador = novo acumulador (1); // valor inicial 1
acumulator.read (); // adiciona o valor introduzido pelo usuário
acumulator.read (); // adiciona o valor introduzido pelo usuário
alerta (acumulador.valor); // mostra a soma destes valores
`` `

[demo]
