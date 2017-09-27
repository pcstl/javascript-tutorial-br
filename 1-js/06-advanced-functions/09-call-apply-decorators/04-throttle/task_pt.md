importância: 5

---

# Decorador de aceleração

Crie um "acelerador" decorador `throttle (f, ms)` - que retorna um invólucro, passando a chamada para `f` no máximo uma vez por milissegundos. As chamadas que se enquadram no período de "cooldown" são ignoradas.

** A diferença com `debounce` - se uma chamada ignorada for a última durante o processo de cooldown, então ela será executada no final do atraso. **

Vamos verificar o aplicativo da vida real para entender melhor esse requisito e para ver de onde ele vem.

** Por exemplo, queremos acompanhar os movimentos do mouse. **

No navegador, podemos configurar uma função para executar em cada micro-movimento de mouse e obter a localização do ponteiro à medida que ela se move. Durante um uso de mouse ativo, esta função geralmente é executada com muita freqüência, pode ser algo como 100 vezes por segundo (a cada 10 ms).

** A função de rastreamento deve atualizar algumas informações na página da Web. **

Atualizar a função `update ()` é muito pesado para fazê-lo em cada micro-movimento. Também não faz sentido fazê-lo com mais freqüência do que uma vez por 100ms.

Então, atribuiremos `throttle (update, 100)` como a função a ser executada em cada movimento do mouse em vez da atualização original () `. O decorador será chamado frequentemente, mas `update ()` será chamado no máximo uma vez por 100ms.

Visualmente, ficará assim:

1. Para o primeiro movimento do mouse, a variante decorada passa a chamada para 'atualização'. Isso é importante, o usuário vê nossa reação ao seu movimento imediatamente.
2. Então, à medida que o mouse se move, até `100ms` nada acontece. A variante decorada ignora as chamadas.
3. No final de `100ms` - uma atualização mais 'acontece com as últimas coordenadas.
4. Então, finalmente, o mouse pára em algum lugar. A variante decorada aguarda até `100ms` expirar e depois executa as atualizações` update` com as últimas coordenadas. Então, talvez o mais importante, as coordenadas do mouse final são processadas.

Um exemplo de código:

`` `js
função f (a) {
console.log (a)
};

// f1000 passa chamadas para f no máximo uma vez por 1000 ms
Deixe f1000 = acelerador (f, 1000);

f1000 (1); // mostra 1
f1000 (2); // (aceleração, 1000ms ainda não está disponível)
f1000 (3); // (aceleração, 1000ms ainda não está disponível)

// quando o tempo limite de 1000 ms ...
// ... saídas 3, o valor intermediário 2 foi ignorado
`` `

P.S. Argumentos e o contexto `this` passado para` f1000` devem ser passados ​​para o `f` original.
