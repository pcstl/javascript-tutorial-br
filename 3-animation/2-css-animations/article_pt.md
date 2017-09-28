# CSS-animações

As animações CSS permitem fazer animações simples sem JavaScript.

O JavaScript pode ser usado para controlar a animação CSS e torná-los ainda melhores com um pouco de código.

[cortar]

## CSS transitions [# css-transition]

A idéia de transições CSS é simples. Nós descrevemos uma propriedade e como suas mudanças devem ser animadas. Quando a propriedade muda, o navegador pinta a animação.

Isto é: tudo o que precisamos é mudar a propriedade. E a transição fluente é feita pelo navegador.

Por exemplo, o CSS abaixo anima as mudanças de `background-color` por 3 segundos:

`` `css
.animado {
transition-property: background-color;
duração de transição: 3s;
}
```

Agora, se um elemento tiver uma classe `.animated`, qualquer alteração de` background-color` é animada durante 3 segundos.

Clique no botão abaixo para animar o plano de fundo:

`` `html executar autorun height = 60
<button id = "color"> clique em mim </ button>

<style>
#cor {
transition-property: background-color;
duração de transição: 3s;
}
</ style>

<script>
color.onclick = function () {
this.style.backgroundColor = 'vermelho';
};
</ script>
```

Existem 5 propriedades para descrever transições CSS:

- `transição de propriedade '
- `duração de transição '
- `transição-timing-function`
- `atraso de transição '

Vamos cobri-los em um momento, por agora vamos notar que a propriedade comum de `transição 'permite declará-los juntos na ordem:` tempo de duração da propriedade - atraso da função', e também animar múltiplas propriedades ao mesmo tempo.

Por exemplo, este botão anima tanto a `cor 'quanto a` font-size`:

`` `html run height = 80 autorun no-embellecer
<button id = "growing"> Clique-me </ button>

<style>
#crescendo {
*!*
transição: font-size 3s, color 2s;
*/!*
}
</ style>

<script>
growing.onclick = function () {
this.style.fontSize = '36px';
this.style.color = 'red';
};
</ script>
```

Agora, cubramos os recursos de animação um a um.

## transição-propriedade

Em `transition-property`, escrevemos uma lista de propriedades para animar, por exemplo:` left`, `margin-left`,` height`, `color`.

Nem todas as propriedades podem ser animadas, mas [muitas delas] (http://www.w3.org/TR/css3-transitions/#animatable-properties-). O valor `all` significa" animar todas as propriedades ".

## duração da transição

Em `transition-duration` podemos especificar quanto tempo a animação deve levar. O tempo deve estar em [formato de hora CSS] (http://www.w3.org/TR/css3-values/#time): em segundos `s` ou milissegundos` ms`.

## transição-atraso

Em `delay de transição 'podemos especificar o atraso * antes * da animação. Por exemplo, se `transition-delay: 1s`, a animação começa após 1 segundo após a alteração.

Valores negativos também são possíveis. Então a animação começa a partir do meio. Por exemplo, se `transition-duration` for` 2s`, e o delay for `-1s`, a animação leva 1 segundo e começa a partir da metade.

Aqui está a animação muda números de `0 'para` 9' usando a propriedade CSS `tradutor`:

[codetabs src = "digits"]

A propriedade `transform` está animada assim:

`` `css
# stripe.animate {
transformar: traduzir (-90%);
transição-propriedade: transformar;
TransitDirect: Cut;
}
```

No exemplo acima, o JavaScript adiciona a classe `.animate` ao elemento - e a animação começa:

`` `js
stripe.classList.add ('animate');
```

Podemos também iniciá-lo "do meio", do número exato, e. correspondente ao segundo atual, usando o "atraso de transição" negativo.

Aqui se você clicar no dígito - ele inicia a animação a partir do segundo atual:

[codetabs src = "digits-negative-delay"]

O JavaScript faz isso por uma linha extra:

