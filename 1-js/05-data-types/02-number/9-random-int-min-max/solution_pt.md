# A solução simples, mas errada

A solução mais simples, mas errada, seria gerar um valor de `min` para` max` e arredondá-lo:

`` `js run
function randomInteger (min, max) {
let rand = min + Math.random () * (max - min);
retornar Math.round (rand);
}

alerta (randomInteger (1, 3));
`` `

A função funciona, mas está incorreta. A probabilidade de obter valores de limite 'min` e `max` é duas vezes menor do que qualquer outro.

Se você executar o exemplo acima muitas vezes, você veria facilmente que `2` aparece o mais frequentemente.

Isso acontece porque `Math.round ()` obtém números aleatórios do intervalo `1..3` e os arreda da seguinte maneira:

`` `js no-embellecer
valores de 1 ... a 1.4999999999 tornam-se 1
valores de 1,5 ... a 2.4999999999 tornam-se 2
valores de 2,5 ... a 2.9999999999 tornam-se 3
`` `

Agora podemos ver claramente que `1` obtém dois vezes menos valores do que` 2`. E o mesmo com `3`.

# A solução correta

Existem muitas soluções corretas para a tarefa. Um deles é ajustar limites de intervalo. Para garantir os mesmos intervalos, podemos gerar valores de `0,5 a 2,5`, adicionando as probabilidades necessárias às bordas:

`` `js run
*! *
function randomInteger (min, max) {
// now rand é de (min-0.5) para (max + 0.5)
Deixe rand = min - 0.5 + Math.random () * (max - min + 1);
retornar Math.round (rand);
}
* /! *

alerta (randomInteger (1, 3));
`` `

Uma maneira alternativa poderia ser usar `Math.floor` para um número aleatório de` min` para `max + 1`:

`` `js run
*! *
function randomInteger (min, max) {
// aqui rand é de min para (max + 1)
let rand = min + Math.random () * (max + 1 - min);
retornar Math.floor (rand);
}
* /! *

alerta (randomInteger (1, 3));
`` `

Agora, todos os intervalos são mapeados desta maneira:

`` `js no-embellecer
valores de 1 a 1.9999999999 tornam-se 1
valores de 2 ... a 2.9999999999 tornam-se 2
valores de 3 a 3.9999999999 tornam-se 3
`` `

Todos os intervalos têm o mesmo comprimento, tornando a distribuição final uniforme.
