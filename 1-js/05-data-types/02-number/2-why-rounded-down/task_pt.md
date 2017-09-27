importância: 4

---

# Por que 6.35.toFixed (1) == 6.3?

De acordo com a documentação `Math.round` e` toFixed`, ambas as rodadas para o número mais próximo: `0..4` conduzem para baixo enquanto` 5..9` levam.

Por exemplo:

`` `js run
alerta (1.35.toFixed (1)); // 1.4
`` `

No exemplo semelhante abaixo, por que `6.35 'é arredondado para` 6.3`, não `6.4?

`` `js run
alerta (6.35.toFixed (1)); // 6.3
`` `

Como rodar `6.35? O caminho certo?