`` `js
stripe.onclick = function () {
deixe sec = new Date (). getSeconds ()% 10;
*!*
// por exemplo, -3s aqui inicia a animação a partir do 3º segundo
stripe.style.transitionDelay = '-' + seg + 's';
*/!*
stripe.classList.add ('animate');
};
```

## função de transição-temporização

A função Timing descreve como o processo de animação é distribuído ao longo do tempo. Será que vai começar devagar e depois vai rápido ou vice-versa.

Essa é a propriedade mais complicada desde a primeira vista. Mas torna-se muito simples se dedicarmos um pouco de tempo a isso.

Essa propriedade aceita dois tipos de valores: uma curva de Bezier ou etapas. Vamos começar a partir da curva, pois é usado com mais freqüência.

### Curva Bezier

A função de temporização pode ser definida como uma [curva Bezier] (/ bezier-curve) com 4 pontos de controle que satisfazem as condições:

1. Primeiro ponto de controle: `(0,0)`.
2. Último ponto de controle: `(1,1)`.
3. Para pontos intermediários, os valores de `x` devem estar no intervalo` 0..1`, `y` pode ser qualquer coisa.

A sintaxe de uma curva Bezier em CSS: `beiculto cúbico (x2, y2, x3, y3)`. Aqui precisamos especificar apenas os pontos de controle 2º e 3º, porque o 1º é fixado em `(0,0)` e o 4º é `(1,1)`.

A função de temporização descreve a velocidade do processo de animação no tempo.

- O eixo `x` é o tempo:` 0` - o momento inicial, `1` - o último momento de` transição-duração '.
- O eixo `y especifica a conclusão do processo:` 0` - o valor inicial da propriedade, `1` - o valor final.

A variante mais simples é quando a animação vai uniformemente, com a mesma velocidade linear. Isso pode ser especificado pela curva `beiculto cúbico (0, 0, 1, 1)`.

Veja como essa curva se parece:

!] [] (bezier-linear.png)

... Como podemos ver, é apenas uma linha reta. À medida que passa o tempo (`x`), a conclusão (` y`) da animação vai de "0" para "1".

O trem no exemplo abaixo vai da esquerda para a direita com a velocidade permanente (clique nele):

[codetabs src = "train-linear"]

O CSS `transition` é baseado nessa curva:

`` `css
.trem {
esquerda: 0;
transição: 5 cv cúbico-bezier (0, 0, 1, 1);
/ * O JavaScript define a esquerda para 450px * /
}
```

... E como podemos mostrar um trem desacelerando?

Podemos usar outra curva de Bezier: `beicot cúbico (0.0, 0.5, 0.5, 1.0)`.

O gráfico:

!] [] (train-curve.png)

Como podemos ver, o processo começa rápido: a curva aumenta, e depois mais lenta e mais lenta.

Aqui está a função de temporização em ação (clique no trem):

[codetabs src = "train"]

CSS:
`` `css
.trem {
esquerda: 0;
transição: 5 polegadas cúbico-bezier (0, .5, .5, 1);
/ * O JavaScript define a esquerda para 450px * /
}
```

