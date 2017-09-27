Diferenças:

1. `clientWidth` é numérico, enquanto` getComputedStyle (elem) .width` retorna uma string com `px` no final.
2. `getComputedStyle` pode retornar largura não numérica como` "auto" `para um elemento inline.
3. `clientWidth` é a área de conteúdo interno do elemento mais cofres, enquanto a largura CSS (com" box-sizing padrão ") é a área interna e sem ambiente * sem paddings *.
4. Se houver uma barra de rolagem e o navegador se reserva o espaço para isso, algum navegador suporta esse espaço de largura CSS (porque não está disponível para conteúdo mais), e outros não. A propriedade `clientWidth` é sempre a mesma: o tamanho da barra de rolagem é subtraído se reservado.
