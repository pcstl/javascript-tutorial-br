
O algoritmo parece simples:
1. Coloque os manipuladores `onmouseover / out` no elemento. Também pode usar `onmouseenter / leave` aqui, mas eles são menos universais, não funcionarão se introduzirmos a delegação.
2. Quando um cursor do mouse inseriu o elemento, comece a medir a velocidade no `mousemove`.
3. Se a velocidade for lenta, execute "over".
4. Mais tarde, se estivermos fora do elemento, e `over` foi executado, execute` out`.

A questão é: "Como medir a velocidade?"

A primeira idéia seria: executar nossa função a cada 100 ms e medir a distância entre coordenadas anteriores e novas. Se é pequeno, então a velocidade é pequena.

Infelizmente, não há como obter "coordenadas de mouse atuais" em JavaScript. Não há nenhuma função como `getCurrentMouseCoordinates ()`.

A única maneira de obter coordenadas é ouvir eventos do mouse, como `mousemove`.

Então, podemos configurar um manipulador no `mousemove` para rastrear as coordenadas e lembrá-las. Então podemos compará-los, uma vez por `100ms`.

P.S. Observe: os testes de solução usam `dispatchEvent` para ver se a dica de ferramenta funciona corretamente.