Existem várias curvas embutidas: `linear`,` facilidade ', `facilidade',` facilidade 'e `facilidade de entrada'.

O `linear` é uma abreviatura para` beiculto cúbico (0, 0, 1, 1) `- uma linha reta, já a vimos.

Outros nomes são abreviaturas para o seguinte `cubic-bezier`:

| <code> facilidade </ code> <sup> * </ sup> | <code> ease-in </ code> | <code> easy-out </ code> | <code> easy-in-out </ code> |
|-------------------------------|----------------------|-----------------------|--------------------------|
| <code> (0.25, 0.1, 0.25, 1.0) </ code> | <code> (0.42, 0, 1.0, 1.0) </ code> | <code> (0, 0, 0.58, 1.0) </ code> | <code> (0.42, 0, 0.58, 1.0) </ code> |
| ! [facilidade, figura] (facilidade.png) | ! [facilidade, figura] (facilidade-in.png) | ! [facilidade, figura] (facilidade-out.png) | ! [facilidade de entrada, figura] (facilidade-em-out.png) |

`*` - por padrão, se não houver função de temporização, a "facilidade" é usada.

Então, poderíamos usar `easy-out` para o nosso trem de desaceleração:


`` `css
.trem {
esquerda: 0;
transição: esquerda 5s facilidade;
/ * transição: esquerdo 5s cúbico-bezier (0, .5, .5, 1); * /
}
```

Mas parece um pouco diferente.

** Uma curva Bezier pode fazer a animação "pular" da sua gama. **

Os pontos de controle na curva podem ter quaisquer coordenadas `y`: mesmo negativas ou enormes. Então, a curva de Bezier também pulará muito baixa ou alta, tornando a animação além da sua faixa normal.

No exemplo abaixo, o código de animação é:
`` `css
.trem {
esquerda: 100px;
transição: esquerdo 5s cúbico-bezier (.5, -1, .5, 2);
/ * JavaScript define esquerda para 400px * /
}
```

A propriedade `left` deve animar de` 100px` para `400px`.

Mas se você clicar no trem, você verá isso:

- Primeiro, o trem passa * de volta *: `left` torna-se inferior a` 100px`.
- Então ele vai para frente, um pouco mais do que `400px`.
- E depois novamente - para `400px`.

[codetabs src = "train-over"]

Por que isso acontece - bastante óbvio se olharmos para o gráfico da curva de Bezier dada:

!] [] (bezier-train-over.png)

Movemos a coordenada `y 'do segundo ponto abaixo de zero, e para o terceiro ponto que fizemos colocamos sobre` 1, então a curva sai do quadrante "regular". O `y` está fora do intervalo" padrão "0..1`.

Como sabemos, `` `mede" a conclusão do processo de animação ". O valor `y = 0` corresponde ao valor da propriedade inicial e` y = 1` - o valor final. Então, os valores `y <0` movem a propriedade mais baixa do que a partida` `esquerda 'e` y> 1` - sobre a final `esquerda`.

Essa é uma variante "suave" com certeza. Se colocarmos valores `y` como` -99` e `99`, então o trem salta do alcance muito mais.

Mas como fazer a curva Bezier para uma tarefa específica? Existem muitas ferramentas. Por exemplo, podemos fazê-lo no site <http://cubic-bezier.com/>.

### Passos

Função de temporização `etapas (número de etapas [, início / fim])` permite dividir a animação em etapas.

Vamos ver isso em um exemplo com dígitos. Vamos fazer com que os dígitos não mudem de forma suave, mas de forma discreta.

Para isso dividimos a animação em 9 etapas:

`` `css
# stripe.animate {
transformar: traduzir (-90%);
transição: transforma 9s *! * etapas (9, start) * /! *;
}
```

Na ação `passo (9, start)`:

[codetabs src = "step"]

O primeiro argumento de `etapas 'é o número de etapas. A transformação será dividida em 9 partes (10% cada). O intervalo de tempo também está dividido: 9 segundos divididos em intervalos de 1 segundo.

O segundo argumento é uma das duas palavras: `start` ou` end`.

O `start` significa que, no início da animação, precisamos fazer o primeiro passo imediatamente.

Podemos observar que durante a animação: quando clicamos no dígito, ele muda para `1` (o primeiro passo) imediatamente e depois muda no início do próximo segundo.

O processo está progredindo assim:

