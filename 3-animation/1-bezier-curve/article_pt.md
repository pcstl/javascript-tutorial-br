# Curva de Bezier

As curvas Bezier são usadas em computação gráfica para desenhar formas, animação CSS e em muitos outros lugares.

Eles são realmente uma coisa muito simples, vale a pena estudar uma vez e depois sentir-se confortável no mundo dos gráficos vetoriais e animações avançadas.

[cortar]

## Pontos de controle

A [curva de bezier] (https://en.wikipedia.org/wiki/B%C3%A9zier_curve) é definida por pontos de controle.

Pode haver 2, 3, 4 ou mais.

Por exemplo, dois pontos de curva:

!] [bezier2.png]

Curva de três pontos:

! [] (bezier3.png)

Curva de quatro pontos:

! [] (bezier4.png)

Se você observar de perto essas curvas, você pode notar imediatamente:

1. ** Os pontos não estão sempre na curva. ** Isso é perfeitamente normal, depois veremos como a curva é construída.
2. ** A ordem da curva é igual ao número de pontos menos um **.
Para dois pontos, temos uma curva linear (essa é uma linha reta), para três pontos - curva quadrática (parabólica), para três pontos - curva cúbica.
3. ** Uma curva está sempre dentro do [casco convexo] (https://en.wikipedia.org/wiki/Convex_hull) dos pontos de controle: **

!] [] (bezier4-e.png)! [] (bezier3-e.png)

Por causa dessa última propriedade, nos gráficos por computador é possível otimizar os testes de interseção. Se os cascos convexos não se cruzam, então as curvas também não. Portanto, verificar se o cruzamento de cascos convexos primeiro pode dar um resultado muito rápido "sem interseção". Verificando a interseção ou os cascos convexos é muito mais fácil, porque são retângulos, triângulos e assim por diante (veja a figura acima), figuras muito mais simples do que a curva.

O valor principal das curvas de Bezier para o desenho - movendo os pontos a curva está mudando * de maneira intuitivamente óbvia *.

Tente mover os pontos de controle usando um mouse no exemplo abaixo:

[iframe src = "demo.svg? nocpath = 1 & p = 0,0,0,5,0,0,5,1,1,1" height = 370]

** Como você pode notar, a curva se estende ao longo das linhas tangentes 1 -> 2 e 3 -> 4. **

Após alguma prática, torna-se óbvio como colocar pontos para obter a curva necessária. E ao conectar várias curvas, podemos obter praticamente qualquer coisa.

aqui estão alguns exemplos:

!] [] (bezier-car.png)! [] (bezier-letter.png)! [] (bezier-vase.png)

## Matemáticas

Uma curva de Bezier pode ser descrita usando uma fórmula matemática.

Como veremos em breve - não há necessidade de conhecê-lo. Mas por completo - aqui está.

Dado as coordenadas dos pontos de controle <code> P <sub> i </ sub> </ code>: o primeiro ponto de controle tem coordenadas <code> P <sub> 1 </ sub> = (x <sub> 1 </ sub>, y <sub> 1 </ sub>) </ code>, o segundo: <code> P <sub> 2 </ sub> = (x <sub> 2 </ sub>, y <sub> 2 </ sub>) </ code>, e assim por diante, as coordenadas da curva são descritas pela equação que depende do parâmetro `t` do segmento` [0,1] `.

- A fórmula para uma curva de 2 pontos:

<code> P = (1-t) P <sub> 1 </ sub> + tP <sub> 2 </ sub> </ code>
- Para três pontos:

<code> P = (1-t) <sup> 2 </ sup> P <sub> 1 </ sub> + 2 (1-t) tP <sub> 2 </ sub> + t <sup> 2 < / sup> P <sub> 3 </ sub> </ code>
- Para quatro pontos:

<code> P = (1-t) <sup> 3 </ sup> P <sub> 1 </ sub> + 3 (1-t) <sup> 2 </ sup> tP <sub> 2 </ sub > +3 (1-t) t <sup> 2 </ sup> P <sub> 3 </ sub> + t <sup> 3 </ sup> P <sub> 4 </ sub> </ code>

Estas são equações vetoriais.

Podemos reescrevê-los coordenadas por coordenadas, por exemplo, a curva de 3 pontos:

- <cod> x = (1-t) <sup> 2 </ sup> x <sub> 1 </ sub> + 2 (1-t) tx <sub> 2 </ sub> + t <sup> 2 </ sup> x <sub> 3 </ sub> </ code>
- <code> y = (1-t) <sup> 2 </ sup> y <sub> 1 </ sub> + 2 (1-t) ty <sub> 2 </ sub> + t <sup> 2 </ sup> y <sub> 3 </ sub> </ code>

Em vez de <code> x <sub> 1 </ sub>, y <sub> 1 </ sub>, x <sub> 2 </ sub>, y <sub> 2 </ sub>, x <sub> 3 </ sub>, y <sub> 3 </ sub> </ code> devemos colocar coordenadas de 3 pontos de controle.

Por exemplo, se os pontos de controle forem `(0,0)`, `(0.5, 1)` e `(1, 0)`, as equações são:

- <código> x = (1-t) <sup> 2 </ sup> * 0 + 2 (1-t) t * 0,5 + t <sup> 2 </ sup> * 1 = (1-t) t + t <sup> 2 </ sup> = t </ code>
- <code> y = (1-t) <sup> 2 </ sup> * 0 + 2 (1-t) t * 1 + t <sup> 2 </ sup> * 0 = 2 (1-t) t = -t <sup> 2 </ sup> + 2t </ code>

Agora, como `t` corre de` 0 'para `1, o conjunto de valores` (x, y) `para cada` t` forma a curva.

Isso provavelmente é muito científico, não é muito óbvio por que as curvas são assim e como elas dependem de pontos de controle.

Então, aqui está o algoritmo de desenho que pode ser mais fácil de entender.

## De Casteljau's algorithm

