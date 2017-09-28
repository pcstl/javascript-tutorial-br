# Animações de JavaScript

As animações de JavaScript podem lidar com coisas que o CSS não pode.

Por exemplo, movendo-se ao longo de um caminho complexo, com uma função de temporização diferente das curvas de Bezier ou uma animação em uma tela.

[cortar]

## setInterval

Do ponto de vista HTML / CSS, uma animação é uma mudança gradual da propriedade de estilo. Por exemplo, mudar `style.left` de` 0px` para `100px` move o elemento.

E se o aumentarmos em `setInterval`, fazendo 50 pequenas mudanças por segundo, então parece suave. Esse é o mesmo princípio que no cinema: 24 ou mais quadros por segundo são suficientes para fazê-lo parecer suave.

O pseudo-código pode ser assim:

`` `js
Deixe atrasar = 1000/50; // em 1 segundo 50 quadros
let timer = setInterval (function () {
se (animação completa) clearInterval (timer);
senão aumentar style.left
}, atraso)
```

Exemplo mais completo da animação:

`` `js
deixe start = Date.now (); // lembre-se da hora de início

let timer = setInterval (function () {
// quanto tempo passou desde o início?
Deixe timePassed = Date.now () - start;

se (timePassed> = 2000) {
clearInterval (timer); // termine a animação após 2 segundos
Retorna;
}

// desenhe a animação no momento em que o TimePassed
desenhar (timePassed);

}, 20);

// como timePassed vai de 0 a 2000
// esquerda obtém valores de 0px a 400px
desenho de função (timePassed) {
train.style.left = timePassed / 5 + 'px';
}
```

Clique para a demo:

[codetabs height = 200 src = "move"]

## requestAnimationFrame

Imagine que temos várias animações executadas simultaneamente.

Se os executamos separadamente, cada um com o seu próprio `setInterval (..., 20)`, então o navegador teria que repetir muito mais vezes do que todos os `20ms`.

Cada `setInterval` dispara uma vez por` 20ms`, mas eles são independentes, então nós temos várias execuções independentes dentro de `20ms`.

Esses vários redatros independentes devem ser agrupados, para facilitar o navegador.

Em outras palavras, isto:

`` `js
setInterval (function () {
animate1 ();
animate2 ();
animate3 ();
}, 20)
```

... É mais leve do que isso:

`` `js
setInterval (animate1, 20);
setInterval (animate2, 20);
setInterval (animate3, 20);
```

Há mais uma coisa a ter em mente. Às vezes, quando a CPU está sobrecarregada, ou há outras razões para redesenhar menos vezes. Por exemplo, se a aba do navegador estiver escondida, então não há nenhum ponto no desenho.

Há um padrão [Tempo de animação] (http://www.w3.org/TR/animation-timing/) que fornece a função `requestAnimationFrame`.

Ele aborda todos esses problemas e ainda mais.

A sintaxe:
`` `js
permitir requestId = requestAnimationFrame (retorno de chamada)
```

Isso agende a função `callback` para executar no tempo mais próximo quando o navegador deseja fazer animação.

Se fizermos alterações nos elementos em 'callback`, eles serão agrupados com outros calllinks' requestAnimationFrame 'e com animações CSS. Então, haverá um novo cálculo de geometria e repintar em vez de muitos.

O valor retornado `requestId` pode ser usado para cancelar a chamada:
`` `js
// cancela a execução agendada do retorno de chamada
cancelAnimationFrame (requestId);
```

O `callback` obtém um argumento - o tempo passado do início da página carregado em microssegundos. Este tempo também pode ser obtido chamando [performance.now ()] (mdn: api / Performance / now).

Normalmente, o "callback" é executado em breve, a menos que a CPU esteja sobrecarregada ou a bateria do laptop esteja quase descarregada, ou há outro motivo.

O código abaixo mostra o tempo entre as primeiras 20 corridas para `requestAnimationFrame`. Normalmente é 10-20ms:

`` `html run height = 40 refresh
<script>
deixe prev = performance.now ();
Deixe tempos = 0;

requestAnimationFrame (função medida (tempo) {
document.body.insertAdjacentHTML ("beforeEnd", Math.floor (time-prev) + "");
prev = time;

se (vezes ++ <10) requestAnimationFrame (measure);
})
</ script>
```

## Animação estruturada

Agora podemos criar uma função de animação mais universal com base em `requestAnimationFrame`:

`` `js
função animada ({timing, desenho, duração}) {

Deixe start = performance.now ();

requestAnimationFrame (função animate (time) {
// timeFraction vai de 0 a 1
Deixe TimeFraction = (time-start) / duration;
se (timeFraction> 1) timeFraction = 1;

// calcula o estado atual da animação
Deixe progress = timing (timeFraction)

desenhar (progresso); // desenhe isso

se (timeFraction <1) {
requestAnimationFrame (animate);
}

});
}
```

Função `animate` aceita 3 parâmetros que descrevem essencialmente a animação:

`duração '
: Tempo total de animação. Como, `1000`.

`timing (timeFraction)`
: Função de temporização, como "função de temporização de transição" da propriedade CSS que obtém a fração de tempo que passou (`0` no início,` 1 no final) e retorna a conclusão da animação (como `y` no Bezier curva).