- `0s` -` -10% `(primeira alteração no início do 1º segundo, imediatamente)
`1s` - ー ー 20%`
- ...
- `8s` -` -80%`
- (o último segundo mostra o valor final).

O valor alternativo "fim" significaria que a mudança deveria ser aplicada não no início, mas no final de cada segundo.

Então, o processo seria assim:

- `0s` -` 0`
- `1s` -` -10% `(primeira alteração no final do 1º segundo)
`2s` - ー ー 20%`
- ...
- `9s` -` -90% a

Na ação `passo (9, fim)`:

[codetabs src = "step-end"]

Há também valores abreviados:

- `step-start` - é o mesmo que` steps (1, start) `. Ou seja, a animação começa imediatamente e leva 1 passo. Então, começa e termina imediatamente, como se não houvesse animação.
- `step-end` - o mesmo que` steps (1, end) `: faça a animação em uma única etapa no final de` transition-duration`.

Esses valores raramente são usados, porque isso não é realmente animação, mas sim uma mudança de um único passo.

## Event transitionend

Quando a animação CSS termina os disparadores do evento `transitionend`.

É amplamente utilizado para fazer uma ação depois que a animação é feita. Também podemos juntar-se a animações.

Por exemplo, o navio no exemplo abaixo começa a nadar lá e de volta ao clique, cada vez mais e mais à direita:

[iframe src = "barco" height = 300 editar link]

A animação é iniciada pela função `go` que re-executa cada vez que a transição termina e vira a direção:

`` `js
boat.onclick = function () {
//...
deixe times = 1;

function go () {
se (vezes% 2) {
// navega para a direita
boat.classList.remove ('back');
boat.style.marginLeft = 100 * times + 200 + 'px';
} outro {
// nadar para a esquerda
boat.classList.add ('back');
boat.style.marginLeft = 100 * times - 200 + 'px';
}

}

ir();

boat.addEventListener ('transitionend', function () {
vezes ++;
ir();
});
};
```

O objeto de evento para `transitionend` tem poucas propriedades específicas:

`event.propertyName`
: A propriedade que acabou de animar. Pode ser bom se nós animamos várias propriedades ao mesmo tempo.

`event.elapsedTime`
: O tempo (em segundos) que a animação teve, sem "atraso de transição".

## Keyframes

Podemos juntar múltiplas animações simples em conjunto usando a regra CSS `@ keyframes`.

Especifica o "nome" da animação e regras: o que, quando e onde animar. Em seguida, usando a propriedade `animation`, anexamos a animação ao elemento e especificamos parâmetros adicionais para isso.

Aqui está um exemplo com explicações:

`` `html run height = 60 autorun =" no-epub "não-embelezar
<div class = "progress"> </ div>

<style>
*!*
@keyframes go-left-right {/ * atribua-lhe um nome: "go-left-right" * /
de {left: 0px; } / * animar da esquerda: 0px * /
para {esquerda: calc (100% - 50px); } / * animar para a esquerda: 100% -50px * /
}
*/!*

.progresso {
*!*
animação: ir-esquerda-direita 3s infinito alternativo;
/ * aplique a animação "go-left-right" para o elemento
duração 3 segundos
número de vezes: infinito
direção alternada toda vez
*/
*/!*

posição: relativa;
borda: 2px verde sólido;
largura: 50px;
altura: 20px;
fundo: limão;
}
</ style>
```

Existem muitos artigos sobre @ keyframes` e uma [especificação detalhada] (https://drafts.csswg.org/css-animations/).

Provavelmente, você não precisará de "quadros-chave" sempre, a menos que tudo esteja no movimento constante em seus sites.

## Resumo

As animações CSS permitem suavemente (ou não) animar mudanças de uma ou várias propriedades CSS.

Eles são bons para a maioria das tarefas de animação. Também podemos usar JavaScript para animações, o próximo capítulo é dedicado a isso.

Limitações das animações CSS em comparação com animações JavaScript:

`` `compare plus =" Animações CSS "menos =" Animações JavaScript "
+ Simples coisas feitas simplesmente.
+ Rápido e leve para CPU.
- As animações de JavaScript são flexíveis. Eles podem implementar qualquer lógica de animação, como uma "explosão" de um elemento.
- Não apenas alterações de propriedade. Podemos criar novos elementos em JavaScript para fins de animação.
```

A maioria das animações pode ser implementada usando CSS como descrito neste capítulo. E o evento `transitionend` permite executar JavaScript após a animação, então ele se integra bem com o código.

Mas no próximo capítulo, faremos algumas animações de JavaScript para cobrir casos mais complexos.