[O algoritmo de De Casteljau] (https://en.wikipedia.org/wiki/De_Casteljau%27s_algorithm) é idêntico à definição matemática da curva, mas mostra visualmente como é construído.

Vamos ver isso no exemplo de 3 pontos.

Aqui está a demonstração, e a explicação segue.

Os pontos podem ser movidos pelo mouse. Pressione o botão "play" para executá-lo.

[iframe src = "demo.svg? p = 0,0,0,5,1,1,0 & animate = 1" height = 370]

** Algoritmo de De Casteljau para construir a curva do bezier de 3 pontos: **

1. Desenhe os pontos de controle. Na demo acima, eles são rotulados: `1`,` 2`, `3`.
2. Crie segmentos entre os pontos de controle 1 -> 2 -> 3. Na demo acima eles são <span style = "color: # 825E28"> brown </ span>.
3. O parâmetro `t` move de` 0` para `1`. No exemplo acima, o passo `0.05 é usado: o loop passa por '0, 0.05, 0.1, 0.15, ... 0.95, 1`.

Para cada um desses valores de `t`:

- Em cada segmento <span style = "color: # 825E28"> marrom </ span> tomamos um ponto localizado na distância proporcional a `t 'desde o início. Como há dois segmentos, temos dois pontos.

Por exemplo, para `t = 0` - ambos os pontos serão no início dos segmentos e para` t = 0.25` - nos 25% do comprimento do segmento desde o início, para `t = 0.5` - 50 % (o meio), para `t = 1` - no final dos segmentos.

- Conecte os pontos. Na imagem abaixo, o segmento de conexão é pintado <span style = "color: # 167490"> azul </ span>.


| Para `t = 0,25` | Para `t = 0,5` |
| ------------------------ | ---------------------- |
| !] [] (bezier3-draw1.png) | !] [] (bezier3-draw2.png) |

4. Agora, o segmento <span style = "color: # 167490"> azul </ span> leva um ponto na distância proporcional ao mesmo valor de `t`. Ou seja, para `t = 0.25` (a imagem esquerda), temos um ponto no final do trimestre esquerdo do segmento e para` t = 0.5` (a imagem certa) - no meio do segmento. Nas imagens acima desse ponto é <span style = "color: red"> vermelho </ span>.

5. Como `t` é executado de` 0` para `1`, cada valor de` t` adiciona um ponto à curva. O conjunto de tais pontos forma a curva de Bezier. É vermelho e parabólico nas imagens acima.

Esse foi um processo para 3 pontos. Mas o mesmo é para 4 pontos.

A demonstração para 4 pontos (os pontos podem ser movidos pelo mouse):

[iframe src = "demo.svg? p = 0,0,0.5,0,0,5,1,1,1 & animate = 1" height = 370]

O algoritmo:

- Os pontos de controle estão conectados por segmentos: 1 -> 2, 2 -> 3, 3 -> 4. Temos segmentos <span style = "color: # 825E28"> marrom </ span>.
- Para cada `t` no intervalo de` 0` para `1`:
- Tomamos pontos nesses segmentos na distância proporcional ao `t 'desde o início. Esses pontos estão conectados, de modo que nós temos dois <span style = "color: # 0A0"> segmentos verdes </ span>.
- Nesses segmentos tomamos pontos proporcionais a `t`. Obtemos um <span style = "color: # 167490"> segmento azul </ span>.
- No segmento azul, tomamos um ponto proporcional ao `t`. No exemplo acima é <span style = "color: red"> vermelho </ span>.
- Estes pontos juntos formam a curva.

O algoritmo é recursivo e pode ser generalizado para qualquer número de pontos de controle.

Dado N dos pontos de controle, nós os conectamos para obter N-1 segmentos iniciais.

Então, para cada `t` de` 0` para `1`:
- Pegue um ponto em cada segmento na distância proporcional a `t` e conecte-os - haverá N-2 segmentos.
- Pegue um ponto em cada um desses segmentos na distância proporcional a `t` e conecte - haverá N-3 segmentos, e assim por diante ...
- Até que tenhamos um ponto. Esses pontos fazem a curva.

Mover exemplos de curvas:

[iframe src = "demo.svg? p = 0,0,0,0.75,0.25,1,1,1 & animate = 1" height = 370]

Com outros pontos:

[iframe src = "demo.svg? p = 0,0,1,0.5,0,0.5,1,1 & animate = 1" height = 370]

Forma de loop:

[iframe src = "demo.svg? p = 0,0,1,0.5,0,1,0.5,0 & animate = 1" height = 370]

Não é liso Curva Bezier:

[iframe src = "demo.svg? p = 0,0,1,1,0,1,1,0 & animate = 1" height = 370]

Como o algoritmo é recursivo, podemos construir curvas Bezier de qualquer ordem: usando 5, 6 ou mais pontos de controle. Mas, na prática, eles são menos úteis. Normalmente, nós levamos 2-3 pontos e, para linhas complexas, colam várias curvas juntas. Isso é mais simples de desenvolver e calcular.

`` `cabeçalho inteligente =" Como desenhar uma curva * através de * pontos dados? "
Usamos pontos de controle para uma curva de Bezier. Como podemos ver, eles não estão na curva. Ou, para ser preciso, o primeiro e o último pertencem à curva, mas outros não.

Às vezes, temos outra tarefa: desenhar uma curva * através de vários pontos *, de modo que todos eles estejam em uma única curva suave. Essa tarefa é chamada de [interpolação] (https://en.wikipedia.org/wiki/Interpolation), e aqui não a cobrimos.

Existem fórmulas matemáticas para tais curvas, por exemplo [polinômio Lagrange] (https://en.wikipedia.org/wiki/Lagrange_polynomial).

Em gráficos de computador [interpolação de spline] (https://en.wikipedia.org/wiki/Spline_interpolation) é freqüentemente usado para criar curvas suaves que conectam muitos pontos.
```

## Resumo

As curvas de Bezier são definidas pelos seus pontos de controle.

Vimos duas definições das curvas de Bezier:

1. Usando fórmulas matemáticas.
2. Usando um processo de desenho: o algoritmo de De Casteljau

Boas propriedades das curvas de Bezier:

- Podemos desenhar linhas suaves com um mouse movendo-se em torno dos pontos de controle.
- Formas complexas podem ser feitas de várias curvas Bezier.

Uso:

- Em computação gráfica, modelagem, editores de gráficos vetoriais. Fontes são descritas pelas curvas de Bezier.
- No desenvolvimento web - para gráficos em Canvas e no formato SVG. A propósito, os exemplos "ao vivo" acima estão escritos em SVG. Eles são, na verdade, um único documento SVG que possui diferentes pontos como parâmetros. Você pode abri-lo em uma janela separada e ver a fonte: [demo.svg] (demo.svg? P = 0,0,1,0.5,0,0.5,1,1 & animate = 1).
- Na animação CSS para descrever o caminho e a velocidade da animação.