Por exemplo, uma função linear significa que a animação continua uniformemente com a mesma velocidade:

`` `js
função linear (timeFraction) {
tempo de retornoFração;
}
```

É um gráfico:
! [] (linear.png)

Isso é exatamente como `transition-timing-function: linear`. Existem variantes mais interessantes mostradas abaixo.

`draw (progress)`
: A função que leva o estado de conclusão da animação e o desenha. O valor `progress = 0` indica o estado inicial de animação e` progress = 1` - o estado final.

Esta é essa função que realmente desenha a animação.

Pode mover o elemento:
`` `js
desenho de função (progresso) {
train.style.left = progresso + 'px';
}
```

... Ou faça qualquer outra coisa, podemos animar qualquer coisa, de qualquer forma.


Vamos animar o elemento `width` de` 0` para `100%` usando nossa função.

Clique no elemento para a demo:

[height de codetabs = 60 src = "width"]

O código para isso:

`` `js
animar ({
duração: 1000,
timing (timeFraction) {
tempo de retornoFração;
},
desenhar (progresso) {
elem.style.width = progresso * 100 + '%';
}
});
```

Ao contrário da animação CSS, podemos fazer qualquer função de temporização e qualquer função de desenho aqui. A função de temporização não está limitada pelas curvas de Bezier. E `draw` pode ir além das propriedades, criar novos elementos para animação de fogos de artifício ou algo assim.

## Funções de temporização

Vimos a função de temporização linear mais simples acima.

Vamos ver mais deles. Vamos tentar animações de movimento com diferentes funções de temporização para ver como eles funcionam.

### Potência de n

Se quisermos acelerar a animação, podemos usar `progress` na potência` n`.

Por exemplo, uma curva parabólica:

`` `js
função quad (timeFraction) {
retornar Math.pow (timeFraction, 2)
}
```

O gráfico:

[] (Quad.png)

Veja em ação (clique para ativar):

[iframe height = 40 src = "quad" link]

... Ou a curva cúbica ou o evento maior `n`. Aumentar o poder torna-o mais rápido.

Aqui está o gráfico para `progress` na potência` 5`:

! [] (quint.png)

Em ação:

[iframe height = 40 src = "quint" link]

### O arco

Função:

`` `js
função circ (timeFraction) {
retornar 1 - Math.sin (Math.acos (timeFraction));
}
```

O gráfico:

!] (circ.png)

[iframe height = 40 src = "circ" link]

### Voltar: tiro de arco

Esta função faz o "arco tiro". Primeiro, "puxamos a corda do arco", e depois "disparamos".

Ao contrário das funções anteriores, depende de um parâmetro adicional `x`, o" coeficiente de elasticidade ". A distância de "puxar o arco" é definida por ele.

O código:

`` `js
função de volta (x, timeFraction) {
retornar Math.pow (timeFraction, 2) * ((x + 1) * timeFraction - x)
}
```

** O gráfico para `x = 1.5`: **

! [] (back.png)

Para animação, usamos isso com um valor específico de `x`. Exemplo para `x = 1.5`:

[iframe height = 40 src = "back" link]

### Pulo

Imagine que estamos jogando uma bola. Ele cai, depois volta para trás várias vezes e pára.

A função `bounce` faz o mesmo, mas na ordem inversa:" saltando "começa imediatamente. Ele usa poucos coeficientes especiais para isso:

`` `js
função de rebote (timeFraction) {
para (deixe a = 0, b = 1, resultado; 1; a + = b, b / = 2) {
se (timeFraction> = (7 - 4 * a) / 11) {
retornar -Math.pow ((11 - 6 * a - 11 * timeFraction) / 4, 2) + Math.pow (b, 2)
}
}
}
```

Em ação:

[iframe height = 40 src = "bounce" link]

### Animação Elástica

Mais uma função "elástica" que aceita um parâmetro adicional `x` para o" intervalo inicial ".

`` `js
função elástica (x, tempo de fração) {
retorna Math.pow (2, 10 * (timeFraction - 1)) * Math.cos (20 * Math.PI * x / 3 * timeFraction)
}
```

** O gráfico para `x = 1.5`: **
! [] (elastic.png)

Em ação para `x = 1.5`:

[iframe height = 40 src = link "elástico"]

## Reversão: facilidade *

Então, temos uma coleção de funções de temporização. Sua aplicação direta é chamada de "facilidade".

Às vezes, precisamos mostrar a animação na ordem inversa. Isso é feito com a transformação "easeOut".

### easeOut

No modo "easeOut", a função `timing` é colocada em um wrapper` timingEaseOut`:

`` `js
timingEaseOut (timeFraction) = 1 - timing (1 - TimeFraction)
```

Em outras palavras, temos uma função de "transformar" makeEaseOut que leva uma função de temporização "regular" e retorna o wrapper em torno dela:

`` `js
// aceita uma função de temporização, retorna a variante transformada
function makeEaseOut (timing) {
função de retorno (timeFraction) {
retornar 1 - cronometragem (1 - furação temporal);
}
}
```

Por exemplo, podemos usar a função `bounce` descrita acima e aplicá-la:

`` `js
Deixe BounceEaseOut = makeEaseOut (bounce);
```

Então o salto não será no início, mas no final da animação. Parece ainda melhor:

[codetabs src = "bounce-easeout"]

Aqui podemos ver como a transformação altera o comportamento da função:

! [] (bounce-inout.png)

Se houver um efeito de animação no início, como saltar - será mostrado no final.

No gráfico acima, o <span style = "color: # EE6B47"> rebote regular </ span> tem a cor vermelha e o <span style = "color: # 62C0DC"> easeOut bounce </ span> é azul.

- Rejeição regular - o objeto salta na parte inferior e, no final, salta bruscamente até o topo.
- Depois de `easeOut` - primeiro pula para o topo, então salta para lá.

### easeInOut

Também podemos mostrar o efeito tanto no início como no final da animação. A transformação se chama "easeInOut".

Dada a função de temporização, calculamos o estado da animação como este:

`` `js
if (timeFraction <= 0.5) {// primeira metade da animação
tempo de retorno (2 * timeFraction) / 2;
} else {// segunda metade da animação
retorno (2 - timing (2 * (1 - timeFraction))) / 2;
}
```

O código do wrapper:

`` `js
function makeEaseInOut (timing) {
função de retorno (timeFraction) {
se (timeFraction <.5)
tempo de retorno (2 * timeFraction) / 2;
outro
retorno (2 - timing (2 * (1 - timeFraction))) / 2;
}
}

bounceEaseInOut = makeEaseInOut (salto);
```

Em ação, `bounceEaseInOut`:

[codetabs src = "bounce-easeinout"]

A transformação "easeInOut" junta dois gráficos em um: `easeIn` (regular) para a primeira metade da animação e` easeOut` (invertido) - para a segunda parte.

O efeito é claramente visto se compararmos os gráficos de `easeIn`,` easeOut` e `easeInOut` da função de temporização` circ`:

! [] (circ-ease.png)

- <span style = "color: # EE6B47"> Red </ span> é o variantof `circ` regular (` easeIn`).
- <span style = "color: # 8DB173"> Verde </ span> - `easeOut`.
- <span style = "color: # 62C0DC"> Azul </ span> - `easeInOut`.

Como podemos ver, o gráfico da primeira metade da animação é o "facilidade" reduzido, e a segunda metade é a redução do 'facilidade'. Como resultado, a animação começa e termina com o mesmo efeito.

## Mais interessante "desenhar"

Em vez de mover o elemento, podemos fazer outra coisa. Tudo o que precisamos é escrever a gravação do "desenho" adequado.

Aqui está o texto de texto animado "saltando":

[codetabs src = "text"]

## Resumo

A animação de JavaScript deve ser implementada via `requestAnimationFrame`. Esse método incorporado permite configurar uma função de retorno de chamada para executar quando o navegador preparará uma repetição. Normalmente, isso é muito breve, mas o tempo exato depende do navegador.

Quando uma página está em segundo plano, não há repintantes, portanto, o retorno de chamada não será executado: a animação será suspensa e não consumirá recursos. Isso é ótimo.

Aqui está a função `animate` do auxiliar para configurar a maioria das animações:

`` `js
função animada ({timing, desenho, duração}) {

Deixe start = performance.now ();

requestAnimationFrame (função animate (time) {
// timeFraction vai de 0 a 1
Deixe TimeFraction = (time-start) / duration;
se (timeFraction> 1) timeFraction = 1;

// calcula o estado atual da animação
Deixe o progresso = tempo (timeFraction);

desenhar (progresso); // desenhe isso

se (timeFraction <1) {
requestAnimationFrame (animate);
}

});
}
```

Opções:

- `duration` - o tempo total de animação em ms.
- `timing` - a função para calcular o progresso da animação. Obtém uma fração de tempo de 0 a 1, retorna o progresso da animação, geralmente de 0 a 1.
- `draw` - a função para desenhar a animação.

Certamente poderíamos melhorar, adicionar mais sinos e assobios, mas as animações de JavaScript não são aplicadas diariamente. Eles são usados ​​para fazer algo interessante e não padrão. Então, você gostaria de adicionar os recursos que você precisa quando precisa deles.

As animações de JavaScript podem usar qualquer função de temporização. Cobrimos muitos exemplos e transformações para torná-los ainda mais versáteis. Ao contrário do CSS, não estamos limitados às curvas de Bezier aqui.

O mesmo é sobre "desenhar": podemos animar qualquer coisa, não apenas as propriedades CSS.
